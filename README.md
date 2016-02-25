# Bright Nucleus Config Component

[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/brightnucleus/config/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/brightnucleus/config/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/brightnucleus/config/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/brightnucleus/config/?branch=master)
[![Build Status](https://scrutinizer-ci.com/g/brightnucleus/config/badges/build.png?b=master)](https://scrutinizer-ci.com/g/brightnucleus/config/build-status/master)
[![Codacy Badge](https://api.codacy.com/project/badge/grade/e718c34afc46409b90fd72a107158009)](https://www.codacy.com/app/BrightNucleus/config)
[![Code Climate](https://codeclimate.com/github/brightnucleus/config/badges/gpa.svg)](https://codeclimate.com/github/brightnucleus/config)

[![Latest Stable Version](https://poser.pugx.org/brightnucleus/config/v/stable)](https://packagist.org/packages/brightnucleus/config)
[![Total Downloads](https://poser.pugx.org/brightnucleus/config/downloads)](https://packagist.org/packages/brightnucleus/config)
[![Latest Unstable Version](https://poser.pugx.org/brightnucleus/config/v/unstable)](https://packagist.org/packages/brightnucleus/config)
[![License](https://poser.pugx.org/brightnucleus/config/license)](https://packagist.org/packages/brightnucleus/config)

This is a very lean Config component to help you write reusable code. It only offers basic functionality and is meant to be used in libraries and small projects. If you need a Config component for more complex projects, you should take a look at the [Symfony Config Component](http://symfony.com/doc/current/components/config/index.html).

## Table Of Contents

* [Installation](#installation)
* [Basic Usage](#basic-usage)
	* [Example - Configuration File](#example-configuration-file)
	* [Example - Configurable Class](#example-configurable-class)
	* [Example - Getting The Config Into The Class](#example-getting-the-config-into-the-class)
* [Advanced Usage](#advanced-usage)
	* [Configuration Schema](#configuration-schema)
	* [Configuration Validation](#configuration-validation)
	* [Custom Implementations](#custom-implementations)
* [Contributing](#contributing)

## Installation

The best way to use this component is through Composer:

```BASH
composer require brightnucleus/config
```

## Basic Usage

A class that wants to be configurable should accept a `ConfigInterface` in its constructor, so that the Config can be injected. The surrounding code then should inject an instance of an object (for example the generic `Config` that is provided with this component). This way, the class that accepts the Config can be written in a 100% reusable way, while all project-specific stuff will be injected through the Config.

### Example - Configuration File

The snippet below shows the basic structure of a config file.

```PHP
<?php namespace BrightNucleus\Example;

/*
 * Example class main settings.
 */
$example = [
	'test_key' => 'test_value',
];

return [
	'BrightNucleus' => [
		'Example' => $example,
	],
];
```

### Example - Configurable Class

Here is an example setup of how you could feed this configuration into a plugin.

```PHP
<?php namespace BrightNucleus\Example;

use BrightNucleus\Config\ConfigInterface;
use BrightNucleus\Config\ConfigTrait;
use BrightNucleus\Exception\RuntimeException;

class Example {

	use ConfigTrait;

	/**
	 * Instantiate an Example object.
	 *
	 * @param ConfigInterface $config Config to parametrize the object.
	 * @throws RuntimeException       If the Config could not be parsed correctly.
	 */
	public function __construct( ConfigInterface $config ) {
		$this->processConfig( $config );
	}

	/**
	 * Do something.
	 */
	public function run() {
		$key = 'test_key';

		return sprintf(
		_( 'The value of the config key "$1%s" is "$2%s".'),
			$key,
			$this->getConfigKey( $key )
		);
	}
}
```

### Example - Getting The Config Into The Class

You can combine all of your configurations into 1 single file and only pass "Sub-Configurations" to the individual components using the `getSubConfig()` method. This way, you can avoid an additional file access and an additional validation pass for each component.

Here's how you can pass the configuration file into the class:

```PHP
<?php namespace BrightNucleus\Example;

function init() {
	$configFile = __DIR__ . '/config/example_settings.php';
	$config     = new Config( include( $configFile ) );
	$example    = new Example( $config->getSubConfig( 'BrightNucleus\Example' ) );

	// Outputs:
	// The value of the config key "test_key" is "test_value".
	echo $example->run();
}
```

## Advanced Usage

### Configuration Schema

> TODO

### Configuration Validation

> TODO

### Custom Implementations

> TODO

## Contributing

All feedback / bug reports / pull requests are welcome.

Please use the provided `pre-commit` hook. To install it, run the following command from the project's root:
```BASH
ln -s ../../.pre-commit .git/hooks/pre-commit
```
