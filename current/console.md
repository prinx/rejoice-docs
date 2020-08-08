---
title: Console
layout: default
nav_order: 130
---

<h1>Console commands</h1>

- [Console commands](#console-commands)
- [Running the simulator](#running-the-simulator)
  - [Console simulator](#console-simulator)
  - [Web simulator](#web-simulator)
- [Create a new menu](#create-a-new-menu)
  - [Directly in the Menus folder](#directly-in-the-menus-folder)
  - [In a Menu sub-folder](#in-a-menu-sub-folder)
- [Other console commands](#other-console-commands)
- [Help on a command](#help-on-a-command)


## Console commands
Rejoice ships with useful console features to allow you test quickly your application and other more.

The console application in Rejoice is name `smile`.

## Running the simulator
You have access to two simulators, one in console, the other as a web interface.
You can provide the configuration of the simulator in the .env file:

### Console simulator

Run the console simulator by running the command
```php
php smile simulator:console
```
You can use the shortcut `php smile sim:con`
> Note: You can use shortcuts for any simulator command, provided it does not conflict with any other command.

### Web simulator

Run the console simulator by running the command
```php
php smile simulator:web
```

## Create a new menu
### Directly in the Menus folder
```php
php smile menu:new MyMenu
```

Will create the class `MyMenu` in a newly created `app/Menus/MyMenu.php`.

### In a Menu sub-folder
```php
php smile menu:new FirstFlow/MyMenu
```

Will create the class `MyMenu` in a newly created `app/Menus/FirstFlow/MyMenu.php`.
If the sub-folder does not exist, it will be created.

## Other console commands

You can retrieve any other console command by running:
```php
php smile
```

## Help on a command

To read the help of a command, run:
```php
php smile help name_of_the_command
```