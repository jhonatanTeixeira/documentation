Behaviors
=========

These are annotations that categorizes your components and adds specific behaviors to them, and can as well add AOP design to your code, there are some built in behaviors on the framework, 
however is really easy to create custom ones

Controller
----------

Marks your class as a Controller and define a base route to its methods, commonly used for HTTP apis, however may be used on another protocols as well

.. code-block:: php

  use Vox\Framework\Behavior\Controller;
  
  #[Controller('/')]
  class SomeController {
    
  }


Get, Post, Put, Patch, Delete
-----------------------------

Marks a controller methods as a HTTP endpoints, theres one behavior for each HTTP verb

.. code-block:: php

  use Vox\Framework\Behavior as Behavior;
  
  #[Behavior\Controller('/foo')]
  class SomeController {
      #[Behavior\Get('/list')]
      public function list() {}
      
      #[Behavior\Post('/save')]
      public function save() {}
      
      #[Behavior\Put('/update')]
      public function update() {}
      
      #[Behavior\Patch('/partial/update')]
      public function edit() {}
      
      #[Behavior\Delete('/delete')]
      public function delete() {}
  }
