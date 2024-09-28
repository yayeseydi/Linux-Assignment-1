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
8. Create SSH Config File
9. Citations

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

## Adding SSH Keys to DigitalOcean

Go to the DigitalOcean Control Panel.
Navigate to Settings > Security.
Under SSH Keys, click Add SSH Key.
Copy the contents of your public key file:
bash
Paste it into the SSH Key field and give it a descriptive name.
Click Add SSH Key.

Command examples for users:
In PowerShell:
```
Get-Content C:\Users\your-user-name\.ssh\do-key.pub | Set-Clipboard
```

For MacOS users:
```
pbcopy < ~/.ssh/do-key.pub
```


For Linux users it depends on your system. For me it is:
```
wl-copy < ~/.ssh/do-key.pub
```

## Adding a Custom Arch Linux Image

Download the Arch Linux image from https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/
To do this, click the most recent image file link. 

<img src = "./assets/archlinux_recentimage.jpg">

---

Then select the link with "cloudimg". The ".qcow2" and download the most recent image.

<img src = "./assets/arch linux_imagelink.jpg">

---
To upload a custom Arch Linux image to your DigitalOcean account:

Go to the images section of the DigitalOcean Control Panel.

>Note Once you do so, you will see that there are multiple tabs including Snapshots, Backups, and Custom Images.

Click on custom images to upload a new one.

Click the upload image button and select the arch linux file you downloaded.

Once the upload is complete, you will now have your Arch linux file uploaded onto Digitalocean.


## Creating an Arch Linux Droplet

Steps to Create a Droplet:

Find the Create Button: In the upper right corner of the screen, you should see a green Create button. If it’s not visible, navigate to a different section using the left menu, and the button should reappear.

Click the green Create button, and a dropdown menu will appear.
Select Droplets from the dropdown. 

<img src = "./assets/create_droplet screenshot.jpg">
---

This will take you to a screen where you can configure your new droplet.
Configure Important Settings: You’ll now see various options for your droplet’s configuration. Follow these steps to ensure your droplet is properly set up:


Choose a Data Center Region:

Set Region to San Francisco (SF03). 

<img src = "./assets/choose_region.jpg">

---
Choose an Image:

Under Choose an Image, select the Custom Images tab.
Your custom Arch Linux image should appear here. Select it.

Choose a Size:

Leave the default Basic plan selected unless you need more resources. 
For CPU options, you can choose the $7 Premium AMD option or the $8 Premium Intel option depending on your needs.

<img src = "./assets/basic_plan screenshot.jpg">

Authentication Method:

Under Choose Authentication, select SSH Key and then choose the SSH key you added to your DigitalOcean account in the previous steps.
Hostname:

Create a hostname for your droplet.  This name will appear in your terminal prompt when you are connected to your server.
You can leave the remaining settings at their defaults.

Once all the settings are configured, click Create Droplet at the bottom of the screen.
Find Your Droplet's IP Address:

After the droplet has been created, it will appear in your DigitalOcean dashboard.
Copy the IP address of your droplet, as you’ll need it to connect via SSH.
Connecting to Your Droplet via SSH
To connect to your droplet via SSH, find your Droplet's IP address in the DigitalOcean Control Panel.
Open your terminal and connect using the following command:
bash
```
ssh -i .ssh/do-key arch@your-droplets-ip-address
```
If your key is correctly configured, you should be logged in without a password.

## Automating Setup with cloud-init

Cloud-init is a tool used to automate tasks when your server first boots. We'll use it to:
Create a new user.
Install essential packages.
Set up your SSH key for the new user.
Disable root SSH access for security.
Creating the cloud-init File

Install neovim using the code below if you dont already have it installed
```
sudo pacman -S neovim
```

To create a Cloud-Init configuration file, run code: 
```
nvim cloud-init.yml
```
Once you run the code, Add the following content to the file:
```
#cloud-config
users:
  - name: newuser
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    groups: sudo
    shell: /bin/bash
    ssh-authorized-keys:
      - ssh-rsa AAAAB3...your_public_key_here


packages:
  - ripgrep
  - rsync
  - neovim
  - fd
  - less
  - man-db
  - bash-completion
  - tmux


ssh_pwauth: false
```

>Note: This creates a user newuser with sudo privileges.
Installs vim, curl, and git.
Disables password authentication for SSH.
Upload this file during the Droplet creation process by selecting the User Data option.

After pasting, replace “user name” and “user group” with the name and group of the user.

Once those are replaced, you can run the command and receive a public key front your SSH Key.

You should now have cloud init running  on your server.

## Creating an SSH Config File

What is an SSH Config file?
SSH config files are used to simplify SSH connections after the server has been set up.
To set up an SSH Configeration 



## Conclusion 

Congratulations! You’ve successfully set up an Arch Linux server on DigitalOcean, created SSH keys, automated its initial setup using cloud-init, and connected to it using SSH.

## Citations

