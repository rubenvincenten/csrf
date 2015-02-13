# Volnix\CSRF

[![Build Status](https://img.shields.io/travis/volnix/csrf.svg?style=flat-square)](https://travis-ci.org/volnix/csrf) [![Downloads](https://img.shields.io/packagist/dt/volnix/csrf.svg?style=flat-square)](https://packagist.org/packages/volnix/csrf) [![Latest Stable Version](https://img.shields.io/packagist/v/volnix/csrf.svg?style=flat-square)](https://packagist.org/packages/volnix/csrf) [![Code Coverage](https://img.shields.io/scrutinizer/coverage/g/volnix/csrf.svg?style=flat-square)](https://scrutinizer-ci.com/g/volnix/csrf/?branch=master)


CSRF protection library that compares provided token to session token to ensure request validity.  Token values are stored in session and validated against that session store to verify the request.  Tokens are changed on every request so ensure frequently changes and increase the difficulty in guessing the token value.

### Installation

This package is installed via composer and is pulled in as the `volnix/csrf` package.

```json

"require": {
    "volnix/csrf": "~1.0"
}

```

### Unit Tests

The unit tests for this application live in the `./tests` directory.  There is a test for each library in the security package.  The PHPUnit xml file is in the root of the project (`./phpunit.xml`).

### Usage

This library is used for preventing cross-site request forgery.  Currently you can retrieve the token, a pre-built query string, or a pre-built hidden input tag.

Value only:

```php
	
<?php

require_once('vendor/autoload.php');

if (\Volnix\CSRF\CSRF::valid()) {
	// valid request
}
else {
	// invalid CSRF
}

?>

<form action="index.php" method="post">
	<input type="hidden" name="<?= \Volnix\CSRF\CSRF::TOKEN_NAME ?>" value="<?= \Volnix\CSRF\CSRF::getToken() ?>"/>
	<input type="text" name="action" placeholder="Enter an action."/>
	<input type="submit" value="Submit" name="sub"/>
</form>

```

Hidden input string:

```php

<form action="index.php" method="post">
	<?= \Volnix\CSRF\CSRF::getAsHiddenInput() ?>
	<input type="text" name="action" placeholder="Enter an action."/>
	<input type="submit" value="Submit" name="sub"/>
</form>

```

Query string:

```php

<form action="index.php?<?= \Volnix\CSRF\CSRF::getQueryString() ?>" method="get">
	<input type="text" name="action" placeholder="Enter an action."/>
	<input type="submit" value="Submit" name="sub"/>
</form>

```

If a different token name is desired, you may pass it in on any call that retrieves the token (`getToken()`, `getHiddenInputString()`, `getQueryString()`):

```php

<form action="index.php" method="post">
	<?= \Volnix\CSRF\CSRF::getAsHiddenInput('custom_token_name') ?>
	<input type="text" name="action" placeholder="Enter an action."/>
	<input type="submit" value="Submit" name="sub"/>
</form>

```