
Access control is a complex thing. It involves users, resources, and applications, and you have to set it up properly to prevent bad surprises. However, setting up access control is not just a matter of writing code or configuring a system. It includes (or better, it relies on) understanding a few basic concepts. Things can get even more complicated when delegated authorization comes into the picture, such as when using OAuth.

In this context, three core concepts are usually misunderstood and confused: **permissions**, **privileges**, and **scopes**. Let's try to go to the basics to better understand these concepts and their relationship.

By the way, here is a video version of this article:

## Actors of Access Control

Consider a scenario with the following actors:

- The **user**, the entity that wants to perform an action on an object.
- The **resource**, the object that a user wants to use.
- The **application**, the software that mediates the interaction between the user and the resource.

In this scenario, **access control is the selective restriction of access to a resource**. It determines who can do what on a resource. The relationship between the three actors described above contributes to the complexity of access control, as we will see in a moment.

## What Are Permissions?

Usually, you have a clear understanding of what a permission is. Commonly, you say that you have the permission to drive your car, load stuff in its trunk compartment, inspect its hood, clean it, sell it, and so on.

These common activities make you think that a permission is something related to you, the user. Actually, in the computer security context, a permission is something related to an object, to a resource, not to the user of that resource.

**A permission is a declaration of an action that can be executed on a resource**. It describes intrinsic properties of resources, which exist independently of the user. In the scenarios depicted above, driving and filling the trunk are actions you can do with your car: they are permissions attached to your car. The same happens with the action of accessing your computer, reading your emails, and so on. They all are permissions on those resources.

I know, that looks weird and a bit pedantic, even philosophical in some ways. But it is fundamental to accurately define this concept to understand the rest.

## What Are Privileges?

If permissions are bound to the resource, how can you express the ability to perform an action on that resource? How can you express the fact that someone is authorized to drive their car? What you need are privileges.

Simply put, **privileges are assigned permissions**. When you assign a permission to a user, you are granting them a privilege. If you assign a user the permission to read a document, you are granting them the privilege to read that document. Do you see the subtle difference?

I know. Usually, permissions and privileges are used interchangeably, but technically they have precise and different meanings in the context of computer security.

So, resources expose permissions. Users have privileges.

> For simplicity, we'll talk about privileges assigned to users, but keep in mind that privileges can also be assigned to applications. Consider, for example, an application running without user involvement that needs to access a resource.

## Authorization Scenarios

Once you understand the exact definition of permissions and privileges, we can easily talk about authorization scenarios. I mean, you can easily imagine a scenario where an entity protects a resource, and a user wants to exercise their privilege to use that resource. **The entity that protects the resource is responsible for restricting access to it, i.e., it is doing access control**.

Consider the following example. The concierge of a hotel is the entity that supervises the hotel's rooms (the resources) from unauthorized access. Each room has the ability to accommodate one guest (permission). A customer is assigned one room, so they have the privilege of being accommodated in that room. When the customer goes for a walk and then comes back to the hotel, the concierge checks if actually the customer has been assigned the permission to be accommodated in that room by consulting the booking list. If everything fits, the customer can come in, and we're all happy.

Imagine that one day, while the customer is out, a person shows up at the concierge desk. That person says they were requested by the customer to come into the room to get their briefcase. The concierge doesn't allow them to come in, of course. They need to do their own verifications to confirms what that person is asserting, such as calling the customer on the phone, for example.

However, even after everything is clear, the concierge needs to make sure that person will only do what they came to the hotel to do: take the customer's briefcase. Nothing else. The concierge will likely walk the person to the customer's room and make sure nothing else is done other than what was agreed upon.

The first scenario looks quite straightforward: it reduces the access control to comparing the customer booking data with the hotel's booking list. The second scenario is a bit more complex: it requires an additional check on what the person declares and control on what they are allowed to do.

These two scenarios may seem fictitious, but they have equivalent scenarios in authorization systems. The first scenario corresponds to the situation where a user attempts to access a protected resource for which they have privileges. This is the typical and most known authorization scenario.

The second scenario represents the case in which a third-party application wants to access a protected resource on behalf of a user. This case is known as a delegated authorization scenario.

The delegated authorization scenario is the typical scenario introduced by the [OAuth 2 Authorization Framework](https://auth0.com/docs/protocols/protocol-oauth2). This framework allows a third-party application to operate on a resource on behalf of a user. In other words, the application exercises the user's privilege to use a resource. But how does it happen? How can the application be delegated to access the user’s resource? Here, scopes come into the scene.

## What Are Scopes?

**Scopes enable a mechanism to define what an application can do on behalf of the user**. Recalling the hotel example, taking the customer's briefcase is the scope of the person who introduced themselves to the concierge.

The application requests these scopes to the authorization server, i.e., the server responsible for authorizing the third-party application in the delegated scenario. [Scopes were introduced in OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc6749#section-3.3) and are specific to that authorization framework.

In practical terms, **scopes are strings that represent what the application wants to do on behalf of the user**, as shown in the following authorization request example:

```bash
https://YOUR_DOMAIN/authorize?
  response_type=code&
  client_id=YOUR_CLIENT_ID&
  redirect_uri=https://YOUR_APP/callback& 
  scope=read:email&   #👈 here is the requested scope
  audience=YOUR_API_AUDIENCE&
  state=YOUR_STATE_VALUE
```

Typically, scopes are permissions of a resource that the application wants to exercise on behalf of the user. In other words, the application wants to exercise user's privileges on a resource such as reading their emails.

Actually, it is not the authorization server that allows the application to exercise the user's privileges. The final word on granting delegated access to the application is the user's own. The user can approve or reject delegated access to their resource with specific scopes by using a consent screen similar to the following:

![Consent screen](https://images.ctfassets.net/23aumh6u8s0i/1TrkMwEcKSR149zd878WEJ/8ae011dc08b89698c0dce24e8ce01e43/consent-screen.png)

You can experience it firsthand by [signing up for Auth0](https://a0.to/blog_signup) and securing an application built with [several supported technologies](https://auth0.com/docs/libraries).

The granted scopes allow the application to access only the user's resources, say the user's emails. Just as the user cannot access other users' emails, the application cannot access other users' emails either. Usually, **the scopes granted to a third-party application are a subset of the permissions granted to the user**.

Keep in mind, however, that an application can request scopes corresponding to privileges that the user doesn't have. And the user can grant them. This may look tricky, but it covers situations where the user doesn't have a given privilege in the moment they grant the scopes to the application, but they will have that privilege when the application exercises that scope. The vice versa can happen too: a user has a given privilege and grants a delegated access for the corresponding scope, but the user loses the privilege when the application tries to exercise its scope. At the end of the day, what is relevant when the application exercises its scopes is the possession of the corresponding privileges by the user at that time. This means that **the application can't do more than the user can do**.

Once the user approves the application's request, the authorization server communicates the granted scopes to the application so that it can access the resource with the limited granted privileges.

## A Complicated Relationship

Now you have a clearer picture of what permissions, privileges, and scopes are. Unfortunately, this may not be enough. In fact, these items are strictly related, and often this relationship gets confusing. Let's try to clarify the most common misunderstandings.

### Scopes and privileges

Often, developers think that scopes are application's privileges. After all, the user granted their consent to use them. However, they usually miss a little detail: applications are authorized to exercise those privileges **on behalf of the user**. If the user doesn't have a privilege, the application cannot exercise it.

This means that, on the resource side, **users' privileges must be checked even in the presence of granted scopes**. Remember that, in a delegated authorization scenario, the application may act on behalf of the user even when the user is not logged in. If the user no longer has those privileges between their consent and the exercise by the application, the application must be prevented from exercising its delegated access. This should clarify why **scopes should not be considered application's privileges**.

These may look like philosophical issues, but they have practical implications. Check out [this article on the nature of scopes](https://auth0.com/blog/on-the-nature-of-oauth2-scopes/) to learn more about this.

### Scopes and permissions

Once you know what permissions, privileges, and scopes are, you might assume that there is a natural mapping between permissions and scopes. In other words, if a resource has permissions A and B, the scopes X and Y respectively will exist to allow an application to exercise those permissions on the resource and vice versa. While in most cases permissions are exposed as scopes, this mapping is not strictly correct for a few reasons.

In a delegated scenario, not all the permissions on a resource should necessarily be made available to be requested as scopes. You could reserve some permissions for the user and decide that they are not delegable. For example, you could decide that the permission to delete a document is never delegated to a third-party application.

So you might think that this supposed mapping is not between scopes and permissions but between scopes and privileges. This way, you can't map a scope to a permission not assigned to the user. Actually, this is not accurate either. There are scopes that don't have a match among the resource's permissions or the user's privileges. Consider the 

```javascript
openid
```

 scope defined by [OpenID Connect specifications](https://openid.net/specs/openid-connect-core-1_0.html#AuthRequest). This scope does not correspond to any permission on a specific user's resource. It is a request for the authorization server to return an ID token as the result of the user's authentication. The same applies to the other [scopes defined by OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims): 

```javascript
profile
```

, 

```javascript
email
```

, 

```javascript
address
```

, and 

```javascript
phone
```

.

So, you should avoid this unconscious mapping between scopes and permissions.

## Summary

At the end of this journey, you learned the subtle differences between permissions, privileges, and scopes and their mutual relationship. Understanding these concepts and their thin borders will help you build more secure applications. The following picture tries to summarize what you learned in this article:

![Permissions, privileges and scopes](https://images.ctfassets.net/23aumh6u8s0i/2wSDOXUzAKuCAA0rw79Fx8/8f66ad24881e3da2a73249a2a7b05e27/permissions-privileges-scopes.jpg)


### References 

https://auth0.com/blog/permissions-privileges-and-scopes/

https://www.youtube.com/watch?v=vULfBEn8N7E&t=405s