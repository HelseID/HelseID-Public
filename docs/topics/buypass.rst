Using Buypass as an identityprovider in HelseID
================


Bypass is a commercial e-ID and payment provider. You can read more about buypass and their services here https://www.buypass.no/.
Buypass is certified as an eID provider that offers an LoA of 4 (Nivå 4), which makes it suitable for the identification of health personel in Norway.

.. Note:: To use your Buypass for authentication through HelseID your users will need a smartcard issued by Buypass.


Should I use Buypass through HelseID or ID-Porten?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
HelseID lets the users with authenticate using their Buypass smart-card directly through HelseID, or via ID-Porten.
This choice might depend on policy or existing agreements, but could also be cost-related. 
ID-Porten requires that an agreement of use exists between Difi and your organization, and calculates a cost based on your organizations total number of authentications (not only through HelseID).


Buypass front-end: Java applet or "Javafri" client
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Buypass implements two different front-end technologies for user authentication: with Java applet or with html ("Javafri").
The support for Java applets in modern web browsers is decreasing rapidly. Our recommendation is to use the html version of the front-end called "Javafri".
We acknowledge that not all enviroments are able to update their web browsers, so our integration with Buypass is set up to in way that automatically selects the suitable front-end client for you. 
So, if the users machine is set up with support for "Javafri" authentication the Buypass front-end will be html, otherwise Buypass will try to load the Java applet.


Preselect Buypass as the authentication provider
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you always know that your users will authenticate using their smartcard issued by Bypass, it is possible to preselect Buypass by giving HelseID a "hint" in the authentication request.
This hint will redirect the user directly to Buypass authentication, skipping the HelseID portals overview of our different identity providers. 

You can preselect Buypass by using the ``acr_values`` request parameter in the protocol message sent to HelseID. 

**Example**
::
    GET /connect/authorize?
        client_id=client1&
        scope=openid&
        response_type=id_token&
        redirect_uri=https://myapp/callback&
        state=abc&
        nonce=xyz&
        acr_values=idp:buypass
(URL encoding removed, and line breaks added for readability)

