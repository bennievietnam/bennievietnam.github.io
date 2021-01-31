---
title: Using multiple Github accounts with SSH keys
parent: Test
has_children: false
nav_order: 1
---

# Using multiple Github accounts with SSH keys

## Problem
I have several Github accounts. I want to use all of them on the same computer without typing password everytime I pull or push code.

## Solution
Use SSH keys and define host aliases for each account in ssh config file

## How to?
1. [Generate ssh key pairs for accounts](https://help.github.com/articles/generating-a-new-ssh-key/) and [add them to GitHub accounts](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/).
2. Create/Edit ssh config file (`~/.ssh/config`):

   ```conf
   # Default github account
   Host github.com
      HostName github.com
      IdentityFile ~/.ssh/bennie_work_private_key
      IdentitiesOnly yes
      
   # Another github account
   Host github-personal
      HostName github.com
      IdentityFile ~/.ssh/bennie_personal_private_key
      IdentitiesOnly yes
   ```

3. [Add ssh private keys to your agent](https://help.github.com/articles/adding-a-new-ssh-key-to-the-ssh-agent/):

   ```shell
   $ ssh-add ~/.ssh/bennie_work_private_key
   $ ssh-add ~/.ssh/bennie_personal_private_key
   ```

4. Test your connection

   ```shell
   $ ssh -T git@github.com
   $ ssh -T git@github-personal
   ```

   With each command, you may see this kind of warning, type `yes`:

   ```shell
   The authenticity of host 'github.com (192.30.252.1)' can't be established.
   RSA key fingerprint is xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:
   Are you sure you want to continue connecting (yes/no)?
   ```

   If everything is OK, you will see these messages:

   ```shell
   Hi bennie_work! You've successfully authenticated, but GitHub does not provide shell access.
   ```
   
   ```shell
   Hi bennie_personal! You've successfully authenticated, but GitHub does not provide shell access.
   ```

5. Now all are set, just clone your repositories with corresponding aliases

   ```shell
   # clone the work repo
   $ git clone git@github.com:bennie_work/project.git /path/to/project
   $ cd /path/to/project
   $ git config user.email "bennie@work.com"
   $ git config user.name  "Bennie Nguyen"

   # clone the personal repo
   $ git clone git@github-personal:bennie_personal/project.git /path/to/project
   $ cd /path/to/project
   $ git config user.email "bennie@personal.com"
   $ git config user.name  "Bennie Nguyen"
   ```

Done! Goodluck!