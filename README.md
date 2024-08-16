# YubiKey GitHub Setup with GPG Signing and SSH Authentication

This guide walks you through configuring Git to use your YubiKey for GPG commit signing and SSH authentication with GitHub. The setup includes adding both your SSH and GPG keys to GitHub, initializing a Git repository, and pushing the initial commit.

## Summary

- **GPG Signing**: Configures Git to sign commits with your GPG key.
- **SSH Authentication**: Sets up SSH authentication using your YubiKey.
- **GitHub Integration**: Adds both your SSH key and GPG key to GitHub for secure authentication and commit verification.
- **Repository Setup**: Initializes a new Git repository, commits the initial changes, and pushes them to a newly created GitHub repository.

## Listing GPG Secret Keys and Configuring Git to Use GPG Signing

### Template

```bash
gpg --list-secret-keys --keyid-format LONG
git config user.signingkey [YOUR_SIGNING_KEY_ID]
git config commit.gpgsign true
```

### Example

```plaintext
main@mains-MacBook-Air yubikey-demo % gpg --list-secret-keys --keyid-format LONG
[keyboxd]
---------
sec>  rsa4096/6B370D35165F17C7 2024-08-16 [SC]
      BE9A5569730A7EB1AB8CB7AB6B370D35165F17C7
      Card serial no. = 0001 08238096
uid                 [ultimate] William Veith (Tie Demo) <veithwilliam@yahoo.com>
ssb>  rsa4096/8E39155530A67B41 2024-08-16 [E]
ssb>  rsa4096/5A7FD483F1825381 2024-08-16 [S]
```

```bash
main@mains-MacBook-Air yubikey-demo % git config user.signingkey 5A7FD483F1825381
main@mains-MacBook-Air yubikey-demo % git config commit.gpgsign true
```

## Configuring SSH to Use Your YubiKey

### Template

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

### Example

```bash
main@mains-MacBook-Air yubikey-demo % nano ~/.ssh/config
```

```plaintext
  UW PICO 5.09                 File: /Users/will/.ssh/config                    

Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519

Host github-yubikey
  HostName github.com
  User git
  IdentityAgent $SSH_AUTH_SOCK











^G Get Help  ^O WriteOut  ^R Read File ^Y Prev Pg   ^K Cut Text  ^C Cur Pos   
^X Exit      ^J Justify   ^W Where is  ^V Next Pg   ^U UnCut Text^T To Spell
```

## Enabling SSH Support in the GPG Agent

```bash
echo "enable-ssh-support" >> ~/.gnupg/gpg-agent.conf
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent
```

## Configuring the Shell to Use the GPG Agent for SSH

### Template

```bash
nano ~/.zshrc
```

Add the following line:

```plaintext
export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
```

Apply the changes:

```bash
source ~/.zshrc
```

### Example

```bash
main@mains-MacBook-Air yubikey-demo % nano ~/.zshrc
```

```plaintext
  UW PICO 5.09                    File: /Users/will/.zshrc                      

# Set PATH for Homebrew
export PATH="/opt/homebrew/bin:$PATH"

# Set PATH for Node.js
export PATH="/opt/homebrew/opt/node/bin:$PATH"

# Set PATH for Python
export PATH="/opt/homebrew/opt/python@3.12/bin:$PATH"
export PATH="/opt/homebrew/opt/python@3.12/libexec/bin:$PATH"
export PATH="$HOME/project-init-scripts:$PATH"
export PATH="$PATH:~/bin"
export PATH="$PATH:/Users/main/Projects/Bash/bin/willtree"
export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)







^G Get Help  ^O WriteOut  ^R Read File ^Y Prev Pg   ^K Cut Text  ^C Cur Pos   
^X Exit      ^J Justify   ^W Where is  ^V Next Pg   ^U UnCut Text^T To Spell 
```

```bash
main@mains-MacBook-Air yubikey-demo % source ~/.zshrc
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

### Template

```bash
gpg --armor --export [YOUR_GPG_KEY_ID] | gh gpg-key add
```

### Example

```bash
gpg --armor --export BE9A5569730A7EB1AB8CB7AB6B370D35165F17C7 | gh gpg-key add
```

## Initializing a Git Repository and Making the First Commit

```bash
git init
git add .
git commit -m "First commit"
```

## Creating a GitHub Repository and Pushing the First Commit

### Template

```bash
gh repo create REPO-NAME --public --source=. --remote=origin --push
```

### Example

```bash
main@mains-MacBook-Air yubikey-demo % gh repo create yubikey-demo --public --source=. --remote=origin --push
```
