# Users, Service Accounts, and Workloads

## Identity Types

| Type | Definition | Example |
|---|---|---|
| User identity | Represents a person. | An employee account |
| Service account | A non-human account that software uses to access resources. | A document-processing account |
| Workload | Running software that performs tasks. | An API, container, or background worker |
| Workload identity | The identity a workload uses to authenticate to another system. | An identity assigned to a worker |

A workload can use a service account as its identity.
Multiple instances of the same workload can share an identity.

## Related Terms

| Term | Definition |
|---|---|
| Principal | An entity recognized by a system that can receive permissions. |
| Credential | Evidence used to authenticate, such as a password, token, or certificate. |
| Authentication | Verifying the caller's identity. |
| Authorization | Deciding whether the caller may perform an action on a resource. |

## Request Flow

Alice requests a document summary. An API queues the job.
A worker reads the document and calls a model.

| Request | Acting identity |
|---|---|
| Submit the summary request | Alice |
| Enqueue the job | API identity |
| Read the document | Worker identity |
| Call the model | Worker identity |

Alice initiates the operation. The worker performs the background requests.

User authorization determines whether Alice may request a summary
of the document. Workload authorization determines whether the worker
may access the required services.

The job must also be restricted to its approved document.

## Identity, Credential, and Process Lifetimes

| Component | Lifetime |
|---|---|
| Identity | Exists until it is retired or deleted. |
| Credential | Remains usable until expiration, revocation, or another invalidating event. |
| Process | Runs until it exits or is stopped. |

Stopping a process does not automatically retire its identity
or invalidate its credentials.

A workload may obtain a new credential after an existing one expires
if it remains authorized to do so.

## Identity Separation

Use separate identities for workloads with different permission needs.

An API that submits jobs and a worker that reads documents
can have separate identities and permissions.

Replicas performing the same function can share an identity.
Instance identifiers in logs help distinguish individual replicas.

## Lifecycle

1. Create the identity and assign an owner.
2. Grant the permissions required for its purpose.
3. Establish how it authenticates and obtains credentials.
4. Review its access as responsibilities change.
5. Retire the identity and remove access when it is no longer needed.

Identity retirement must account for credentials already issued
and any dependent workloads.
