---
title: Request
layout: default
nav_order: 90
---

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

<div class="note note-warning">WARNING:

```php
$this->session('id');
```
will NOT work.
</div>

### The service code or service operator code
```php
$this->ussdRequestType();
```

### The USSD string
```php
$this->userResponse();
```

### The channel
Useful when building an application not only for USSD. You can do extra stuff, customize the response, etc., based on the channel.

```php
$this->channel(); // Will be USSD by default.
```
### Parameters defined in the `config/app.php` file
```php
$this->config('app.param_name'); // Will be USSD by default.
```

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
