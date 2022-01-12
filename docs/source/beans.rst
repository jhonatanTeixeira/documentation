Beans
=====

This framework borrows the spring framework beans concept, a bean is an object that is instanced and managed by the IoC container, the IoC container will instance and inject all of its dependencies.
A bean must be either: 

1. Annotated by a behavior/stereotype

.. code-block:: php

  use PhpBeans\Annotation\Component;

  #[Component]
  class BarComponent
  {
    ...
  }


2. Exposed by a factory method on a configuration class


Configuration classes are basicaly classes full of factory methods, the methods has to be annotated with Bean, so the container builder can locate these methods and start to instance them

.. code-block:: php

  use PhpBeans\Annotation\Bean;
  use PhpBeans\Annotation\Configuration;
  
  #[Configuration]
  class BeanConfiguration
  {
      #[Bean] // you can also alias the bean, pass as first paramater Bean('bean.name')
      public function beanComponent(BarComponent $fooComponent): BeanComponent
      {
          return new BeanComponent($fooComponent);
      }
  }

Configuration values
--------------------

You can set a configuration value directly into the container, and inject it on any property usin the Value annotation. You can set the configuration value either by:

1. Set directly on the container

.. code-block:: php
  
  $app->getContainer()->set('app.some_value', 'some value')

2. Use a configuration file:

.. code-block:: yml
  
  # configs/application.yaml
  
  app:
    some-value: some value

.. code-block:: php
  
  # boostrap.php
  
  $app->getContainerBuilder()->withConfigFile('configs/application.yaml');

Injecting the value:
--------------------

.. code-block:: php

  use PhpBeans\Annotation\Bean;
  use PhpBeans\Annotation\Configuration;
  use PhpBeans\Annotation\Value;
  
  
  #[Configuration]
  class BeanConfiguration
  {
      #[Value('app.some_value', defaultValue: 'default')]
      private string $defaultValue;

      #[Bean]
      public function beanComponent(BarComponent $fooComponent): BeanComponent
      {
          return new BeanComponent($fooComponent, $this->defaultValue);
      }
  }
  
Injecting the beans
-------------------
 
Once you have the beans declare somehow, its time to inject them, and there are several ways for doing that:
 
1. Autowired

Inject the bean directly into a property (this is made via reflection under the hood)
 
.. code-block:: php
  
  use PhpBeans\Annotation\Autowired;

  #[Component]
  class SomeComponent {
      #[Autowired]
      private OtherComponent $otherComponent;
  }
 
 
2. Via constructor
 
 If the service is not aliased, it will be resolved by its type, so all you need is to declare the constructor with type hints
 
.. code-block:: php
  
  use PhpBeans\Annotation\Autowired;

  #[Component]
  class SomeComponent {
      
      public function __constructor(OtherComponent $otherComponent) {
      }
  }
 
However if the bean is aliased or you don't want to typehint the constructor you can use the Injects parameter annotation
 
.. code-block:: php
  
  use PhpBeans\Annotation\Autowired;

  #[Component]
  class SomeComponent {
      
      public function __constructor(
          #[Injects('app.some_bean')] // it will also work for configuration values
          OtherComponent $otherComponent
      ) {
      }
  }
