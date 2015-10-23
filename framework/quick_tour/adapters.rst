Adapters
--------

In Transfer, we work with sources and targets. Generally, a source is any object or location from which data is 
*received*. In a similar way, targets are any object or location to which data can be *sent*.

These sources and targets are implemented as adapters. An adapter helps two incompatible interfaces to work together. 
Here is an example of the stream adapter we looked at in :doc:`the_big_picture` chapter:

.. code-block:: php

    use Transfer\Adapter\SourceAdapterInterface;
    use Transfer\Adapter\TargetAdapterInterface;

    class StreamAdapter implements SourceAdapterInterface, TargetAdapterInterface
    {
        private $stream;

        public function __construct()
        {
            $this->stream = fopen('./');
        }

        public function receive(Request $request)
        {
            fseek($this->stream, 0);

            return new Response(array(stream_get_contents($this->stream)));
        }

        public function send(Request $request)
        {
            foreach ($request->getData() as $data) {
                fwrite($this->stream, $data);
            }
            
            return new Response();
        }
    }

An adapter can implement either `SourceAdapterInterface` and `TargetAdapterInterface`, or both. Whether an adapter will
function as a source or a target, depends on which interfaces have been implemented. 

In our example, both interfaces are implemented. The `receive` method is part of `SourceAdapterInterface` and is 
responsible for receiving data from the source. In a similar way, the `send` method is part of `TargetAdapterInterface`
and is responsible for sending data to the target.

Note that the same stream functions as both the source and the target, as it is used whenever data is received or sent.

Both `receive` and `send` methods take a `Request` object as an argument and return a `Response` object. The data format
and the overall way these are used greatly depends on the end interface which the adapter uses.


