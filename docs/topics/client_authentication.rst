Client Authentication
=====================

In certain situations, clients need to authenticate with HelseID, e.g.

* confidential applications (aka clients) requesting tokens at the token endpoint
* APIs validating reference tokens at the introspection endpoint

For that purpose you can assign a list of secrets to a client or an API resource.

Shared secret
^^^^^^^^^^^^^
The following code sets up a hashed shared secret::

    var secret = new Secret("secret".Sha256());

This secret can now be assigned to either a ``Client`` or an ``ApiResource``. 
Notice that both do not only support a single secret, but multiple. This is useful for secret rollover and rotation.

You can either send the client id/secret combination as part of the POST body::

    POST /connect/token
    
    client_id=client1&
    client_secret=secret&
    ...

..or as a basic authentication header::

    POST /connect/token
    
    Authorization: Basic xxxxx

    ...

You can manually create a basic authentication header using the following C# code::

    var credentials = string.Format("{0}:{1}", clientId, clientSecret);
    var headerValue = Convert.ToBase64String(Encoding.UTF8.GetBytes(credentials));

    var client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", headerValue);

The `IdentityModel <https://github.com/IdentityModel/IdentityModel2>`_ library has helper classes called ``TokenClient`` and ``IntrospectionClient`` that encapsulate
both authentication and protocol messages.

Cryptography
^^^^^^^^^^^^
There are other techniques to authenticate clients, e.g. based on public/private key cryptography.
HelseID includes support for private key JWT client secrets (see `RFC 7523 <https://tools.ietf.org/html/rfc7523>`_).
The supported methods are self signed X509 certificates, RSA keypair and enterprise certificates. 

X509 certificates
"""""""""""""""""

To request a token, you need to supply the client certificate to the HTTP client and add the client ID to the post body. 
The following example uses the IdentityModel OAuth2 client ::

    async Task<TokenResponse> RequestTokenAsync()
    {
        var cert = new X509Certificate2("Client.pfx");

        var handler = new WebRequestHandler();
        handler.ClientCertificates.Add(cert);

        var client = new OAuth2Client(
            new Uri("https://identityserver.io/core/connect/token"),
            "certclient",
            handler);

        return await client.RequestClientCredentialsAsync("read write");
    }

Client assertion
""""""""""""""""
To use a enterprise certificate or a RSA keypair involves creating a client assertion, a jwt structure with client information, signed with the private key. 


The following code creates the base of a client assertion ::

   private static string GenerateJwt(string clientId, string audience, DateTime? expiryDate,
            SigningMethod signingMethod, SecurityKey securityKey)
        {	
            var signingCredentials = new SigningCredentials(securityKey, SecurityAlgorithms.RsaSha512);	
	
            var jwt = CreateJwtSecurityToken(clientId, audience, expiryDate, signingCredentials);	
	
            var tokenHandler = new JwtSecurityTokenHandler();	
            return tokenHandler.WriteToken(jwt);	
        }	

    private static JwtSecurityToken CreateJwtSecurityToken(string clientId, string audience, DateTime? expiryDate,
            SigningCredentials signingCredentials)
        {	
            var exp = new DateTimeOffset(expiryDate ?? DateTime.Now.AddHours(DefaultExpiryInHours));	
	
            var claims = new List<Claim>
            {	
                new Claim(JwtClaimTypes.Subject, clientId),	
                new Claim(JwtClaimTypes.IssuedAt, exp.ToUnixTimeSeconds().ToString()),	
                new Claim(JwtClaimTypes.JwtId, Guid.NewGuid().ToString("N"))	
            };	
	
            var token = new JwtSecurityToken(clientId, audience, claims, DateTime.Now, DateTime.Now.AddHours(10), signingCredentials);	
	
            return token;	
        }

Enterprise certificates
"""""""""""""""""""""""
An addition of enterprise certificates is that the certificate is added as an x5c header which HelseID can use to validate the assertion.
This is done in the following way ::

    private static void UpdateJwtHeader(SecurityKey key, JwtSecurityToken token)
        {	
            var thumbprint = Base64Url.Encode(x509Key.Certificate.GetCertHash());	
            var x5c = GenerateX5c(x509Key.Certificate);	
            var pubKey = x509Key.PublicKey as RSA;	
            var parameters = pubKey.ExportParameters(false);	
            var exponent = Base64Url.Encode(parameters.Exponent);	
            var modulus = Base64Url.Encode(parameters.Modulus);	

            token.Header.Add("x5c", x5c);	
            token.Header.Add("kty", pubKey.SignatureAlgorithm);	
            token.Header.Add("use", "sig");	
            token.Header.Add("x5t", thumbprint);	
            token.Header.Add("e", exponent);	
            token.Header.Add("n", modulus);	
        }	

Using an enterprise certificate provides HelseID with some organizational claims which can be used in the generation of tokens. They consist of: 

- `helseid://claims/client/ec/orgnr_parent`
- `helseid://claims/client/ec/orgnr_child`

- `helseid://claims/client/ec/exp`






