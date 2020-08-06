---
title: Request
layout: default
nav_order: 90
---

<h1>Request</h1>

- [Retrieving the request parameters](#retrieving-the-request-parameters)
  - [The phone number of the user](#the-phone-number-of-the-user)
  - [The network (actually the mnc)](#the-network-actually-the-mnc)
  - [The session Id sent by the mobile operator](#the-session-id-sent-by-the-mobile-operator)
  - [The service code or service operator code](#the-service-code-or-service-operator-code)
  - [The user's response (USSD string)](#the-users-response-ussd-string)
  - [The channel](#the-channel)
  - [Parameters defined in the `config/` folder](#parameters-defined-in-the-config-folder)
  - [Any other request POST parameters](#any-other-request-post-parameters)
  - [Any other request GET parameters](#any-other-request-get-parameters)


## Retrieving the request parameters
### The phone number of the user
```php
$this->tel();
// or
$this->msisdn();
```

### The network (actually the mnc)
```php
$this->network();
```

### The session Id sent by the mobile operator
```php
$this->sessionId();
```

### The service code or service operator code
```php
$this->ussdRequestType();
```

### The user's response (USSD string)
```php
$this->userResponse();
```

### The channel
Useful when building an application not only for USSD. You can do extra stuff, customize the response, etc., based on the channel.

```php
$this->channel(); // Will be USSD by default.
```

### Parameters defined in the `config/` folder
```php
$this->config('app.environment');
$this->config('menu.default_end_message');
```
The `config` method takes the dot-separated value representing the name of the config file and the index of the value in the array returned by the specific config file.

### Any other request POST parameters
```php
$this->request()->input('name_of_the_input');
```
For post parameters, the request method can be used directly:
```php
$this->request('name_of_the_input');
// Equivalent to
$this->request()->input('name_of_the_input');
```

### Any other request GET parameters
```php
$this->request()->query('name_of_the_input');
```
