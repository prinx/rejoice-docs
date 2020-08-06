---
title: Validation
layout: default
parent: Menus
nav_order: 44
---

<h1>Validation</h1>

- [Validation](#validation)
  - [In the menu entity](#in-the-menu-entity)
    - [USing your custom validation logic](#using-your-custom-validation-logic)
    - [Using the user response validator instance](#using-the-user-response-validator-instance)
  - [In the menu flow](#in-the-menu-flow)
- [Validation rules](#validation-rules)
  - [alphabetic](#alphabetic)
  - [minLength](#minlength)
  - [maxLength](#maxlength)
  - [integer](#integer)
  - [min](#min)
  - [max](#max)
  - [float](#float)
  - [amount](#amount)
  - [name](#name)
  - [age](#age)
  - [alphanumeric](#alphanumeric)
  - [date](#date)
  - [tel](#tel)
  - [regex](#regex)
  - [string](#string)
  
## Validation

Rejoice has made it very easy to validate the user sent on each particular menu.
You can validate directly inside the menu flow in the `menu.php` file or validate in the menu entity.

### In the menu entity
#### USing your custom validation logic
```php

class EnterAge extends Menu
{
  //
  public function message($userPreviousResponses)
  {
      return '';
  }

  public function validate($response)
  {
      $age = intval($response);

      if ($age < 5 || $age > 150) {
        return false;
      }
      
      return true;
  }
}
```

You can modify the default error message:
```php

class EnterAge extends Menu
{
  //

  public function validate($response)
  {
      $age = intval($response);

      // You can also use $this->setError()
      if ($age < 5) {
        $this->addError('You must be more than 5 years to continue');
        return false;
      } elseif ($age > 150) {
        $this->addError('You must be less than 150 years to continue');
        return false;
      }
      
      return true;
  }
}
```

A more advisable way to validate will be as follow because an error can also be defined somewhere else by the framework itself (especially if you specify a `validate` parameter for the same menu in the `menus.php` file):
```php

class EnterAge extends Menu
{
  //

  public function validate($response)
  {
      $age = intval($response);

      if ($age < 5) {
        $this->addError('You must be more than 5 years to continue');
      } elseif ($age > 150) {
        $this->addError('You must be less than 150 years to continue');
      }
      
      return empty($this->error());
  }
}
```    
#### Using the user response validator instance
The user response validator is actually the class that the framework uses to automatically validate the response when you specify the validation rules in the `menus.php`. You can also use the same class inside your `validate` method:
```php
// Import the class from its namespace
use Prinx\Rejoice\UserResponseValidator as Validator;

class EnterAge extends Menu
{
  //

  public function validate($response)
  {
      $rules = 'integer|min:5|max:150';
      return Validator::validate($response, $rules);
  }
}
``` 
Or with the custom error messages:
```php
// Import the class from its namespace
use Prinx\Rejoice\UserResponseValidator as Validator;

class EnterAge extends Menu
{
  //

  public function validate($response)
  {
      $rules = [
          'integer',
          ['min:5', 'You must be more than 5 years to continue'],
          ['max:150', 'You must be less than 150 years to continue'],
      ];

      return Validator::validate($response, $rules);
  }
}
```
Or
```php
// Import the class from its namespace
use Prinx\Rejoice\UserResponseValidator as Validator;

class EnterAge extends Menu
{
  //

  public function validate($response)
  {
      $rules = [
          'integer',
          [
              'rule' => 'min:5',
              'error' => 'You must be more than 5 years to continue'
          ],
          [
              'rule' => 'max:150',
              'error' => 'You must be less than 150 years to continue'
          ],
      ];

      return Validator::validate($response, $rules);
  }
}
```
Or
```php
// Import the class from its namespace
use Prinx\Rejoice\UserResponseValidator as Validator;

class EnterAge extends Menu
{
  //

  public function validate($response)
  {
      $rules = [
          'integer',
          'min:5' => 'You must be more than 5 years to continue',
          'max:150' => 'You must be less than 150 years to continue',
      ];

      return Validator::validate($response, $rules);
  }
}
```
Or We can be less verbose by directly returning only the rules:
```php
// No need to import the class the validator class

class EnterAge extends Menu
{
  //

  public function validate($response)
  {
      return [
          'integer',
          'min:5' => 'You must be more than 5 years to continue',
          'max:150' => 'You must be less than 150 years to continue',
      ];
  }
}
```
<div class="note note-warning">
An empty validation rule will throw a `RuntimeException`.</div>

### In the menu flow

```php
// menus.php
return [
  //

  'enter_age' => [
    'message' => 'Enter your age'
    'actions'=> [
      '0' => [
        'display' => 'Back',
        'next_menu' => '__back',
      ]
    ]

    'default_next_menu' => 'retrieve_birth_year'
    'validate' => 'integer|min:5|max:150'
  ]
];
```
On the previous example, if the user input `0`, they will go back, if not any other input will be validated against the rules specified in the `validate` parameter of the menu.

<div style="">
<strong>Note</strong><br>
Whatever action is specified in the `actions` parameter has the priority over the any other input the user may send.*

<br>Those values are not checked in the validating process because the are self-validating, meaning the user has to input them exactly as they are. Only the values that the developer does not have a control over, will be validated (like the name of the user; the user can send a number at the place of their name.)</div>

## Validation rules
Some rules accept parameters. You specify the parameter by following the rule with **colon** (`:`) and the value of the parameter.

In the case a rule requires multiple variables, you can pass the variables by seperating them with a **comma** (`,`). This means, **you cannot pass a colon or a comma as argument to a rule**. If you are required to pass a colon or a comma as argument, you must create a custom validation inside the menu entity.

You can combine several rules by separating them with a pipe (`|`).
These are the default rules you can use:

<div class="note note-info">The rules can be camelCase, snake_case, PascalCase or even kebab-case. For example, you can use 'maxLength', 'max_length', 'MaxLength' or even 'max-length'.</div>


### alphabetic
Check if the string contains only letters

```php
return [

  'enter_name' => [
        'message' => 'Enter your name'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'alpha'
        // or
        // 'validate' => 'alphabetic'
    ]
];
```


### minLength
Takes the minimum length required as parameter.

```php
return [

  'enter_name' => [
        'message' => 'Enter your name'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'alpha|minLength:3'
    ]
];
```

<div class="note note-info">You can use `minLength` or `minLen`.</div>

### maxLength
Takes the maximum length required as parameter.

```php
return [

  'enter_name' => [
        'message' => 'Enter your name'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'alpha|maxLength:30'
    ]
];
```
<div class="note note-info">You can use `maxLength`, `maxLen`.</div>

### integer
```php
return [

  'enter_age' => [
        'message' => 'Enter your age'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'integer'
    ]
];
```

### min
```php
return [

  'enter_age' => [
        'message' => 'Enter your age'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'integer|min:10'
    ]
];
```
### max
```php
return [

  'enter_age' => [
        'message' => 'Enter your age'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'integer|max:50'
    ]
];
```

### float
```php
return [

  'enter_length' => [
        'message' => 'Enter the desired length'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'float'
    ]
];
```
<div class="note note-info">The rules `min` and `max` also work for float values.</div>

```php
return [

  'enter_length' => [
        'message' => 'Enter the desired length'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'float|min:1.55|max:70.95'
    ]
];
```
### amount
```php
return [

  'choose_option_screen' => [
        'message' => 'Select an option'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'amount'
    ]
];
```
The validation `amount` is just a compilation of `float|min:0`.

### name
```php
return [

  'enter_name' => [
        'message' => 'Enter your name'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'name'
    ]
];
```
The validation `name` is a compilation of `alpha|min_len:3|max_len:50`.

### age
```php
return [

  'enter_age' => [
        'message' => 'Enter your age'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'age'
    ]
];
```
The validation `age` is a compilation of `integer|min:0|max:100`.

### alphanumeric
```php
return [

  'choose_option_screen' => [
        'message' => 'Select an option'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'alphanum'
        // or
        // 'validate' => 'alphanumeric'
    ]
];
```
<div class="note note-warning">To validate alphanumeric, the response is validate against this regular expression: /(\W?\w)+/</div>

### date
The date validation takes the format of the date as argument. If no format is passed, the default format `j/n/Y` is used.

```php
return [

  'enter_date' => [
        'message' => 'Enter your date of birth'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'date:d/m/Y'
    ]
];
```
<div class="note note-info">The format must be a valid PHP date format.</div>
<div class="note note-info">The default format (j/n/Y) accepts dates like '1/5/2025', '07/6/2030', etc. (The date and month do not need the '0' at their begining.)</div>
<div class="note note-info">In case you need to perform some calculation on the date, like check if the date is too old or too far from another date, you can use the methods in the Date Utilities class to perform the validation inside the menu entity.</div>

### tel
Checks if the response is a valid telephone number.

```php
return [

  'enter_date' => [
        'message' => 'Enter your date of birth'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'date:d/m/Y'
    ]
];
```
<small>Due to the various way a telephone number can be specified, this does not check if the phone number is a real phone number. But rather just check it against the general format of phone numbers, meaning the number can start with the '+' sign or '00', must contain only digits, parenthesis, spaces or hyphens, must be longer than 7 digits and shorter than 15 digits. You can use the `internationaliseNumber` method of the `Str` utils class to format the number to the specific country's phone number pattern.</small>'

### regex
You can use a regular expression to define your own rule.
```php
return [

  'enter_name' => [
        'message' => 'Enter your name starting by with your title (Mrs/Mr)'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'regex:/^Mrs? [a-z]{3,50}/i'
    ]
];
```

The regex must be enclosed in delimiters (`/` in the regex above). It allows you to easily add flags (notice the case-insensitive flag `i` in the regex above)

### string
Check if the response is a string. It is the validation that is applied by default. When using the framework for a USSD application, there is no need to specify this validation. It is done automatically by the framework. It will become iseful when using the framwork outside the scope of a USSD application.

```php

return [

  'enter_name' => [
        'message' => 'Enter your name'
        'actions'=> [
            '0' => [
              'display' => 'Back',
              'next_menu' => '__back',
            ]
        ]

        'validate' => 'string'
   ],
];
```
