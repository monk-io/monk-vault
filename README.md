# Tensorflow & Monk
This repository contains Monk.io template to deploy Vault either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

# Prerequisites
- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

#### Make sure monkd is running.
```bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

## Clone Repository
```bash
git clone https://github.com/Burakhan/monk-vault
```

## Load Template
```bash
cd monk-vault
monk load MANIFEST
```


#### Let's take a look at the themes I have installed.
```bash
foo@bar:~$ monk list monk-vault
✔ Got the list
Type      Template          Repository  Version  Tags
runnable  monk-vault/vault  local       -        -

```

## Deploy Stack
```bash
foo@bar:~$ monk run monk-vault/vault
? Select tag to run [local/monk-vault/vault] on: mnk
✔ Starting the job: local/monk-vault/vault... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% vault:latest mnk-1
✔ Checking/pulling images DONE
✔ Started local/monk-vault/vault

🔩 templates/local/monk-vault/vault
 └─🧊 Peer mnk-1
    └─🔩 templates/local/monk-vault/vault
       └─📦 74a4ab0f23148a6308353df53d2bd468-al-monk-vault-vault-monk-vault
          ├─🧩 vault:latest
          └─🔌 open 16.171.45.206:8201 (0.0.0.0:8201) -> 8201

💡 You can inspect and manage your above stack with these commands:
        monk logs (-f) local/monk-vault/vault - Inspect logs
        monk shell     local/monk-vault/vault - Connect to the container's shell
        monk do        local/monk-vault/vault/action_name - Run defined action (if exists)
💡 Check monk help for more!
```
## Show Root Token
```bash
foo@bar:~$ monk logs 74a4ab0f23148a6308353df53d2bd468-al-monk-vault-vault-monk-vault
.....
.....
Unseal Key: 2aI4xUumrgaIJrCI8HBDbj1qGgYgXFa9kPcRenu6e6A=
Root Token: hvs.7DO7Sf7C92NL7BTCalm9Nu4Y
.....
.....

```

## Variables
The variables are in `vault.yml` file. You can quickly setup by editing the values here.

| Variable                     	| Description                               	|
|------------------------------	|-------------------------------------------	|
| monk_vault_port               | Vault Port, Default: 8201 	               |
| monk_image_tag             	| Image tag, Default latest                     	|
| monk_skip_setcap             	| Set cap                      	|
| monk_skip_chown             	| set chown                     	|


## Stop, remove and clean up workloads and templates

```bash
monk purge -x -a
```