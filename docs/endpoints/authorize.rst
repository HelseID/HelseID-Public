Authorize Endpoint
==================

The authorize endpoint can be used to request tokens or authorization codes via the browser.
This process typically involves authentication of the end-user.

``client_id``
    identifier of the client (required).
``scope``
    one or more registered scopes (required)
``redirect_uri`` 
    must exactly match one of the allowed redirect URIs for that client (required)
``response_type`` 
    ``id_token`` requests an identity token. Implicit flow.

    ``token`` requests an access token. Implicit flow.

    ``id_token token`` requests an identity token and an access token. Implicit flow.

    ``code`` requests an authorization code. Code flow.

    ``code id_token`` requests an authorization code and identity token. Hybrid flow.

    ``code id_token token`` requests an authorization code, identity token and access token.
    
``response_mode``
    ``form_post`` sends the token response as a form post instead of a fragment encoded redirect (optional)
``state`` 
    HelseID will echo back the state value on the token response, 
    this is for round tripping state between client and provider, correlating request and response and CSRF/replay protection. (recommended)
``nonce`` 
    HelseID will echo back the nonce value in the identity token, this is for replay protection)

    *Required for identity tokens via implicit grant.*
``prompt``
    ``none`` no UI will be shown during the request. If this is not possible (e.g. because the user has to sign in or consent) an error is returned
    
    ``login`` the login UI will be shown, even if the user is already signed-in and has a valid session
``code_challenge``
    sends the code challenge for PKCE
``code_challenge_method``
    ``plain`` indicates that the challenge is using plain text (not recommended)
    ``S256`` indicates the the challenge is hashed with SHA256
``login_hint``
    can be used to pre-fill the username field on the login page
``ui_locales``
    gives a hint about the desired display language of the login UI

    ``nb-no``
        norwegian bokmål (default)
    ``en-us``
        english        
``max_age``
    if the user's logon session exceeds the max age (in seconds), the home realm will be shown
``acr_values``
    allows passing in additional authentication related information - identityserver special cases the following proprietary acr_values:
        
    ``idp:name_of_idp`` 
    bypasses the home realm screen and forwards the user directly to the selected identity provider (if allowed per client configuration)        
        ``idporten-oidc`` 
            redirect directly to ID-porten

    ``amr:name_of_idp_at_idporten`` 
    bypasses the home realm screen at ID-porten and forwards the user to the selected identity provider. You do not need to use ``idp:name_of_idp`` if this is set - the user will be forwarded to ID-porten.
        ``amr:minid``
        forward user to MinID

        ``amr:bankid``
        forward user to BankID

        ``amr:bankidmobil``
        forward user to BankID på mobil

        ``amr:buypass``
        forward user to Buypass

        ``amr:commfides``
        forward user to Commfides