^^^^^^^^^^
UserObject
^^^^^^^^^^

This is used to handle User ::

    new UserObject(array $data, array $properties = array())



Available keys ::

    $data = [
        username => string
        email => string
        password => string
        main_language_code => string
        enabled => bool
        max_login => int
        fields => [
            first_name => string
            last_name => string
            ...
        ],
    ],
    $properties = [
        action => int {@link see \Transfer\EzPlatform\Data\Action\Enum\Action}
    ]


Requirements when **creating** User:
    $data:
        - all fields required by the `user`-ContentType
        - username
        - password
        - email

UserObject create example ::

    new UserObject(
        array(
            'fields' => array(
                'first_name' => 'Harald',
                'last_name'  => 'Tollefsen',
            ),
            'username' => 'htollefsen',
            'password' => 'strong_password_123',
            'email'    => 'an@example.com',
        ),
    );


Requirements when **updating** User:
    $data:
        - username

UserObject update example ::

    new UserObject(
        array(
            'username' => 'htollefsen',
            'password' => 'superSTRong321',
            'fields' => array(
                'first_name' => 'Haaarald',
            ),
        ),
    );


UserObject complete update example ::

    new UserObject(
        array(
            'username' => 'htollefsen',
            'password' => 'superSTRong321',
            'fields' => array(
                'first_name' => 'Haaarald',
                'last_name'  => 'Toooollefsen',
            ),
            'max_login' => 10,
            'enabled' => true,
            'main_language_code => 'nor-NO',
        ),
        array(
            'action' => \Transfer\EzPlatform\Data\Action\Enum\Action::CREATEORUPDATE,
        ),
    );

