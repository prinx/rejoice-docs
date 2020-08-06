---
title: Commands
layout: default
nav_order: 140
---
<h1>Commands</h1>

- [Creating a command](#creating-a-command)
- [Configuring the command](#configuring-the-command)
  - [Adding arguments](#adding-arguments)
  - [Adding options](#adding-options)
- [The fire method](#the-fire-method)
- [Writing](#writing)
- [Display in color](#display-in-color)
  - [Supported colors](#supported-colors)
  - [Get a colorized string](#get-a-colorized-string)
- [Colors helper methods](#colors-helper-methods)
  - [Display a green text:](#display-a-green-text)
  - [Error text](#error-text)
  - [Question](#question)
- [Tables](#tables)
- [Ask question](#ask-question)
- [Ask for confirmation](#ask-for-confirmation)

## Creating a command
```php
php smile command:new MyCommand
```

Will create a new command in the `app/Console/Commands`{:.php} folder.

## Configuring the command
It helps you set the name of the command, the help message associated, the arguments the command takes, the options the command takes.
```php
public function configure()
{
    $this->setName('create:file')
        ->setDescription('Create a new file')
        ->setHelp('Create a new file in the root project directory. You must have write-right on the root folder.');
}
```
### Adding arguments
```php
public function configure()
{
    $this->setName('create:file')
        ->setDescription('Create a new file')
        ->setHelp('Create a new file in the root project directory. You must have write-right on the root folder.')
        ->addArgument();
}
```
### Adding options
```php
public function configure()
{
    $this->setName('create:file')
        ->setDescription('Create a new file')
        ->setHelp('Create a new file in the root project directory. You must have write-right on the root folder.')
        ->addArgument()
        ->addOption(
            'fill',
            'f',
            InputOption::OPTIONAL,
            'Feel free to edit this file'
        );
}
```
The `addOption` method takes four parameters.
- The name of the option
- A one-letter shortcut for the option
- A constant specifying if the option is required or not. The possible values are `InputOption::OPTIONAL`{:.php} and `InputOption::REQUIRED`{: .language-php .php }. You need to import the `InputOption`{: .language-php .php } class first.

```php
use Prinx\Rejoice\Console\InputOption;
```
 
## The fire method
```php
public function fire()
{
    // The code of your command here

    return SmileCommand::SUCCESS;
}
```

The `fire`{:.php} method must return `SmileCommand::SUCCESS`{:.php} if the command performs everything ok, and `SmileCommand::FAILURE`{:.php} in case an error happens.

## Writing

```php
$this->write('Cool');
```

```php
// end with a new line
$this->writeln('Cool');
```
The variable in writeln can be an array of string. Each string will be displayed on a new line.
```php
$this->writeln(['Hi Prince!', 'This is amazing.']);
```

## Display in color
```php
$this->writeWithColor('Cool', 'red', 'cyan');
```
The first color is the foreground color. The second, the background color. Both are optional. The default is grey for the foreground, black for the background.

### Supported colors
- <span class="color-box white"></span> white
- <span class="color-box grey"></span> grey
- <span class="color-box black"></span> black
- <span class="color-box red"></span> red
- <span class="color-box green"></span> green
- <span class="color-box yellow"></span> yellow
- <span class="color-box blue"></span> blue
- <span class="color-box cyan"></span> cyan

### Get a colorized string
If you don't want to write directly but rather want to get the colorized string in a variable, you can use the `colorize` method.

```php
$greetings = $this->colorize('Cool', 'red', 'cyan');
```

## Colors helper methods
### Display a green text:
```php
$this->info('Cool');
$this->success('Cool');

// Equivalent to
$this->writeWithColor('Cool', 'green', 'black');
```

### Error text
```php
$this->error('Uhhh');

// Equivalent to
$this->writeWithColor('Uhhh', 'white', 'red');
```

### Question
```php
$this->question('Why?');

// Equivalent to
$this->writeWithColor('Why?', 'black', 'cyan');
```

## Tables
```php
$this->table([
    ['ID', 'Name', 'Age'], // Row
    [ 1,   'Edna',   22 ], // Row
    [ 2,   'Prince', 25 ], // Row
    [ 3,   'Edwige', 24 ], // Row
]);
```

The table takes an array of arrays.
Each array represents a row of the table. Each value respresents the value of the specific row and column.

## Ask question
```php
$filename = $this->ask('What is the file name?');
```
The `ask` method returns the response given by the user.

## Ask for confirmation
```php
if ($this->confirm('Do you want to overwrite this file?', 'no');) {
    //
}
```
Takes as first argument, the question and as second argument the default response.
The default response must be one of: `y`, `yes`, `n`, `no`.
