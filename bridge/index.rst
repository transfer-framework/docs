Bridge
======

Introduction
------------

Writing your own API client from scratch may often take some time. Bridge helps you accelerate and
ease this process by providing a set of tools for API consumption. These tools help you fetch
resources, cache API responses and transform data.

Bridge is written in PHP and uses :doc:`Transfer </framework/index>` components internally.

Features
--------

Caching
*******

Another challenge with working with external web APIs is performance loss due to repeated API calls. Bridge has
built-in support for caching API responses. This means that subsequent calls will be executed faster, thus improving
the overall performance.

Bridge uses the file system as its default cache storage provided by the |symfony_cache_component_link| component,
although any implementation compatible with the |psr_6_standard_link| is supported by Bridge.

Event system
************

Bridge comes with a built-in event system that lets you hook your own event listeners to a set of events. The listeners can
access events before and after the execution of an action. This lets an event listener do activities such as modifying
call arguments or changing an action response.

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

Installing Bridge
-----------------

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

Using services in PHP
---------------------

To execute an action, we first fetch the group to which the action is related, and then call a method
corresponding to the name of the action and pass the necessary arguments.

.. code-block:: php

    $result = $registry->get('acme_service.math')->add(1, 2);

Adding event listeners/subscribers
----------------------------------

If you're not familiar with event listeners/subscribers, please read the Symfony's `Event Dispatcher <http://symfony.com/doc/master/event_dispatcher.html>`_
documentation before moving on.

Event listeners/subscribers are registered by services. To add an event listener, call the `addEventListener` method
on a service and pass the event type the listener should be activated on and the event listener callable.

.. code-block:: php

    use Bridge\Events\BridgeEvents;
    use Bridge\Event\PreActionEvent;
    use Bridge\Event\PostActionEvent;

    class Listener {
        public function preAction(PreActionEvent $event)
        {
        }

        public function postAction(PostActionEvent $event)
        {
        }
    }

    // The callable will be invoked before an action is executed
    $service->addEventListener(BridgeEvents::PRE_ACTION, array(new Listener(), 'preAction'));

    // The callable will be invoked after an action is executed
    $service->addEventListener(BridgeEvents::POST_ACTION, array(new Listener(), 'postAction'));

Cache pools
-----------

Using the default cache pool
****************************

The default cache pool can be accessed through the registry:

.. code-block:: php

    $pool = $registry->getCachePool('default');

The object returned implements `Psr\\Cache\\CacheItemPoolInterface`.

Adding new cache pools
**********************

Adding new cache pools is done through the registry:

.. code-block:: php

    $pool = $registry->addCachePool('pool_name', $cachePool);

The `$cachePool` argument must implement `Psr\\Cache\\CacheItemPoolInterface`.

To learn more about how cache pools are used, check out the |psr_6_standard_link| documentation.
If you are interested in cache pool implementations, check out the |symfony_cache_component_link| component.

Using Bridge with Symfony
-------------------------

Installing Bridge Bundle
************************

Install the latest version using Composer:

.. code-block:: bash

    composer require transfer/bridge-bundle

This requires |composer_download_link| to be installed
on your system.

Then, enable the bundle by adding the following line in the app/AppKernel.php file of your project:

.. code-block:: php
    :caption: app/AppKernel.php
    :name: Application kernel file

    class AppKernel extends Kernel
    {
      public function registerBundles()
      {
          $bundles = array(
              // ...
              new Bridge\Bundle\BridgeBundle(),
          );

          // ...
      }
    }

Defining services in YAML
************************

Services can also be defined in YAML. The following YAML configuration reflects the same configuration shown
previously in `Defining services in PHP`_. Such configuration is added in `app/config/config.yml`.
You can also create a separate configuration file and import it into the main Symfony configuration file.

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

Using services via service container
************************************

With Bridge bundle for Symfony, the registry is going to be registered as a service. All services configured in YAML
will be automatically connected to the Symfony service.

.. code-block:: php

    $registry = $this->container->get('bridge.registry');
    $result = $registry->get('acme_service.math')->add(1, 2);

If you're fetching the registry in a controller action, you can use a shortcut:

.. code-block:: php

    $registry = $this->get('bridge.registry');

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

Note that the filename of the registry is arbitrary.

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

The following section lists all options that are available to configure Bridge.

.. code-block:: yml

    # Default configuration for extension with alias: "bridge"
    bridge:

        # List of services
        services:

            # Prototype: Service definition
            name:

                # Service type
                type:                 ~

                # Service class
                class:                Bridge\Service

                # Custom options
                options:              []

                # List of groups
                groups:

                    # Prototype: Group definition
                    name:

                        # Group type
                        type:                 ~

                        # Group class
                        class:                Bridge\Group

                        # Custom options
                        options:              []

                        # List of actions
                        actions:

                            # Prototype: Action definition
                            name:

                                # Action type
                                type:                 ~

                                # Action class
                                class:                Bridge\Action\NullAction

                                # Custom options
                                options:              ~


.. |composer_download_link| raw:: html

   <a href="https://getcomposer.org/download/" target="_blank">Composer</a>

.. |symfony_serializer_link| raw:: html

   <a href="http://symfony.com/doc/current/components/serializer.html" target="_blank">Symfony Serializer</a>

.. |symfony_cache_component_link| raw:: html

  <a href="http://symfony.com/blog/new-in-symfony-3-1-cache-component" target="_blank">Symfony Cache</a>

.. |psr_6_standard_link| raw:: html

  <a href="http://www.php-fig.org/psr/psr-6/" target="_blank">PSR-6 standard</a>
