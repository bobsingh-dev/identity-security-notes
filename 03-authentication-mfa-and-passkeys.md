# 🔐 Authentication, MFA, and Passkeys

> Authentication verifies who is requesting access.
> Authorization determines what they may do.

## 🪪 Authentication Factors

| Factor | Meaning | Examples |
|---|---|---|
| **Knowledge** | Something you know | Password or PIN |
| **Possession** | Something you have | Security key or authenticator device |
| **Inherence** | Something you are | Fingerprint or face recognition |

Two passwords belong to the same factor category.

## 🔑 Passwords

A password is a shared secret used to authenticate a user.

Applications should store passwords using a dedicated password
hashing algorithm with a unique salt for each password.

Common weaknesses include:

- Password reuse.
- Phishing.
- Credential stuffing using previously stolen credentials.
- Online guessing.
- Offline cracking after password hashes are stolen.

Rate limiting helps control online guessing.
Password hashing helps resist offline cracking.

## 📱 Multifactor Authentication

**Multifactor authentication (MFA)** combines distinct factor
categories.

| Combination | MFA? |
|---|---|
| Password + another password | No |
| Password + authenticator code | Yes |
| Password + security key | Yes |

MFA strength depends on the authentication method.

## 🧰 Common Methods

| Method | How it works | Main limitation |
|---|---|---|
| **SMS code** | Sends a code to a phone number | Vulnerable to phishing and phone-number takeover |
| **TOTP** | An authenticator generates time-based codes | Codes can be phished and relayed |
| **Push approval** | A device receives an approval request | Users may approve fraudulent requests |
| **FIDO security key** | A hardware authenticator proves possession of a private key | Requires device enrollment and recovery planning |
| **Passkey** | Uses a public-key credential associated with a service | Security also depends on device protection and recovery |

## 🗝️ Passkeys

A passkey uses a public and private key pair.

- The service stores the **public key**.
- The authenticator protects the **private key**.
- During login, the service sends a fresh challenge.
- The authenticator signs the challenge.
- The service verifies the signature using the public key.

The private key is not sent to the service.

A fingerprint, face scan, or device PIN can unlock use of the
credential. Biometric verification normally happens locally
on the device.

## 🛡️ Phishing Resistance

Passkeys use FIDO authentication, with credentials scoped to
the relying party.

The browser and authenticator enforce the service binding.
A lookalike website cannot simply collect a reusable passkey
secret.

Passwords and one-time codes can be entered into a fraudulent
website and relayed to the real service.

## ☁️ Synced and Device-Bound Passkeys

| Type | Characteristics |
|---|---|
| **Synced** | Available across devices through a credential provider |
| **Device-bound** | Remains on a specific authenticator, such as a security key |

Synced passkeys improve convenience and device recovery.
Device-bound credentials provide tighter control over where
the credential can be used.

A passkey can satisfy multiple authentication factors when
possession and local user verification are required.
Passwordless authentication does not automatically mean MFA.

## 🍪 Sessions and Reauthentication

After authentication, an application commonly creates a session.

The browser presents a session cookie on later requests.
The application checks the session to recognize the user.

A session has its own lifetime and security controls.

Sensitive operations may require fresh authentication or a
stronger authentication method. This is called **step-up
authentication**.

## ♻️ Enrollment and Recovery

Authentication security includes the entire credential lifecycle.

| Stage | Responsibility |
|---|---|
| **Enrollment** | Verify who may register an authenticator |
| **Use** | Apply the required authentication policy |
| **Replacement** | Securely register a replacement device |
| **Recovery** | Restore access when authenticators are lost |
| **Revocation** | Remove compromised or retired authenticators |

A weak recovery process can undermine strong authentication.

Recovery codes are credentials and must be protected.
Lost authenticators should be removed from the account.

## 🤖 Workload Authentication

Workloads commonly authenticate using:

- Short-lived access tokens.
- Certificates.
- Cloud-managed identities.
- Federated workload credentials.

Interactive MFA is primarily intended for people.
Workload authentication relies on securely establishing the
software's identity and controlling credential issuance.

## 🔎 Common Failures

| Failure | Consequence |
|---|---|
| **Weak recovery checks** | Attackers bypass strong login methods |
| **Unprotected enrollment** | Attackers register their own authenticator |
| **Repeated push prompts** | Users approve an attacker’s login |
| **Stolen session cookie** | Attackers use an already authenticated session |
| **Unrevoked lost device** | An old authenticator remains usable |
| **Missing reauthentication** | Sensitive changes rely on an old session |

## 🧠 Main Takeaways

- **Authentication factors** are knowledge, possession, and inherence.
- **MFA** combines distinct factor categories.
- **Passkeys** authenticate through public-key cryptography.
- **Phishing resistance** depends on the authentication method.
- **Sessions** need protection after login.
- **Enrollment, recovery, and revocation** are part of authentication security.
- **Workloads** need authentication mechanisms suited to automated operation.
