# Setting Up a Virtual Machine (VM) with Root Access Using SSH Private Key Authentication

This guide provides instructions for setting up a Linux virtual machine (VM), allowing communication between the VM and the host system, and configuring **root access** using **SSH private key authentication**.

## Prerequisites

- A **virtual machine software** like **VirtualBox**, **VMware**, or similar.
- A **Linux distribution** (e.g., Kali Linux) installed on the VM.
- A **host system** running either Windows or Linux.


## Steps to Set Up the Linux VM and Enable Communication

### 1. **Create and Install Linux on the VM**

- **Create a new VM** in your preferred virtualization software (e.g., VirtualBox, VMware).
- **Install Kali Linux** or any other Linux distribution on the VM.
- Ensure the network interface is configured correctly to allow communication with the host. Use a **Bridged Network Adapter** or **NAT**:
  - **Bridged Adapter**: This will give the VM a separate IP address on the same network as the host, making it easier to communicate with the host and other devices on the network.
  - **NAT**: This shares the host's IP address with the VM. The VM can access the internet, but the host may need to use port forwarding for inbound connections.

### 2. Enable SSH 
```
sudo apt install openssh-server
```
```
sudo systemctl start ssh
```
```
sudo systemctl enable ssh
```
you can verify if the ssh is running by this code in the terminal

```
sudo systemctl status ssh
```

### 2. **Check the VM’s IP Address**

After the VM is up and running, find its IP address by running the following command inside the VM:
 ```
 ifconfig -a
 ```

 ### 3. Now we will use the SSH protocol to access the linux machine.

 The following tools can be used to perform such operations

*  cmd
*  powershell
*  linux terminal
*  PuTTY
*  MobaXter
and etc.
I am using MobaXterm, because of its UI and versatile features.

if we are using windows command line from CMD.
write the following command

```
ssh username@hostname
```
then provide the password and we are in.

### 4 configuring key for the SSH access.
now we want to secure the access using a privatekey.
in this case I will secure the root user.

now we will open the powershell and type ssh-keygen
```
ssh-keygen -t rsa -b 4096
```
then after setting our passphrase *[<mark> !!! important: you have to remember the passphrase for further use</mark>]* you can see two files [id_rsa, id_rsa.pub] generating in this location 

C:\Users\<YourUsername>\.ssh\

now we will login as a root user and configure the private key authentication.
We will now enable key authentication
on the terminal we will write this command to access file
```
sudo nano /etc/ssh/sshd_config
```
now we will make sure this lines are there

```
PermitRootLogin yes
```
```
PubkeyAuthentication yes
```
now we will save the file.

now we will add the key to the root.
so logged in as a root user  we will copy the id_rsa.pub file's content and paste it to

```
sudo nano /root/ssh/authorized_keys
```
and then we can set the permission

```
chmod 700 /root/.ssh
chmod 600 /root/.ssh/authorized_keys
```

 and lastly we will restart the ssh service

```
sudo systemctl restart
```

### now we will use the key to login to our machine

again we can use any form of command line, PuTTY or MobaXterm makes it easier, it has option for SSH selecting the RSA keys.
for command lines the command is like this.


```
ssh -i "rsa_id" root@hostname
```
the in the hostname section it will have your ip address.



