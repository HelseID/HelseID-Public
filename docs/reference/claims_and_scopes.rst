Claims and Scopes
=================
In addition to the standard scopes defined by the OpenID Connect, HelseID also supports a set of custom scopes and associated claims which can be used to retreive information about the authenticated user or client.


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







