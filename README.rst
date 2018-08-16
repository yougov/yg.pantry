An array of configuration settings (ingredients) in YAML with composition
logic for use by applications.


Concept
=======

The pantry server essentially hosts simple YAML files over HTTP.

Applications that require information about the environment can
use the client to load that YAML config::

    >>> from yg.pantry import Client
    >>> config = Client.load('https://pantrysrv.example.com/app/myapp')

The URL can may omitted if PANTRY_ROOT is defined in the environment.

Configuration ingredients may exist on a file system or in a database, but
may reference other ingredients in the same pantry or other external
resources. Consider this definition for ``app/myapp``::

    name: My App
    environment: production
    number of threads: 20
    AWS secrets: !include /aws/secrets
    Partner Config: !include https://yamlfiles.partner.org/shared.yaml
    environment_settings: !include myapp/production

The Pantry Client will load the config, inject the includes using absolute
or relative URLs, and return a Python object of the resolved configuration.

For clients not on Python, the Pantry server also provides a RESOLVE method,
which will run the Pantry client on the requested resource and return
the resolved config.

    $ curl -X RESOLVE https://pantrysrv.example.com/app/myapp
