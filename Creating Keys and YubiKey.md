# Generating and Managing GPG Keys with YubiKey

This section walks you through generating a new GPG key, adding subkeys, and transferring them to your YubiKey for secure storage.

## Generating a New GPG Key

### Template

```bash
gpg-connect-agent reloadagent /bye
gpg --full-generate-key
```

Follow the prompts:

1. **Select the key type**:
   ```plaintext
   9 ECC (sign and encrypt) *default*
   ```

2. **Select the curve**:
   ```plaintext
   1 Curve 25519 *default*
   ```

3. **Set the key expiration**:
   ```plaintext
   0 key does not expire
   ```

4. **Confirm**:
   ```plaintext
   y
   ```

5. **Enter your user details**:
   ```plaintext
   Real name: William Veith
   Email address: williamveith@gmail.com
   Comment: TIE Demo
   ```

6. **Confirm**:
   ```plaintext
   O
   ```

### Example

```bash
main@mains-MacBook-Air yubikey-demo % gpg-connect-agent reloadagent /bye
main@mains-MacBook-Air yubikey-demo % gpg --full-generate-key
```

### Adding Subkeys

### Template

```bash
gpg --edit-key [YOUR_KEY_ID]
```

Follow the prompts:

1. **Add a signing subkey**:
   ```bash
   addkey
   ```

   Select the key type and curve:
   ```plaintext
   10 ECC (sign only)
    1 Curve 25519 *default*
    0 key does not expire
    y
    y
   ```

2. **Save the key**:
   ```bash
   save
   ```

### Example

```bash
main@mains-MacBook-Air yubikey-demo % gpg --edit-key BE9A5569730A7EB1AB8CB7AB6B370D35165F17C7

main@mains-MacBook-Air yubikey-demo % addkey

main@mains-MacBook-Air yubikey-demo % save
```

### Transferring Keys to Your YubiKey

### Template

```bash
gpg --edit-key [YOUR_KEY_ID]
```

Follow the prompts:

1. **Select the primary key**:
   ```bash
   key 0
   ```

2. **Transfer the primary key to YubiKey**:
   ```bash
   keytocard
   ```

3. **Select the first subkey**:
   ```bash
   key 1
   ```

4. **Transfer the subkey to YubiKey**:
   ```bash
   keytocard
   ```

5. **Save the keys**:
   ```bash
   save
   ```

### Example

```bash
main@mains-MacBook-Air yubikey-demo % gpg --edit-key BE9A5569730A7EB1AB8CB7AB6B370D35165F17C7

main@mains-MacBook-Air yubikey-demo % key 0
main@mains-MacBook-Air yubikey-demo % keytocard

main@mains-MacBook-Air yubikey-demo % key 1
main@mains-MacBook-Air yubikey-demo % keytocard

main@mains-MacBook-Air yubikey-demo % save
```

### Final Steps

```bash
gpg-connect-agent reloadagent /bye
gpg --card-status
gpg --list-secret-keys --keyid-format LONG
```

### Example

```bash
main@mains-MacBook-Air yubikey-demo % gpg-connect-agent reloadagent /bye
main@mains-MacBook-Air yubikey-demo % gpg --card-status
main@mains-MacBook-Air yubikey-demo % gpg --list-secret-keys --keyid-format LONG
```
