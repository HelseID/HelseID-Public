Introduction
============
HelseID is a national autentication service for the health sector in Norway.  This documentation is targeted at technical personnel such as application architects and developers.

You can find more general info about HelseID `here <https://www.nhn.no/helseid/>`_.

HelseID enables the following features in your applications:

**Authentication as a Service**
    HelseID is a `federation gateway <http://docs.identityserver.io/en/release/topics/federation_gateway.html>`_ which acts as a gateway to identity providers suitable for the health sector. 

**Single Sign-on / Sign-out**
    Single sign-on (and out) over multiple application types.

**Access Control for APIs**
    Issue access tokens for APIs for various types of clients, e.g. server to server, web applications, SPAs and native/mobile apps.

 
Where to start
^^^^^^^^^^^^^^
The foundation of HelseID is the protocols `OpenID Connect <http://openid.net/connect/>`_ and `OAuth 2 <https://oauth.net/2/>`_. It is recommended to gain some basic understanding of these if you consider using HelseID.

Additionally, we use the excellent open source framework `IdentityServer4 <http://docs.identityserver.io/en/release/index.html>`_. All client sample code for IdentityServer should also work for HelseID.

We also maintain sample code on our `public GitHub <https://github.com/HelseID/HelseID.Samples>`_ page.
