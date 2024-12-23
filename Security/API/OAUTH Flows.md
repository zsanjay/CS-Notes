The [OAuth 2.0 Authorization Framework](https://auth0.com/docs/authenticate/protocols/oauth) supports several different flows (or grants). Flow are ways of retrieving an Access Token. Deciding which one is suited for your use case depends mostly on your [application type](https://auth0.com/docs/get-started/applications), but other parameters weigh in as well, like the level of trust for the client, or the experience you want your users to have.

## OAuth 2.0 terminology

- **Resource Owner**: Entity that can grant access to a protected resource. Typically, this is the end-user.
    
- **Client**: Application requesting access to a protected resource on behalf of the Resource Owner.
    
- **Resource Server**: Server hosting the protected resources. This is the API you want to access.
    
- **Authorization Server**: Server that authenticates the Resource Owner and issues Access Tokens after getting proper authorization. In this case, Auth0.
    
- **User Agent**: Agent used by the Resource Owner to interact with the Client (for example, a browser or a native application).


## 1. Is the Client the Resource Owner?

The first decision point is about whether the party that requires access to resources is a machine. In the case of machine-to-machine authorization, the Client is also the Resource Owner, so no end-user authorization is needed. An example is a cron job that uses an API to import information to a database. In this example, the cron job is the Client and the Resource Owner since it holds the Client ID and Client Secret and uses them to get an Access Token from the Authorization Server.

If this case matches your needs, then to learn how this flow works and how to implement it, see[Client Credentials Flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow).

### Client Credentials Flow

The Client Credentials Flow (defined in [OAuth 2.0 RFC 6749, section 4.4](https://tools.ietf.org/html/rfc6749#section-4.4)) involves an application exchanging its application credentials, such as client ID and client secret, for an access token.

This flow is best suited for Machine-to-Machine (M2M) applications, such as CLIs, daemons, or backend services, because the system must authenticate and authorize the application instead of a user.

## How it works

![Flows - Client Credentials - Authorization sequence diagram(w/Border)](https://images.ctfassets.net/cdy7uua7fh8z/4Ph562CAccmCUkKNXuEIlQ/47581872e40e87b2cca95aecf7f42c5b/diagram.png)

1. Application sends application's credentials to the Auth0 Authorization Server. To learn more about client authentication methods, read [Application Credentials](https://auth0.com/docs/secure/application-credentials).

2. Auth0 Authorization Server validates application's credentials.

3. Auth0 Authorization Server responds with an access token.

4. Application can use the access token to call an API on behalf of itself. For more information on this process, see [Validate JSON Web Tokens](https://auth0.com/docs/secure/tokens/json-web-tokens/validate-json-web-tokens).

5. API responds with requested data.


## 2. Is the Client a web app executing on the server?

If the Client is a regular web app executing on a server, then the Authorization Code Flow is the flow you should use. Using this the Client can retrieve an Access Token and, optionally, a Refresh Token. It's considered the safest choice since the Access Token is passed directly to the web server hosting the Client, without going through the user's web browser and risking exposure.

If this case matches your needs, then to learn how this flow works and how to implement it, see [Authorization Code Flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow).

###  Authorization Code Flow

The Authorization Code Flow (defined in [OAuth 2.0 RFC 6749, section 4.1](https://tools.ietf.org/html/rfc6749#section-4.1)), involves exchanging an authorization code for a token.

This flow can only be used for confidential applications (such as Regular Web Applications) because the application's authentication methods are included in the exchange and must be kept secure.

## How Authorization Code Flow works

![Auth - Auth code flow- Authorization sequence diagram](https://images.ctfassets.net/cdy7uua7fh8z/7mWk9No612EefC8uBidCqr/821eb60b0aa953b0d8e4afe897228844/Auth-code-flow-diagram.png)

1. User selects **Login** within application.

2. Auth0's SDK redirects user to Auth0 Authorization Server ([`/authorize`endpoint](https://auth0.com/docs/api/authentication#authorization-code-grant)).

3. Auth0 Authorization Server redirects user to login and authorization prompt.

4. User authenticates using one of the configured login options, and may see a consent prompt listing the permissions Auth0 will give to the application.

5. Auth0 Authorization Server redirects user back to application with single-use authorization code.

6. Auth0's SDK sends authorization code, application's client ID, and application's credentials, such as client secret or Private Key JWT, to Auth0 Authorization Server ([`**/oauth/token**`endpoint](https://auth0.com/docs/api/authentication?http#authorization-code-flow43)).

7. Auth0 Authorization Server verifies authorization code, application's client ID, and application's credentials.

8. Auth0 Authorization Server responds with an ID token and access token (and optionally, a refresh token).

9. Application can use the access token to call an API to access information about the user.

10. API responds with requested data.


## Is the Client absolutely trusted with user credentials?

This decision point may result in the Resource Owner Password Credentials Grant. In this flow, the end-user is asked to fill in credentials (identifier/password), typically using an interactive form. This information is sent to the backend and from there to Auth0. It is therefore imperative that the Client is absolutely trusted with this information.

This grant should only be used when redirect-based flows (like the [Authorization Code Flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow)) are not possible. If this is your case, then to learn about how this flow works and how to implement it, see [Resource Owner Password Flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/resource-owner-password-flow).

### Resource Owner Password Flow

**Because the Resource Owner Password (ROP) Flow involves the application handling the user's password, it must not be used by third-party clients.**

Though we do not recommend it, highly-trusted applications can use the Resource Owner Password Flow (defined in [OAuth 2.0 RFC 6749, section 4.3](https://tools.ietf.org/html/rfc6749#section-4.3) and sometimes called Resource Owner Password Grant or ROPG), which requests that users provide credentials (username/email/phone and password), typically using an interactive form. Because credentials are sent to the backend and can be stored for future use before being exchanged for an Access Token, it is imperative that the application is absolutely trusted with this information. 

Even if this condition is met, the Resource Owner Password Flow should only be used when redirect-based flows (like the [Authorization Code Flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow)) cannot be used.

## How it works

![Diagram - Resource Owner Password Flow](https://images.ctfassets.net/cdy7uua7fh8z/4EeYNcnVX1RFcTy5z4lP4v/c3e4d22e6f8bf558caf07338a7388097/ROP_Grant.png)

1. The user clicks **Login** within the application and enters their credentials.

2. Your application forwards the user's credentials to your Auth0 Authorization Server ([`**/oauth/token**`endpoint](https://auth0.com/docs/api/authentication#resource-owner-password)).

3. Your Auth0 Authorization Server validates the credentials.

4. Your Auth0 Authorization Server responds with an Access Token (and optionally, a Refresh Token).

5. Your application can use the Access Token to call an API to access information about the user.

6. The API responds with requested data.

## Is the Client a Single-Page App?

If the Client is a Single-Page App (SPA), an application running in a browser using a scripting language like JavaScript, there are two grant options: the Authorization Code Flow with Proof Key for Code Exchange (PKCE) and the Implicit Flow with Form Post. For most cases, we recommend using the Authorization Code Flow with PKCE because the Access Token is not exposed on the client side, and this flow can return [Refresh Tokens](https://auth0.com/docs/secure/tokens/refresh-tokens).

To learn more about how this flow works and how to implement it, see [Authorization Code Flow with Proof Key for Code Exchange (PKCE)](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow-with-pkce). The [Auth0 Single-Page App SDK](https://auth0.com/docs/libraries/auth0-single-page-app-sdk)provides high-level API for implementing Authorization Code Flow with PKCE in SPAs.

If your SPA doesn't need an Access Token, you can use the Implicit Flow with Form Post. To learn more about how this flow works and how to implement it, see [Implicit Flow with Form Post](https://auth0.com/docs/get-started/authentication-and-authorization-flow/implicit-flow-with-form-post)
### Implicit Flow with Form Post

[Video Link](https://www.youtube.com/watch?v=guvhHTyyAUo)

You can use OpenID Connect (OIDC) with many different flows to achieve web sign-in for a traditional web app. In one common flow, you obtain an ID token using authorization code flow performed by the app backend. This method is effective and robust, however, it requires your web app to obtain and manage a secret. You can avoid that burden if all you want to do is implement sign-in and you don’t need to obtain access tokens for invoking APIs.

Implicit Flow with Form Post flow uses OIDC to implement web sign-in that is very similar to the way SAMLand WS-Federation operates. The web app requests and obtains tokens through the front channel, without the need for secrets or extra backend calls. With this method, you don’t need to obtain, maintain, use, and protect a secret in your application.

## How it works

You should use this flow for login-only use cases; if you need to request Access Tokens while logging the user in so you can call an API, use the [Authorization Code Flow with PKCE](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow-with-pkce) or the [Hybrid Flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/hybrid-flow).

![Flows - Implicit with Form Post - Authorization sequence diagram](https://images.ctfassets.net/cdy7uua7fh8z/6m0uE4E7Hpzbdhyh9dEuYK/e36c910ff47a7540bf27e23c02822624/auth-sequence-implicit-form-post.png)

1. The user clicks **Login** in the app.

2. Auth0's SDK redirects the user to the Auth0 Authorization Server (`/authorize` endpoint) passing along a `response_type` parameter of `id_token` that indicates the type of requested credential. It also passes along a `response_mode` parameter of `form_post` to ensure security.

3. Your Auth0 Authorization Server redirects the user to the login and authorization prompt.

4. The user authenticates using one of the configured login options and may see a consent page listing the permissions Auth0 will give to the app.

5. Your Auth0 Authorization Server redirects the user back to the app with an ID Token.


### Authorization Code Flow with Proof Key for Code Exchange (PKCE)

[Video Link](https://www.youtube.com/watch?v=h_1JAh3JPkI)
#### Overview

Key Concepts

- Learn about the OAuth 2.0 grant type, Authorization Code Flow with Proof Key for Code Exchange (PKCE).

- Use this grant type for applications that cannot store a client secret, such as native or single-page apps.

- Review different implementation methods with Auth0 SDKs.


When public clients (e.g., native and single-page applications) request access tokens, some additional security concerns are posed that are not mitigated by the Authorization Code Flow alone. This is because:

**Native apps**

- Cannot securely store a Client Secret. Decompiling the app will reveal the Client Secret, which is bound to the app and is the same for all users and devices.

- May make use of a custom URL scheme to capture redirects (e.g., MyApp://) potentially allowing malicious applications to receive an Authorization Code from your Authorization Server.


**Single-page apps**

- Cannot securely store a Client Secret because their entire source is available to the browser.


Given these situations, OAuth 2.0 provides a version of the Authorization Code Flow which makes use of a Proof Key for Code Exchange (PKCE) (defined in [OAuth 2.0 RFC 7636](https://tools.ietf.org/html/rfc7636)). 

The PKCE-enhanced Authorization Code Flow introduces a secret created by the calling application that can be verified by the authorization server; this secret is called the Code Verifier. Additionally, the calling app creates a transform value of the Code Verifier called the Code Challenge and sends this value over HTTPS to retrieve an Authorization Code. This way, a malicious attacker can only intercept the Authorization Code, and they cannot exchange it for a token without the Code Verifier.

## How it works

Because the PKCE-enhanced Authorization Code Flow builds upon the [standard Authorization Code Flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow), the steps are very similar.

![Flows - Authorization Code with PKCE - Authorization sequence diagram](https://images.ctfassets.net/cdy7uua7fh8z/3pstjSYx3YNSiJQnwKZvm5/33c941faf2e0c434a9ab1f0f3a06e13a/auth-sequence-auth-code-pkce.png)

1. The user clicks **Login** within the application.

2. Auth0's SDK creates a cryptographically-random `code_verifier` and from this generates a `code_challenge`.

3. Auth0's SDK redirects the user to the Auth0 Authorization Server ([`**/authorize**`endpoint](https://auth0.com/docs/api/authentication#authorization-code-grant-pkce-)) along with the `code_challenge`.

4. Your Auth0 Authorization Server redirects the user to the login and authorization prompt.

5. The user authenticates using one of the configured login options and may see a consent page listing the permissions Auth0 will give to the application.

6. Your Auth0 Authorization Server stores the `code_challenge` and redirects the user back to the application with an authorization `code`, which is good for one use.

7. Auth0's SDK sends this `code` and the `code_verifier` (created in step 2) to the Auth0 Authorization Server `(`[`**/oauth/token**`endpoint](https://auth0.com/docs/api/authentication?http#authorization-code-flow-with-pkce44)).

8. Your Auth0 Authorization Server verifies the `code_challenge` and `code_verifier`.

9. Your Auth0 Authorization Server responds with an ID token and access token (and optionally, a refresh token).

10. Your application can use the access token to call an API to access information about the user.

11. The API responds with requested data.


If you have [Refresh Token Rotation](https://auth0.com/docs/secure/tokens/refresh-tokens/refresh-token-rotation) enabled, a new Refresh Token is generated with each request and issued along with the Access Token. When a Refresh Token is exchanged, the previous Refresh Token is invalidated but information about the relationship is retained by the authorization server.

### References 

https://auth0.com/docs/get-started/authentication-and-authorization-flow/which-oauth-2-0-flow-should-i-use#oauth-2-0-terminology