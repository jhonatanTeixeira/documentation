Welcome to Vox Framework documentation!
===================================

**Vox Framework** is a PHP framework heavily inspired by Java's Spring-boot framework, it borrows its main concepts to create an simple AOP environment for fast development with high decoupling in mind. It's fully PSR compatible. Here's a basic vox framework controller example using PHP 8:

```php

use PhpBeans\Annotation\Autowired;
use Vox\Framework\Behavior\Controller;
use Vox\Framework\Behavior\Delete;
use Vox\Framework\Behavior\Get;
use Vox\Framework\Behavior\Post;
use Vox\Framework\Behavior\Put;
use Vox\Framework\Behavior\RequestBody;
use Vox\Framework\Exception\HttpNotFoundException;

#[Controller('/foo')]
class FooController
{
    #[Autowired]
    private FooService $service;

    #[Autowired]
    private MockableService $mockableService;

    #[Get('/mock')]
    public function getMockData() {
        return $this->mockableService->getMockData();
    }

    #[Get]
    public function list() {
        return $this->service->list();
    }

    #[Get('/{id}')]
    public function get($id) {
        $value = $this->service->get($id);

        if (!$value) {
            throw new HttpNotFoundException();
        }

        return $value;
    }

    #[Post]
    public function post(FooDto $data) {
        return $this->service->post($data);
    }

    #[Put('{id}')]
    #[RequestBody('data')]
    public function put($id, FooDto $data) {
        return $this->service->put($id, $data);
    }

    #[Delete]
    public function delete($id) {
        return $this->service->delete($id);
    }
}

```

Check out the :doc:`bootstraping` section for further information, including
how to :ref:`installation` the project.

.. note::

   This project is under active development.

Contents
--------

.. toctree::

   bootstraping
   beans
   behaviors
   serialization
   persistence
   event-system
