# 🔐 Users, Service Accounts, and Workloads

> Identity defines who is acting. Authentication verifies that identity.
> Authorization determines what the identity can do.

## 👤 Identity Types

| Type | Meaning | Example |
|---|---|---|
| **User identity** | Represents a person. | An employee account |
| **Service account** | A non-human account used by software. | A document-processing account |
| **Workload** | Running software that performs tasks. | An API, container, or background worker |
| **Workload identity** | The identity a workload uses to authenticate. | An identity assigned to a worker |

A workload can use a service account as its identity. Multiple
instances of the same workload can share that identity while
using separate credentials.

## 🗝️ Core Terms

| Term | Meaning |
|---|---|
| **Principal** | An entity recognized by a system that can receive permissions. |
| **Credential** | Evidence used to authenticate, such as a password, token, or certificate. |
| **Authentication** | Verifying the caller’s identity. |
| **Authorization** | Deciding whether the caller may perform an action on a resource. |

## 🤖 Example · AI Document Summarization

Alice requests a document summary. The API queues a job.
A background worker reads the document and calls a model.

| Action | Acting identity | Access required |
|---|---|---|
| Request a summary | **Alice** | Request a summary of the selected document |
| Enqueue the job | **API identity** | Submit jobs to the queue |
| Read the document | **Worker identity** | Read the approved job input |
| Call the model | **Worker identity** | Invoke the model endpoint |

**Alice initiates the job. The worker executes it.**

Three authorization decisions matter:

- **User access** — Can Alice request a summary of this document?
- **Workload access** — Can the worker access the required services?
- **Job scope** — Is this the document approved for this job?

## ⏳ Separate Lifetimes

An identity, its credentials, and its running processes have
separate lifetimes.

| Component | Lifetime |
|---|---|
| **Identity** | Exists until retired or deleted. Its access can change during that time. |
| **Credential** | Remains usable until it expires or is otherwise invalidated. |
| **Process** | Runs until it exits or is stopped. |

- Stopping a worker does not automatically invalidate its credentials.
- Expiring a credential does not automatically retire the identity.
- An authorized workload may obtain fresh credentials.
- Disabling a user does not automatically determine what happens
  to jobs they previously submitted.

## 🧩 Identity Separation

Assign separate identities to workloads with different
permission requirements.

| Workload | Example permissions |
|---|---|
| **API** | Submit jobs to a queue |
| **Worker** | Read approved inputs and invoke a model |

Sharing an identity combines the permissions available to both
workloads and makes attribution harder.

Replicas performing the same function can share an identity.
Include an instance identifier in logs to distinguish them.

## ♻️ Identity Lifecycle

| Stage | Responsibilities |
|---|---|
| **Create** | Define the identity’s purpose and assign an owner. |
| **Grant access** | Assign the permissions required for its function. |
| **Authenticate** | Establish how it proves its identity and obtains credentials. |
| **Maintain** | Review permissions, ownership, and credential handling. |
| **Retire** | Remove access, prevent new credential issuance, and account for existing credentials. |

Retirement also requires checking which applications and
workloads depend on the identity.

## 🔎 Audit Context

Record enough context to connect an action to its identity,
initiator, and purpose.

| Field | Purpose |
|---|---|
| **Principal** | Identifies who executed the action |
| **Initiating user** | Identifies who requested the operation |
| **Job ID** | Connects actions belonging to the same job |
| **Resource** | Identifies what was accessed |
| **Action and outcome** | Records what was attempted and whether it succeeded |
| **Timestamp** | Establishes when the action occurred |
