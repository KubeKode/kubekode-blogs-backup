---
title: "Having problems working with the primary and another GitHub account on the same machine."
datePublished: Tue Aug 01 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clmf80c5g000108ii2i0s4ueu
slug: having-problems-working-with-the-primary-and-another-github-account-on-the-same-machine
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/HLQDfaJUTVI/upload/075d433a7a6b6e7f830af59da56dd353.jpeg
tags: github, error-handling, ssh-keys, ssh-git, ssh-keygen

---

Sometimes when we are working with multiple accounts on the same machine with GitHub we get errors where we are authorized to push and pull from a remote repository and are stuck with notifications like:

### <mark>You are not the authorized author of this repository please create a fork from the GitHub branch.</mark>

---

Let's make it easy in just 5-6 simple and easy steps, follow each step one by one.

Suppose you are logged in with your primary account, and also have a client GitHub account from which you need to work with them. You don't need to signOut your primary account simply open your command palette like cmd, bash, and any editor.

* **Step 1: Generate and setup SSH Key :**
    
    Please ensure that first you have SSH keys for both accounts, if you don't have an SSH key for the client account, first we need to generate.
    
    ```bash
    ssh-keygen -t rsa -b 4096 -C "client@example.com"
    ```
    
    Describe given above bash command line:
    
    **ssh-keygen:** used to generate SSH (Secure Shell) key pairs, which consist of a public key and a private key. These keys are used for secure authentication and communication between your computer and remote servers.
    
    **\-t rsa:** This option specifies the type of key to **be** **created.** **The** **commonly** used asymmetric encryption **algorithm** **in** **this** **case** **is** **RSA** **(Rivest-Shamir-Adleman).**
    
    **\-b 4096:** This option sets the number of bits within the key. A longer key gives more prominent security but may take longer to create **(a** 4096-bit key is very secure and is commonly used for SSH).
    
    **\-c "**[**client@example.com**](mailto:your-email@example.com)**":** It's typically used to associate the key with an email address. In this case, you're associating the key with the email address "[**client@example.com**](mailto:your-email@example.com)."
    
* **Step 2: Adding SSH Key to SSH Agent:**
    
    Firstly, we need to start the SSH agent
    
    ```bash
    eval "$(ssh-agent -s)"
    ```
    
    Describe given above bash command line:
    
    **eval:** command is used to evaluate the commands produced by `ssh-agent -s.`
    
    **ssh-agent:** responsible for managing SSH keys and facilitating secure SSH connections.
    
    **\-s:** that need to be evaluated by the shell to set up the agent environment.
    
    After starting SSH we need to add the client key to the agent:
    
    ```bash
    ssh-add ~/.ssh/your-client-ssh-key
    ```
    
* **Step 3: configure ssh key to Github :**
    
    We can use any of them like Vim, nano any other bash editor. Open the editor and create a plaintext file -
    
    ```bash
    nano ~/.ssh/config
    ```
    
    Describe given above bash command line:
    
    **nano:** It is a text Editor.
    
    **~:** Symbol represents your home directory.
    
    **.ssh/config:** Path of sub-directory where key is stored.
    
    ```plaintext
    Host github.com-client
      HostName github.com
      User git
      IdentityFile ~/.ssh/your-client-ssh-key
    ```
    
* **Step 4: Configuration of Git**
    
    Open the bash editor and run these commands for the configuration your SSH key to GitHub
    
    ```bash
    git config --global user.name "Your Client's Username"
    git config --global user.email "client@example.com"
    git config --global --replace-all url."git@github.com:".insteadOf "https://github.com/"
    ```
    
* **Step 5: Work with Repositories**:
    
    clone or create a new Git repository, always **remember** to use **a** custom **name** when specifying the repository URL.
    
    ```scss
    git@github.com-client:client-username/client-repo.git
    ```
    

If you want to test your configuration use this command.

```bash
ssh -T git@github.com-client
```

---

I hope this blog finds something knowledgeable for you.

<center>🙌🙌🙌🙌Thank You for reading. 🙌🙌🙌🙌</center>