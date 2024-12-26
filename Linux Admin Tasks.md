
## 1. Using SSH with Username and Password
### Edit SSH Configuration File: Open the sshd_config file:
```bash
sudo vim /etc/ssh/sshd_config
```
### Ensure the following lines are set:
```bash
PasswordAuthentication yes
PermitRootLogin no    # Optional: Disable root login for security.
```
### Restart SSH Service:
```bash
sudo systemctl restart sshd
```
### Create a User
```bash
sudo adduser username
sudo passwd username  # Set a password for the user.
```
### Access the Server: Use an SSH client like ssh (Linux/macOS) or PuTTY (Windows) to log in:
```bash
ssh username@server_ip
```
---
# 2. Key-Based Authentication for Server Access

Providing server access using public and private keys (key-based authentication) is a secure alternative to username and password login. Here’s how to set it up step-by-step for a Linux server using SSH:

---

## **Step 1: Generate SSH Key Pair**

### **On the Client Machine**:
Run the following command to generate a key pair (private and public keys):
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
- **`-t rsa`**: Specifies the RSA algorithm.
- **`-b 4096`**: Sets the key size to 4096 bits.
- **`-C`**: Adds a comment (like your email) for identification.

### **Save the Keys**:
- You’ll be prompted to save the keys. Press Enter to save them in the default location (`~/.ssh/id_rsa`) or specify a custom path.
- Optionally, set a passphrase for added security.

### **Check the Generated Keys**:
- The public key: `~/.ssh/id_rsa.pub`.
- The private key: `~/.ssh/id_rsa`.

---

## **Step 2: Add the Public Key to the Server**

### **Log in to the Server Using a Password**:
```bash
ssh username@server_ip
```

### **Create the `.ssh` Directory** (if it doesn’t exist):
```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

### **Add the Public Key to `authorized_keys`**:
- From the client machine, copy the public key to the server:
  ```bash
  ssh-copy-id -i ~/.ssh/id_rsa.pub username@server_ip
  ```
- Alternatively, manually add the public key:
  - Copy the public key content:
    ```bash
    cat ~/.ssh/id_rsa.pub
    ```
  - On the server, open or create the `authorized_keys` file:
    ```bash
    nano ~/.ssh/authorized_keys
    ```
  - Paste the public key and save the file.

### **Set Proper Permissions**:
```bash
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

---

## **Step 3: Configure SSH to Use Key-Based Authentication**

### **Edit the SSH Configuration File**:
```bash
sudo nano /etc/ssh/sshd_config
```

### **Update the Following Settings**:
```text
PasswordAuthentication no
PubkeyAuthentication yes
ChallengeResponseAuthentication no
```

### **Restart the SSH Service**:
```bash
sudo systemctl restart sshd
```

---

## **Step 4: Test the Key-Based Authentication**

### **Log in Using the Private Key**:
- Use the default location:
  ```bash
  ssh username@server_ip
  ```
- Specify a custom private key path:
  ```bash
  ssh -i /path/to/private_key username@server_ip
  ```

### **Verify Password Authentication is Disabled**:
- If key-based login works and password login is disabled, you’ve successfully configured access.

---

## **Step 5: Secure Your Setup**

### **Disable Root Login**:
In `/etc/ssh/sshd_config`, set:
```text
PermitRootLogin no
```

### **Restrict Permissions**:
- Ensure `.ssh` and `authorized_keys` have strict permissions as shown in Step 2.

### **Backup Keys**:
- Keep your private key secure and back it up in a safe location.

### **Use Firewall**:
- Allow only SSH traffic:
  ```bash
  sudo ufw allow 22
  sudo ufw enable
  ```

### **Optional**: Use a Passphrase for the Private Key:
- When generating the key, set a strong passphrase to add another layer of security.

---

By following these steps, users can securely access the server using their private keys, without relying on passwords.

