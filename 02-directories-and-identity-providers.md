# 📚 Directories and Identity Providers

> A directory stores identity information. An identity provider
> authenticates users and supplies trusted identity information
> to applications.


## 🗂️ Core Concepts

| Concept | Purpose | Example |
|---|---|---|
| **Directory** | Stores identities, groups, and attributes. | Employee accounts and departments |
| **Identity provider (IdP)** | Authenticates users and supplies identity information to applications. | Signing in through Keycloak |
| **Application** | Uses identity information and enforces access decisions. | A FastAPI application |

A product can provide both directory and identity-provider capabilities.


## 🔌 LDAP

**Lightweight Directory Access Protocol** is a protocol for
accessing directory services.

Common operations include:

- Searching for users.
- Reading identity attributes.
- Looking up group memberships.
- Authenticating through an LDAP bind.

A directory server implements LDAP.


## 🏢 Active Directory

**Active Directory Domain Services (AD DS)** is Microsoft’s
directory service for Windows domains.

It manages:

- User and computer accounts.
- Groups and domain membership.
- Authentication, commonly through Kerberos.
- Centralized configuration through Group Policy.

Applications can query Active Directory through LDAP.


## ☁️ Microsoft Entra ID

**Microsoft Entra ID**, formerly Azure Active Directory, is
Microsoft’s cloud identity service.

It supports:

- User, group, and application identities.
- Single sign-on.
- Multifactor authentication.
- Conditional Access.
- Application authentication and federation.
- Managed identities for supported Azure resources.

AD DS and Entra ID have different architectures and capabilities.
Organizations can integrate them for hybrid identity.


## 🔐 Keycloak

**Keycloak** is an open-source identity and access management platform.

It provides:

- Authentication and single sign-on.
- OpenID Connect and SAML support.
- Multifactor authentication.
- Connections to LDAP and Active Directory.
- Integration with external identity providers.

| Term | Meaning |
|---|---|
| **Realm** | An isolated collection of users, clients, roles, and authentication configuration. |
| **Client** | An application or service registered with Keycloak. |


## 🟦 Okta

**Okta** is a managed identity platform.

It provides capabilities for:

- Single sign-on.
- Multifactor authentication.
- Application integrations.
- User provisioning and deprovisioning.
- Connections to existing directories.

Organizations commonly use it to manage employee access
across applications.


## 🔄 How They Connect

An organization might use the following arrangement:

| Component | Responsibility |
|---|---|
| **Active Directory** | Store employee accounts and groups |
| **Keycloak or Okta** | Connect to the directory and authenticate users |
| **Application** | Accept trusted identity information and enforce permissions |

An identity provider can also maintain users in its own identity store.


## 🚪 Single Sign-On

**Single sign-on (SSO)** lets users access connected applications
through an existing identity-provider session, subject to
authentication policies.

Each application still needs to determine what the user may access.

| Function | Question |
|---|---|
| **Authentication** | Who is this user? |
| **Authorization** | What may this user do? |
| **Provisioning** | Does this user have an account in this application? |


## ♻️ Provisioning and Deprovisioning

Creating or disabling an identity does not automatically update
every connected application.

Lifecycle integration can involve:

- Directory synchronization.
- Just-in-time account creation during login.
- SCIM provisioning.
- Application-specific APIs.

**SCIM** is a standard protocol for managing identity records
across systems, including creating, updating, and deactivating users.

Disabling an account also requires considering existing
application sessions and previously issued tokens.


## 🔎 Common Failures

| Failure | Effect |
|---|---|
| **Incorrect group mapping** | Users receive incorrect application roles. |
| **Delayed synchronization** | Identity changes take time to reach connected systems. |
| **Incomplete deprovisioning** | Former users retain application access. |
| **Incorrect federation configuration** | Login fails or identity information is rejected. |
| **Active sessions remain** | Access can continue after an account is disabled. |


## 🧠 Main Takeaways

- **Directories** store identities, groups, and attributes.
- **Identity providers** authenticate users and supply trusted identity information.
- **LDAP** enables communication with directory services.
- **Applications** enforce access decisions.
- **Authentication, authorization, and provisioning** serve different purposes.
- **Account changes** must propagate across connected systems.
