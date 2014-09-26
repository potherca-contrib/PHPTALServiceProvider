# PHPTALServiceProvider, a part of Silex Framework Providers

[Silex] is a lightweight PHP microframework, based on [Symfony2] components.
[PHPTAL] is a PHP template engines which implementations the excellent Zope Page Template (ZPT) system for PHP.

This extension allows you to use PHPTAL as a template engine in Silex.

## Installation

The best way to install this service provider is [using Composer].
Just require Silex, PHPTAL and the PHPTALServiceProvider from your `composer.json` file:

        {
            "require": {
                "brtriver/PHPTALServiceProvider": "dev-master"
                "phptal/phptal": "~1.3"
                "silex/silex": "~1.1",
            }
        }


When composer update (or install) is run, the PHPTALServiceProvider, the PHPTAL library an the Silex framework will be downloaded to the `vendor/` directory. PHPTAL templates should be put in the  `views/` directory.

    /project_directory
        │  ├── composer.json
        │  └── composer.lock
        ├── vendor
        │   ├── bin
        │   ├── brtriver
        │   │   └── PHPTALServiceProvider
        │   │       └─ PHPTALServiceProvider.php
        │   └── phptal
        │   │   └── phptal
        │   └── silex
        │   │   └── silex
        │   └── autoload.php
        ├── views
        │   └── test.html (PHPTAL template files should be placed here)
        └── web
            ├── .htaccess
            └── index.php

## Sample Code

Registering the PHPTALServiceProvider to the Silex Application will make PHPTAL available from `$app['phptal']`. 
After setting a template path, this can be used like you would use PHPTAL itself.

A full example would look like this:

### index.php

    <?php

    require_once 'vendor/autoloader.php';

    use Silex\Application;
    use Silex\Provider\PHPTALServiceProvider;

    $app = new Application();
    $app['phptal.class_path'] = __DIR__ . '/vendor/phptal/phptal';
    $app->register(new PHPTALServiceProvider());

    $app->get('/hello/{name}', function($name) use ($app) {
        // Set a view file, should be located in the /views directory
        $app['phptal.view'] = "test.html";
        
        // Set a property on the Template directly 
        $app['phptal']->title = "PHPTAL in Silex";
        // Alternatively, set a property on the Template using the set method
        $app['phptal']->set('name', $name);
        
        return $app['phptal']->execute();
    });

    $app->run();

### test.html

    <!DOCTYPE html>
    <html xmlns:tal="http://xml.zope.org/namespaces/tal">
    <head>
        <title tal:content="title">
            Place for the page title
        </title>
    </head>  
    <body>
        <h1 tal:content="title">sample title</h1>
        <table>
            <thead>
                <tr>
                    <th>Name</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td tal:content="name">person's name</td>
                </tr>
                <tr tal:replace="">
                    <td>sample name</td>
                </tr>
            </tbody>
        </table>
    </body>
    </html>

## License

PHPTALExtension is licensed under the [MIT license].

[PHPTAL]: http://phptal.org/manual/en/split/introduction.html
[Silex]: http://silex.sensiolabs.org/
[Symfony2]: http://symfony.com
[using Composer]: https://getcomposer.org/doc/01-basic-usage.md#installing-dependencies
[MIT license]: LICENSE
