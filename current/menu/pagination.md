---
title: Pagination
layout: default
nav_order: 100
---

<h1>Pagination</h1>

- [Implementing pagination](#implementing-pagination)
  - [The `Paginator` trait](#the-paginator-trait)
  - [The paginator required methods](#the-paginator-required-methods)
    - [`PaginationFetch`](#paginationfetch)
    - [`PaginationCountAll`](#paginationcountall)
    - [`itemAction`](#itemaction)


## Implementing pagination
### The `Paginator` trait
To implement pagination, you will probably need to use the paginator trait. It allows you to quickly deploy a paginable menu.

Note: The `Rejoice` framework handles two types of pagination.
The first one that we call `soft-pagination` is when all the data is retrieved and passed to the framework. It works fine when the data to paginate is small.
The second type of pagination called the `hard-pagination` is the one that requires the use of the first one is when a menu overflows

### The paginator required methods
#### `PaginationFetch`
Retrieve the data from the database. Must return the retrieved data.

#### `PaginationCountAll`
Must Return the total of rows fetched from the database.

#### `itemAction`
When using the paginator trait, instead of the `actions` methods, you will use the `itemAction` method to specify how to insert the actions in your menu. This method will be used under the hood by the actions method to insert the actions automatically for you. 
**Example**
```php
namespace App\Menus;

use Prinx\Rejoice\Menu\Paginator;

class BetsHistory extends Menu
{
    use Paginator;

    /**
     * Number of items to display per page
     */
    protected $maxItemsPerPage = 4;

    public function message()
    {
        if (!$this->paginationTotal()) {
            return "You don't have any bet.";
        } else {
            return $this->isPaginationFirstPage() ? 'Your betting history' : '';
        }
    }

  /**
   * Defines how the items will be displayed to the user.
   * 
   * This method will automatically be called for each rows of the array 
   * returned by `paginationFetch`. And its return value will be added to the 
   * actions 
   * 
   * The option is what will be displayed to the user as option to select.
   * It's automatically handled by the Paginator
   * 
   * @param array $row 
   * @param string $option
   * @return array
   */
    public function itemAction($row, $option)
    {
        return [
            $option => [
                'message' => 'Bet ' . $row['id'],
                'next_menu' => 'bet::bet_details',
                'save_as' => $row['id'],
            ],
        ];
    }

    /**
     * Fetches the items from the database
     * 
     * @return array
     */
    public function paginationFetch()
    {
        if (isset($this->bets)) {
            return $this->bets;
        }

        $req = $this->db()->prepare("SELECT * FROM bets
        WHERE id > :offset
        AND initiator_id = :initiator_id
        ORDER BY id
        LIMIT :limit");

        $offset = $this->lastRetrievedId();
        $userId = $this->user('id');
        $limit = $this->maxItemsPerPage();

        $req->bindParam('offset', $offset, \PDO::PARAM_INT);
        $req->bindParam('initiator_id', $userId);
        $req->bindParam('limit', $limit, \PDO::PARAM_INT);
        $req->execute();

        $this->bets = $req->fetchAll(\PDO::FETCH_ASSOC);

        $req->closeCursor();

        return $this->bets;
    }

    /**
     * Returns the total number of the data to be displayed
     * 
     * @return  int
     */
    public function paginationCountAll()
    {
        if ($total = $this->paginationGet('total')) {
            return $total;
        }

        $req = $this->db()->prepare("SELECT COUNT(*) FROM bets WHERE  initiator_id = ?");
        $req->execute([
            $this->user('id')
        ]);

        $paginationTotal = intval($req->fetchColumn());
        $req->closeCursor();

        $this->paginationSave('total', $paginationTotal);

        return $paginationTotal;
    }
}
```
The paginator makes available some methods for our use:
- `maxItemsPerPage` The maximum number of items that can be showed on the pagination screen; It's configured as protected property of the menu entity;
- `currentItemsCount` The actual number of items showed on the current screen;
- `isPaginationFirstPage` Check if the current screen is the first screen of the pagination;
- `isPaginationLastPage` Check if the current screen is the last screen of the pagination;
- `lastRetrievedId` Get the id of the last fetched row - the next query to the database to fetch, will begin at that index;
- `paginationSave` Saves a data for pagination purpose;
- `paginationGet` Get a pagination data;
- `paginationHas` Check if a pagination data exists;

<div class="note note-warning">When using the pagination, always prefer 

```php
$this->userPreviousResponses('the_name_of_the_previous_menu');
```
to retrieve the response of the previous menu, over

```php
$this->userResponse(); 
//or
$this->userSavedResponse();
```
</div>
