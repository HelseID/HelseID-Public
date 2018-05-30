Revocation Endpoint
===================

This endpoint allows revoking access tokens (reference tokens only) and refresh token. 
It implements the token revocation specification `(RFC 7009) <https://tools.ietf.org/html/rfc7009>`_.

``token``
    the token to revoke (required)
``token_type_hint``
    either ``access_token`` or ``refresh_token`` (optional)

Example
^^^^^^^

::

    POST /connect/revocation HTTP/1.1
    Host: server.example.com
    Content-Type: application/x-www-form-urlencoded
    Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW

    token=45ghiukldjahdnhzdauz&token_type_hint=refresh_token
