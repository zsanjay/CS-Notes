## What is an identity provider (IdP)?

An identity provider (IdP) stores and manages users' [digital identities](https://www.cloudflare.com/learning/access-management/what-is-identity/). Think of an IdP as being like a guest list, but for digital and cloud-hosted applications instead of an event. An IdP may check user identities via username-password combinations and other factors, or it may simply provide a list of user identities that another service provider (like an SSO) checks.

IdPs are not limited to verifying human users. Technically, an IdP can authenticate any entity connected to a network or a system, including computers and other devices. Any entity stored by an IdP is known as a "principal" (instead of a "user"). However, IdPs are most often used in [cloud computing](https://www.cloudflare.com/learning/cloud/what-is-the-cloud/) to manage user identities.

## What is user identity?

Digital user identity is associated with quantifiable factors that can be verified by a computer system. These factors are called "authentication factors." The three authentication factors are:

- **Knowledge:** something you know, such as a username and password
- **Possession:** something you have, such as a smartphone
- **Intrinsic qualities:** something you are, such as your fingerprint or a retina scan

An IdP may use one or more of these factors to identify a user. Using multiple factors to verify user identify is called [multi-factor authentication (MFA)](https://www.cloudflare.com/learning/access-management/what-is-multi-factor-authentication/).

## Why are IdPs necessary?

Digital identity must be tracked somewhere, especially for cloud computing, where user identity determines whether or not someone can access sensitive data. Cloud services need to know exactly where and how to retrieve and verify user identity.

Records of user identities also need to be stored in a [secured](https://www.cloudflare.com/learning/cloud/what-is-cloud-security/) fashion to ensure that attackers cannot use them to impersonate users. A cloud identity provider will typically take extra precautions to protect user data, whereas a service not dedicated solely to storing identity may store it in an unsecured location, such as a server open to the Internet.

## How do IdPs work with SSO services?

A [single sign-on service](https://www.cloudflare.com/learning/access-management/what-is-sso/), often called an 'SSO,' is a unified place for users to sign in to all their cloud services at once. In addition to being more convenient for users, implementing SSO often makes user logins more secure.

For the most part, SSOs and IdPs are separate. An SSO service uses an IdP to check user identity, but it does not actually store user identity. An SSO provider is more of a go-between than a one-stop shop; think of it as being like a security guard firm that is hired to keep a company secure but is not actually part of that company.

Even though they are separate, IdPs are an essential part of the SSO login process. SSO providers check user identity with the IdP when users log in. Once that is done, the SSO can verify user identity with any number of connected cloud applications.

However, this is not always the case. An SSO and IdP can theoretically be one and the same. But this setup is much more open to [on-path attacks](https://www.cloudflare.com/learning/security/threats/on-path-attack/) in which an attacker forges a [SAML](https://www.cloudflare.com/learning/access-management/what-is-saml/) assertion* in order to gain access to an application. For this reason, IdP and SSO are typically separated.

_*A SAML assertion is a specialized message sent from SSO services to any cloud application that confirms user authentication, allowing the user to access and use the application._

**How does all this look in practice?**

Suppose Alice is using her work laptop at her employer's office. Alice needs to log in to the company's live chat application in order to better coordinate with her coworkers. She opens a tab on her browser and loads the chat application. Assuming her company uses an SSO service, the following steps take place behind the scenes:

- The chat app asks the SSO for Alice's identity verification.
- The SSO sees that Alice has not signed in yet.
- The SSO prompts Alice to log in.

At this point Alice's browser redirects her to the SSO login page. The page has fields for Alice to enter her username and password. Since her company requires [two-factor authentication](https://www.cloudflare.com/learning/access-management/what-is-two-factor-authentication/), Alice also has to enter a short code that the SSO automatically texts to her smartphone. After this is done, she clicks "Log in." Now, the following things happen:

- The SSO sends a SAML request to the IdP used by Alice's company.
- The IdP sends a SAML response to the SSO confirming Alice's identity.
- The SSO sends a SAML assertion to the chat application Alice originally wanted to use.

Alice is redirected back to her chat application. Now she can chat with her coworkers. The whole process only took seconds.