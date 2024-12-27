### SSH Key Setup and Configuration

#### **1. Generate SSH Key Pair**
Run the following command to generate an SSH key pair:
```bash
ssh-keygen
```
- When prompted, specify the file path to save the key:
  ```
  Enter file in which to save the key (/root/.ssh/id_rsa): /home/ec2-user/.ssh/ansi_demo
  ```

#### **2. View the Public Key**
Display the contents of the public key:
```bash
cat /home/ec2-user/.ssh/ansi_demo.pub
```

#### **3. Add Public Key to Authorized Keys**
1. Open the `authorized_keys` file for the user:
   ```bash
   vim /home/ec2-user/.ssh/authorized_keys
   ```
2. Append the public key content from `ansi_demo.pub` to this file.

#### **4. Verify SSH Access**
Test the connection using the private key:
```bash
ssh -i /home/ec2-user/.ssh/ansi_demo ec2-user@54.198.6.124
```

#### **5. Check SSH Server Configuration on the Remote Host**
1. Open the SSH daemon configuration file:
   ```bash
   sudo vi /etc/ssh/sshd_config
   ```
2. Ensure the following settings are enabled:
   ```plaintext
   PubkeyAuthentication yes
   AuthorizedKeysFile .ssh/authorized_keys
   ```

3. Restart the SSH service:
   ```bash
   sudo systemctl restart sshd
   ```




