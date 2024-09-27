# Creating a Remote Server on DigitalOcean with Arch Linux

## Introduction

In this tutorial, you will be guided through the process of setting up an Arch Linux server (Droplet) on DigitalOcean. You will create SSH keys to securely connect to the server, add a custom Arch Linux image, and automate server setup using cloud-init. You should have a good understand on how to securely access your server without a password, and why automation is crucial in server administration.


## Table of Contents

1. What are SSH Keys?
2. Creating SSH Keys
3. Adding SSH Keys to DigitalOcean
4. Adding a Custom Arch Linux Image
5. Creating an Arch Linux Droplet
6. Connecting to Your Droplet via SSH
7. Automating Setup with cloud-init
8. Citations

>Note: Before starting, ensure you have the following:
    A DigitalOcean account
    Basic familiarity with terminal commands
    A local machine running Linux or macOS in order to generate SSH keys and connect via SSH.

    ## What Are SSH Keys and why do we use them?

SSH (Secure Shell) is a secure protocol that is used to send commands and transfer data between devices over an unsecured network. SSH uses encryption to make sure confidentiality is maintained. There is no need for passwor based logins with SSH keys. SSH keys have both a public and private key. The public key is used to encrypt data that only the private key can decrypt. When it comes to authentication,the private key remains on your machine, while the public key is placed on the server, enabling secure, passwordless login. SSH keys are better than passwords because oftentimes, passwords can be guessed or stolen, while SSH keys are harder to hack. Once SSH keys are setup you will no longer need a password to login.

## Creating SSH Keys

Open a terminal on your local machine.
Run the following command to generate an SSH key pair:
bash
```
ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address"
```

If you are using Windows PowerShell, you can enter the full path. Just replace "your-user-name" with your Windows user name.

Example:
ssh-keygen -t ed25519 -f C:\Users\your-user-name\.ssh\do-key -C "youremail@email.com"
-t = type (this is the type of encryption used for the key)
-f = filename (specify filename and location)
-C = comment (attaches a comment to a key)
