
-  AWS Service that offers **Authentication** and **Authorization** features.
- Allows you to add user registration, sign in and access control.
- Scalable and Highly available supporting millions of users.
- Supports standards based Identity Providers (OAuth 2.0, OIDC, SAML).
- Useful in a variety of contexts:
	- Keeping an active directory of Users
	- Securing your APIs
	- Providing temporary access to AWS resources.

-  Your application or client doesn't need to worry about user registration, storing user information such as username and password in the database and securing the API. All will be handled by Cognito.

## Core Concepts

-  User Pools
- Identity Pools


![[Pasted image 20241124184836.png]]

### Creating a User Pool

![[Pasted image 20241124184941.png]]


### User Pool Hosted UI

![[Pasted image 20241124185400.png]]


There are two users created in the user pool, one with sign in page in the left of the image and other with the identity provider - Google in this case.

### Application Integration with User Pools

![[Pasted image 20241124190103.png]]


1. Authenticate the user with User pool hosted UI and cognito will generate the access token and use this access token to access the backend APIs, backend service needs to verify JWT token using cognito service and after verification grant access to the user.
2. Integrate API gateway with cognito and grant access to aws services like lambda.
3. Use AWS Amplify service which is mainly used by Mobile and Web Client like React.
4. Use AWS SDK and use api's of cognito to create user , authenticate and verification.


### User Pools

- Cognito **User Pools** at their heart are User Directories.
- Users can sign up / sign in either directly in Cognito or via an SSO.
- Cognito leaverages common OAuth 2.0 flows such as Implicit/ Auth Flow.
- Triggers allow you to inject hooks into auth stages.
- Integrate with Cognito via Amplify or AWS SDK.

### Identity Pools

- Cognito **Identity Pools** provide short term AWS access for users.
- Integrate your Identity Provider with your **Identity Pool**.
- Guest access for restrictive authorization.
- Dynamically select IAM Roles via **Token Attributes** or **IAM Policies**


![[Pasted image 20241124192334.png]]


![[Pasted image 20241124192514.png]]


![[Pasted image 20241124192758.png]]


### Integration of AWS Cognito User Pool with API Gateway

###  Create App Client

![[Pasted image 20241124193452.png]]


### Get the App Client Id and Secret.

![[Pasted image 20241124193611.png]]

### App integration with Cognito User Pool.

![[Pasted image 20241124193918.png]]


### Configure a domain name to activate the hosted UI

![[Pasted image 20241124194315.png]]

### Create an API Gateway for REST APIs

![[Pasted image 20241124194938.png]]


### Create a Resource

![[Pasted image 20241124195118.png]]

### Create a Get API and integrate with Lambda Function

![[Pasted image 20241124195057.png]]


### Create new Authorizers to connect API gateway with User Pools

![[Pasted image 20241124195911.png]]

### Connect API Gateway with Authorizer

![[Pasted image 20241124200802.png]]


There are three types of tokens:

1. Id Token - Used by API gateway to get the details of the user and verify the identity of the user.
2. Access Token - Used by user or client to get access of API
3. Refresh Token - To bypass the authentication and refresh the expired access token.

### **ID Token in AWS Cognito**

An **ID Token** in **AWS Cognito** is a **JWT (JSON Web Token)** that contains **user identity information** (claims) after successful authentication. It is part of the **Cognito Authentication Flow** and is issued alongside the **Access Token** and **Refresh Token** when a user successfully authenticates via a federated login (such as a username/password, Facebook, Google, or other identity providers).

The ID token is primarily used for **identifying the authenticated user** and is intended to be used by the application to obtain user-specific information (e.g., user attributes).

### **When is the ID Token Issued?**

The **ID Token** is issued as part of the authentication flow when:

- The user logs in via a **Cognito User Pool** (with username/password, social login, or other authentication mechanisms).
- The ID Token is returned as part of the **OAuth 2.0 Authorization Code Flow** or **Implicit Flow**.
- It can also be issued through **Federated Identities** if using an external identity provider (such as Google, Facebook, or other OIDC-compliant providers).
### **Use Cases for ID Token**

1. **User Profile**: You can use the ID token to fetch user information, such as the user's email, username, and other attributes.
2. **Authorization**: While the **Access Token** is used to authorize API requests, the **ID Token** is used to verify the identity of the user.

### **How to Get the ID Token in AWS Cognito**

1. **User Sign-in**: The first step is to authenticate the user with the Cognito User Pool via an authentication method (such as username/password, or social login).
2. **Obtain Tokens**: After a successful login, the Cognito Authentication service issues a set of tokens:
    - **ID Token**
    - **Access Token**
    - **Refresh Token**

If you’re using **OAuth 2.0** or **Cognito’s hosted UI**, the ID token is returned as part of the **authorization code flow** or the **implicit flow**.

### Payload

```
{
  "sub": "a4fb5075-b570-4cba-a8d7-5485d6b3e7a7",
  "email": "user@example.com",
  "email_verified": true,
  "preferred_username": "user123",
  "given_name": "John",
  "family_name": "Doe",
  "iat": 1612345678,
  "exp": 1612349278,
  "aud": "6b2b6632-1c1f-4b5d-b5a0-12345678bde0",
  "iss": "https://cognito-idp.us-east-1.amazonaws.com/us-east-1_XXXXXX",
  "token_use": "id"
}

```
The payload part contains **claims** that identify the authenticated user. Here are some common claims found in an ID token:

#### **Common Claims in an ID Token**

- **sub**: The **subject** of the token, typically a unique user ID in Cognito.
- **email**: The user's email address.
- **email_verified**: Boolean flag indicating if the email is verified.
- **preferred_username**: The username associated with the user's Cognito account.
- **given_name**: The user's first name (if available).
- **family_name**: The user's last name (if available).
- **iat**: Issued at time.
- **exp**: Expiration time of the token.
- **aud**: The audience for which the token is intended (typically the app or client).
- **iss**: Issuer of the token (typically Cognito).
- **token_use**: Specifies the token's use; for ID tokens, it’s always `id`.