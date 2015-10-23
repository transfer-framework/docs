Workers
-------

Usually, whenever data is imported or exported from one system to another, the data is incompatible between the two.
This introduces the need for transforming data into a more compliant format. In Transfer, this is achieved by using
workers. A worker is a callable (class method or a function) that takes some data as the argument and returns modified 
data. 

Here is an example of a worker that transforms a string from XML to CSV format:

.. code-block:: php

    use Transfer\Procedure\ProcedureBuilder;
    use Transfer\Commons\Stream\Adapter\StreamAdapter;

    $pb = new ProcedureBuilder();

    $pb
        ->addSource(new StreamAdapter(fopen('xml_file.xml', 'r')))
        ->addWorker(function ($rawXmlString) {
            $node = simplexml_load_string($rawXmlString);

            return sprintf("First Name,Last Name\n%s,%s",
                $node->firstName,
                $node->lastName
            );
        })
        ->addTarget(new StreamAdapter(fopen('csv_file.csv', 'w')));

Given an XML string:

.. code-block:: xml

    <xml>
        <firstName>John</firstName>
        <lastName>Doe</lastName>
    </xml>

It will be transformed into a CSV string, similary to this:

.. code-block:: csv

    First Name,Last Name
    John,Doe

