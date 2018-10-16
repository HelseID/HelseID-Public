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



On Behalf Of
^^^^^^^^^^^^
Use of ID-porten requires that the organization using their services have accepted the terms of use ('bruksvilkår'). 
As part of this, an organization should identity itself during an user authentication. The term for this mechanism is "On Behalf Of" (OBO from now on).

A client in HelseID which uses ID-porten may be configured with one or more OBOs. A single OBO configuration in HelseID consists the following information:

- On Behalf Of ID: Internal ID used between HelseID and ID-porten
- The organization number used when accepting the terms of use of ID-porten (required)
- Name: The name displayed for the organization or client application in ID-porten (required)
- Description: Detailed information
- Client URL: The link which should be used for the "Back" button in ID-porten (required)

At authentication time a ``on_behalf_of`` parameter with value set to the organization number configured in the OBO entry should be passed to the authorization endpoint of HelseID.

**Example - redirect directly to ID-porten and authenticate the user on behalf of the organization Udelt**
::
    GET /connect/authorize?
        client_id=client1&
        scope=openid&
        response_type=id_token&
        redirect_uri=https://myapp/callback&
        state=abc&
        nonce=xyz&
        acr_values=idp:idporten-oidc&
        on_behalf_of=912159523
(URL encoding removed, and line breaks added for readability)
