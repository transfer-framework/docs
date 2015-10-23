Procedures and processors
-------------------------

In Transfer, a procedure is a configuration of sources, workers and targets. These procedures are stored as objects and 
are then passed into a processor for processing. 

One intuitive way of explaining procedures and processors is by a cooking example. Imagine you want to prepare a meal. 
You have a cooking recipe which consists of the necessary ingredients and instructions. Now, in order to prepare the 
meal you, as the cook, might look into the recipe to see which ingredients should be used and  how they should be put 
together. In this analogy, the recipe is the procedure consisting of all the  sources, workers and targets, and the 
processor is the cook that reads the procedure and carries out all the operations.

We use the `ProcedureBuilder` to create procedures:

.. code-block:: php

    use Transfer\Procedure\ProcedureBuilder;
    use Transfer\Processor\SequentialProcessor;
    // ...

    $builder = new ProcedureBuilder();

    $procedure = $builder
      ->addSource(new StreamAdapter(fopen('articles.xml'))
      ->addWorker(new XmlStringToXmlObjectTransformer())
      ->addWorker(new XmlObjectToDoctrineEntityTransformer())
      ->addTarget(new DatabaseAdapter());
      ->getProcedure();

    $processor = new SequentialProcessor();
    $processor->addProcedure($procedure);

    $processor->process();

In the example above, a procedure is created and stored in the `$procedure` variable. This procedure is then added
to the processor's procedure queue. When all is set, the processor is told start processing the the procedures.


