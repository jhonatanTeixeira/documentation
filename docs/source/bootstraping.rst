Bootstraping
=====

.. installation:

Installation
------------
Install the library using composer

.. code-block:: console

  $ composer install vox/framework
  
Bootstraping
------------

Bootstrap the application by instantianting and configuring the Application class:

.. code-block:: php

  $app = new Application();
  
Now you can configure the application and the container builder.

.. code-block:: php

  $app->configure();

You can configure the builder by passing a callback to the configurer:

.. code-block:: php

  use PhpBeans\Factory\ContainerBuilder;
  
  $app->configure(function (ContainerBuilder $cb) {
    $cb->withNamespaces('My\\Namespace');
  });

Or you can access the builder instance

.. code-block:: php

  $app->getContainerBuilder()->withNamespaces('My\\Namespace');
 
Then run the application:

.. code-block:: php

  $app->run();

PSR-7 Compatible
----------------

Under the hood, vox framework uses slim app, so its psr-7 compliant, meaning that you can handle calls using a request interface:

.. code-block:: php
  
  use Slim\Psr7\Request;
  
  $app->handle(new Request('GET', '/foo'));
