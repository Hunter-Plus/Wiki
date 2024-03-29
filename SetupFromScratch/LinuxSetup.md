# Basic Tools(root)
```console
sudo add-apt-repository main
sudo add-apt-repository universe
sudo add-apt-repository restricted
sudo add-apt-repository multiverse
apt-get update
sudo apt install nautilus -y
sudo apt install x11-apps -y
sudo apt install ubuntu-desktop mesa-utils
xauth add $(xauth -f ~root/.Xauthority list|tail -1)
xauth add $(xauth -f ~hunterzhang/.Xauthority list|tail -1)
```

# Add User
To ensure the stability of the development environment, it is recommended to develop as a normal user instead of root. In the following wiki, I will use the username "hunterzhang" as an example. \
Log in to your Linux system as the root user:
```console
adduser hunterzhang
passwd hunterzhang
```
Alternatively, you can set the password of the new user by:
```console
echo "hunterzhang:qwert12345" | chpasswd
```
## Change Home Path(optional)
Sometimes it is necessary to set your home path to another disk. Using "data/home/hunterzhang" as an example, you can change the home path manually by:
```console
vi /etc/passwd
```
Or get the UID first by:
```console
id hunterzhang
```
Then
```console
cd /data/home
chown -R hunterzhang:hunterzhang  hunterzhang
chmod -R 700 hunterzhang
```
## Set Default User(WSL)
```console
ubuntu config --default-user <username>
```
# Configure SSH connection(from Windows)
## Reinstall and Configure openssh server
```console
sudo apt-get remove openssh-server
sudo apt-get install openssh-server
```

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
Upload "id_rsa.pub" from Windows OS to Linux OS at your SSH directory, for example, "~/.ssh$". Add your public key into the Linux OS:\
```console
cat id_rsa.pub >> authorized_keys
cd ..
chmod -R 700 .ssh/
chmod 600 .ssh/authorized_keys
```
## Test SSH Connection
Make sure the IP address and port for the SSH connection by:
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
Then you can change or use the default file path/password. To change the file permissions:
```console
chmod -R 700 .ssh/
chmod 600 authorized_keys 
```

# Git
Git has been preinstalled by most Linux OSs, but we still need to add our public key to Github or your Git service. \
Copy the text in the ~/.ssh/id_rsa.pub and add it to your Git then it is done. \

If your SSH connection to Github is now working, see: \
https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port

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
![ssh_targets](../SetupFromScratch/img/ssh_targets.jpg) 

# Remix IDE

## npm & Node.js & docker & pip
```console
sudo apt install npm -y
sudo apt install docker.io -y
sudo apt install python3-pip -y
```

## Remixd/AppImage
Install remixd for the deb version: \
```console
npm install -g @remix-project/remixd
remixd -i slither
```

Or download the AppImage version from Github: \
```console
wget https://github.com/ethereum/remix-desktop/releases/download/v1.3.6/Remix-IDE-1.3.6.AppImage
```

Run it: 
```console
./Remix-IDE-1.3.6.AppImage
# or
remix-ide
```

For some Error info:
```console
export LIBGL_ALWAYS_INDIRECT=1
export DISPLAY=:0
```



# Ganache

```console
wget https://github.com/trufflesuite/ganache-ui/releases/download/v2.7.1/ganache-2.7.1-linux-x86_64.AppImage
```
# VS Code + Git + Remix(Smart contract dev environment)
Before starting this part, ensure the following tools are functional in your dev environment:\
### 1.VS Code(remote, as the IDE)
Access your remote file system on WSL/Linux through VS Code Remote Development Extension with proper keys and settings. Develop locally on your Windows OS, but compile and execute code on the remote machine/WSL.
### 2.Remix IDE(on Linux, as the project manager)
Use Remix IDE installed on the Remote/WSL OS with GUI and ensure dependencies are installed, especially remixd.
### 3.Git(Github or your own git system, as the version controller)
Ensure your user on the Remote/WSL has SSH access to your Git system by configuring your RSA key on your Git system.

## Introduction
With the VS Code + Git + Remix environment, you can create, run, and deploy your smart contract projects on Linux OS using Remix and other supported tools. Although the Ethereum Remix Extension of VS Code was deprecated, Remix IDE remains the best project manager tool for Solidity developers, especially beginners. You can replace it with customized tools as needed. \

Next, link Remix IDE to Git. Since Remix IDE is not an ideal IDE, especially when running remotely, using VS Code offers many useful features such as Editor Split and shortcuts. To develop on VS Code while running on Remix, use a third-party version controller like Git. \

Finally, use Git through VS Code with its various extensions. Enjoy the smooth experience of developing locally on Windows. \

## Generate&Set Github Token
To generate your Github token, open the setting panel of Remix IDE and follow the link. Generate the token and configure it on Remix.
![remix_token_setting](../SetupFromScratch/img/remix_token_setting.jpg) 

## Create A Git Repository

![create_on_git](../SetupFromScratch/img/create_on_git.jpg) 
## Create Workspace as Git Repo
Remix IDE is workspace-based. Check the checkbox to create your workspace as a Git repository.
![create_as_git](../SetupFromScratch/img/create_as_git.jpg) 
When this is done, Remix IDE will automatically create a local Git commit.
## Push Workspace to Git
Make sure you have installed the DGIT plugin in Remix IDE.
![local_commit](../SetupFromScratch/img/local_commit.jpg) 
You can see the initial local commit by clicking the DGIT logo on the left. Then, go to **CLONE, PUSH, PULL & REMOTES -> GIT REMOTE**.
![add_git_remote](../SetupFromScratch/img/add_git_remote.jpg) 
The new Git remote will be available. Check it and push to the master branch.
![push_git](../SetupFromScratch/img/push_git.jpg) 
## Pull Your Project from VS Code
```console
git clone https://github.com/Hunter-Plus/Workspace2Git.git
```
![code_on_vscode](../SetupFromScratch/img/code_on_vscode.jpg) 
When you want to run a test, push your code to Git from VS Code after setting your Git information.
## Pull your Workspace from Remix IDE
To pull your workspace from Remix IDE, click the PULL button.