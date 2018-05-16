Use of ID-porten
================

ID-porten is a national authentication service supporting national e-services.
You can read more about ID-porten `here <http://eid.difi.no/nb/id-porten/>`_.


.. Note:: To use ID-porten in your application, it is required that you have accepted their terms of use.


Redirecting the user directly to ID-porten
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
You can bypass the available identityproviders in HelseID, and redirect the user directly to ID-porten. Do this by using the ``acr_values`` mechanism in HelseID for preselecting IDP. 

**Example**
::
    GET /connect/authorize?
        client_id=client1&
        scope=openid&
        response_type=id_token&
        redirect_uri=https://myapp/callback&
        state=abc&
        nonce=xyz&
        acr_values=idp:idporten-oidc
(URL encoding removed, and line breaks added for readability)


Preselecting one of the available IDPs in ID-porten
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
To preselect the IDP in ID-porten, you can add one of the following values to ``acr_values`` in the request to the authorize endpoint.

``amr:bankid``
    Preselect BankID
``amr:bankidmobil``
    Preselect BankID på mobil
``amr:buypass``
    Preselect Buypass
``amr:commfides``
    Preselect Commfides
``amr:minid``
    Preselect MinID

**Example - redirect directly to ID-porten and use MinID**
::
    GET /connect/authorize?
        client_id=client1&
        scope=openid&
        response_type=id_token&
        redirect_uri=https://myapp/callback&
        state=abc&
        nonce=xyz&
        acr_values=idp:idporten-oidc amr:minid
(URL encoding removed, and line breaks added for readability)


    





