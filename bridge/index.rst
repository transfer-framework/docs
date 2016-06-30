Bridge
======

Introduction
------------

Bridge is a PHP library that provides you with common building blocks for consuming APIs. Bridge uses
:doc:`Transfer </framework/index>` components.

Features
--------

Seamless integration
********************

Bridge allows you to convert raw API responses into PHP objects.

.. code-block:: json

    {
        "name": "John Doe",
        "age": 22
    }

So a response from an API, such as the one shown above, will be converted to a `Person` object:

.. code-block:: php

    $person->getName(); // "John Doe"
    $person->getAge(); // 22

Bridge uses the |symfony_serializer_link| component under the hood.

Virtual properties
******************

In addition to being able to work with objects, Bridge allows you to define virtual properties. Virtual properties are
object properties to which values are assigned only when it is needed.

.. code-block:: php

    class Person
    {
        private $name;
        private $age;
        private $addresses = array();
    }

Let’s assume you’ve already instantiated `Person` object `$person`. Calling `$person->getAddresses();` will result in
another call to the API, the response will be saved in `$addresses` and returned by the `getAddresses()` getter method.
Using virtual properties allows to build your own relational model around the web API.

Caching
*******

Another major challenge with working with external web APIs is performance loss due to repeated API calls. Bridge has
built-in support for caching API responses. This means that subsequent calls will be executed faster, thus improving
the overall performance.

Bridge uses the file system as its default cache storage provided by the |symfony_cache_component_link| component,
although any implementation compatible with the |psr_6_standard_link| is supported by Bridge.

Web Toolbar in Symfony
**********************

In Symfony, all activity will be presented in the profiler toolbar which is commonly seen at the bottom of the page
when you’re application runs in dev environment.

The Bridge part of the toolbar shows the total execution time and count of all the calls that were part of a request.

.. figure:: /_static/bridge/features/toolbar_1.png
   :align: center

   *Symfony Profiler web toolbar*

By clicking into the Bridge profiler page, you’ll see more details on every call that was made.

.. figure:: /_static/bridge/features/toolbar_2.png
   :align: center

   *Symfony Profiler details page*

Installation
------------

Install the latest version using Composer:

.. code-block:: bash

    composer require transfer/bridge

This requires |composer_download_link| to be installed
on your system.

Defining services in PHP
------------------------

Bridge services are defined by using `Service` objects with unique names.

.. code-block:: php

    use Bridge\Service;

    $service = new Service('acme_service');

Each service defines a list of groups. Similarly, each group defines a list of actions. Groups are defined by using
`Group` objects, which are added to services afterwards.

.. code-block:: php

    use Bridge\Group;

    $group = new Group('math');

    $service->addGroup($group);

Actions, which contain the execution logic, are defined by using objects that extend `AbstractAction`. Bridge comes with
several implementations. You are encouraged to create your own implementations actions.

.. code-block:: php

    use Bridge\Action\AbstractAction;

    class AddAction extends AbstractAction
    {
      public function execute(array $arguments = array())
      {
        return $arguments[0] + $arguments[1];
      }
    }

    $group->addAction($action);

Finally, services with defined groups and actions have to be added to the registry.

.. code-block:: php

    use Bridge\Registry;

    $registry = new Registry();

    $registry->addService($service);

Defining services in YAML
-------------------------

Services can also be defined with YAML. The following YAML configuration reflects the same configuration shown
previously. Such configuration is added in `app/config/config.yml`. You can also create a separate configuration file
and import it into the main Symfony configuration file.

.. code-block:: yml
    :caption: app/config/config.yml
    :name: Symfony configuration file

    imports:
      - { resource: acme-bridge.yml }

.. code-block:: yml
    :caption: app/config/acme-bridge.yml
    :name: Bridge service configuration

    bridge:
      services:
        acme_service:
          groups:
            math:
              actions:
                add:
                  class: AddAction # Fully qualified class name

In addition, it is also possible to provide options for services, groups and actions.

.. code-block:: yml
    :caption: app/config/acme-bridge.yml
    :name: Bridge service configuration

    bridge:
      services:
        acme_service:
          options: ~
          groups:
            math:
              options: ~
              actions:
                options: ~
                add:
                  class: AddAction # Fully qualified class name

The option array will be passed as the second argument in the constructor method.

.. code-block:: php

    $service = new Service($name, $options);
    $group = new Group($name, $options);
    $action = new AddAction($name, $options);


Note that YAML configuration is currently only supported by the Bridge Bundle in Symfony.

Using services in PHP
---------------------

To execute an action, we first fetch the group to which the action is related, and then call a method
corresponding to the name of the action and pass the necessary arguments.

.. code-block:: php

    $result = $registry->get('acme_service.math')->add(1, 2);

Using service with Symfony
-------------------------

With Bridge bundle for Symfony, the registry is going to be registered as a service. All services configured in YAML
will be automatically connected to the Symfony service.

.. code-block:: php

    $registry = $this->container->get('bridge.registry');
    $result = $registry->get('acme_service.math')->add(1, 2);

If you're fetching the registry in a controller action, you can use a shortcut:

.. code-block:: php

    $registry = $this->get('bridge.registry');

Cache configuration
-------------------

TODO: write about cache pools

Commands
--------

Bridge comes with a simple console application that provides a set of commands for listing registered services,
executing actions, and managing cache.

If you have configured your `composer.json` file to symlink library apps in the `bin` folder located in the root folder
of your project, the Bridge console apps will also be available in that folder.

.. code-block:: bash

    php bin/bridge

If `bin/bridge` is not presented, for purposes of convenience you can create a symlink on your own. Assuming your on
Linux or Mac OS X you can do it as follows:

.. code-block:: bash

    mkdir bin/
    ln -s vendor/transfer/bridge/bin/bridge bin/bridge
    chmod +x bin/bridge

The last command will mark the `bridge` script as executable.

Providing a registry file
*************************

If you're using Bridge outside Symfony you will need to specify the registry file each time you run any command. The
registry file contains the Bridge registry with all the associated services.

.. code-block:: php
    :caption: registry.php
    :name: Bridge registry

    <?php

    $registry = new Bridge\Registry();

    // define services
    // $service = ...

    // add services
    // $registry->addService($service);

    // return the registry
    return $registry;

Listing services
****************

.. code-block:: bash

    # Without Symfony
    php bin/bridge list -r registry.php

    # With Symfony
    php app/console bridge:list

The `list` command will return a list of registered services, its groups and actions.

Executing actions
*****************

.. code-block:: bash

    # Without Symfony
    php bin/bridge execute [action] [arg1] [arg2] [..] [argn] -r registry.php

    # With Symfony
    php app/console bridge:execute [action] [arg1] [arg2] [..] [argn]

    # Sample calls
    php bin/bridge execute acme_service.math.add 1 2 -r registry.php
    php app/console bridge:execute acme_service.math.add 1 2

The `execute` command takes the full action name (including service and group names) as its first argument, and variable
list of arguments. The action is executed and the response is outputted in the console.

Listing cache pools
*******************

.. code-block:: bash

    # Without Symfony
    php bin/bridge cache:pools [pool_name] -r registry.php

    # With Symfony
    php app/console bridge:cache:pools [pool_name]

This `cache:pools` command will output the list of registered cache pools.

Clearing cache for a cache pool
*******************************

.. code-block:: bash

    # Without Symfony
    php bin/bridge cache:clear [pool_name] -r registry.php

    # With Symfony
    php app/console bridge:cache:clear [pool_name]

This `cache:clear` command will clear all cache for a specific cache pool.

Removing specific cache in a cache pool
***************************************

.. code-block:: bash

    # Without Symfony
    php bin/bridge cache:remove [pool_name] [action] [arg1] [arg2] [..] [argn] -r registry.php

    # With Symfony
    php app/console bridge:cache:remove [pool_name] [action] [arg1] [arg2] [..] [argn]

This `cache:remove` command will remove cache associated with an action call with specific arguments.

Configuration reference
-----------------------

TODO: paste in output of `php debug:config bridge`

.. |composer_download_link| raw:: html

   <a href="https://getcomposer.org/download/" target="_blank">Composer</a>

.. |symfony_serializer_link| raw:: html

   <a href="http://symfony.com/doc/current/components/serializer.html" target="_blank">Symfony Serializer</a>

.. |symfony_cache_component_link| raw:: html

  <a href="http://symfony.com/blog/new-in-symfony-3-1-cache-component" target="_blank">Symfony Cache</a>

.. |psr_6_standard_link| raw:: html

  <a href="http://www.php-fig.org/psr/psr-6/" target="_blank">PSR-6 standard</a>
