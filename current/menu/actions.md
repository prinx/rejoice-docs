---
title: Menu actions
layout: default
parent: Menus
nav_order: 43
---
<h1>Actions</h1>

- [Inserting predefined actions](#inserting-predefined-actions)
  - [Inserting Back action](#inserting-back-action)
  - [Inserting Welcome action](#inserting-welcome-action)
  - [Inserting Paginate forward actions](#inserting-paginate-forward-actions)
  - [Inserting Paginate back actions](#inserting-paginate-back-actions)
- [Magic menus](#magic-menus)
- [Later menu](#later-menu)
- [Calling a remote USSD application](#calling-a-remote-ussd-application)


## Inserting predefined actions
The available predefined actions are:
- backAction
- mainMenuAction
- paginateForwardAction
- paginateBackAction

### Inserting Back action
You can add a go-back action to your actions by using the `__back` next menu or by calling the `$this->backAction()` method.

```php
namespace App\Menus;

class ProcessBalanceRequest extends Menu
{
  public function actions($userPreviousResponses)
  {
    $actions = [
      // Define your actions here
      
      // Add back action
      '0' => [
        'display' => 'Back',
        'next_menu' => '__back',
      ]
    ];

    return $actions;
  }
}
```
The trigger is `0` and the message attached is `Back`.

or

```php
namespace App\Menus;

class ProcessBalanceRequest extends Menu
{
  public function actions($userPreviousResponses)
  {
    $actions = [
      // Define your actions here
    ];

    // Add back action
    $actions = $this->mergeAction($actions, $this->backAction());

    return $actions;
  }
}
```
The method will return the exact array that we harcoded in the first example.
You can modify the trigger by passing your custom trigger to the method as first parameter. You can custom the message attached by passing the sustom message as second parameter.

### Inserting Welcome action
You can add a go-to-welcome action to your actions by using the `__welcome` next menu or by calling the `$this->mainMenuAction()` method.

```php
namespace App\Menus;

class ProcessBalanceRequest extends Menu
{
  public function actions($userPreviousResponses)
  {
    $actions = [
      // Define your actions here
      
      // Add back action
      '00' => [
        'display' => 'Main menu',
        'next_menu' => '__welcome',
      ]
    ];

    return $actions;
  }
}
```
The trigger is `00` and the message attached is `Main menu`.

or

```php
namespace App\Menus;

class ProcessBalanceRequest extends Menu
{
  public function actions($userPreviousResponses)
  {
    $actions = [
      // Define your actions here
    ];

    // Add main menu action
    $actions = $this->mergeAction($actions, $this->mainMenuAction());

    return $actions;
  }
}
```
### Inserting Paginate forward actions
You can add a go-forward-in-pagination action by using the `__paginate_forward` next menu or by calling the `$this->paginateForwardAction()` method.

```php
namespace App\Menus;

class ProcessBalanceRequest extends Menu
{
  public function actions($userPreviousResponses)
  {
    $actions = [
      // Define your actions here
      
      // Add back action
      '00' => [
        'display' => 'More',
        'next_menu' => '__paginate_forward',
      ]
    ];

    return $actions;
  }
}
```
Since we are going back, we are using the same trigger and message attached as the normal go-back action. The trigger is `0` and the message attached is `Back`.

or

```php
namespace App\Menus;

class ProcessBalanceRequest extends Menu
{
  public function actions($userPreviousResponses)
  {
    $actions = [
      // Define your actions here
    ];

    // Add paginate back action
    $actions = $this->mergeAction($actions, $this->paginateBackAction());

    return $actions;
  }
}
```
### Inserting Paginate back actions
You can add a go-to-welcome action to your actions by using the `__paginate_back` next menu or by calling the `$this->paginateBackAction()` method.

```php
namespace App\Menus;

class ProcessBalanceRequest extends Menu
{
  public function actions($userPreviousResponses)
  {
    $actions = [
      // Define your actions here
      
      // Add back action
      '0' => [
        'display' => 'Back',
        'next_menu' => '__paginate_back',
      ]
    ];

    return $actions;
  }
}
```
Since we are going back, we are using the same trigger and message attached as the normal go-back action. The trigger is `0` and the message attached is `Back`.

or

```php
namespace App\Menus;

class ProcessBalanceRequest extends Menu
{
  public function actions($userPreviousResponses)
  {
    $actions = [
      // Define your actions here
    ];

    // Add paginate back action
    $actions = $this->mergeAction($actions, $this->paginateBackAction());

    return $actions;
  }
}
```


## Magic menus
Rejoice offers the possibility of navigating easily from a menu to another especially when it comes to doing things like going back or returning to the welcome menu, etc. Those menus are called magic menus. Their names starts with double undescore. They are automatically configure on any application. These are all the magic menus that Rejoice ships with.
- `__back` for going back.
- `__welcome` for returning to the main menu
- `__end` to return a last menu message (a.k.a end the session)
- `__same` for displaying the same menu again
- `__paginate_forward` for implementing a forward pagination (this is used by the [Paginator](#implementing-pagination))
- `__paginate_back` for implementing a back pagination (this is used by the [Paginator](#implementing-pagination))

Those are the magic menus you can use directly in your application to navigate. There are other magic menus that are reserved to the framework:
- `__split_next` When a menu overflows, Rejoice automatically detects it and splits it under the hood for you. `__split_next` is the name that will be used to reference any next screen of the splitted menu.
- `__split_back` will be used to reference any back screen of a splitted menu.
- `__continue_last_session` The name of the *continue from last session* menu.


<div class="note note-danger">It is highly recommended not to name any of your own menu with double underscore at the beginning!</div>

## Later menu
Later menu is a powerful concept that helps you to modify the normal flow of the menu. Actually, every menu defined is attached to another menu through the `next_menu` parameter. It restrict you to use a menu for a specific purpose out of it flow. The later menu bypass that limitation and allows you to call a menu outside of its flow. View from other side, it allows us to create a menu that is not dependent on a particular flow but is rather *alone* and used by multiple flows. This is a scenario to help understand the concept and a use case of a later menu.
```php
//
    'choose_option_screen' => [
        'message' => 'Select an option'
        'actions'=> [
            '1' => [
              'display' => 'Get balance',
              'next_menu' => '',
              'later' => '',
            ]
        ]
    ]

```

The later menu can be either an array or a string.
If an array, the menus inside the array will be called one after the other till the end of the array. It's important to know that a later menu has priority on the normal flow. Whenever the later menu stack is not empty, Rejoice will pick the next menu from the later menu stack till the stack is empty before returning to the normal flow.


## Calling a remote USSD application
You can call a remote USSD simply by setting the URL of the USSD application as *next_menu* to an action.
```php
// menus.php
return [
    //...

    'choose_option_screen' => [
        'message' => 'Select an option'
        'actions'=> [
            '1' => [
              'display' => 'Get balance',
              'next_menu' => 'http://address.ip/or/url/to/the/remote/ussd',
            ]
        ]
    ]

    //...
];
```
<div class="note note-warning">
It's important to know that as soon as the USSD has switched to a remote USSD, all subsequent requests will be routed to that remote USSD and this current USSD will be just a relay. No menu of the current USSD can be called again. Everything happens as if the current USSD application has completely handed over to the remote USSD.</div>
