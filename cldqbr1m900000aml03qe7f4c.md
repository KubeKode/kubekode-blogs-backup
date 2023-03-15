---
title: "What is Hashicorp Vault? Manage your secrets in production."
datePublished: Sat Feb 04 2023 19:04:34 GMT+0000 (Coordinated Universal Time)
cuid: cldqbr1m900000aml03qe7f4c
slug: what-is-hashicorp-vault-manage-your-secrets-in-production
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1675537422896/78a0e5d5-e6dd-450f-b6b6-2d737c23127d.jpeg
tags: security, devops, hacking, hashnode, hashicorp-vault

---

## What is hashicorp vault?

* It's a cloud-agnostic secrets management system.
    
* API-driven
    
* It allows you to safely store and manage sensitive data in hybrid cloud environments
    
* Used to generate dynamic short-lived credentials, or encrypt application data on the fly.
    
    ## What is a secret?
    
* Usernames and passwords
    
* Certificates
    
* SSH Keys
    
* API keys
    
* Encryption keys
    
    * ## What problems does vault solve?
        
    * Secrets sprawl
        
        * Below are some common places where secrets get stored:
            
            * On a developer's computer in excel or notepad files
                
            * Hard-coded into the source code.
                
            * On a sticky note under an engineer's keyboard
                
            * In a version control system such as GitHub and sometimes exposed publicly.
                
        
        ## Vault use cases
        
        1. Secrets management
            
            * Centrally store, access, and distribute secrets.
                
        2. Encrypting application data
            
            * Keep application data secure with centralized key management
                
        3. Identity-based Access
            
            * Authenticate and access different clouds, systems, and endpoints using trusted identities.
                

* ## Secret management
    
    ### KV Secrets Engine
    
* The idea is to share between the client and the vault. (A client could be a person, user, or application)
    
* The client makes a call to the vault with a specific path(In the vault everything is path based)
    
* Vault checks its policy for authorizing the client to share the secrets.
    

* ## Encrypting Application Data
    
* Vault provides an EAAS(Encryption as a service), also called a transit secrets engine inside the vault.
    
* So after encrypting the application data using a vault, now the web server can use or store that data in a database for the next use cases.
    
* Vault will not store data, only pass it back to requesting client.
    

# Basic Vault CLI commands

* Vault by itself will give you a list of many Vault CLI commands.(starts with common ones)
    
    ```bash
    $ vault
    $ vault version # tells the version of vault
    $ vault read # used to read secrets from vault
    $ vault write #used to write secrets to vault
    $ vault write -h # -h, -help and --help flags can be added to get help for any vault CLI command.
    ```
    
* ## Vault Server Modes
    
* Vault servers can be run in two different modes:
    
    1. Dev mode that is only intended for development.
        
        * It's not secure
            
        * It stores everything in memory
            
        * The vault is automatically unsealed
            
        * The root token can be specified before launching
            
            ```bash
            $ vault server -dev -h # for help on this command
            $ vault server -dev -dev-root-token-id=root
            # checking the status of our vault server
            $ vault status
            ```
            
            > Never store actual secrets on a server run in "Dev" mode.
            
    2. Prod mode that can be used in QA and production
        

* ## Interacting with vault
    
    ```bash
    $ vault kv -h
    ```
    
* Adding secrets:
    
    ```bash
    $ vault kv put secret/foo bar=baz
    ```
    
* updating the secret
    
    ```bash
    $ vault kv put secret/foo bar=baz hello=world
    # now you will see version=2
    ```
    
* Reading the secret
    
    ```bash
    $ vault kv get secret/foo
    # we will get the latest version
    ```
    
* Deleting a secret
    
    ```bash
    $ vault kv delete secret/foo
    # secret latest version will be deleted for specific version deletion use this flag
    $ vault kv delete -versions=2 secret/foo
    # for storing back our secret use this command
    $ vault kv undelete -versions=2 secret/foo
    ```
    
* Destroying the secret permanently
    
    ```bash
    $ vault kv destroy -versions=2 secret/foo
    ```
    
    ## Seal and unseal concept
    
    * Vault starts up in a sealed state.
        
    * Vault can access the physical storage, but can't decrypt it.
        
    * No operations are possible with the vault except to unseal the vault and check its status of the vault.
        
    * Vault data is encrypted using the encryption key in the keyring; the keyring is encrypted by the master key; and the master key is encrypted by the unsealed key.
        

---