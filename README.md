# What is Vault?    
__Vault__, developed by __HashiCorp__, functions as an identity-centric platform for managing secrets and encryption. It provides encryption services that are gated by authentication and authorization methods to ensure secure, auditable and restricted access to secrets. It is used to secure, store and protect secrets and other sensitive data using a __UI__, __CLI__, or __HTTP API__. A secret is anything important that you want to keep safe, like passwords, keys, or certificates. Vault makes it easy to manage all these secrets securely, controlling who can access them and keeping track of who does.

## How it does work?
Vault operates by using tokens, which are linked to client policies. These policies determine what actions and paths a client can access. Tokens can be manually created and assigned to clients, or clients can obtain them by logging in. The main steps in Vault's workflow are:

- `Authenticate: Clients prove their identity to Vault, which then generates a token linked to their policy`
- `Validation: Vault validates the client against third-party trusted sources, such as Github`
- `Access: Vault grants access to secrets, keys, and encryption capabilities by issuing a token based on policies associated with the client’s identity. The client can then use their Vault token for future operations`

## Why Vault?


1. Many companies nowadays face a challenge known as "credentials sprawl." This means that passwords, API keys, and other credentials are scattered throughout their systems, stored in various places like plain text, application source code, configuration files, and more. This widespread distribution of credentials makes it hard to keep track of who has access to what. Moreover, storing credentials in plain text poses a significant security risk, leaving companies vulnerable to both internal and external threats.
2. Vault offers secure secret storage by allowing the storage of arbitrary key/value secrets. Before writing these secrets to persistent storage, Vault encrypts them. This means that even if someone gains access to the raw storage, they won't be able to access your secrets without the proper authorization.
3. Leasing and Renewal: All secrets in Vault have a lease associated with them. At the end of the lease, Vault will automatically revoke that secret. Clients are able to renew leases via built-in renew APIs.

## Why Raft storage as backend?
1. __Reduced Configuration:__ With Raft backend, there's no need to configure Vault to connect to external providers as a client, which eliminates additional setup steps and potential points of failure.
2. __Enhanced Security:__ Since Raft is an integral part of Vault, it allows for tighter integration and control over security measures. This can lead to improved security posture as there are fewer external dependencies and potential attack vectors.
3. __Performance:__ Raft backend can offer improved performance compared to external storage systems, as it operates within the same infrastructure as Vault itself, reducing latency and overhead associated with external network communication.