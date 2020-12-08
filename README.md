# TEST [phpcs-psr4-sniff] with non-namespaced

## purpose
It seems like [phpcs-psr4-sniff] v2.2.1 has [issue]s
when there is no namespace defined.

This should be no issue, since the following is totally fine
according to [the docs](https://getcomposer.org/doc/04-schema.md#psr-4):
```json
{
    "autoload": {
        "psr-4": { "": "src/" }
    }
}
```

## tests

### how its tested

setup a `conposer.json` with a PSR4 compatible autoloader
that does not use a namespace at all.

### expected behaviour

* classes are found since they follow PRS4 and composer autoloader is configured correctly.

### observed test results

* classes reported to have wrong names paces or names.

running the tests:

```
$ phpcs src

FILE: src/Bar/Bazz.php
-------------------------------------------------------------------------------------------------------------------
FOUND 1 ERROR AFFECTING 1 LINE
-------------------------------------------------------------------------------------------------------------------
 6 | ERROR | Class name is not compliant with PSR-4 configuration. It should be `\Bar\Bazz` instead of `Bar\Bazz`.
-------------------------------------------------------------------------------------------------------------------


FILE: src/Foo.php
---------------------------------------------------------------------------------------------------------
FOUND 1 ERROR AFFECTING 1 LINE
---------------------------------------------------------------------------------------------------------
 3 | ERROR | Class name is not compliant with PSR-4 configuration. It should be `\Foo` instead of `Foo`.
---------------------------------------------------------------------------------------------------------

Time: 61ms; Memory: 4MB
```

additional info:

```
$ php -r 'require_once "vendor/autoload.php"; var_dump(class_exists("\\Bar\\Bazz"));'
bool(true)
```

```
$ php -r 'require_once "vendor/autoload.php"; var_dump(class_exists("Foo"));'
bool(true)
```

```
$ php --version
PHP 7.3.25 (cli) (built: Dec  1 2020 05:35:27) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.3.25, Copyright (c) 1998-2018 Zend Technologie
```

## how to run the tests

after installing [phpcs-psr4-sniff] via `composer install` run the following:

```shell script
composer run test
```

[phpcs-psr4-sniff]: https://packagist.org/packages/suin/phpcs-psr4-sniff
[issue]: https://github.com/suin/php/issues/4
