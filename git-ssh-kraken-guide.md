# `git-ssh-kraken` starters guide

## Intro

If you need to work with multiple SSH keys, this is a good opportunity to see an overview on how SSH config and agents work from a user's perspective. Kraken is used as example, because it's popularity is skyrocketing recently.

## General

- Collect your key files in your `~/.ssh` folder, prefixed with `id_rsa_` for consistency
- Create a `~/.ssh/config` file once, and list all your domains there, linking them to the corresponding files:
```
Host *
    AddKeysToAgent yes
    UseKeychain yes                            # macOS only, otherwise remove it

# e.g.:

Host github.com
    IdentityFile ~/.ssh/id_rsa_github

Host bitbucket.company1.com
    IdentityFile ~/.ssh/id_rsa_company1_bitbucket

Host gitlab.company2.com
    IdentityFile ~/.ssh/id_rsa_company2_gitlab

# etc...
```

- Note that if you only have a single key, you can just do the following below and stop — CLI will work and Kraken supports configuring single SSH keys with no further effort.
```
Host *
    AddKeysToAgent yes
    UseKeychain yes
    IdentityFile ~/.ssh/id_rsa_master
```

## Create keys

- `ssh-keygen -t rsa -b 4096 -C "some_comment"`
- When naming it, either enter the full path to your `~/.ssh` folder, or copy it there once created

## macOS — connect the dots

### Background

- `ssh-agent` is already shipped and runs on your macOS; if you wire up new keys, you might need to stop the running process of the same name, and it would automatically restart and work

### Git Clients

- CLI is expected to work out of the box
- Kraken needs to be configured to "[x] Use local SSH agent"

## Windows — connect the dots

### Background

- An SSH Agent is needed
- OpenSSH is not supported by Kraken at this point on Windows [early 2020]

### Setup

- Install PuTTY
- Convert each of your private keys into a Pageant supported format: start `puttygen`, load the private key `id_rsa_example`, and save it as `id_rsa_example.ppk`
- Start `pageant`, and load all your `.ppk` keys into it

### Git Clients

- CLI is expected to work out of the box
- Kraken needs to be configured to "[x] Use local SSH agent"
