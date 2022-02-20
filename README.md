# Microservices.Infrastructure.Secrets
```
Manage secrets with the Vault and the Consul datacenter backend.
```

## Start infrastructure secrets
- docker-compose up -d --build



## HASHICORP VAULT
### Initialize and Unseal
- docker-compose exec vault-master bash

### Initialize Vault
- vault operator init

```
After execute the command will get:
Unseal Key 1 - 5
- unseal_key_1
- unseal_key_2
- unseal_key_3
- unseal_key_4
- unseal_key_5

Initial Root Token:
- initial_root_token
```

Additional notification after initialization:
```
Vault initialized with 5 key shares and a key threshold of 3. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 3 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated master key. Without at least 3 key to
reconstruct the master key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.
```

### Unseal the Vault
```
Now we can unseal Vault using three of the keys.
```
- vault operator unseal unseal_key_1
- vault operator unseal unseal_key_2
- vault operator unseal unseal_key_3
```
Every time when we restart the container we have to unseal the Vault using three of five keys.
```

### First login to Vault
```
Let's authenticate using the root token.
```
- vault login initial_root_token

### Auditing
#### Detailed log
- vault audit enable file file_path=/vault/logs/audit.log

#### View all enabled audit devices
- vault audit list

#### Docker logs
- docker logs container_vault_name

### Vault WEB Login
```
Vault is available in the browser on the port number: 8200
Address: http://server-address:8200
```

## HASHICORP CONSUL
```
We can use the Consul datacenter as KV storage Vault backend.
Key features:
	Service Discovery
	Health Checking
	KV Store
	Multi Datacenter
```

### Run Consul bash
- docker-compose exec consul-master-datacenter1 bash

or

- docker exec -it consul-master-datacenter1 ash

### Encrypt data
```
We can generate a new encrypt password (32 bytes, base64) with the Consul command.
Source: https://www.consul.io/docs/security/encryption
```
- consul keygen

### Consul Configure TLS Connection
```
Generate certificates for datacenter.
Source:
https://learn.hashicorp.com/tutorials/consul/tls-encryption-secure
https://learn.hashicorp.com/tutorials/consul/tls-encryption-openssl-secure#create-certificates
```
- consul tls ca create
- consul tls cert create -server -dc datacenter-dc1

### Consul WEB Login
```
Consul is available in the browser on the port number: 8500
Address: http://server-address:8500
```