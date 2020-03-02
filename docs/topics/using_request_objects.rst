Background
==========
To use Kjernejournal (the norwegian Core Journal -\“KJ\”from now on), information about the place of treatment (in addition to\“juridisk person\”- in most cases the organization number of the municipality) is required to be included in access tokens issued by HelseID.
This is information which only the system accessing KJ knows at the time of treatment. Therefore, mechanisms have been implemented to transmit this\information to HelseID when the user authenticates.

Terms
=====
Kjernejournal (“KJ”)

Pleie- og omsorgssektoren (“PLL”)

Subunit

Unit - Municiplaity / Place of treatment

Underlying technical mechanisms
===============================
In order to be able to pass information to HelseID in a secure way, support has been implemented for a mechanism in OpenID Connect called `Request Objects <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://openid.net/specs/openid-connect-core-1_0.html%2523JWTRequests%23JWTRequests&sa=D&ust=1582561394163000>`_. 

This allows you to use a signed JWT to send parameters to the `authorization endpoint <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://helseid.readthedocs.io/no/latest/endpoints/authorize.html&sa=D&ust=1582561394163000>`_ \in HelseID. Note that HelseID does not support the "Passing a Request Object by Reference" mechanism.

The actual information is included in the signed JWT is wrapped in a structure based on the `Rich Authorization Requests specification <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://tools.ietf.org/html/draft-lodderstedt-oauth-rar-03&sa=D&ust=1582561394163000>`_ and the FHIR format. This results in a somewhat complex structure, but this is the basis for sending other information from a client to HelseID (role, "tjenestlig behov", technical information about the client application etc.) in a secure, standardized way.

Prerequisites
=====================
The following must be in place for HelseID to approve a submitted place of treatment.

POST must be used
*****************

Signed JWTs can be large, and there are `size restrictions in browsers and web servers <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://stackoverflow.com/a/812962&sa=D&ust=1582561394164000>`_ on GET requests. POST must be used when passing requests to the authorization endpoint in HelseID. This causes some complexity in native applications (\“tykke klienter\”) that use an integrated browser for authentication to HelseID. See our `sample code <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://github.com/HelseID/HelseID.Samples/tree/master/HelseId.Samples.RequestObjectsDemo&sa=D&ust=1582561394165000>`_ \for how this can be handled.

Valid subunits must be pre-registered in HelseID
************************************************

When HelseID receives a sub-unit from the subject system, a validation is made if it is valid. The subunits that are valid for a professional system must therefore be pre-registered in HelseID. This is currently done in the HelseID portal.

Public key for validating the signed Request Object JWT must be pre-registered in HelseID
******************************************************************************************

When HelseID receives a sub-unit from the professional system, the signature on the JWT in which it is located is validated. The public key to be used for this must be pre-registered in HelseID. This is currently done in the HelseID portal.

Note that this is not necessarily the same key used against the `token endpoint <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://helseid.readthedocs.io/no/latest/endpoints/token.html&sa=D&ust=1582561394166000>`_ \in HelseID.

Description
===================
Request Object
*********************
A Request Object is sent as follows to the authorization endpoint:

::

    request: signed_jwt_base64_code

\

Rules for signed_jwt_base_64



* JWT MUST be signed with minimum RS256.
* JWT MUST include the claim ``nbf`` (which indicates when JWT is valid from).
* JWT MUST include claim ``exp`` (which indicates when JWT is no longer valid). Max life of JWT is 60 seconds.
* JWT MUST include the claim ``iss`` with value set to the current client_id.
* JWT MUST include the claim ``aud`` which is set to url to the HelseID for the relevant environment. This value can be found in the "issuer" claim in our metadata, for our test environment the value is "https://helseid-sts.test.nhn.no\, see `https://helseid-sts.test.nhn.no/.well-known / openid-configuration <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://helseid-sts.test.nhn.no/.well-known/openid-configuration&sa=D&ust=1582561394168000>`_ .
* The JWT MAY contain other parameters to the authorization endpoint in accordance with `the specification <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://openid.net/specs/openid-connect-core-1_0.html%2523JWTRequests%23JWTRequests&sa=D&ust=1582561394168000>`_ .


**Example of signed Request Object JWT**

  .. code-block:: JSON

    {
        JWK
    }.
    {
        "nbf":1575463285,
        "exp":1575463345,
        "iss": "client_id",
        "aud":"https://helseid-sts.nhn.no",
        "authorization_details": {LOOK BELOW FOR FORMAT} 
    }.
    {
        SIGNATURE
    }

**Authorization Details**

The following structure must be used to provide the organization number for the place of treatment. This structure expresses the following:

[org number] is an identifier from the unit registry that represents the place of treatment that a health professional currently has a role in. In the future, more context will be given in this structure, eg the role of the health care professional or more detailed location.


  .. code-block:: JSON

    "authorization_details": 
    {
        "type":"helseid_authorization",
        "practitioner_role": 
        {
            "organization": 
            {
                "identifier": 
                {
                    "system":"urn:oid:2.16.578.1.12.4.1.2.101",
                    "type":"ENH",
                    "value":"[org number]",
                }
            }
        }
    }


.. csv-table:: Explanation of Authorization Details Items
   :header: "Name", "Defined By", "Description"
   :widths: 20, 20, 100

   "authorization_details", "The Rich Authorization Requests specification", "Rich Authorization Requests Parameter Name"
   "type", "Type of authorization request. Only valid value is ``helseid_authorization``", "FHIR links organization and health professionals through the role of health care professionals."
   "practitioner_role", "FHIR", "FHIR links organization and health professionals through the role of health care professionals."
   "organization", "FHIR", "The organization health personnel has a role in."
   "identifier", "FHIR", "Unique identification for an organization."
   "system", "FHIR", "Organization.system contains a uri (in this case an OID) that uniquely identifies the registry in which the organization is registered. Must have the value ``urn: oid: 2.16.578.1.12.4.1.2.10`` indicating the Unit Registery in Norway."
   "type", "FHIR", "Description of the register in which the organization is registered. Must have value ``ENH`` indicating the Unit Register in Norway."
   "value", "FHIR", "The identifier of the treatment site / sub-unit, ie a Norwegian organization number. Nine digits."


Example of full request
^^^^^^^^^^^^^^^^^^^^^^^

Below is an example of an http request to the authorization endpoint which includes a claim for a place of treatment.

::

    POST connect/authorize

    client_id: request_object_demo_client_id
    redirect_uri: https:// min_url / return
    response_type: code
    scope: [scopes]
    response_mode: form_post
    nonce: [nonce]
    request: eyJhbGci .... (abbreviated, see below)
    state: [state]


The decoded version of the base64-encoded ``request`` parameter looks like this

  .. code-block:: JSON
    {
        JWK
    }.
    {
        "nbf":1575640351,
        "exp":1575640411,
        "iss":"request_object_demo_client_id",
        "aud":"https://helseid-sts.nhn.no",
        "authorization_details": 
        {
            "type":"helseid_authorization",
            "practitioner_role": 
            {
                "organization": 
                {
                    "identifier": 
                    {
                        "system":"urn:oid:2.16.578.1.12.4.1.2.101",
                        "type":"ENH",
                        "value":"123123123"
                    }
                }
            }
        }    
    }.
    {
        SIGNATURE
    }.

Sample Code
===================
An example of Request Objects implementation can be found at `https://github.com/HelseID/HelseID.Samples/tree/master/HelseId.Samples.RequestObjectsDemo <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://github.com/HelseID/HelseID.Samples/tree/master/HelseId.Samples.RequestObjectsDemo&sa=D&ust=1582561394190000>`_ \.

References
==================
This is a list of specifications and codes we have based on.


.. csv-table:: References
   :header: "Name", "Link" 
   :widths: 20, 100

   "Request Objects", "`https://openid.net/specs/openid-connect-core-1_0.html#JWTRequests <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://openid.net/specs/openid-connect-core-1_0.html%2523JWTRequests%23JWTRequests&sa=D&ust=1582561394193000>`_"
   "Rich Authorization Objects", "`https://tools.ietf.org/html/draft-lodderstedt-oauth-rar-03 <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://tools.ietf.org/html/draft-lodderstedt-oauth-rar-03&sa=D&ust=1582561394194000>`_"
   "FHIR PractionerRole", "`https://www.hl7.org/fhir/practitionerrole.html <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://www.hl7.org/fhir/practitionerrole.html&sa=D&ust=1582561394195000>`_"
   "FHIR Organization", "`https://www.hl7.org/fhir/organization.html <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://www.hl7.org/fhir/organization.html&sa=D&ust=1582561394196000>`_"
   "FHIR Identifier", "`https://www.hl7.org/fhir/datatypes.html#Identifier <https://www.google.com/url?q=https://translate.google.com/translate?hl%3Den%26prev%3D_t%26sl%3Dauto%26tl%3Den%26u%3Dhttps://www.hl7.org/fhir/datatypes.html%2523Identifier%23Identifier&sa=D&ust=1582561394197000>`_"
   "Coding for Identifier", "`https://git.sarepta.ehelse.no/utvikling/FHIR/blob/402f626162e33e7ef488b44a5057c0c9aa553baa/norway/StructureDefinition/no-Organization.structuredefinition-profile.xml <https://www.google.com/url?q=https://git.sarepta.ehelse.no/utvikling/FHIR/blob/402f626162e33e7ef488b44a5057c0c9aa553baa/norway/StructureDefinition/no-Organization.structuredefinition-profile.xml&sa=D&ust=1582561394198000>`_"
