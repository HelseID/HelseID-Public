Token Endpoint
==============

The token endpoint can be used to programmatically request tokens.
It supports the ``password``, ``authorization_code``, ``client_credentials``, ``refresh_token`` and ``token_exchange``  grant types).


``client_id``
    client identifier (required)
``client_secret``
    client secret either in the post body, or as a basic authentication header. Optional.
``grant_type``
    ``authorization_code``, ``client_credentials``, ``password``, ``refresh_token`` or ``token_exchange``
``scope``
    one or more registered scopes. If not specified, a token for all explicitly allowed scopes will be issued.
``redirect_uri`` 
    required for the ``authorization_code`` grant type
``code``
    the authorization code (required for ``authorization_code`` grant type)
``code_verifier``
    PKCE proof key
``username`` 
    resource owner username (required for ``password`` grant type)
``password``
    resource owner password (required for ``password`` grant type)
``refresh_token``
    the refresh token (required for ``refresh_token`` grant type)
