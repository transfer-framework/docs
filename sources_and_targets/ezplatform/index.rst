eZ Platform
===========

Prerequisites
-------------

This documentation assumes that you know how to setup eZ Platform (previously
known as eZ Publish) and have done it before. We also assume that you have a
basic understanding of the content model used by eZ Platform, and how services
are used to work with content on eZ Platform.

To learn how to install eZ Platform, check out the
`Get Started with eZ Platform <https://doc.ez.no/display/DEVELOPER/Get+Started+with+eZ+Platform>`_
article published by eZ.

To learn about the eZ Platform content model and services, checkout the
`eZ Platform Public API Guide <https://doc.ez.no/display/DEVELOPER/Public+API+Guide>`_.

This guide covers everything you will need to know about eZ Platform before
proceeding with using the eZ Platform Transfer extension.

Introduction
------------

The eZ Platform extension for Transfer is a set of components meant to simplify
the process of importing data into eZ Platform.

Along with a simplified way of representing eZ Platform objects, the extension
also features functionality for automating many of the verbose procedures related to
publishing content in eZ Platform.

Features
--------

Automatic location creation
***************************

Whenever a Content object is published, you can choose whether a location should
automatically be created.

Content tree import
*******************

The extension supports importing a content tree consisting of content objects
that are hierarchically pre-arranged. When the content tree is published through
the extension, content objects and their locations will be created according to
the tree structure.

Object model
------------

The extension defines a set of proxy objects, each which corresponding to its
eZ Platform counterpart. The intention behind an additional abstraction layer is
to simplify the creation of objects, such as content objects, and work with these
objects in a similar way it is done with entities with Doctrine ORM.

Unless noted otherwise, when we say location object, we refer to the location proxy
object ``Transfer\EzPlatform\Repository\Values\LocationObject`` defined by the extension.

When we say location, we refer to a value object in the eZ Platform content model:
``eZ\Publish\API\Repository\Values\Content\Location``

Objects
-------

This section introduces a list of the proxy objects defined by the extension.

All of these have a one-to-one relation with the eZ Platform objects. In addition to
built-in eZ Platform capabilities, Transfer proxy objects may feature extra options
for simplifying imports or doing more advanced tasks.

The proxy objects hold a list of field values and additional properties that affect
how an object is going to be translated to an eZ Platform object and imported.


=========================================================================================== ============================================================================================================================
Transfer object                                                                             Corresponding eZ Platform object
=========================================================================================== ============================================================================================================================
:doc:`ContentObject </sources_and_targets/ezplatform/the_objects/contentobject>`            Content
:doc:`LocationObject </sources_and_targets/ezplatform/the_objects/locationobject>`          Location
:doc:`ContentTypeObject </sources_and_targets/ezplatform/the_objects/contenttypeobject>`    ContentType
:doc:`UserObject </sources_and_targets/ezplatform/the_objects/userobject>`                  User
:doc:`UserGroupObject </sources_and_targets/ezplatform/the_objects/usergroupobject>`        UserGroup
:doc:`LanguageObject </sources_and_targets/ezplatform/the_objects/languageobject>`          Language
=========================================================================================== ============================================================================================================================


.. toctree::
    the_objects/contentobject
    the_objects/locationobject
    the_objects/contenttypeobject
    the_objects/userobject
    the_objects/usergroupobject
    the_objects/languageobject
