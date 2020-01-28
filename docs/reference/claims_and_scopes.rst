Claims and Scopes
=================
In addition to the standard scopes defined by the OpenID Connect, HelseID also supports a set of custom scopes and associated claims which can be used to retreive information about the authenticated user or client.


Scopes
^^^^^^


============================================= ======== =====================================  
Name                                          Type      Resulting Claims             
============================================= ======== =====================================  
``openid``                                    Standard Request use of the openid protocol, with support for id_tokens containing the ``sub`` claim.
``profile``                                   Standard ``given_name``, ``middle_name``, ``family_name``, ``name``
``helseid://scopes/client/dcr``               Custom   ``helseid://claims/client/dcr``                 
``helseid://scopes/hpr/hpr_number``           Custom   ``helseid://claims/hpr/hpr_number``               
``helseid://scopes/identity/assurance_level`` Custom   ``helseid://claims/identity/assurance_level``
``helseid://scopes/identity/pid``             Custom   ``helseid://claims/identity/pid``
``helseid://scopes/identity/pid_pseudonym``   Custom   ``helseid://claims/identity/pid_pseudonym``
``helseid://scopes/identity/security_level``  Custom   ``helseid://claims/identity/security_level``
``helseid://scopes/identity/network``         Custom   ``helseid://claims/identity/network``

============================================= ======== =====================================

Claims
^^^^^^

===============================================  ===============     =====================================
Name                                             Example             Description
===============================================  ===============     =====================================  
``helseid://claims/hpr/hpr_number``              181000001           Health personel number according to `NHN's coding standard <https://register-web.test.nhn.no/docs/api/html/01a38db9-e5d0-4568-81ee-15448341b564.htm>`_ 
``helseid://claims/identity/assurance_level``    substantial         Defined by eIDAS. Possible values are low, substantial or high.
``helseid://claims/identity/security_level``     3                   Defined by `Rammeverk for autentisering og uavviselighet i elektronisk kommunikasjon med og i offentlig sektor <https://www.regjeringen.no/no/dokumenter/rammeverk-for-autentisering-og-uavviseli>`_ . Possible values are 2, 3 or 4. 
``helseid://claims/identity/pid``                04048900181         Personal identifier
``helseid://claims/identity/pid_pseudonym``                          Personal identifier pseudonymized with HMAC
``helseid://claims/identity/network``            helsenett           Indicates whether an enduser authenticated using a HelseID server on Internet or 'Helsenett'. Note that local infrastructure may route users from Helsenett to HelseID nodes exposed on the Internet. Possible values are 'internett' and 'helsenett'. 
``helseid://claims/client/ec/orgnr_parent``      912159523           The organizational number in the SUBJECT field of a certificate (Only present when a client is authenticated with an enterprise certificate).
``helseid://claims/client/ec/orgnr_child``       922734046           The organizational number in the OU field of the certificate (Only present when a client is authenticated with an enterprise certificate).
``helseid://claims/client/ec/exp``               1639353540          The expiration date of the certificate (Only present when a client is authenticated with an enterprise certificate).
``helseid://claims/client/ec/common_name``                           The common name derived from the SUBJECT field of a certificate (Only present when a client is authenticated with an enterprise certificate).
``helseid://claims/client/claims/orgnr_parent``  912159523           The parent organization for an end user / client. See below.    
``helseid://claims/client/claims/orgnr_child``   922734046           The child organization (underenhet/behandlingssted) for an end user / client. See below.   
``client_amr``                                   private_key_jwt     The type of secret used for authenticating the client. Possible values are 'client_secret', 'private_key_jwt' and 'virksomhetssertifikat'. If this claims is missing, it indicates that no secret was used.   
===============================================  ===============     =====================================



Organization Number claims
^^^^^^^^^^^^^^^^^^^^^^^^^^
HelseID provides two main groups of claims relating to organization identificators from Enhetsregisteret.

The first group is related to use of norwegian enterprise certificates (virksomhetssertifat) for authenticating the client. This includes all claims prefixed with the namespace ``helseid://claims/client/ec/``.

The second group are claims indicating the organization numbers independent of the mechanism used to derive them. This includes ``helseid://claims/client/claims/orgnr_parent`` and ``helseid://claims/client/claims/orgnr_child``.

Organization Number may be derived in the following ways:

 - Using the trust model currently being established in HelseID (partially dependent on self-service)
 - Using enterprise certificates
 - Using static claims on client configurations verified by NHN operations.












Standard claims
^^^^^^^^^^^^^^^

A token will also contain a set of standard claims originating from the `JSON Web Token <https://tools.ietf.org/html/rfc7519>`_ and `OpenID Connect <http://openid.net/specs/openid-connect-core-1_0.html#Claims>`_.