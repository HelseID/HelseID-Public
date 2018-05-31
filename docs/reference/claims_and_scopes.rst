Claims and Scopes
=================
In addition to the standard scopes defined by the OpenID Connect, HelseID also supports a set of custom scopes and associated claims which can be used to retreive information about the authenticated user or client.

We've created a small project containing all our custom claims and scopes (in addition to some other handy magical strings) which can be found on `nuget <https://www.nuget.org/packages/HelseId.Constants>`_ or `github <https://www.nuget.org/packages/HelseId.Constants>`_.


Scopes
^^^^^^


============================================= ======== =====================================  
Name                                          Type      Resulting Scopes             
============================================= ======== =====================================  
``openid``                                    Standard Request use of the openid protocol, with support for id_tokens containing the ``sub`` claim.
``profile``                                   Standard ``given_name``, ``middle_name``, ``family_name``, ``name``
``helseid://scopes/client/dcr``               Custom   ``helseid://claims/client/dcr``                 
``helseid://scopes/hpr/hpr_number``           Custom   ``helseid://claims/hpr/hpr_number``               
``helseid://scopes/identity/assurance_level`` Custom   ``helseid://claims/identity/assurance_level``
``helseid://scopes/identity/pid``             Custom   ``helseid://claims/identity/pid``
``helseid://scopes/identity/pid_pseudonym``   Custom   ``helseid://claims/identity/pid_pseudonym``
``helseid://scopes/identity/security_level``  Custom   ``helseid://claims/identity/security_level``
============================================= ======== =====================================

Claims
^^^^^^

============================================= ============ ===================================== 
Name                                          Example      Description
============================================= ============ =====================================  
``helseid://claims/hpr/hpr_number``           181000001    Health personel number according to `NHN's coding standard <https://register-web.test.nhn.no/docs/api/html/01a38db9-e5d0-4568-81ee-15448341b564.htm>`_ 
``helseid://claims/identity/assurance_level`` substantial  Defined by eIDAS. Possible values are low, substantial or high.
``helseid://claims/identity/pid``             04048900181  Personal identifier
``helseid://claims/identity/pid_pseudonym``                Personal identifier pseudonymized with HMAC
``helseid://claims/identity/security_level``  3            Defined by `Rammeverk for autentisering og uavviselighet i elektronisk kommunikasjon med og i offentlig sektor <https://www.regjeringen.no/no/dokumenter/rammeverk-for-autentisering-og-uavviseli>`_ . Possible values are 2, 3 or 4. 
``helseid://claims/client/ec/orgnr_parent``                The organizational number in the SUBJECT field of a certificate (Occures only when the client is authenticated with an enterprise certificate).
``helseid://claims/client/ec/orgnr_child``                 The organizational number in the OU field of the certificate (Occures only when the client is authenticated with an enterprise certificate).
``helseid://claims/client/ec/exp``                         The expiration date of the certificate (Occures only when the client is authenticated with an enterprise certificate).
============================================= ============ =====================================


Standard claims
^^^^^^^^^^^^^^^

The token will also contain a set of standard claims originating from the `JSON Web Token <https://tools.ietf.org/html/rfc7519>`_ and `OpenID Connect <http://openid.net/specs/openid-connect-core-1_0.html#Claims>`_.