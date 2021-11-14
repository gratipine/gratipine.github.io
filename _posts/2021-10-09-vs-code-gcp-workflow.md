---
layout: page
title: "How to get VS Code working with GCP"
date: 2021-10-09 19:59:00 -0000
categories: how-to
---
## Overview
There are multiple ways to do data science. 
This is true both in the case of analysis (lots of ways to answer your questions) as it is in terms of workflows.

There are two components of a workflow:
- where you write the code
- where the code gets executed.

For a lot of simple tasks you can execute locally and write in whichever editor (including 
Notepad) you feel most comfortable using. For heavier processing you might want to consider switching 
to a cloud provider for the code execution.

Here we will cover how to structure your workflow so that the code executes in a Google Cloud instance
and is written within Visual Studio Code.

**WARNING:** Just because it is written on the internet does not mean that it is allowed in your school or organization.
Please check for security risks before executing. Especially check the configs at the end - it is possible that some
options leave you vulnerable to attacks.

## Requirements
You need to have installed:
- [Visual Studio Code](https://code.visualstudio.com/Download)
- [Python](https://www.python.org/downloads/)
- [Cloud SDK](https://cloud.google.com/sdk/docs/install)

## Steps
### Authenticate against Google
In a command window run the following command. Follow the prompts. 
```bash
gcloud auth login
gcloud config set project PROJECT_ID
```
### Install extension for remote work in VS Code
1. Open VS Code
2. Open the extensions panel
On Windows Ctrl + Shift + X gives you the extensions on the side. 
On Macs Command key + Shift + X works.
3. Look for Remote Development by Microsoft and install it.

### Make an instance within a project.
This assumes that you already have a google cloud account and a project within it.
If you do not you will have to set those up.
To work with google cloud you will need a google address + a card / some payment method to set a trial up.
- [Getting started with GCP](https://console.cloud.google.com/getting-started?pli=1)
- [Creating a project](https://cloud.google.com/appengine/docs/standard/nodejs/building-app/creating-project)

Make an instance to work on in Google cloud.
It can be a notebook instance or a regular one. 
If you do want the option of the standard DS workflow (Jupyter Lab within GCP) make one from within the Notebooks.

You can find the general documentation [here](https://cloud.google.com/compute/docs/instances/create-start-instance).
For the Notebook instance you can read [here](https://cloud.google.com/notebooks/docs/create-new).

### Connect to the instance via a IAP tunnel
This is one of the easiest ways to get keys for the instance locally.
To do that open a command window and type the following command:
```bash
gcloud compute ssh INSTANCE_NAME --zone ZONE
```
Take note of the name specified in the window that pops up. 
The output could look like:
```bash
Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: time from place
USER_NAME@INSTANCE_NAME:~$ 
```

### Create a connection config in the config file of VSCode
Create a config file in which the configs for remote machines are stored.
The private + public keys are created in the same folder.
In the case of Windows everything is in the user path + .ssh.
Typically looks something like this - C:/Users/USERNAME/.ssh. The USERNAME
here is the one you have locally on your laptop, rather than the one on the cloud instance.

We need to make an adjustment to the config file without an extension there.
If you do not have one and cannot make one without extension (for whatever reason),
the easiest way to do that is to try to connect to some host.

Click on the green square with >< symbol at the bottom left panel. A window comes up on the top.
Type 
```bash
ssh trial
```
This will create a config file for you. 
Open the file and delete the unnecessary connection. 
Put the following config in it. 

The ID you can find in the .ssh/google_compute_known_hosts. You do need to keep the "compute." part in.

The path_to_google_cloud_executable can look something like C:/Program Files/Google/Cloud SDK/google-cloud-sdk/lib/gcloud.py


```bash
Host gcp-vm
    HostName compute.ID
    IdentityFile the_path_to_the_ssh_google_compute_engine_file_locally
    CheckHostIP no
    HostKeyAlias compute.ID
    IdentitiesOnly yes
    StrictHostKeyChecking yes
    UserKnownHostsFile path_to_ssh_google_compute_know_hosts_file_locally
    ProxyCommand path_to_python_executable -S "path_to_google_cloud_executable" compute start-iap-tunnel INSTANCE_NAME %p --listen-on-stdin --project=PROJECT_NAME --zone=ZONE --verbosity=warning 
    ProxyUseFdpass no
    User USER_NAME
```

### Try to connect
Start the instance in UI.
At the bottom left pane you have the green square. Press that and from the menu select "Connect to Host".
This will open a new window, in which your connection will exist. Whatever you execute against that window 
is now executed on GCP. 

### Lastly
You will need to install everything on the instance that does not come with the 
instance. That includes the extensions of VS Code you might want to work with 
(Python, R, linting, etc.) and any software (Python or R packages).

### Troubleshooting
#### The general case
If any of these steps end up being a blocker try restarting your computer.
Some installed programs do not work unless there has been a restart.

The progamming world is a friendly place. Try looking around google for 
someone who has had a similar problem like you. The more concrete the error message 
you have, the better. A quick google can save a lot of trouble. 

If everything works up to the point when you try to connect, the output can be informative when 
trying to diagnose why you cannot connect.

#### Cannot find from collections Mapping
I experienced this error when working with Python 3.10.
Python 3.9.7 worked fine.

#### Permission denied (public key)
Delete the config files and keys, restart Visual Studio Code, restart the process.
It is possible that something got corrupted / changed from the time you 
created the keys and the time you were trying to use them. Restart from the 
Connect to the instance via a IAP tunnel.