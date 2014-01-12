---
title: Using composer packages with FuelPHP
description: Using the new standard with your already awesome FuelPHP App.
tags:
  - PHP
  - Composer
  - Packagist
  - FuelPHP
layout: post
---

I've been using [FuelPHP](http://fuelphp.com) for a while now, and it has a really nice package system built in that makes integrating third party services really nice, up until about 2 months ago, when i needed a library that wasn't already available as a package, i would write my own, i did this for *PostmarkApp*, and now *Mandrill*, which we use at [Simply Satisfied](http://simplysatisfied.net) to deliver email.

But then i stumbled across [Composer](http://getcomposer.org/) and [Packagist](https://packagist.org/), and the already vast library of fully unit-tested packages already available there.

### Autoloading ###

FuelPHP already has a pretty awesome autoloader, and it allows you to add classes quickly:

{% highlight php startinline %}
Autoloader::add_classes(array(
	'MyClass' => 'path/to/my/class.php'
));
{% endhighlight %}

But it has limitations, and it's hard to know when you've gotten it right (at least from my experience).

But composer has a magic bullet - it's own `autoload.php` file that it generates when you run install.

Essentially, you'll want to have your files like below, with your `composer.json` file sitting in the fuel/app/ directory:

{% highlight text %}
fuel/
-- app/
---- composer.json
---- composer.phar
---- vendor/
{% endhighlight %}

Then, run `php composer.phar install` to install all the listed dependencies.

The magic bit? Open up `fuel/app/boostrap.php` and change it in the following:

{% highlight php startinline %}

// Load in the Autoloader
require COREPATH.'classes'.DIRECTORY_SEPARATOR.'autoloader.php';
class_alias('Fuel\\Core\\Autoloader', 'Autoloader');

// Bootstrap the framework DO NOT edit this
require COREPATH.'bootstrap.php';

// ADD THIS! - This brings in the composer autoload files.
require APPPATH.'vendor/autoload.php';

Autoloader::add_classes(array(
	// Add classes you want to override here
	// Example: 'View' => APPPATH.'classes/view.php',
));

// Register the autoloader
Autoloader::register();

/**
 * Your environment.  Can be set to any of the following:
 *
 * Fuel::DEVELOPMENT
 * Fuel::TEST
 * Fuel::STAGE
 * Fuel::PRODUCTION
 */
Fuel::$env = (isset($_SERVER['FUEL_ENV']) ? $_SERVER['FUEL_ENV'] : Fuel::DEVELOPMENT);

// Initialize the framework with the config file.
Fuel::init('config.php');

{% endhighlight %}

And there we go! All the libraries, namespaces and classes you've added with composer should now be available.
