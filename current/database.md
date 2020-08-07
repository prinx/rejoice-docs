---
title: Database
layout: default
nav_order: 70
---

<h1>Connecting to the database</h1>

## Configuring databases
Application database connections in Rejoice are configured in the `config/database` folder.

You can define as many database connections as you want by giving each of them a name. There is a default database configured by default.

This is an example of configuration:
```php
// config/database.php

return [

    'default' => [
        'user'     => env('APP_DEFAULT_DB_USER', ''),
        'password' => env('APP_DEFAULT_DB_PASS', ''),
        'host'     => env('APP_DEFAULT_DB_HOST', ''),
        'port'     => env('APP_DEFAULT_DB_PORT', ''),
        'dbname'   => env('APP_DEFAULT_DB_NAME', ''),
    ],

    'log_db' => [
        'user'     => env('LOG_DB_USER', ''),
        'password' => env('LOG_DB_PASS', ''),
        'host'     => env('LOG_DB_HOST', ''),
        'port'     => env('LOG_DB_PORT', ''),
        'dbname'   => env('LOG_DB_NAME', ''),
    ]

];
```

You will define the environment variables in the `.env` file in the root directory.

<div class="note note-warning">
We do not recommend specifying the database credentials directly in the config files.
</div>

## Using a database connection
Using a database connection in a menu class is as easy as calling the `db` method.
The db method takes one parameter which is the name of the connection you want to use.
You can also call the `db` method without parameter. The `default` connection will be automatically used.
The `db` method returns a `PDO` connection to the database. So you will use it exactly how a PDO connection is used.

This is an example:
```php

public function before()
{
    $name = $this->userPreviousResponses('enter_username');

    $statement = $this->db()->prepare("INSERT INTO users (name) VALUES (?)");
    $statement->execute([$name]);

    $logStatement = $this->db('log_db')->prepare("INSERT INTO logs (log_text) VALUES (?)");
    $statement->execute(["User {$name} has been created"]);
}
```

The first insertion uses the default database connection. The second uses the log database configurations.