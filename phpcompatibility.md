# PHPCompatibilitySymfony

Using PHPCompatibilitySymfony, you can analyse the codebase of a project using any of the [Symfony polyfill libraries](https://github.com/symfony?utf8=?&q=polyfill), for PHP cross-version compatibility.

## Available Polyfill Libraries
Symfony Polyfill Library | Corresponding PHPCompatibility Ruleset | Includes
--- | --- | ---
[`polyfill-php54`](https://github.com/symfony/polyfill-php54) | `PHPCompatibilitySymfonyPolyfillPHP54` |
[`polyfill-php55`](https://github.com/symfony/polyfill-php55) | `PHPCompatibilitySymfonyPolyfillPHP55` | [`PHPCompatibilityPasswordCompat`](https://github.com/PHPCompatibility/PHPCompatibilityPasswordCompat)
[`polyfill-php56`](https://github.com/symfony/polyfill-php56) | `PHPCompatibilitySymfonyPolyfillPHP56` |
[`polyfill-php70`](https://github.com/symfony/polyfill-php70) | `PHPCompatibilitySymfonyPolyfillPHP70` | [`PHPCompatibilityParagonieRandomCompat`](https://github.com/PHPCompatibility/PHPCompatibilityParagonie)
[`polyfill-php71`](https://github.com/symfony/polyfill-php71) | `PHPCompatibilitySymfonyPolyfillPHP71` |
[`polyfill-php72`](https://github.com/symfony/polyfill-php72) | `PHPCompatibilitySymfonyPolyfillPHP72` |
[`polyfill-php73`](https://github.com/symfony/polyfill-php73) | `PHPCompatibilitySymfonyPolyfillPHP73` |
[`polyfill-php74`](https://github.com/symfony/polyfill-php74) | `PHPCompatibilitySymfonyPolyfillPHP74` |
[`polyfill-php80`](https://github.com/symfony/polyfill-php80) | `PHPCompatibilitySymfonyPolyfillPHP80` |


## Requirements

* [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer).
    * PHP 5.3+ for use with [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) 2.3.0+.
    * PHP 5.4+ for use with [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) 3.0.2+.

    Use the latest stable release of PHP_CodeSniffer for the best results.
    The minimum _recommended_ version of PHP_CodeSniffer is version 2.6.0.
* [PHPCompatibility](https://github.com/PHPCompatibility/PHPCompatibility) 9.0.0+.
* [PHPCompatibilityParagonie](https://github.com/PHPCompatibility/PHPCompatibilityParagonie) 1.0.0+.
* [PHPCompatibilityPasswordCompat](https://github.com/PHPCompatibility/PHPCompatibilityPasswordCompat) 1.0.0+.


## Install
run the following from the command-line for install either PHP_CodeSniffer and PHPCompatibility:
```bash
composer config allow-plugins.dealerdirect/phpcodesniffer-composer-installer true
composer require --dev dealerdirect/phpcodesniffer-composer-installer:"^0.7" phpcompatibility/phpcompatibility-symfony:"*"
```
```bash
root@fd2159965127:/var/www/spgamepack.net# composer config allow-plugins.dealerdirect/phpcodesniffer-composer-installer true
 dealerdirect/phpcodesniffer-composer-installer:"^0.7" phpcompatibility/phpcompatibility-symfony:"*"root@fd2159965127:/var/www/spgamepack.net# composer require --dev dealerdirect/phpcodesniffer-composer-installer:"^0.7" phpcompatibility/phpcompatibility-symfony:"*"
./composer.json has been updated
Running composer update dealerdirect/phpcodesniffer-composer-installer phpcompatibility/phpcompatibility-symfony
Loading composer repositories with package information
Updating dependencies
Lock file operations: 6 installs, 0 updates, 0 removals
  - Locking dealerdirect/phpcodesniffer-composer-installer (v0.7.2)
  - Locking phpcompatibility/php-compatibility (9.3.5)
  - Locking phpcompatibility/phpcompatibility-paragonie (1.3.1)
  - Locking phpcompatibility/phpcompatibility-passwordcompat (1.0.3)
  - Locking phpcompatibility/phpcompatibility-symfony (1.2.0)
  - Locking squizlabs/php_codesniffer (3.7.1)
Writing lock file
Installing dependencies from lock file (including require-dev)
Package operations: 6 installs, 0 updates, 0 removals
  - Downloading squizlabs/php_codesniffer (3.7.1)
  - Downloading dealerdirect/phpcodesniffer-composer-installer (v0.7.2)
  - Downloading phpcompatibility/php-compatibility (9.3.5)
  - Downloading phpcompatibility/phpcompatibility-passwordcompat (1.0.3)
  - Downloading phpcompatibility/phpcompatibility-paragonie (1.3.1)
  - Downloading phpcompatibility/phpcompatibility-symfony (1.2.0)
  - Installing squizlabs/php_codesniffer (3.7.1): Extracting archive
  - Installing dealerdirect/phpcodesniffer-composer-installer (v0.7.2): Extracting archive
  - Installing phpcompatibility/php-compatibility (9.3.5): Extracting archive
  - Installing phpcompatibility/phpcompatibility-passwordcompat (1.0.3): Extracting archive
  - Installing phpcompatibility/phpcompatibility-paragonie (1.3.1): Extracting archive
  - Installing phpcompatibility/phpcompatibility-symfony (1.2.0): Extracting archive
4 package suggestions were added by new dependencies, use `composer suggest` to see details.
Package doctrine/doctrine-cache-bundle is abandoned, you should avoid using it. No replacement was suggested.
Package doctrine/reflection is abandoned, you should avoid using it. Use roave/better-reflection instead.
Package twig/extensions is abandoned, you should avoid using it. No replacement was suggested.
Package sensio/generator-bundle is abandoned, you should avoid using it. Use symfony/maker-bundle instead.
Generating autoload files
29 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
PHP CodeSniffer Config installed_paths set to ../../phpcompatibility/php-compatibility,../../phpcompatibility/phpcompatibility-paragonie,../../phpcompatibility/phpcompatibility-passwordcompat,../../phpcompatibility/phpcompatibility-symfony
Class Sensio\Bundle\DistributionBundle\Composer\ScriptHandler is not autoloadable, can not call post-update-cmd script
Class Sensio\Bundle\DistributionBundle\Composer\ScriptHandler is not autoloadable, can not call post-update-cmd script
Class Sensio\Bundle\DistributionBundle\Composer\ScriptHandler is not autoloadable, can not call post-update-cmd script
Class Sensio\Bundle\DistributionBundle\Composer\ScriptHandler is not autoloadable, can not call post-update-cmd script
No security vulnerability advisories found
root@fd2159965127:/var/www/spgamepack.net#
```

Next, run:
```bash
vendor/bin/phpcs -i
```
```bash
root@fd2159965127:/var/www/spgamepack.net# vendor/bin/phpcs -i
The installed coding standards are MySource, PEAR, PSR1, PSR2, PSR12, Squiz, Zend, PHPCompatibility, PHPCompatibilityParagonieRandomCompat, PHPCompatibilityParagonieSodiumCompat, PHPCompatibilityPasswordCompat, PHPCompatibilitySymfonyPolyfillPHP54, PHPCompatibilitySymfonyPolyfillPHP55, PHPCompatibilitySymfonyPolyfillPHP56, PHPCompatibilitySymfonyPolyfillPHP70, PHPCompatibilitySymfonyPolyfillPHP71, PHPCompatibilitySymfonyPolyfillPHP72, PHPCompatibilitySymfonyPolyfillPHP73, PHPCompatibilitySymfonyPolyfillPHP74 and PHPCompatibilitySymfonyPolyfillPHP80
root@fd2159965127:/var/www/spgamepack.net#
```


## How to use
Now you can use the following commands to inspect the code in your project for PHP cross-version compatibility:
```bash
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP54
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP55
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP56
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP70
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP71
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP72
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP73
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP74
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP80
```

By default, you will only receive notifications about deprecated and/or removed PHP features.

To get the most out of the PHPCompatibilitySymfony rulesets, you should specify a `testVersion` to check against. That will enable the checks for both deprecated/removed PHP features as well as the detection of code using new PHP features.

For a project which should be compatible with PHP 7.1:
```bash
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP71 --extensions=php --runtime-set testVersion 7.1
```
Where

`--standard` -> choose PHP cross-version compatibility

`--extensions=php` -> PHP files only

`testVersion` -> choose PHP version for `deprecated`/`removed` PHP features as well as the detection of code using new PHP features.

Result:
```bash
bin/phpcs -p . -root@fd2159965127:/var/www/spgamepack.net# ./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP71 --extensions=php --runtime-set testVersion 7.1
.W..........................................................    60 / 10197 (1%)
.....W.W.......W.....................W...E..W...............   120 / 10197 (1%)
....................W......................W.W............W.   180 / 10197 (2%)
....W...............................W.......................   240 / 10197 (2%)
............................................................   300 / 10197 (3%)
............................................................   360 / 10197 (4%)
............................................................   420 / 10197 (4%)
............................................................   480 / 10197 (5%)
............................................................   540 / 10197 (5%)
.................................E................W.........   600 / 10197 (6%)
............................................................   660 / 10197 (6%)
............................................................   720 / 10197 (7%)
............................................................   780 / 10197 (8%)
.................................E............E.............   840 / 10197 (8%)
.....................................E......................   900 / 10197 (9%)
............................................................   960 / 10197 (9%)
...............................................W............  1020 / 10197 (10%)
# begining of result. it will be continued to 100%
# ...
# end of result:
FILE: /var/www/spgamepack.net/vendor/squizlabs/php_codesniffer/src/Standards/Generic/Tests/PHP/DisallowAlternativePHPTagsUnitTest.php
-------------------------------------------------------------------------------------------------------------------------------------
FOUND 0 ERRORS AND 1 WARNING AFFECTING 1 LINE
-------------------------------------------------------------------------------------------------------------------------------------
 31 | WARNING | INI directive 'asp_tags' is removed since PHP 7.0
-------------------------------------------------------------------------------------------------------------------------------------


FILE: /var/www/spgamepack.net/vendor/squizlabs/php_codesniffer/src/Standards/Generic/Sniffs/PHP/DisallowAlternativePHPTagsSniff.php
-----------------------------------------------------------------------------------------------------------------------------------
FOUND 0 ERRORS AND 1 WARNING AFFECTING 1 LINE
-----------------------------------------------------------------------------------------------------------------------------------
 51 | WARNING | INI directive 'asp_tags' is removed since PHP 7.0
-----------------------------------------------------------------------------------------------------------------------------------

Time: 2 mins, 34.95 secs; Memory: 84MB
```

`W` - Warning

`E` - Error

`S` - Skip



For more detailed information about setting the `testVersion`, see the README of the generic [PHPCompatibility](https://github.com/PHPCompatibility/PHPCompatibility#sniffing-your-code-for-compatibility-with-specific-php-versions) standard.


## Reference
https://github.com/PHPCompatibility/PHPCompatibilitySymfony

https://github.com/PHPCompatibility/PHPCompatibility#sniffing-your-code-for-compatibility-with-specific-php-versions

https://github.com/squizlabs/PHP_CodeSniffer
