#ssh

SSH (Secure Shell) is a protocol used to securely access and manage remote systems over an encrypted connection.

quick guide:

- **`ssh [username]@[hostname or IP address]`**
- `scp [source] [username]@[hostname]:[destination]` copying files with `scp` (`-r` if folder)
- Key Authentication (no password required)
    - `ssh-keygen` and `ssh-copy-id`
    - `ssh -i [private key] [username]@[host]`
- Port Forwarding:
    - `-L [local_port]:[remote_host]:[remote_port]` Local
    - `-R [remote_port]:[local_host]:[local_port]` Remote
- [[#SSH Config]]: `~/.ssh/config`

## SSH Apps for Transferring Files

- [FileZilla](https://filezilla-project.org/) (Uncheck recommended software when installing it)
- [CarotDAV](http://rei.to/carotdav_en.html)

# Basic Usage

1. **Connecting to a Remote Server**:

```bash
ssh [username]@[hostname or IP address]
```

2. **Specifying a Port (if not the default 22)**:

```bash
ssh -p [port] [username]@[hostname]
```

3. **Executing a Command on the Remote Server**:

```bash
ssh [username]@[hostname] [command]
```

Example:

```bash
ssh user@localhost "ls -l /var/www"
```

4. **Copying Files with `scp`**: Securely copy files between local and remote machines:

```bash
scp [local file] [username]@[hostname]:[destination]
scp [username]@[hostname]:[remote file] [local destination]
# folder with -r
```

# Key Authentication

SSH key authentication provides a more secure and convenient alternative to password-based login.

1. **Generate SSH Key Pair**: On your local machine, run:

```bash
ssh-keygen -t rsa -b 4096
```

This creates a public key (`id_rsa.pub`) and private key (`id_rsa`) in the `~/.ssh/` directory.

2. **Add SSH Key to SSH Agent**:

```bash
eval "$(ssh-agent -s)" # start SSH agnet in the background
ssh-add ~/.ssh/id_rsa
```

3. **Copy the Public Key to the Remote Server**:

```bash
ssh-copy-id [username]@[hostname] # NOT Working on Windows
```

On Windows you need to manually append the public key to the `~/.ssh/authorized_keys` file on the remote server.

4. **Set Proper Permissions**: Ensure correct file permissions on the server:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

5. **Login Using the Key**: After setup, SSH will automatically use the key pair for authentication:

```bash
ssh [username]@[hostname]
scp [local file] [username]@[hostname]:[destination]
rsync -r [local folder] [username]@[hostname]:[destination]
```

# Port Forwarding

SSH can forward ports for secure communication between local and remote systems.

1. **Local Port Forwarding (Access Remote Services Locally)**:

```bash
ssh -L [local_port]:[remote_host]:[remote_port] [username]@[hostname]
```

Example:

```bash
ssh -L 8080:localhost:8080 user@192.168.1.100
```

This forwards your local port `8080` to the remote server's port `80`.

2. **Remote Port Forwarding (Expose Local Services to Remote Systems)**:

This is very helpful when using cluster (expose port to login node).

```bash
ssh -R [remote_port]:[local_host]:[local_port] [username]@[hostname]
```

Example:

```bash
ssh -R 9090:localhost:3000 log-1 # same username as whoami
```

This forwards the login node's port `9090` to compute node port `3000`.

# SSH Config

- **Configuration File**: You can simplify SSH commands by using a configuration file (`~/.ssh/config`):

```
Host myserver
    HostName 192.168.1.100
    User user
    Port 2222  # most of time you don't need this, default 22
    IdentityFile ~/.ssh/id_rsa
    LocalForward 8080:localhost:8080
```

Then connect with:

```bash
ssh myserver
```
