^^^^^^^^^^^^^^^^^
ContentTypeObject
^^^^^^^^^^^^^^^^^

This is used to handle ContentType ::

    new ContentTypeObject(array $data, array $properties = array())


Available keys ::

    $data = [
        identifier               => string
        contenttype_groups       => string[]
        name                     => string
        names                    => string[]
        description              => string
        descriptions             => string[]
        name_schema              => string
        url_alias_schema         => string
        is_container             => bool
        default_always_available => bool
        default_sort_field       => int 1-12 Location::SORT_FIELD_*
        default_sort_order       => int 0-1  Location::SORT_ORDER_DESC/SORT_ORDER_ASC
        fields                   => FieldDefinitionObject[] {@link see FieldDefinitionObject}
    ],
    $properties = [
        id                       => int
        content_type_groups      => \eZ\Publish\API\Repository\Values\ContentType\ContentTypeGroup[]
        action                   => int {@link see \Transfer\EzPlatform\Data\Action\Enum\Action}
    ]


Requirements when **creating** ContentType:
    $data:
        - A unique `identifier`
        - Atleast one field in `fields`

ContentTypeObject create example ::

    new ContentTypeObject(
        array(
            'identifier' => 'banner',
            'fields' => array(
                'title' => array(
                    'type' => 'ezstring',
                    'position' => 10,
                ),
                'image' => array(
                    'type' => 'ezimage',
                    'position' => 20,
                ),
            ),
        ),
    );


Requirements when **updating** ContentType:
    $data:
        - An existing `identifier`
        - Atleast one field in `fields`

ContentTypeObject update example ::

    new ContentTypeObject(
        array(
            'identifier' => 'banner',
            'is_container' => false,
            'fields' => array(
                'title' => array(
                    'type' => 'ezstring',
                    'position' => 10,
                ),
                'image' => array(
                    'type' => 'ezimage',
                    'position' => 20,
                ),
                'alt' => array(
                    'type' => 'ezstring',
                    'position' => 30,
                ),
            ),
        ),
    );


ContentTypeObject complete update example ::

    new ContentTypeObject(array(
        'identifier' => $identifier,
        'main_language_code' => 'eng-GB',
        'contenttype_groups' => array('Content'),
        'name_schema' => '<title>',
        'url_alias_schema' => '<title>',
        'names' => array('eng-GB' => 'Article'),
        'descriptions' => array('eng-GB' => 'An article'),
        'is_container' => true,
        'default_always_available' => false,
        'default_sort_field' => Location::SORT_FIELD_PUBLISHED,
        'default_sort_order' => Location::SORT_ORDER_ASC,
        'fields' => array(
            'title' => array(
                'type' => 'ezstring',
                'names' => array('eng-GB' => 'Title'),
                'descriptions' => array('eng-GB' => 'Title of the article'),
                'field_group' => 'content',
                'position' => 10,
                'is_required' => true,
                'is_translatable' => true,
                'is_searchable' => true,
                'is_info_collector' => false,
            ),
            'description' => array(
                'type' => 'ezstring',
                'names' => array('eng-GB' => 'Description'),
                'descriptions' => array('eng-GB' => 'Description of the article'),
                'field_group' => 'content',
                'position' => 20,
                'is_required' => false,
                'is_translatable' => true,
                'is_searchable' => true,
                'is_info_collector' => false,

            ),
        ),
        array(
            'action' => \Transfer\EzPlatform\Data\Action\Enum\Action::CREATEORUPDATE,
        ),
    ));

