Saml Token endpoint
===================

This endpoint combines data from the authenticated user with data from a signed Jwt into a signed Saml Attribute statement. The endpoint is made specifically for the use of the Kjernejournal XDS project and is not meant for general use. 

To use the endpoint one must do a Http Post request with the Authorization header set to the string ``Bearer`` followed by the access token. Further, the form post must consist of a signed Jwt containing the following values:


+--------------------------+------------------------------------------------------------------------------+----------------------------------------------------------+
|Name                      |Description                                                                   |Notes                                                     |
+==========================+==============================================================================+==========================================================+
|subject:organization      |The name of the organization                                                  |                                                          |
+--------------------------+------------------------------------------------------------------------------+----------------------------------------------------------+
|subject:organization-id   |The orgnr of the organization                                                 |Specify only the orgnr, not the whole Instance Identifier |
+--------------------------+------------------------------------------------------------------------------+----------------------------------------------------------+
|subject:child-organization|The name of the child organization if available                               |                                                          |
+--------------------------+------------------------------------------------------------------------------+----------------------------------------------------------+
|subject:facility          |The name of the child organization if available                               |                                                          |
+--------------------------+------------------------------------------------------------------------------+----------------------------------------------------------+
|subject:role              |The role of the authenticated user                                            |                                                          |
+--------------------------+------------------------------------------------------------------------------+----------------------------------------------------------+
|homeCommunityId           |Unique id of the community of the user making the request                     |                                                          |
+--------------------------+------------------------------------------------------------------------------+----------------------------------------------------------+
|subject:purposeofUse      |Purpose of use for the request                                                |Specify either 1 for Clinical Care or 2 for Emergency Care|
+--------------------------+------------------------------------------------------------------------------+----------------------------------------------------------+
|resource:resource-id      |The national identity number of the patient that is the target of the request |Specify only the orgnr, not the whole Instance Identifier |
+--------------------------+------------------------------------------------------------------------------+----------------------------------------------------------+
|scope                     |Should always be set to the same value, "Journaldokumenter"                   |                                                          |
+--------------------------+------------------------------------------------------------------------------+----------------------------------------------------------+


Signing of Jwt tokens
---------------------

The Saml statement is built with information from several sources. Most of the information is supplied directly from HelseId with information about the authenticated user, 
but som of the elements must be passed by the caller as a signed Jwt (https://jwt.io/introduction/).

To ensure the authenticity of the information in the Jwt it must be signed by the caller. The Saml Token endpoint supports two mechanisms for validaiton of the signature. 

The first (and recommended) mechanism is by using Enterprise Certificates. Here the caller must have access to an enterprise certificate for the organization that owns the 
calling client. The Saml Token endpoint will validate the orgnr from the certificate against the orgnr stored as a client secret for the calling client. 

The second supported mechanism is by using RSA key pairs. In this case the public key of the RSA key pair must be stored as a secret for the calling client. The Jwt must 
then be signed using the private key of the RSA key pair. This mechanism is very useful for development and testing, but requires more maintenance in production since the 
key pairs has a limited life time. 


Service endpoints
-----------------

The only endpoint the service supports is the /Token-endpoint. This is available at the following addresses:

Utvikling
  ``https://helseid-xdssaml.utvikling.nhn.no/Token``
Test
  ``https://helseid-xdssaml.test.nhn.no/Token``
Prod
  ``https://helseid-xdssaml.nhn.no/Token``


In addition the services replies at /ping with the string "pong". This can be called to verify that the service is available. 