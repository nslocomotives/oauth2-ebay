# Ebay Provider for OAuth 2.0 Client

[![Latest Version](https://img.shields.io/github/release/nslocomotives/oauth2-ebay.svg?style=flat-square)](https://github.com/nslocomotives/oauth2-ebay/releases)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE)
[![Build Status](https://img.shields.io/travis/nslocomotives/oauth2-ebay/master.svg?style=flat-square)](https://travis-ci.org/nslocomotives/oauth2-ebay)

This package provides Ebay OAuth 2.0 support for the PHP League's [OAuth 2.0 Client](https://github.com/thephpleague/oauth2-client).

## Installation

To install, use composer:

```
composer require nslocomotives/oauth2-ebay
```

## Usage

Usage is the same as The League's OAuth client, using `\Nslocomotives\OAuth2\Client\Provider\Ebay` as the provider.

### Authorization Code Flow

```php
$provider = new Nslocomotives\OAuth2\Client\Provider\Ebay([
    'clientId'          => '{ebay-app-id}',
    'clientSecret'      => '{debay-app-secret}',
    'redirectUri'       => 'https://example.com/callback-url'
]);

if (!isset($_GET['code'])) {

    // If we don't have an authorization code then get one
    $authUrl = $provider->getAuthorizationUrl();
    $_SESSION['oauth2state'] = $provider->getState();
    header('Location: '.$authUrl);
    exit;

// Check given state against previously stored one to mitigate CSRF attack
} elseif (empty($_GET['state']) || ($_GET['state'] !== $_SESSION['oauth2state'])) {

    unset($_SESSION['oauth2state']);
    exit('Invalid state');

} else {

    // Try to get an access token (using the authorization code grant)
    $token = $provider->getAccessToken('authorization_code', [
        'code' => $_GET['code']
    ]);

    // Optional: Now you have a token you can look up a users profile data
    try {

        // We got an access token, let's now get the user's details
        $user = $provider->getResourceOwner($token);

        // Use these details to create a new profile
        printf('Hello %s!', $user->getId());

    } catch (Exception $e) {

        // Failed to get user details
        exit('Oh dear...');
    }

    // Use this to interact with an API on the users behalf
    echo $token->getToken();
}
```

## Refreshing a Token
Ebay's OAuth implementation does not uses refresh tokens.  Once I have coded this part ill drop notes here :-)

## Testing

``` bash
$ ./vendor/bin/phpunit
```

## Contributing

Please see [CONTRIBUTING](https://github.com/nslocomotives/oauth2-ebay/blob/master/CONTRIBUTING.md) for details.


## Credits

- [Nigel Smith](https://github.com/nslocomotives)
- [All Contributors](https://github.com/nslocomotives/oauth2-ebay/contributors)

In order to learn about oauth2 and building this module cleanly with unit tests I used this excellent module to template my own.  Please checkout this for dropbox integrations.
- [Steven Maguire](https://github.com/stevenmaguire)
- [oauth2-dropbox](https://github.com/nslocomotives/oauth2-ebay/contributors)


## License

The MIT License (MIT). Please see [License File](https://github.com/nslocomotives/oauth2-ebay/blob/master/LICENSE) for more information.

