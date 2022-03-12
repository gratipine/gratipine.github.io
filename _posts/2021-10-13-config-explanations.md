---
title: "Config explanations for VS code"
date: 2021-10-13 19:59:00 -0000
categories: how-to
# https://www.pexels.com/photo/person-using-gray-laptop-3861964/
# ![Photo by ThisIsEngineering from Pexels](./../../../../assets/2021-10-13.jpg)
---

## Overview

Here is a short overview of what is in the config file when connecting to a google cloud machine and why 
([post here]({{ site.baseurl }}{% link _posts/2021-10-09-vs-code-gcp-workflow.md %})).
This info comes from the documentation about the ssh protocol and 
is not specific to either VS Code or Google Cloud. 

You do not have to understand this unless:
- you are curious
- you have hit a snag 

From the description: 

*The SSH protocol (also referred to as Secure Shell) is a method for secure remote login from one computer to another. It provides several alternative options for strong authentication, and it protects the communications security and integrity with strong encryption. It is a secure alternative to the non-protected login protocols (such as telnet, rlogin) and insecure file transfer methods (such as FTP).*

Given the emphasis on security there are lots of configurations to make the 
connections more secure. You can read about all of them on the
[Ubuntu page](https://manpages.ubuntu.com/manpages/focal/en/man5/ssh_config.5.html)
and in the [ssh config page](https://www.ssh.com/academy/ssh/config).

## Details
### Config for instance connection
Here is the connection file we used to connect to Google Cloud.
We'll walk through what each of those params does.
```bash
Host gcp-vm
    HostName compute.ID
    IdentityFile the_path_to_the_ssh_google_compute_engine_file
    CheckHostIP yes
    HostKeyAlias compute.ID
    IdentitiesOnly yes
    StrictHostKeyChecking yes
    UserKnownHostsFile path_to_ssh_google_compute_know_hosts_file
    ProxyCommand path_to_python_executable -S "path_to_google_cloud_executable" compute start-iap-tunnel INSTANCE_NAME %p --listen-on-stdin --project=PROJECT_NAME --zone=ZONE --verbosity=warning 
    User USER_NAME
```
### HostName
Can be:
- numeric IP address (an external one)
- a predefined alias or abbreviation given to the host

You need to figure out what the name of the machine you are connecting to is. 
In the google instance example the easiest way to get the server name of the
instance is to connect using the name you gave it upon creation and a IAP tunnel.

### IdentityFile
When using a public key authentication you need to specify in this line where the 
private key is stored on the local machine.

### CheckHostIP
If set to yes (the default), ssh(1) will additionally check the host IP address in
the known_hosts file.  This allows it to detect if a host key changed due to DNS
spoofing and will add addresses of destination hosts to ~/.ssh/known_hosts in the
process, regardless of the setting of StrictHostKeyChecking.  If the option is set
to no, the check will not be executed.

### HostKeyAlias
From the documentation - Specifies an alias that should be used instead of the real
host name when looking up or saving the host key in the host key database files.
You don't have to set this up, but it can help if you have multiple machines you 
are connecting to, such as work, projects, school, etc.

### IdentitiesOnly
Specifies that ssh(1) should only use the configured authentication identity and
certificate files (either the default files, or those explicitly configured in the
ssh_config files or passed on the ssh(1) command-line), even if ssh-agent(1) or a
PKCS11Provider or SecurityKeyProvider offers more identities.  The argument to this
keyword must be yes or no (the default).  This option is intended for situations
where ssh-agent offers many different identities.

### StrictHostKeyChecking
If this flag is set to yes, ssh(1) will never automatically add host keys to the
~/.ssh/known_hosts file, and refuses to connect to hosts whose host key has changed.
This provides maximum protection against man-in-the-middle (MITM) attacks, though it
can be annoying when the /etc/ssh/ssh_known_hosts file is poorly maintained or when
connections to new hosts are frequently made.  This option forces the user to
manually add all new hosts.
If this flag is set to “accept-new” then ssh will automatically add new host keys to
the user known hosts files, but will not permit connections to hosts with changed
host keys.  If this flag is set to “no” or “off”, ssh will automatically add new
host keys to the user known hosts files and allow connections to hosts with changed
hostkeys to proceed, subject to some restrictions.  If this flag is set to ask (the
default), new host keys will be added to the user known host files only after the
user has confirmed that is what they really want to do, and ssh will refuse to
connect to hosts whose host key has changed.  The host keys of known hosts will be
verified automatically in all cases.

### UserKnownHostsFile
Can be one or more. If more than one, separate them by white space.
They hold keys for each host. 

### ProxyCommand
Specifies what command you are using to connect to the server. Can be just "ssh" or 
something more involved, like the case of the google cloud instance.

### User
What user you are connecting as. If you type user123, the machine on the other side will
expect to have an account for user123. If that account does not exist, then the connection
will fail.

## Sources:
- [SSH documentation](https://www.ssh.com/academy/ssh/protocol)
- [Ubuntu documentation](https://manpages.ubuntu.com/manpages/focal/en/man5/ssh_config.5.html)