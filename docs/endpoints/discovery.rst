Discovery Endpoint (Metadata)
=============================

The discovery endpoint can be used to retrieve metadata about HelseID - 
it returns information like the issuer name, key material, supported scopes etc.

The discovery endpoint is available via `/.well-known/openid-configuration` relative to the base address, e.g.::

    https://helseid-sts.test.nhn.no/.well-known/openid-configuration

