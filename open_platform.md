title: WeChat Open Platform
---

## Instantiation

```php
<?php
use EasyWeChat\Foundation\Application;

$options = [
    // ...
    'open_platform' => [
        'app_id'   => 'component-app-id',
        'secret'   => 'component-app-secret',
        'token'    => 'component-token',
        'aes_key'  => 'component-aes-key'
        ],
    // ...
    ];

$app = new Application($options);
$openPlatform = $app->open_platform;
```

## Authorization Events from WeChat

The open platform sends 4 events: `authorized`, `updateauthorized`, `unauthorized` and `component_verify_ticket`.

> Once the open platform is pass the audit from WeChat, WeChat server will send a `component_verify_ticket` to developer's server every 10 minutes. SDK, by default, caches `component_verify_ticket`.

> SDK will caches `component_veirfy_ticket`.
> The rest of the events needs to be handled by developers.

- Note, developers have to call the code in the route / controller, this `component_verify_ticket` is required, or nothing can be done, will throw exception "Component verify ticket does not exists."

Example:

```php
use EasyWeChat\OpenPlatform\Guard;

$server = $openPlatform->server;

$server->setMessageHandler(function($event) use ($openPlatform) {
    // Constants are defined in \EasyWeChat\OpenPlatform\Guard
    switch ($event->InfoType) {
        case Guard::EVENT_AUTHORIZED: // Authorized success
            $authorizationInfo = $openPlatform->getAuthorizationInfo($event->AuthorizationCode);
            // Some database operations...
        case Guard::EVENT_UNAUTHORIZED: // Authorized update
            // Some database operations...
        case Guard::EVENT_UPDATE_AUTHORIZED: // Authorized cancel
            // Some database operations...
    }
});

$response = $server->serve();

$response->send(); // Laravel：return $response;
```


## PreAuthorization

### PreAuthorization Code

Example:

```php
$openPlatform->pre_auth->getCode();
```

### PreAuthorization URL

Example:

```php
// Redirectly
$response = $openPlatform->pre_auth->redirect('https://your-domain.com/callback');

// Get the redirect url
$response->getTargetUrl();

```
It will bring back to `https://your-domain.com/callback?auth_code=xxxxxxx` when the user is authorized.

## API List

### Get authorizer credentials and authorization infomation

```php
$openPlatform->getAuthorizationInfo($authorizationCode = null);
```

### Get authorizer infomation

```php
$openPlatform->getAuthorizerInfo($authorizerAppId);
```

### Get authorizer option

```php
$openPlatform->getAuthorizerOption($authorizerAppId, $optionName);
```

### Set authorizer option

```php
$openPlatform->setAuthorizerOption($authorizerAppId, $optionName, $optionValue);
```

## Calling Authorizer API

It will be obtained an instance of `\EasyWeChat\Foundation\Application`.

> When the API is called, the SDK will automatically obtain and refresh the `AuthorizerAccessToken` expires.
> Developers do not need to handle with the authorizer credentials `AuthorizerAccessToken`.

```php
$app = $openPlatform->createAuthorizer($authorizerAppId, $authorizerRefreshToken);
```
