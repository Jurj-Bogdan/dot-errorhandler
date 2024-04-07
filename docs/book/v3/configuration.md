# Configuration

Register `dot-errorhandler` in you project by adding `Dot\ErrorHandler\ConfigProvider::class` to your configuration aggregator (to `config/config.php` for example),
and add `\Dot\ErrorHandler\ErrorHandlerInterface::class` (to `config/pipeline.php` for example) **as the outermost layer of the middleware** to catch all Exceptions

- Configure the error handler as shown below

configs/autoload/error-handling.global.php

```php
<?php

use Dot\ErrorHandler\ErrorHandlerInterface;
use Dot\ErrorHandler\LogErrorHandler;
use Dot\ErrorHandler\ErrorHandler;

return [
    'dependencies' => [
        'aliases' => [
            ErrorHandlerInterface::class => LogErrorHandler::class,
        ]

    ],
    'dot-errorhandler' => [
        'loggerEnabled' => true,
        'logger' => 'dot-log.default_logger'
    ]
];
```

When declaring the `ErrorHandlerInterface` alias you can choose whether to log or not:
- for the simple Zend Expressive handler user `ErrorHandler`
- for logging use `LogErrorHandler`

The class `Dot\ErrorHandler\ErrorHandler` is the same as the Zend Expressive error handling class
the only difference being the removal of the `final` statement for making extension possible.

The class `Dot\ErrorHandler\LogErrorHandler` is `Dot\ErrorHandler\ErrorHandler` with
added logging support.

As a note: both `LogErrorHandler` and `ErrorHandler` have factories declared in the
package's `ConfigProvider`. If you need a custom ErrorHandler it must have a factory
declared in the config, as in the example.

Example:
```php
<?php

use Dot\ErrorHandler\ErrorHandlerInterface;
use Custom\MyErrorHandler;
use Custom\MyErrorHandlerFactory;


return [
    'dependencies' => [
        'factories' => [
            MyErrorHandler::class => MyCustomHandlerFactory::class,
        ],
        
        'aliases' => [
            ErrorHandlerInterface::class => MyErrorHandler::class,
        ]

    ],
    'dot-errorhandler' => [
        'loggerEnabled' => true,
        'logger' => 'dot-log.default_logger'
    ]
];
```
Config examples can be found in this project's `config` directory.
