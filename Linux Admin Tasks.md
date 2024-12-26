
# 1. Server Access with Username and Password
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

---

# 3. Key-Based Authentication with PuTTY on Windows (Including User Creation)

## Step 1: Generate SSH Key Pair
1. **Download and Install PuTTY** if you haven't already from the [official website](https://www.putty.org/).

2. **Open PuTTYgen** (comes with PuTTY) to generate the SSH key pair:
   - Open `PuTTYgen` (you can find it in the Start menu or the folder where PuTTY is installed).
   - Select the key type as `RSA` or `ED25519` (RSA is commonly used).
   - Set the desired number of bits for the key. For RSA, 2048 bits is typical.
   - Click **Generate**.
   - Move your mouse around the blank area to help generate entropy for the key.
   - After the key is generated, **save the private key** (e.g., `private_key.ppk`).
   - Optionally, set a passphrase for the key for extra security.
   - Copy the **public key** from the "Public key for pasting into OpenSSH authorized_keys file" box (you'll need this later).

## Step 2: Create the User on the Server
1. **Log in to the server** with a user that has administrative privileges (root or a sudo-enabled user).

2. **Create the new user**:
   - Run the following command to create a new user (replace `username` with the desired username):
     ```bash
     sudo adduser username
	   useradd -G <groupname> <username>
     ```
   - Follow the prompts to set the password and other user information (you can skip some of the fields by pressing Enter).

3. **Add the user to the sudo group** (if the user should have sudo privileges):
   - Run:
     ```bash
     sudo usermod -aG <groupname> <username>
	 gpasswd -a <username> wheel
	 groupadd <groupname>   # for creating any costom group
     ```

## Step 3: Configure the Server to Accept the Public Key
1. **Log in as the new user** (or switch to the new user using `su - username`).

2. **Create an SSH directory for the new user**:
   - Run:
     ```bash
     mkdir -p ~/.ssh
     ```

3. **Set the correct permissions for the `.ssh` directory**:
   - Run:
     ```bash
     chmod 700 ~/.ssh
     ```

4. **Edit the `authorized_keys` file**:
   - Run:
     ```bash
     nano ~/.ssh/authorized_keys
     ```
   - Paste the **public key** you copied from PuTTYgen into this file.
   - Save and exit the editor (Ctrl + X, then Y to confirm, and Enter to save).

5. **Set the correct permissions for the `authorized_keys` file**:
   - Run:
     ```bash
     chmod 600 ~/.ssh/authorized_keys
     ```

## Step 4: Configure SSH to Allow Key-Based Authentication (If Not Already Configured)
1. **Edit the SSH configuration file**:
   - Run:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```

2. Ensure the following settings are configured:
   - `PubkeyAuthentication yes`
   - `AuthorizedKeysFile .ssh/authorized_keys`
   - **Optional**: To disable password authentication entirely (for increased security), set:
     - `PasswordAuthentication no`

3. **Restart the SSH service** to apply changes:
   ```bash
   sudo systemctl restart ssh
   ```

## Step 5: Configure PuTTY to Use the Private Key
1. **Open PuTTY**.

2. In the **Host Name (or IP address)** field, enter the server’s IP address.

3. In the **Connection > SSH > Auth** section, browse and select the **private key file** you saved earlier (`private_key.ppk`).

4. **Save this session** by going back to the "Session" category, entering a name under "Saved Sessions", and clicking **Save**.

## Step 6: Connect to the Server
1. Click **Open** to initiate the SSH session.

2. When prompted for the username, enter the **new user’s username** you created earlier.

3. If the key-based authentication is set up correctly, you should be logged into the server without needing a password.

## Step 7: Test and Troubleshoot
- If you encounter any issues, ensure that the `sshd_config` file on the server has `PubkeyAuthentication yes` and `AuthorizedKeysFile .ssh/authorized_keys` set.
- Ensure the permissions on the server's `.ssh` directory and `authorized_keys` file are correct (as described in Step 3).
- Check if the correct user is logging in by verifying that the username matches the one set up for key-based authentication.

That's it! Now, the new user should be able to log into the server using key-based authentication from PuTTY.
