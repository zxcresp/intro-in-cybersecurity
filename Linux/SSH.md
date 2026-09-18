# SSH

**SSH (Secure Shell)** is a protocol that allows you to **securely connect to a remote computer over a network and manage it through a terminal**

For example:

```
Your computer                  Remote server
      │                              │
      │────── SSH connection ───────>│
      │                              │
      │       commands / data        │
      │<─────────────────────────────│
```

After connecting, you can work with the remote computer almost the same way you would work with a terminal on your own machine

---

# What SSH is used for

SSH is commonly used for:

* remote management of Linux servers;
* server administration;
* file transfer;
* executing commands on a remote machine;
* secure access to network equipment;
* automation

The standard SSH port is **22/TCP**.

---

# How to connect

The basic command is:

```
ssh username@ip_address
```

For example:

```
ssh user@192.168.1.10
```

Here:

```
ssh             → SSH client
user            → username on the remote machine
192.168.1.10    → IP address of the remote machine
```

After connecting, the terminal will be working on the remote computer

For example:

```
local@computer:~$ ssh user@192.168.1.10

user@192.168.1.10's password:

user@server:~$
```

Notice that the terminal prompt has changed:

Before:

```
local@computer
```

After:

```
user@server
```

This means that commands are now being executed **on the remote server**

---

# SSH Client and SSH Server

SSH uses two sides:

```
SSH Client                    SSH Server
    │                              │
    │────── connection ───────────>│
    │                              │
```

**SSH client** - a program on the computer from which the connection is made

**SSH server** - a program on the remote machine that accepts SSH connections

On Linux, the SSH server is commonly **OpenSSH Server**

You can check its status with:

```
sudo systemctl status ssh
```

Start it:

```
sudo systemctl start ssh
```

Stop it:

```
sudo systemctl stop ssh
```

Restart it:

```
sudo systemctl restart ssh
```

---

# SSH Port

By default, SSH uses:

```
TCP 22
```

You can specify a different port when connecting:

```
ssh -p 2222 user@192.168.1.10
```

Here:

```
-p 2222 → use port 2222
```

---

# Authentication

SSH must verify **whether the user is allowed to connect to the server**

The main authentication methods are:

1. password;
2. SSH keys

Example of a real connection:

```
ssh server@192.168.1.100

The authenticity of host '192.168.1.100 (192.168.1.100)' can't be established.
ED25519 key fingerprint is SHA256:x8+yZ9a...
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.1.100' (ED25519) to the list of known hosts.

server@192.168.1.100's password:
```

**One important thing to note:**

*When entering a password, no characters are displayed — not letters, asterisks, or dots*

You simply need to type the password blindly and press Enter

Suppose you entered the correct password:

```
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-88-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

Last login: Fri Sep 18 19:37:00 2026 from 192.168.1.50
server@ubuntu-server:~$
```

---

# SSH Keys

SSH keys use a cryptographic key pair:

```
Private key       Public key
```

The user has:

**Private key** - a secret key that should remain only with its owner

**Public key** - a public key that can be placed on the server

Simplified:

```text
Your computer                    Server
     │                              │
Private key                    Public key
     │                              │
     └──── authentication ──────────┘
```

The private key **must not be shared with other people**

---

# Creating an SSH Key

On Linux, you can create a key with:

```
ssh-keygen
```

Keys are usually stored in:

```
~/.ssh/
```

For example:

```
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

Here:

```
id_ed25519
```

* private key

```
id_ed25519.pub
```

* public key

---

# Copying the Public Key

For a Linux server, you can use:

```
ssh-copy-id user@192.168.1.10
```

After this, the user's public key will be added to the server

It is usually stored in:

```
~/.ssh/authorized_keys
```

---

# Connecting Using an SSH Key

After configuring the key, you can connect with:

```
ssh user@192.168.1.10
```

SSH will use the key for authentication

If the private key is stored in a non-standard location, you can specify it:

```
ssh -i ~/.ssh/my_key user@192.168.1.10
```

---

# Executing a Command Without Opening an Interactive Session

SSH allows you to execute a single command on a remote computer:

```
ssh user@192.168.1.10 "whoami"
```

The server will execute:

```
whoami
```

and return the result

For example:

```
user
```

You can also execute a more complex command:

```
ssh user@192.168.1.10 "ip addr"
```

---

# File Transfer

SSH can also be used for secure file transfer

One tool for this is **SCP (Secure Copy Protocol)**

Send a file to the server:

```
scp file.txt user@192.168.1.10:/home/user/
```

Download a file from the server:

```
scp user@192.168.1.10:/home/user/file.txt ./
```

You can also use **SFTP**, which works over SSH

---

# Checking SSH Availability

You can check whether the SSH port is open:

```
nc -zv 192.168.1.10 22
```

or:

```
nmap -p 22 192.168.1.10
```

If the port is accessible, an SSH service may be accepting connections

---

# SSH and Security

SSH **encrypts the connection** between the client and the server

This protects transmitted data from simple network interception

However, the SSH service still needs to be configured properly

For example, it is important to:

* use strong authentication;
* protect private keys;
* regularly update OpenSSH;
* restrict access to SSH;
* use a firewall;
* disable unnecessary authentication methods;
* avoid giving users more privileges than they need

---

# Main SSH Files

The SSH server configuration is usually located at:

```
/etc/ssh/sshd_config
```

The user's SSH client configuration:

```
~/.ssh/config
```

User public keys:

```
~/.ssh/authorized_keys
```

User SSH keys:

```
~/.ssh/
```

---

# Useful Commands

```
ssh user@192.168.1.10
```

Connect to a server

```
ssh -p 2222 user@192.168.1.10
```

Connect using a specific port

```
ssh -i ~/.ssh/my_key user@192.168.1.10
```

Use a specific private key

```
ssh user@192.168.1.10 "command"
```

Execute a command on the server

```
scp file.txt user@192.168.1.10:/home/user/
```

Transfer a file to the server

```
sudo systemctl status ssh
```

Check the status of the SSH server

---

# The Main Things to Remember

```
SSH
│
├── Secure Shell
├── remote management
├── TCP
├── standard port 22
├── SSH Client
├── SSH Server
├── Password authentication
└── SSH Keys
      ├── Private key
      └── Public key
```

The main idea:

> **SSH allows you to securely connect to a remote computer and manage it through the command line**
