
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
