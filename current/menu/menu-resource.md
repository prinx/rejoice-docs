---
title: Menu resource
layout: default
parent: Menus
nav_order: 41
---


The menus that will be displayed are defined in the `resources/menus/menus.php` file. It is actually an associative array containing the menus. Each menu is identified by a name (each index of the array). The name will be used when calling the menu to be displayed and also to create the class that will be responsible of the business logic of the menu (the menu entity). It is recommended to use a meaningful name, in lowercase with the word separated with undescore.
*The only naming rule is: the menu name must not start with a number.*

These are valid menu names:
'get_age_screen', 'user_choice', 'userChoice', 'UserChoice', 'user-choice', 'user1_choice' ..., as long as it does not start with a number.

> ☝️ **Info**
> The rule on the menu name comes from the fact that we will be defining classes derived from the menus' names (those classes are called *menu entities*).

A menu screen is composed of two main parts: the **message** and the **actions**.

Any menu without action will be the last menu displayed to the user, as the user can no more input something. The session will then be ended.

The array returned in the `resources/menus/menus.php` file must contain `welcome` menu. The welcome menu is the only menu required.

Based on these information, let's go to  `resources/menus/menus.php` and write our first menu.

```php
// resources/menus/menus.php

return [
  
  // The welcome menu
  'welcome' => [
    'message' => "Welcome to your first menu\nSelect an option",
    
    'actions' => [
      '1' => [ // If user selects 1
        'display' => 'Get inspiring quote',
        'next_menu' => 'inspiring_quote' // Go to inspiring_quote screen
      ],
    ]
  ],

  'inspiring_quotes' => [
    'message' => "The quote today is 'To be or not to be, that is the question! - Shakespeare'",
  ],
];
```