# Basic Tools(root)
sudo add-apt-repository main
sudo add-apt-repository universe
sudo add-apt-repository restricted
sudo add-apt-repository multiverse
# Add User
Considering the stability of the dev environment, I highly recommend that to develop as a notmal user rather than the root. I will use the username, hunterzhang, as the example in the following wiki. \
Log in you Linux system as the root user:
```console
adduser hunterzhang
passwd hunterzhang
```
Or you can set the password of the new user by:
```console
echo "hunterzhang:qwert12345" | chpasswd
```
## Change Home Path(optional)
Sometimes it is necessary to set your home path to another disk. \
Using data/home/hunterzhang as an example.\
We can change the home path mannually by:
```console
vi /etc/passwd
```
Or get the UID firstly by:
```console
id hunterzhang
```
Then
```console
cd /data/home
chown -R hunterzhang:hunterzhang  hunterzhang
chmod -R 700 hunterzhang
```

# Configure SSH connection(from Windows)
## Reinstall and Configure openssh server
sudo apt-get remove openssh-server
sudo apt-get install openssh-server

Any available port is okay for the config:
```console
sudo vi /etc/ssh/sshd_config
```

```json
Port 36000
PasswordAuthentication yes
AllowTcpForwarding yes
```

```console
sudo service ssh restart
```
You can check if the service is up by:
```console
ps -e | grep ssh
#or
sudo service ssh status
```

## Add Public Key
Upload id_rsa.pub from Windows OS to Linux OS at your ssh directory, for example, ~/.ssh$. \
Add you public key into the Linux OS: \
```console
cat id_rsa.pub >> authorized_keys
cd ..
chmod -R 700 .ssh/
chmod 600 .ssh/authorized_keys
```
## Test SSH Connection
Make sure the Ip address and port for the SSH connection by:
```console
ip addr
```
Then test the connection from Windows CMD: \
WSL's IP is 127.0.0.1, you should use yours here:

```console
ssh hunterzhang@127.0.0.1 -p36000
```
![ssh_ok](../SetupFromScratch/img/ssh_ok.jpg)

# Generate RSA Keys(Linux)
Execute the following command:
```console
ssh-keygen -t rsa
```
Then you can change or use the default file path/password. \
To change the file permissions:
```console
chmod -R 700 .ssh/
chmod 600 authorized_keys 
```

# Git
Git has been preinstalled by most Linux OSs, but we still need to add our public key to Github or your Git service. \
Copy the text in the ~/.ssh/id_rsa.pub and add it to your Git then it is done.

# VS Code(Remote)
Install the Remote Development Extension for VS Code. \
![remote](../SetupFromScratch/img/remote.jpg)
## WSL
Normally, the Remote - WSL will be automatically set if your WSL is ready. \
You can use it here: \
![wsl_remote](../SetupFromScratch/img/wsl_remote.jpg)
## SSH
You should change the setting of each components here: \
![remote_setting](../SetupFromScratch/img/remote_setting.jpg)
![ssh_setting](../SetupFromScratch/img/ssh_setting.jpg) \
Then open the configuration file: \
Ctrl + P \
input '>' \
Remote-SSH:Open Configuration File \
C:\Users\hunterzhang\.ssh\config

Add SSH as you wish: \
![ssh_config](../SetupFromScratch/img/ssh_config.jpg) \
![ssh_targets](../SetupFromScratch/img/ssh_targets.jpg) \
