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