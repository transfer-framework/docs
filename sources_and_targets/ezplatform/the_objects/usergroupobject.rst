^^^^^^^^^^^^^^^
UserGroupObject
^^^^^^^^^^^^^^^
This is used to handle UserGroup ::

    new UserGroupObject(array $data, array $properties = array())


Available keys ::

    $data = [
        remote_id               => string
        parent_id               => int      // Defaults to 12
        main_language_code      => string   // Defaults to eng-GB
        content_type_identifier => string   // Defaults to user_group
        fields                  => array [field definition identifier => value]
    ],
    $properties = [
        id                      => int (same as contentInfo->id)
        content_info            => \eZ\Publish\API\Repository\Values\Content\ContentInfo
        version_info            => \eZ\Publish\API\Repository\Values\Content\VersionInfo
        action                  => int {@link see \Transfer\EzPlatform\Data\Action\Enum\Action}
    ]


Requirements when **creating** UserGroup:
    $data:
        - fields which are required by the ContentType (often `user_group`)

UserGroupObject create example ::

    new UserGroupObject(
        array(
            'remote_id' => 'usergroup_example_1',
            'fields' => array(
                'name' => 'My Usergroup',
            ),
            'parent_id' => 5,
        ),
    );


Requirements when **updating** UserGroup:
    $data:
        - remote_id
        - atleast one field

UserGroupObject update example ::

    new UserGroupObject(
        array(
            'remote_id' => 'usergroup_example_1',
            'fields' => array(
                'name' => 'Best Company customers',
                'description' => 'These are the customers from Best Company',
            ),
        ),
    );


UserGroupObject complete update example ::

    new UserGroupObject(
        array(
            'remote_id' => 'usergroup_example_1',
            'parent_id' => 12,
            'main_langauge_code' => 'nor-NO',
            'content_type_identifier' => 'user_group',
            'fields' => array(
                'name' => 'Best Company customers',
                'description' => 'These are the customers from Best Company',
            ),
        ),
        array(
            'action' => \Transfer\EzPlatform\Data\Action\Enum\Action::CREATEORUPDATE,
        ),
    );

