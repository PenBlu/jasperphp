# JasperReports for PHP

Package to generate reports with [JasperReports 6](http://community.jaspersoft.com/project/jasperreports-library) library through [JasperStarter v3](http://jasperstarter.sourceforge.net/) command-line tool. More information in https://github.com/cossou/JasperPHP

## Install

```
composer require penblu/yii-jasperphp
```

## Introduction

This package aims to be a solution to compile and process JasperReports (.jrxml & .jasper files).


#### Compiling

First we need to compile our `JRXML` file into a `JASPER` binary file. We just have to do this one time.

**Note:** You don't need to do this step if you are using *Jaspersoft Studio*. You can compile directly within the program.

```php
$jasper = new JasperPHP();
JasperPHP->compile(__DIR__ . '/vendor/penblu/jasperphp/examples/Blank_A4_1.jrxml')->execute();
```

This command will compile the `hello_world.jrxml` source file to a `hello_world.jasper` file.

**Note:** If you are using Laravel 4 run `php artisan tinker` and copy & paste the command above.

#### Processing

Now lets process the report that we compile before:

```php
$jasper = new JasperPHP();
$jasper->process(
	base_path(__DIR__ . '/vendor/penblu/jasperphp/examples/Blank_A4_1.jasper'),
	false,
	array('pdf', 'xlsx'),
	array('php_version' => phpversion())
)->execute();
```

Now check the examples folder! :) Great right? You now have 2 files, `Blank_A4_1.pdf` and `Blank_A4_1.xlsx`.

Check the *API* of the  `compile` and `process` functions in the file `vendor/penblu/jasperphp/JasperPHP.php` file.

#### Listing Parameters

Querying the jasper file to examine parameters available in the given jasper report file:

```php
$jasper = new JasperPHP();
$output = $jasper->list_parameters(
		base_path(__DIR__ . '/vendor/penblu/jasperphp/examples/Blank_A4_1.jasper')
	)->execute();

foreach($output as $parameter_description)
	echo $parameter_description;
```

### Advanced example

We can also specify parameters for connecting to database:

```php
$jasper = new JasperPHP();
$jasper->process(
    base_path(__DIR__ . '/vendor/penblu/jasperphp/examples/Blank_A4_1.jasper'),
    false,
    array('pdf', 'xlsx'),
    array('php_version' => phpversion()),
    array(
      'driver' => 'postgres',
      'username' => 'vagrant',
      'host' => 'localhost',
      'database' => 'samples',
      'port' => '5433',
    )
  )->execute();
```

## Requirements

* Java JDK 1.6 or above
* PHP [exec()](http://php.net/manual/function.exec.php) function
* [optional] [Mysql Connector](http://dev.mysql.com/downloads/connector/j/) (if you want to use database)
* [optional] [Jaspersoft Studio](http://community.jaspersoft.com/project/jaspersoft-studio) (to draw and compile your reports)

