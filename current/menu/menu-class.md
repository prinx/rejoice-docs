---
title: Menu class
layout: default
parent: Menus
nav_order: 42
---

- [Creating the menu entity](#creating-the-menu-entity)
  - [The `message` method](#the-message-method)
  - [The `actions` method](#the-actions-method)
  - [The `validate` method](#the-validate-method)
  - [The `saveAs` method](#the-saveas-method)
  - [The `before` hook](#the-before-hook)
  - [The `after` hook](#the-after-hook)
  - [The `onBack` hook](#the-onback-hook)
  - [The `onMoveToNextMenu` hook](#the-onmovetonextmenu-hook)
  - [Order of the methods](#order-of-the-methods)
We've created our first application and that was pretty cool and rather simple. I'm sure you will need more in your applications.
LNowthat we have all the basics, the rest will not be a big deal.

The first thing that worth knowing is Menu Entities.
A menu entity is an object representaion of a menu. The same menu we created in the `menus.php` file above. We will now create it using rather an object. Actually, the menu entity does not replace the menu defined in the `menus.php` file. Both can coexist. But the parameters defined in the menu entity will for most of them overwrite the ones defined in the menus.php file.

A menu entity inherit from a base menu entity class.

The basic scheme looks like this:

```php
namespace App\Menus;

class InspiringQuote extends Menu
{
  /**
     * Returns the message to display before the actions on the current menu
     *
     * @param \Prinx\Rejoice\UserResponse $userPreviousResponses
     * @return string|array
     */
    public function message($userPreviousResponses)
    {
        return "";
    }

    /**
     * Returns the actions of the current menu
     *
     * @param \Prinx\Rejoice\UserResponse $userPreviousResponses
     * @return array
     */
    public function actions($userPreviousResponses)
    {
        return [];
    }

    /**
     * Allow you to validate the user's response.
     *
     * If it returns false, an invalid input error will be send to the user.
     *
     * @param string $response
     * @return boolean
     */
    public function validate($response)
    {
        return true;
    }

    /**
     * Allow you to modify the user's response before saving it in the session.
     *
     * @param string $response
     * @return mixed
     */
    public function saveAs($response)
    {
        return $response;
    }

    /**
     * `before` hook. Runs before any other method in the current menu.
     * You will usually retrieve data to display in database here from database here
     * @param \Prinx\Rejoice\UserResponse $userPreviousResponses
     * @return void
     */
    public function before($userPreviousResponses)
    {
        //
    }

    /**
     * `after` hook. Runs after the user has sent their response.
     * This method runs anytime we move from the current menu to another menu.
     * It means even when we move from this menu to go back, it will run.
     * Sometimes it is useful, Sometimes that is not the behavior we want. 
     * We rather want a method to run a method only when we are moving to a 
     * real next_menu, not a previous menu or the welcome menu. In that 
     * particular case, we will use the `onMoveToNextMenu` method.
     * 
     * @param string $response The response given by the user on this menu screen
     * @param \Prinx\Rejoice\UserResponse $userPreviousResponses All the previous responses given by the user, attached to their menu name.
     * @return void
     */
    public function after($response, $userPreviousResponses)
    {
        //
    }

    /**
     * Similar to the after method but will run only when moving from this menu 
     * to a real next_menu. We call a real next_menu, a menu that has been 
     * defined by you. It then excludes menus like `__back`, `__welcome`, 
     * `__same`, `__paginate_forward`, `__paginate_back`, which are 
     * automagically created and managed by the framework.
     * 
     * @param string $response
     * @param \Prinx\Rejoice\UserResponse $userPreviousResponses
     * @return void
     */
    public function onMoveToNextMenu($response, $userPreviousResponses)
    {
      //
    }

    /**
     * Method to run when moving back.
     * 
     * @param \Prinx\Rejoice\UserResponse $userPreviousResponses
     * @return void
     */
    public function onBack($userPreviousResponses)
    {
      //
    }
}

```
All these methods are optional. Use the one you want when you need it.

### Creating the menu entity 

#### The `message` method

#### The `actions` method

#### The `validate` method

#### The `saveAs` method

#### The `before` hook

#### The `after` hook

#### The `onBack` hook

#### The `onMoveToNextMenu` hook

#### Order of the methods
While writing your menu entity logic, it is important to know the order in which the menu entity methods are called.
This is the order in which the menu entity methods are called.

Methods called before the menu is rendered to the user:
- `before`
- `message`
- `actions`

After the menu is rendered to the user **and the user has sent a response back**:
- `validate`
- `saveAs`
- `after`
- `onMoveToNextMenu`


Knowing the order of the menu entity methods helps you to know how to handle your own-created menu entity properties.

<div class="note note-primary">
You need to consider this:

Any property of the menu entity is accessible in any of the menu entity methods.

Any property created or updated in a `before` method (before, message or actions) will not be reflect in an `after` method (validate, saveAs, after, onMoveToNextMenu, etc.)

Any property created or updated in a particular method will not be updated get the update in a method that has already run, according to the order of the methods (for example, an update of a property in the after method will not reflect in the validate method, because at the time the after method is running, the validate method has already run).
</div>
