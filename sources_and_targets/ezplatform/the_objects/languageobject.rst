^^^^^^^^^^^^^^
LanguageObject
^^^^^^^^^^^^^^

This is used to handle Language ::

    new LanguageObject(array $data, array $properties = array())


Available keys ::

    $data = [
        code    => string
        name    => string
        enabled => bool
    ],
    $properties = [
        id     => int
        action => int {@link see \Transfer\EzPlatform\Data\Action\Enum\Action}
    ]


Requirements when **creating** Language:
    $data:
        - code

LanguageObject create example ::

    new LanguageObject(
        array(
            'code' => 'nor-NO',
        ),
    );

Requirements when **updating** Language:
    $data:
        - code
        - name

LanguageObject update example ::

    new LanguageObject(
        array(
            'code' => 'nor-NO',
            'name' => 'Norwegian',
        ),
    );


LanguageObject complete update example ::

    new LanguageObject(
        array(
            'code' => 'nor-NO',
            'name' => 'Norwegian'
            'enabled' => true,
        ),
    );

