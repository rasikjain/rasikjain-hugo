+++
date = "2019-12-06"
title = "Security - Authentication Workflows"
slug = "security-application-authentication-workflows"
tags = [
    "security",
    "authentication",
    "programming",
    "development",
]
+++

Here are three scenarios for implemeting authentication workflows.

## Internet Applications (Public Facing)

For internet web applications and APIs, Session based (SessionID cookie) and Token Based (JWT) Authentication can be implemented.

**Session Based:** Implemented for majority of traditional and stateful web applications. Once the user is authenticated, A Session state is created and stored in external State server or SQL database. The Session state is identified by unique SessionID. This SessionID is returned to the client and set as a cookie using **"Set-Cookie: sessionid=38askse24d56dh43hrs7a8; HttpOnly; Path=/"**.

**JWT Token:** Implemented for SPA Apps, Restful APIs and Stateless Web applications. Once the user is authenticated with an Auth provider, A JWT token is created and sent to client browser. After successful Authentication, **"Authorization: Bearer {token}"** is sent with every http request.

**Authentication Flow:**

- User try to access the restricted content/resource.
- If the User receives **“Access Denied”** or (401, 403) Http status code, then user is redirected to Login page.
- Once the username and password are submitted, Credentials are **validated against a repository** (e.g. Credential Storage/Database, Auth Provider, etc.).
- Upon successful Authentication, Server creates either **Session cookie or JWT Token** based on the type of Implementation.
- Client tries to access the restricted content/resource along with Session cookie or JWT Token Authorization Header.
- **Authorization check** is performed for the access rights. We check for either user roles, permissions or claims within Session State, JWT Token Payload or SQL Database depending on the setup.
- After **successful Authorization**, the user is presented with content/resource.
- When user triggers logout action or user is in-active for more than certain time (30mins), **session cookie** is expired and session state is terminated on the server-side; and for the **JWT Token**, delete the token from the client and configure the token with expiration time for inactivity.
- User is re-directed to logout/login page once the session or JWT Token is terminated.

## Intranet Applications

Most of the Intranet applications are used by employees within the organization. We have implemented following authentication mechanisms for intranet apps.

- **Basic and Digest Authentication ->** (Slow, Not Secure, Username and Password are sent with every request)
- **Windows Integrated Authentication ->** (Kerberos or NTLMv2 mechanism, Authenticated against Active Directory)
- **Forms Based Authentication ->** (Username and Password validated against a custom database storage or the LDAP)

**Authentication Flow:**

- User try to access the restricted content/resource.
- User receives access denied or **401** Status code.
- Depending on the setup, Server performs authentication using any of the above mentioned authentication methods (see above bulleted list). 
- Once the Authentication is Successful, Server sends the response with "Authorization" attribute for Windows Integrated Authentication or **"Set-Cookie"** for Forms Authentication to the client browser. For the forms authentication, Session state is created for the user on the server-side. The Session state is associated with a SessionID.
- While accessing the resource/content, **Authorization check** is performed for the access rights. We check for either user roles, permissions, groups within the Active Directory or Custom Database depending on the setup.
- After **successful Authorization**, the user is presented with content/resource.
- When user triggers logout action or user is in-active for more than certain time (30mins), session cookie is expired and session state is **terminated** on the server-side.
- User is re-directed to logout/login page once the session is terminated.

## Federated Authentication (SSO)

Sometimes we need to redirect an authenticated user seamlessly to a different third-party application or access a third-party API. During these scenarios, we implemented **SAML authentication** simulating the Single Sign-On **(SSO)** functionality. Once the user is authenticated against Identity Provider, It generates a digitally signed XML document. This XML is exchanged while performing Single Sign-On with other applications.

## **Best practices of Session or Token Validation (Security Implications)**

- **Random generated** Session/Token should be stored on the Client browser
- Validation should be performed on **server-side** for each http request
- Session or Token should be terminated or cleared after **successful logout**.
- Session State or Token should be **expired** after certain time of inactivity (30mins)
- Session Cookie should be created with “Secure” and **“HttpOnly”** flags and restricted with **“path”** or **“domain”** flags.
- For JWT Tokens, **“Secret”** key should be protected.
- JWT Tokens should be generated with a reasonable **“exp”** (expiration time).
