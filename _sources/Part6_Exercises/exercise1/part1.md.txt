# Part 1: Logging in via SSH

This page covers how to log into a remote machine.

## SSH client 

To connect to a remote computer you will need a SSH client.

SSH is a tool that allows us to connect to and use a remote computer as our own.
Please follow the directions below to install an SSH client for your system if you do not 
already have one.

### Windows

Modern versions of Windows have SSH available in Powershell. You can test if it is available by typing `ssh --help` in Powershell. If it is
installed, you should see some useful output. If it is not installed, you will get an error. If SSH is not available in Powershell, then
you should install MobaXterm from [http://mobaxterm.mobatek.net](http://mobaxterm.mobatek.net). You will want to get the Home edition (Installer edition). However, if Powershell works, you do not need this.


### MacOS

macOS comes with SSH pre-installed, so you should not need to install anything. Use your "Terminal" app.


### Linux

Linux users do not need to install anything, you should be set! Use your terminal application.

The instructions below will guide you through setting up a logging account on {{ machine_name }}.


---

{{  '```{include} ../../substitutions/substitutions_REPLACE/ssh_login.md\n```'.replace("REPLACE",machine_name) }}