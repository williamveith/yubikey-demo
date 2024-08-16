# YubiKey GitHub Setup with GPG Signing and SSH Authentication

This guide walks you through configuring Git to use your YubiKey for GPG commit signing and SSH authentication with GitHub. The setup includes adding both your SSH and GPG keys to GitHub, initializing a Git repository, and pushing the initial commit.

## Summary

- **GPG Signing**: Configures Git to sign commits with your GPG key.
- **SSH Authentication**: Sets up SSH authentication using your YubiKey.
- **GitHub Integration**: Adds both your SSH key and GPG key to GitHub for secure authentication and commit verification.
- **Repository Setup**: Initializes a new Git repository, commits the initial changes, and pushes them to a newly created GitHub repository.

## Listing GPG Secret Keys and Configuring Git to Use GPG Signing

```bash
gpg --list-secret-keys --keyid-format LONG
git config user.signingkey [YOUR_SIGNING_KEY_ID]
git config commit.gpgsign true
```

## Configuring SSH to Use Your YubiKey

```bash
nano ~/.ssh/config
```

Add the following to your SSH profile:

```plaintext
Host github-yubikey
  HostName github.com
  User git
  IdentityAgent $SSH_AUTH_SOCK
```

## Enabling SSH Support in the GPG Agent

```bash
echo "enable-ssh-support" >> ~/.gnupg/gpg-agent.conf
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent
```

## Configuring the Shell to Use the GPG Agent for SSH

```bash
open ~/.zshrc
```

Add the following line:

```plaintext
export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
```

Apply the changes:

```bash
source ~/.zshrc
```

## Adding Your YubiKey's SSH Key to GitHub

```bash
ssh-add -L | gh ssh-key add -t "YubiKey SSH Key"
```

## Testing SSH Connection to GitHub

```bash
ssh -T git@github.com
```

## Exporting Your GPG Key and Adding It to GitHub

```bash
gpg --armor --export [YOUR_GPG_KEY_ID] | gh gpg-key add
```

## Initializing a Git Repository and Making the First Commit

```bash
git init
git add .
git commit -m "First commit"
```

## Creating a GitHub Repository and Pushing the First Commit

```bash
gh repo create REPO-NAME --public --source=. --remote=origin --push
```
