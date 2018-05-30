Access on behalf of user
========================
HelseID implements the token exchange pattern loosely based on `OAuth 2.0 Token Exchange <https://tools.ietf.org/html/draft-ietf-oauth-token-exchange-10>`_.
It is implemented to trust tokens from multiple issuers based on configuration on order to support not only selv issued tokens, but also tokens from other trusted issuers.
Exchanges a valid access token from a trusted issuer with a new access token. HelseID specific claims and sub are copied over as claims on the new token. The exchange chain is also represented
in the act claim structure. The requested scopes will also be added to the access token.

Semantics
^^^^^^^^^
In the context of token exchange there are some semantics that are important to specify regarding impersonation and delegation. Imagine the following scenario: a front end client calls a middle tier API using a token acquired via an interactive flow (e.g. hybrid flow). This middle tier API (API 1) now wants to call a back end API (API 2) on behalf of the interactive user.
In other words, the middle tier API (API 1) needs an access token containing the user’s identity, but with the scope of the back end API (API 2). You might have heard of the term poor man’s delegation where the access token from the front end is simply forwarded to the back end.
This has some shortcomings, e.g. API 2 must now accept the API 1 scope which would allow the user to call API 2 directly. Also - you might want to add some delegation specific claims into the token, e.g. the fact that the call path is via API 1.

Impersonation
"""""""""""""
When principal A impersonates principal B, A is given all the rights that B has within some defined rights context and is indistinguishable from B in that context. Thus, when principal A impersonates principal B, then in so far as any entity receiving such a token is concerned, they are actually dealing with B.  It is true that some members of the identity system might have awareness that impersonation is going on, but it is not a requirement.  For all intents and purposes, when A is impersonating B, A is B.

Delegation
""""""""""
Delegation semantics are different than impersonation semantics, though the two are closely related.  With delegation semantics, principal A still has its own identity separate from B and it is explicitly understood that while B may have delegated some of its rights to A, any actions taken are being taken by A representing B. In a sense, A is an agent for B.

Token exchange
^^^^^^^^^^^^^^

Exchanging token issued by HelseId ::

    Original:
    {
        "aud":"https://consumer.example.com",
        "iss":"https://helseid-sts.nhn.no",
        "exp":1443904177,
        "nbf":1443904077,
        "clientId": "testClient",
        "scope" : "https://consumer.example.com.read",
        "sub": "24019491117" 
        "amr" : "bankId"
    }

    Exchanged:
    {
        "aud":"https://consumer2.example.com",
        "iss":"https://helseid-sts.nhn.no",
        "exp":1443904177,
        "nbf":1443904077,
        "clientId": "https://consumer.example.com",
        "scope" : "https://consumer2.example.com.read",
        "sub": "24019491117",
        "act":
        {
            "sub": "24019491117",
            "clientId": "testClient"
        }
        "amr" : "delegation"
    }

Exchanging token issued by "trusted issuer" ::

    Original:
    {
        "aud":"https://consumer.example.com",
        "iss":"https://trustedIssuer-sts.no",
        "exp":1443904177,
        "nbf":1443904077,
        "clientId": "testClient",
        "scope" : "https://consumer.example.com.read",
        "sub": "24019491117" 
        "amr" : "bankId"
    }

    Exchanged:
    {
        "aud":"https://consumer2.example.com",
        "iss":"https://helseid-sts.nhn.no",
        "exp":1443904177,
        "nbf":1443904077,
        "clientId": "https://consumer.example.com",
        "scope" : "https://consumer2.example.com.read",
        "sub": "24019491117",
        "act":
        {
            "sub": "24019491117",
            "clientId": "testClient"
        }
        "amr" : "delegation"
    }

Implementation
""""""""""""""

The following code is an example of how to exchange a token ::

    var client = new TokenClient(_options.TokenEndpoint, _options.ClientId, _options.ClientSecret);
    var payload = new { token : _accessToken };

    var response = await client.RequestCustomGrantAsync("token_exchange", _options.Scope, payload);

The details worth noting are:

- That the grand type is "token_exchange". 
- The payload is an object with a token property which value is an valid access token.

- The scopes requested are the scopes that one wish for the new access token. 


In order to use the token exchange mechanism one needs to:

- Be configured in HelseID to use the grant type `token_exchange`.
- Obtain a valid access token from a third party.

- Be configured to use the scopes requested in the exchange request.
