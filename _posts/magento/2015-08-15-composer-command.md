---
layout: master
category: webdev
title: Composer command for development
---

A quick cheatsheet for composer command

``` shell
composer require vendor/package:version
```

* This is for situation where the package is not yet in the composer.json file
* This command will then add the package into the composer.json and retrieve the corresponding package, then update the composer.lock

***

``` shell
composer install
```

* This is for situation where there is a composer.json and composer.lock
* This command will only look into the composer.lock and install the modules as of the lock file

***

``` shell
composer update [vendor/package]
```

* This is for situation where one would like to update the module according to the composer.json
* This command will look at the composer.json and retrieve up-to-date module qualified in the given version
* If the version qualification is `5.0.*`, the current local version (stored in composer.lock) is `5.0.1`, and the up-to-date version is `5.0.2`. Running this command will update that module to `5.0.2` in local.
* The composer.lock wll get updated to reflect that change.