title: WeChat Open Platform
---

### Instantiation

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
$open_platform = $app->open_platform;
```

### Listening to Authorization Events

The open platform sends 4 events: `authorized`, `updateauthorized`, `unauthorized` and `component_verify_ticket`.

By default, SDK handles as following:

- `authorized` / `updateauthorized`: Retrieves all info of the authorizer and then caches  `authorizer_access_token` and `authorizer_refresh_token`, the authorizer info needs to be handled by developer.
- `unauthorized`: deletes `authorizer_access_token` and `authorizer_refresh_token` from cache.
- `component_verify_ticket`: caches `component_veirfy_ticket`.

It allows custom handlers of course, but above default handling will always run first, as for trouble-free of caching tokens.

```php
// Default handling.
$open_platform->server->serve();

// Custom handling.
$open_platform->server->setMessageHandler(function($event) {
    // Event type constants are defined in class 
    // \EasyWeChat\OpenPlatform\Guard.
    switch ($event->InfoType) {
        case 'authorized':
            // ...
        case 'unauthorized':
            // ...
        case 'updateauthorized':
            // ...
        case 'component_verify_ticket':
            // ...
    }
});
$open_platform->server->serve();

// OR
$open_platform->server->listen(function ($event) {
    switch ($event->InfoType) {
        // ...
    }
});
```

#### Authorized，Update Authorized

By default for these 2 events, SDK retrieves all info of the authorizer and then caches  `authorizer_access_token` and `authorizer_refresh_token`, the authorizer info is the original data returned by WeChat API, these data needs to be handled by developer, e.g. saving to database.

```php
// Custom handling.
// The $event variable contains both the original event message 
// and the full authorizer info.
$open_platform->server->setMessageHandler(function($event) {
    // Event type constants are defined in class 
    // \EasyWeChat\OpenPlatform\Guard.
    switch ($event->InfoType) {
        case 'authorized':
            // Authorization info, mainly about tokens and scopes.
            $info1 = $event->authorization_info;
            // Authorizer info, mainly about the account info.
            $info2 = $event->authorizer_info;
    }
});
```

So far there is no different handling these 2 events.

#### Unauthorized

By default, SDK deletes `authorizer_access_token` 和 `authorizer_refresh_token` from cache. Developers may remove the records in database using custom handler.

#### Push component_verify_ticket

Once the open platform is pass the audit from WeChat, WeChat server will send a `component_verify_ticket` to developer's server every 10 minutes. SDK, by default, caches `component_verify_ticket`.

Note, developers have to call the code in the route / controller, this `component_verify_ticket` is required, or nothing can be done, will throw exception "Component verify ticket does not exists."

### Authorization API

#### Get Authorization URL
```php
$open_platform->pre_auth
    ->setRedirectUri('http://domain.com/callback')
    ->getAuthLink();

```

用户授权后会带上 `code` 跳转到 `$redirect_uri`。

##### Get Authorization Info
```php
$authorizer = $open_platform->authorizer;

$authorizer->getAuthInfo($authorization_code);
```

##### Get Authorizer Info
```php
$authorizer = $open_platform->authorizer;

$authorizer->getAuthorizerInfo($authorizer_appid);

```

##### Get Authorizer Option
```php
$authorizer = $open_platform->authorizer;

$authorizer->getAuthorizerOption($authorizer_appid, $option_name);

```

##### Set Authorizer Option
```php
$authorizer = $open_platform->authorizer;

$authorizer->setAuthorizerOption($authorizer_appid, $option_name, $option_value);

```


