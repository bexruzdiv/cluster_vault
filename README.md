## What is Vault?    
__Vault__, developed by __HashiCorp__, functions as an identity-centric platform for managing secrets and encryption. It provides encryption services that are gated by authentication and authorization methods to ensure secure, auditable and restricted access to secrets. It is used to secure, store and protect secrets and other sensitive data using a __UI__, __CLI__, or __HTTP API__. A secret is anything important that you want to keep safe, like passwords, keys, or certificates. Vault makes it easy to manage all these secrets securely, controlling who can access them and keeping track of who does. [MORE](https://developer.hashicorp.com/vault/docs/what-is-vault)

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

## Why multiNode Vault Cluster?
1. __High Availability:__ With multiple nodes, Vault can continue to operate even if one or more nodes fail. This ensures that critical services relying on Vault can continue running without interruption.
2. __Scalability:__ As the demand for secrets management grows, a multi-node cluster allows for horizontal scaling by adding more nodes to distribute the workload and handle increased traffic.
3. __Fault Tolerance:__ Multi-node clusters provide fault tolerance by replicating data across nodes. In case of node failure, data can be seamlessly retrieved from other nodes, preventing data loss or service disruptions.
4. __Load Balancing:__ A cluster enables load balancing of client requests across multiple nodes, improving performance and resource utilization, reducing the risk of single points of failure and enhancing overall system reliability.

## How to use ansible roles?
__You just need to change the variables in the defaults/main.yml file inside the roles to use these ansible roles!__
## Vault role
__This Ansible role performs the following tasks:__
1. Installs Vault on all servers.
2. Configures Vault according to specified settings.
3. Starts Vault using systemd.
4. Establishes a cluster with three nodes (the roles are adapted to a cluster of 3 nodes)
5. Enables interconnection of Raft storage among the nodes.

### Required of you! Change variables in the defaults/main.yml
Change IP address with your`s
```
vault_server_1:   # * leader ip address
vault_server_2:   # * follower 1 ip address
vault_server_3:   # * follower 2 ip address
```

This allows you cluster metrics to `/v1/sys/metrics` path. You can use it with `true`. It allows you to build a monitor (via Prometheus and Grafana)
```
prometheus: true
```



Save all unseal key and token in leader server `/root/key`. You can use it with `true`
```
vault_save_unseal_file: true
```

Auto unseal when vault restarted. You can customize this by changing the value to `true`. By doing this, a bash scritp is generated and stored in `/usr/local/bin/unseal_vault.sh` This will automatically unseal the vault in systemd when it restarts  Warnig!!! Unseal keys can be dangerous to store. This may give others access to the Vault! 

> [!WARNING]
> Unseal keys can be dangerous to store. This may give others access to the Vault! 

```
vault_auto_unseal: true
```
#### Correct use of the inventory file is also very important!
```
leader
follower1
follower2
```
These names must be used in hostnames. This is used during the task!

## Nginx LoadBalancer role
__This Ansible role performs the following tasks:__
1. Install Certbot and Nginx
2. Configuration Nginx for Vault Cluster LoadBalancer
3. Gets a certificate for secury!

### Required of you!
1. Match your domain name to loadbalancer's IP
2. Change variables in the defaults/main.yml

![image](https://github.com/bexruzdiv/cluster_vault/assets/107495220/ee1b2885-21f3-4942-a79d-ee2913b6b85f)

  - Change gmail to your`s
  - Use Domain which you set for loadbalancer server`s IP
  - Write down the IP addresses of the vault cluster servers (the roles are adapted to a cluster of 3 nodes)


#### Correct use of the inventory file is also very important!
```
loadbalancer
```
These names must be used in hostnames. This is used during the task!
> [!TIP]
> You can run Vault and nginx loadbalancer roles at the same time!

## Vault Backup role
__This Ansible role performs the following tasks:__
1.  Install vault CLI
2.  Backup at any time (through the rack operator)
3.  Set Cronjob for backup

### Required of you!
Change variables in the defaults/main.yml

![image](https://github.com/bexruzdiv/cluster_vault/assets/107495220/82dfaadc-9019-4f30-a35b-019378571186)
  -  Create and specify a token from Vault for backup 
  -  Specify the address of your Vault cluster
  -  Edit Path for save backups
  -   If the value of `backup_cronjob` is `true`. A cronjob is set to take a backup every night at 00:00


