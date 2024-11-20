# SSH Key Management Notes

## Table of Contents

1. [Generate an SSH Key](#generate-an-ssh-key)
2. [Copy the Public Key](#copy-the-public-key)
   - [For Windows](#for-windows)
   - [For Linux](#for-linux)
3. [Create SSH Config File](#create-ssh-config-file)
4. [Using HTTPS with Access Tokens](#using-https-with-access-tokens)
5. [Using `.pem` Files](#using-pem-files)

---

## Generate an SSH Key

To generate a new SSH key:
```bash
ssh-keygen -t rsa -b 4096 -C your_email@abc.com
```

- **Recommendation**: Name the key file based on the service it’s intended for (e.g., `github_rsa`).
- You can leave the passphrase empty if you prefer not to have an additional security layer. However, using a passphrase adds extra protection by requiring the passphrase each time the key is used.

---

## Copy the Public Key

### **For GitHub or Other Platforms**
1. Locate your public key:
   ```bash
   ~/.ssh/your_stored_key.pub
   ```
2. Copy the key content and paste it into the relevant platform’s settings. For GitHub, go to **Settings > SSH and GPG Keys > Add New Key**.

### **For Windows**
To add your key to another machine:
```powershell
type $env:USERPROFILE\.ssh\your_stored_key.pub | ssh {IP-ADDRESS-OR-FQDN} "cat >> .ssh/authorized_keys"
```

### **For Linux**
To add your key to another machine:
```bash
ssh-copy-id -i ~/.ssh/your_stored_key.pub user@host
```

---

## Create SSH Config File

Creating an SSH config file is especially useful when you have multiple keys. 

1. Open the `config` file in the `.ssh` directory:
   ```bash
   vi ~/.ssh/config
   ```
2. Example configuration:
   ```ini
   Host github.com
       User git
       Port 22
       Hostname github.com
       IdentityFile ~/.ssh/your_stored_key
       TCPKeepAlive yes
       IdentitiesOnly yes
   ```

**Notes**:
- Use the private key (`your_stored_key`, without `.pub`).
- Ensure `IdentitiesOnly` is set to `yes` to use only the specified key.

---

## Using HTTPS with Access Tokens

If you’re using HTTPS instead of SSH, you need to generate an access token on GitHub or the relevant platform. Use the token instead of your regular password when prompted.

---

## Using `.pem` Files

When working with `.pem` files (commonly used for AWS), you need to:
1. Start the SSH agent:
   ```bash
   eval $(ssh-agent -s)
   ```
2. Add the `.pem` file to the agent:
   ```bash
   ssh-add /path/to/your-key.pem
   ```

### Automating with `.bashrc`
To automatically start the SSH agent and add the identity on terminal startup:
1. Open your `.bashrc` file:
   ```bash
   vi ~/.bashrc
   ```
2. Add the following lines:
   ```bash
   eval $(ssh-agent -s)
   ssh-add /path/to/your-key.pem
   ```
3. Save and reload:
   ```bash
   source ~/.bashrc
   ```

---

