The Big Picture
---------------

A common data import or export job usually consists of a **source** and a **target**. Generally, a source is any object 
or location from which data is *received*. In a similar way, targets are any object or location to which data can be 
*sent*.

Common examples for both sources and targets include structured files (XML, CSV, Json), databases, or third party APIs.

Often, it is also desirable to transform data so that it becomes compatible with the target. In such cases, **workers**
are used. A worker does necessary changes to data before it is sent to targets.

.. figure:: /_static/framework/quick_tour/flow.png
   :align: center

   *Data flow*

A common example of data import is having a structured file, such as an XML, as a source and a database as the target.
Because databases can not accept XML files as valid input, these files have to be transformed into something a database
can understand. In the example below we transform XML files to XML objects, which then are transformed to Doctrine
entities. Finally, the doctrine entities are sent to the database.

.. figure:: /_static/framework/quick_tour/flow_2.png
   :align: center

   *Data import from XML files to database*

We use **procedures** to define the configuration of various sources, workers and targets. In the case of the XML to
Database example, we have one source, two workers, and one target.

.. figure:: /_static/framework/quick_tour/procedure_3.png
   :align: center

   *XML Files to Database procedure*

When procedures are defined, these are passed into a **processor**. A processor orchestrates the data flow and carries
out communication between sources, workers and targets.

In Transfer, procedures are set up by using the procedure builder.

.. code-block:: php

   use Transfer\Procedure\ProcedureBuilder;
   use Transfer\Processor\SequentialProcessor;

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

In this example, we replicate our procedure in PHP by using the procedure builder. The procedure object is then appended
to a processor. When everything is set up, the processor is invoked and procedure processing starts.
