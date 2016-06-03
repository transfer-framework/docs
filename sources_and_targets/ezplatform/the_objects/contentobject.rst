^^^^^^^^^^^^^
ContentObject
^^^^^^^^^^^^^
This is used to handle Content ::

    new ContentObject(array $data, array $properties = array())


Available keys ::

    $data = [
        field_identifier_1 => 'field_value_1',
        field_identifier_2 => 'field_value_2',
        ...
    ],
    $properties = [
        // Recommended to use this instead of using id as identifier,
        // as we are able to choose our own value.
        remote_id               => string

        // Used to link Content with Location, don't set this yourself.
        // Use 'remote_id' to locate Content instead.
        id                      => int

        // See ContentTreeObject.
        main_object             => bool

        // Locations of the Content. Should be an array which might contain
        // int, LocationObject or Location. Or a mix of these.
        parent_locations        => [int|LocationObject|Location]

        // An existing ContentType identifier
        content_type_identifier => string

        // A language code. Defaults to 'eng-GB'.
        language                => string

        // On create, the first defined parent_location will be used as the
        // main_location_id, unless main_location_id is defined
        main_location_id        => int

        // {@link see \Transfer\EzPlatform\Data\Action\Enum\Action}
        action                  => int
    ]


Requirements when **creating** Content:
    $data:
        - atleast one field
        - all fields marked as is_required on the ContentType, must have a value

    $properties
        - must contain the key ``content_type_identifier`` with a string value of the ContentType you would like to use.

ContentObject create example ::

    new ContentObject(
        array(
            'title' => 'This is my title',
        ),
        array(
            'content_type_identifier' => 'article',
            'remote_id' => 'article_example',
        ),
    );


Requirements when **updating** Content:
    $data:
        - atleast one field

    $properties
        - ``remote_id`` or ``id``, to be able to locate the Content to update

ContentObject update example ::

    new ContentObject(
        array(
            'content' => 'Lorem ipsum dolor sit amet, consectetur adipiscing elit...',
            'author' => 'Harald Tollefsen',
        ),
        array(
            'remote_id' => 'article_example',
        ),
    );


ContentObject complete update example ::

    new ContentObject(
        array(
            'title' => 'This is my title',
            'content' => 'Lorem ipsum dolor sit amet, consectetur adipiscing elit...',
            'author' => 'Harald Tollefsen',
        ),
        array(
            'remote_id'                 => 'article_example_1',
            'content_type_identifier'   => 'article',
            'language'                  => 'eng-GB',
            'parent_locations'          => array(2, new LocationObject(...), 43, ),
            'main_location_id'          => 2,
            'action'                    => \Transfer\EzPlatform\Repository\Values\Action\Enum\Action::CREATEORUPDATE
            'content_info'              => \eZ\Publish\API\Repository\Values\Content\ContentInfo // Should not be set manually, will be defined after create or update
            'version_info'              => \eZ\Publish\API\Repository\Values\Content\VersionInfo // Should not be set manuelly, will be defined after create or update
        ),
    );

