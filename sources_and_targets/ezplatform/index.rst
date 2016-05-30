===============
eZ Platform
===============

---------------
Introduction
---------------

First note: Some parts of this documentation might take for granted that you know most of the wording in the eZPublish/eZPlatform world, so if you have questions regarding eZPlatform, please check out their documentation on https://doc.ez.no/.

Second note: When we're talking about specific objects between Transfer and EzPlatform we only specify their classnames.
None of the classes in transfer/ezplatform has an identical name in ezsystems/ezpublish-kernel.

E.g.:

* When we say ``Location``, we are talking about the eZPlatform object: ``eZ\Publish\API\Repository\Values\Content\Location``.
* When we say ``LocationObject``, we are talking about the Transfer object: ``Transfer\EzPlatform\Repository\Values\LocationObject``.

Hence the somewhat silly appending of **Object** on every valueobject.


---------------
The objects
---------------

With transfer/ezplatform you may store the below types of data to an eZ Platform installation.
Most of these have a one-to-one relation with the corresponding eZPlatform objects, but with features allowing us to mix it up, and do some more powerful or simplified imports.
Whenever you import single pieces of data, wheather it be Users or ContentTypes, we wrap the data in different implementations of EzPlatformObject to let Transfer know what kind of data we want to manipulate.
Whenever we want to create, update or remove a Location, we wrap our identifiers and action in a LocationObject. For Content we wrap it in a ContentObject and so forth.
See this table for more of these:

=========================================================================================== ============================================================================================================================
Transfer object                                                                             Corresponding eZPlatform object
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
