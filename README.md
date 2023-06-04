# Vault & Monk

This repository contains Monk.io template to deploy Vault either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Prerequisites

- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

## Make sure monkd is running

```bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

## Clone Repository

```bash
git clone https://github.com/monk-io/vault
```

## Load Template

```bash
cd vault
monk load MANIFEST
```

```bash
foo@bar:~$ monk list vault
✔ Got the list
Type      Template          Repository  Version  Tags
runnable  vault/vault  local       -        -

```

## Deploy Stack

```bash
foo@bar:~$ monk run vault/vault
? Select tag to run [local/vault/vault] on: mnk
✔ Starting the job: local/vault/vault... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% vault:latest mnk-1
✔ Checking/pulling images DONE
✔ Started local/vault/vault

🔩 templates/local/vault/vault
 └─🧊 Peer mnk-1
    └─🔩 templates/local/vault/vault
       └─📦 74a4ab0f23148a6308353df53d2bd468-al-vault-vault-vault
          ├─🧩 vault:latest
          └─🔌 open 16.171.45.206:8201 (0.0.0.0:8201) -> 8201

💡 You can inspect and manage your above stack with these commands:
        monk logs (-f) local/vault/vault - Inspect logs
        monk shell     local/vault/vault - Connect to the container's shell
        monk do        local/vault/vault/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Show Root Token

```bash
foo@bar:~$ monk logs 74a4ab0f23148a6308353df53d2bd468-al-vault-vault-vault
.....
.....
Unseal Key: 2aI4xUumrgaIJrCI8HBDbj1qGgYgXFa9kPcRenu6e6A=
Root Token: hvs.7DO7Sf7C92NL7BTCalm9Nu4Y
.....
.....

```

## Variables

The variables are in `vault.yml` file. You can quickly setup by editing the values here.

| Variable         | Description               |
| ---------------- | ------------------------- |
| monk_vault_port  | Vault Port, Default: 8201 |
| monk_image_tag   | Image tag, Default latest |
| monk_skip_setcap | Set cap                   |
| monk_skip_chown  | set chown                 |

## Unlock Vault

### unlock

```bash
foo@bar:~$ monk monk do templates/local/vault/vault
✔ Get templates/local/vault/vault actions list success
? Action unlock
✔ Got action parameters
✔ Parse parameters success
✔ Running action:
+ vault operator init
+ grep 'Unseal Key 1:' /vault/file/keys
+ awk '{print $NF}'
+ vault operator unseal SpVzTV5ltimEtGCjt7sNqKfQMcKe0zZuL6E4Uee3/ly8
Key                Value
---                -----
Seal Type          shamir
Initialized        true
Sealed             true
Total Shares       5
Threshold          3
Unseal Progress    1/3
Unseal Nonce       d950d940-b5a8-ac63-1740-c01ea1c50d54
Version            1.11.3
Build Date         2022-08-26T10:27:10Z
Storage Type       file
HA Enabled         false
+ grep 'Unseal Key 2:' /vault/file/keys
+ awk '{print $NF}'
+ vault operator unseal MYI4xZqPdWGJDxBOiJCtdyEI1fMOUAFLsh1EIYyz+u29
Key                Value
---                -----
Seal Type          shamir
Initialized        true
Sealed             true
Total Shares       5
Threshold          3
Unseal Progress    2/3
Unseal Nonce       d950d940-b5a8-ac63-1740-c01ea1c50d54
Version            1.11.3
Build Date         2022-08-26T10:27:10Z
Storage Type       file
HA Enabled         false
+ grep 'Unseal Key 3:' /vault/file/keys
+ awk '{print $NF}'
+ vault operator unseal gJbcnyTGIKMSgdILSyQGZlMNjAk6kpLo6uti7FsMJCZu
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    5
Threshold       3
Version         1.11.3
Build Date      2022-08-26T10:27:10Z
Storage Type    file
Cluster Name    vault-cluster-a7624cc2
Cluster ID      2495d693-5e4f-1756-58f4-09c8bf4adc39
HA Enabled      false
+ grep 'Initial Root Token:' /vault/file/keys
+ awk '{print $NF}'
+ export 'ROOT_TOKEN=hvs.JEWpADvRspp4go5IFTZ38re8'
+ vault login hvs.JEWpADvRspp4go5IFTZ38re8
Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                  Value
---                  -----
token                hvs.JEWpADvRspp4go5IFTZ38re8
token_accessor       GhP2MH9YECBFgXMda62IsnMp
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
✨ Took: 2s
```

## Show Token Vault

### show_token

```bash
foo@bar:~$ monk do templates/local/vault/vault
⠴ Get templates/local/vault/vault actions list starting...
✔ Get templates/local/vault/vault actions list success
? Action show_token
✔ Got action parameters
✔ Parse parameters success
✔ Running action:
+ awk '{print $NF}'
+ grep 'Initial Root Token:' /vault/file/keys
+ echo hvs.JEWpADvRspp4go5IFTZ38re8
hvs.JEWpADvRspp4go5IFTZ38re8
✨ Took: 2s
```

This token value is hvs.JEWpADvRspp4go5IFTZ38re8, we'll use that for login

## Stop, remove and clean up workloads and templates

```bash
monk purge -a
```
