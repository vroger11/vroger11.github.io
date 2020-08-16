---
layout: post
comments: true
title:  "Automatization using ssh key"
ref: ssh_keys
date:   2020-07-21 08:00:00 +0200
categories: blog automatization
lang: en
---

I started my blog with a post telling you how to let Git remember your credentials for websites not supporting ssh keys (the post is [here](/tips/dev/2019/09/09/save-git-credentials.html) for those interested).
In that day, I was struggling with Overleaf Git servers that do not support ssh keys (and still does not at the moment I am writing).
It helped me for this case.
Nevertheless, it is only compatible with Git servers and is not the best way to automatize your identifications (at least in my point of view).
Today we will see how to use ssh keys to automatize many login steps. 

In this post we will see two use cases:
* Automatic identification to servers using the ssh protocol.
* Automatic identification when doing push/pull commands on Git servers (such as GitHub or GitLab).

SSH keys contain a public key to encode messages (destined for servers) and a private key to be able to read those messages (destined for the client of the servers). 
If you want more information of the protocol, have a look [here](https://www.ssh.com/ssh/public-key-authentication).

The distribution used (and tested) for this tutorial is Kubuntu 20.04 LTS (my new main distribution, but this story is for a future post).

# Prepare the client side

In this section we will prepare the client that will do the identifications to the servers.
We will first create a pair of public and private keys.
Afterwards, we will start the ssh-agent to configure it with the private key.

## Check if you already have a pair of SSH keys

Check the directory listing of `~/.ssh/` to see if you already have a public SSH key.
By default, the filename of the public key is `id_rsa.pub`.
You can use a different pair of keys per server if you want.
I prefer to keep one pair of keys per machine (and change it regularly).

## Generate a pair of SSH keys

To generate a pair of keys linked to an email address (to better identify the connected user) you have to type the following line:
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
Then, follow the instructions.
You can let the default key path if you do not have a default pair of keys.
You will also set a password to unlock your private key, be sure to remember it.
**Be careful to not revel your private key (the `id_rsa` file) in any case.**

## Start ssh-agent and add your private key

To let your system remember your private key for your session you can use the ssh-agent as follows: 
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Now we have a pair of private and public key and the ssh-agent configured with it.
We are ready to automatize our SSH identifications to servers or Git servers.

# Configure your SSH servers

To automatize SSH password typing, you can add the public key to the authorized keys of your servers.
Before that, make sure that your remote user space contains a `~/ssh` folder:
```bash
ssh <login>@<server_adress> mkdir -p .ssh
```

Then you have to add your public key into the authorized keys file of your server:
```bash
cat ~/.ssh/id_rsa.pub | ssh <login>@<server_adress> 'cat >> .ssh/authorized_keys'
```

Now your server is configured with your ssh pair of keys.
You can repeat those two steps for each server you can access.

# Configure your Git servers

To automatize your identification (typing your login and password) after using a pull or push command, you can add your public key to your Git server (via their web interface).
To copy your public key on a website (such as GitHub or GitLab) you may want to add your key to the clipboard (to use Ctrl+V inside your web browser).
For this purpose, you need to install `xclip`:
```bash
sudo apt install xclip
```

Then, it is as simple as this:
```bash
xclip -sel clip < ~/.ssh/id_rsa.pub
```

After, follow the steps from this [link](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account) for GitHub and that [link](https://docs.gitlab.com/ee/ssh/#adding-an-ssh-key-to-your-gitlab-account) for GitLab.

Now your configuration is ready for your Git server.
The next subsection will be about testing this configuration without modifying your repositories.

## Test your configuration

To test your new configuration on your Git server, you can use the ssh protocol.
It will avoid doing modifications to one of your Git repository.
The next subsections show you how to do it for different Git servers.

### GitHub

```bash
ssh -T git@github.com
```
### GitLab

```bash
ssh -T git@gitlab.com
```

# Retain ssh-agent identities on Kubuntu after restart

With the above instructions, you have to (in Kubuntu at least) reconfigure the ssh-agent after each logout or restart.
In this section, we will use kwallet to bypass this limitation.

First, we have to install the `ssh-askpass` package:
```bash
sudo apt install ssh-askpass
```

Then, we have to create a script that will automatically unlock your private key when logged in.
To achieve this, type the following lines:
```bash
mkdir -p ~/.config/autostart-scripts
echo '#!/bin/sh' > ~/.config/autostart-scripts/ssh-add.sh
echo 'export SSH_ASKPASS=/usr/bin/ksshaskpass' >> ~/.config/autostart-scripts/ssh-add.sh
echo 'ssh-add </dev/null' >> ~/.config/autostart-scripts/ssh-add.sh
chmod +x ~/.config/autostart-scripts/ssh-add.sh
```

For the next step, type the following command and **check the remember checkbox**:
```bash
~/.config/autostart-scripts/ssh-add.sh
```
It will let the KDE wallet retain the password for your private key and unlocks it after each login.

# Sources and inspirations

* [GitHub Official instructions for SSH keys](https://help.github.com/en/github/authenticating-to-github)
* [GitLab Official instructions for SSH keys](https://docs.gitlab.com/ee/ssh/)
* [Configure ssh-agent on Ubuntu](http://www.linuxproblem.org/art_9.html)
* [Kubuntu and ssh-agent](https://wiki.csnu.org/index.php/Kubuntu_/_KDE_:_login_ssh_automatique_par_cl%C3%A9)

Hope it helps some of you.

Cheers, Vincent
