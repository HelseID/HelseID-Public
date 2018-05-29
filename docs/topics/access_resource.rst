Requesting access to a resource
===================================

A client needs to be configured with explicit rights in order to obtain an access token to a spesific API. This right is expressed in what is known as a scope. 
An example of a scope in OAuth 2.0 therms could e.g. be "https://www.minehelsedata.no/patient/read" which could be used to provide the client access to read patient data from an API. 

When a client issues a request for an access token it also needs to provide which scopes it wants in the token. If the configuration in HelseID reflects that the client is allowed to ask for these scopes they will be added to the access token.

Scope
^^^^^
A scope indicates what type of resource an access token is given right to access. Since a scope gives access to a resource is can be looked on as an authorization. However, it is important to notice that this is an authorization on the client level and not the user level. When securing an resource with HelseID it needs to be expressed as a minimum of one scope. Multiple scopes could be defined for the same resource in order to give the correct resolution of the access.

Access to an API (Resource)
=============================

Resource
^^^^^^^^^^^
A resource is something that you wish to protect with HelseId. Either identity data for your users or an API.
Each resource are identified with an unique name and a set of scopes. This name is used as part of the scope naming which clients uses when requesting access. This name will also be set on the Audience claim in the access token. Giving the resource information regarding to whom the access token apply.
The access token contains information about scopes, the client and the user (if present), which the API can use to allow access to its protected data.

Scope
^^^^^
In its easiest case an API has just one scope. However, in more complex senarios this would not be a adequate solution and one needs more granularity. This is solved by defining multiple scopes that provides access to different resources and actions. E.g. a read and a write scope, or scopes for different subdomains. 
All scopes needs to be predefined in HelseId and the client needs the be provisioned to use them in advance. 

Access to multiple API's (Resources)
====================================
Imagine the senario where a client needs access to multiple resources. This could be solved in several ways.
Either the client asks for an access token that contains all the scopes it needs, or it ask for several access tokens each with one of the needed scopes. 
Both of these ways could be correct given the right circumstances. As long as there are mutual trust between the client and all the resources in an token, there are not problem with the approach where all scopes are in the same access token. 
However, if the trust between the parties are lacking, this approach exposes posibility for a resource to impersonatate the user with the given token and using it to access the other resources in the token.
In these cases we usualy talk about circle of trust. Within a circle of trust scopes can coexist in a token, but between circles on should request different tokens to limit the risk of impersonation. 

All in one token (Circle of trust)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Pros: easy, grouping based on trust Cons: exposure of access information to other resources,  risk of impersonation.

One token for each access need
^^^^^^^^^^^^^^^^^^^^^^^^^^
Pros: Less risk of abuse, Cons: more complexity regarding handling of several accesst tokens, more requests towards HelseID.

Access on behalf of user (Token exchange)
============================================

Imagine the following scenario: a front end client calls a middle tier API using a token acquired via an interactive flow (e.g. hybrid flow). This middle tier API (API 1) now wants to call a back end API (API 2) on behalf of the interactive user.
In other words, the middle tier API (API 1) needs an access token containing the user’s identity, but with the scope of the back end API (API 2). You might have heard of the term poor man’s delegation where the access token from the front end is simply forwarded to the back end.
This has some shortcomings, e.g. API 2 must now accept the API 1 scope which would allow the user to call API 2 directly. Also - you might want to add some delegation specific claims into the token, e.g. the fact that the call path is via API 1.

Impersonation
^^^^^^^^^^^^^
When principal A impersonates principal B, A is given all the rights that B has within some defined rights context and is indistinguishable from B in that context. Thus, when principal A impersonates principal B, then in so far as any entity receiving such a token is concerned, they are actually dealing with B.  It is true that some members of the identity system might have awareness that impersonation is going on, but it is not a requirement.  For all intents and purposes, when A is impersonating B, A is B.
Delegation
^^^^^^^^^^
Delegation semantics are different than impersonation semantics, though the two are closely related.  With delegation semantics, principal A still has its own identity separate from B and it is explicitly understood that while B may have delegated some of its rights to A, any actions taken are being taken by A representing B. In a sense, A is an agent for B.

