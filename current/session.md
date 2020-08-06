---
title: Session
layout: default
nav_order: 80
---

- [Session driver](#session-driver)
- [Getting a session variable](#getting-a-session-variable)
- [Handling the last screen timeout](#handling-the-last-screen-timeout)
- [Killing the session before running long business logic](#killing-the-session-before-running-long-business-logic)
  - [Send the response and continue the script](#send-the-response-and-continue-the-script)
  - [Send the response and terminate the script](#send-the-response-and-terminate-the-script)

## Session driver


## Getting a session variable
A session is created anytime a user access the application.

You can easily save a variable in the session by using the `sessionSave` method.
You can easily retrieve a variable from the session by using the `sessionGet` or its shortcut `session` method.
You can easily check if a variable is in the session by using the `sessionHas` method.
You can easily remove a variable from the session by using the `sessionRemove` method.
```php

class EnterAge extends Menu
{
  //
  public function message($userPreviousResponses)
  {
      if ($this->sessionHas('a_variable')) {
        return $this->sessionGet('a_variable')
      }

      $aVar = 2;
      $this->sessionSave('a_variable', $aVar);
      return $aVar;
  }
}
```

The `session` and `sessionGet` methods support a default value parameter, in case the parameter specify does not exist in the session.
```php
class EnterAge extends Menu
{
  //
  public function message($userPreviousResponses)
  {
      // Return the session value a_variable or 42 if a_variable is not in session
      return $this->session('a_variable', 42);
  }
}
```

<div class="note note-warning">
If the value is not found in session and no default value passed, a RuntimeException is thrown.
</div>

## Handling the last screen timeout
<div class="note note-info">This section applies only to the last message sent to the user by your application</div>

By default, the framework compiles the screen and send it to the user's phone based on the configured `message` and `actions` (both in the menus.php and in the menu entity). But sometimes, the last screen does not show up and the user rather gets a connection MMI error. The usual workaraound is to set the `allow_timeout` parameter to `false` in the `config/app.php` file. This result of this will be, on the last screen, the user will still have the possiblilty to input a response (even though that response will not have any meaning). This is just to make sure the user see the last response no matter how long is the USSD flow. workaround you need to kill the session by yourself to be able to do extra-stuff after the user has received the response on his screen. You can perform operation on database and all the things that take a time after you have sent the response to the user.

## Killing the session before running long business logic
<div class="note note-info">This section applies only to the last message sent to the user by your application</div>

Sometimes, it is uselful, if not required to send the response to the user while continuing a business logic, either because the USSD can timeout or because it is required to be able to call another push request to the user's phone, or calling an API that will take long to respond, etc. In those cases, we will just need to call some functions to send a response to the user, while continuing our business logic.

### Send the response and continue the script
```php
class ProcessBalanceRequest extends Menu
{
  public function before($userPreviousResponses)
  {
    $this->respond('Your request is been processed')
    
    // Continue the script (insert in database, call an API, etc.)
  }
}
```
Instead of using the `respond` method, you can use:
```php
// Same as `respond`
$this->respondAndContinue('Your request is been processed')
```
or
```php
// Same as `respond`
$this->softEnd('Your request is been processed')
```
All the three methods (respond, respondAndContinue, softEnd) are doing the exact same thing. Just choose the one you like.

### Send the response and terminate the script
```php
class ProcessBalanceRequest extends Menu
{
  public function before($userPreviousResponses)
  {
    // Your business logic here

    // Sends the response and terminate the script automatically
    $this->terminate('Your request is been processed'); 
    
    // Anything here will not run
  }
}
```

Instead of using the `terminate` method, you can use:

```php
// Same as `terminate`
$this->respondAndExit('Your request is been processed')
```
or
```php
// Same as `terminate`
$this->hardEnd('Your request is been processed')
```
All the three methods (terminate, respondAndExit, hardEnd) are doing the exact same thing. Just choose the one you like.

