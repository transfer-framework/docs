^^^^^^^^^^^^^^
LocationObject
^^^^^^^^^^^^^^
This is used to handle Location ::

    new LocationObject(array $data, array $properties = array())


Available keys ::

    $data = [
        content_id          => int      // Foreign key to a Content
        remote_id           => string
        parent_location_id  => int
        hidden              => bool
        priority            => int
        sort_field          => int 1-12 Location::SORT_FIELD_*
        sort_order          => int 0-1  Location::SORT_ORDER_DESC
                                                  SORT_ORDER_ASC
    ],
    $properties = [
        id                  => int
        depth               => int
        content_info        => \eZ\Publish\API\Repository\Values\Content\ContentInfo
        invisible           => bool
        path                => array
        path_string         => string
        action              => int {@link see \Transfer\EzPlatform\Data\Action\Enum\Action}
    ]


Requirements when **creating** Location:
    $data:
        - content_id
        - parent_location_id

LocationObject create example ::

    new LocationObject(
        array(
            'content_id' => 43,
            'parent_location_id' => 2,
        ),
    );


Requirements when **updating** Location:
    $data:
        - 'content_id' or 'remote_id'
        - 'parent_location_id'

LocationObject update example ::

    new LocationObject(
        array(
            'content_id' => 43,
            'parent_location_id' => 2,
        ),
    );


LocationObject complete update example ::

    new LocationObject(
        array(
            'content_id'         => 43,
            'remote_id'          => 'location_example_1',
            'parent_location_id' => 2,
            'hidden'             => false,
            'priority'           => 10,
            'sort_field'         => Location::SORT_FIELD_PRIORITY,
            'sort_order'         => Location::SORT_ORDER_DESC,
        ),
        array(
            action              => \Transfer\EzPlatform\Data\Action\Enum\Action::CREATEORUPDATE
        ),
    );

