---
title: SMS
layout: default
nav_order: 110
---

## Sending SMS
> This is only available if your SMS endpoint expect a post request with parameters `message`, `sender`, `recipient`. If it does not, you can implement your custom SMS

```php
// Anywhere in the menu class
$this->sendSms('Your request is been processed');
```

This assumes that you have configured the sms endpoint and the default sender name in the .env file or the app config file.

```ini
SMS_ENDPOINT=https://my-sms-api-endpoint
SMS_SENDER_NAME=MYAPP
```

If not, you can directly send SMS like this:
```php
// Anywhere in the menu class
$sms = 'Your request is been processed';
$receiver = $this->tel();
$endpoint = 'https://...';
$sender = 'MySenderName';

$this->sendSms($sms, $receiver, $sender, $endpoint);
```

By default, the receiver is the current user of the application, which can be retrieved via the `$this->tel()` method. If not passed, it is automatically passed by Rejoice.

### Send and exit the script
```php
// Menu entity
$this->sendSmsAndExit('Your request is been processed')
```