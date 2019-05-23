Using Commfides as an identityprovider in HelseID
================


Commfides is a commercial e-ID and payment provider. You can read more about Commfides and their services here https://www.commfides.com/.
Commfides is certified as an eID provider that offers an LoA of 4 (Nivå 4), which makes it suitable for the identification of health personel in Norway.

.. Note:: To use your Commfides for authentication through HelseID your users will need a smartcard issued by Commfides.


Should I use Commfides through HelseID or ID-Porten?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
HelseID lets the users with authenticate using their Commfides smart-card directly through HelseID, or via ID-Porten.
This choice might depend on policy or existing agreements, but could also be cost-related. 
ID-Porten requires that an agreement of use exists between Difi and your organization, and calculates a cost based on your organizations total number of authentications (not only through HelseID).


Commfides front-end: Java applet or html based client
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Commfides implements two different front-end technologies for user authentication: with Java applet or html based version.
The support for Java applets in modern web browsers is decreasing rapidly. Our recommendation is to use the html version of the front-end. Please see: https://www.commfides.com/applet/ for more information about installing the neccessary components.
We acknowledge that not all enviroments are able to update their web browsers, so we have integrated with Commfides to support both older Java Applet front end and the newer html based client. 


Preselect Commfides as the authentication provider
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you always know that your users will authenticate using their smartcard issued by Commfides, it is possible to preselect Buypass by giving HelseID a "hint" in the authentication request.
This hint will redirect the user directly to Commfides authentication, skipping the HelseID portals overview of our different identity providers. 

You can preselect Commfides by using the ``acr_values`` request parameter in the protocol message sent to HelseID. 

**Example**
::
    GET /connect/authorize?
        client_id=client1&
        scope=openid&
        response_type=id_token&
        redirect_uri=https://myapp/callback&
        state=abc&
        nonce=xyz&
        acr_values=idp:commfides
(URL encoding removed, and line breaks added for readability)
