---
layout: post
comments: true
title:  "Automatization using ssh key"
ref: ssh_keys
date:   2020-07-21 08:00:00 +0200
categories: blog automatization
category: blog
lang: en
excerpt: To push/pull to your GitHub/GitLab account and to connect to other remote servers without having to unlock your SSH key every time.
---

I started my blog with a post telling you how to let Git remember your credentials for websites not supporting ssh keys (the post is [here](/blog/automatization/2019/09/09/save-git-credentials.html) for those interested).
In that day, I was struggling with Overleaf Git servers that do not support ssh keys (and still does not at the moment I am writing).
It helped me for this case.
Nevertheless, it is only compatible with Git servers and is not the best way to automatize your identifications (at least in my point of view).
Today we will see how to use ssh keys to automatize many login steps.

In this post we will see two use cases:

- Automatic identification to servers using the ssh protocol.
- Automatic identification when doing push/pull commands on Git servers (such as GitHub or GitLab).

SSH keys contain a public key to encode messages (destined for servers) and a private key to be able to read those messages (destined for the client of the servers).
If you want more information of the protocol, have a look [here](https://www.ssh.com/ssh/public-key-authentication).

The distribution used (and tested) for this tutorial is Kubuntu 20.04 LTS (my new main distribution, but this story is for a future post).

## Prepare the client side

In this section we will prepare the client that will do the identifications to the servers.
We will first create a pair of public and private keys.
Afterwards, we will start the ssh-agent to configure it with the private key.

### Check if you already have a pair of SSH keys

Check the directory listing of `~/.ssh/` to see if you already have a public SSH key.
By default, the filename of the public key is `id_rsa.pub`.
You can use a different pair of keys per server if you want.
I prefer to keep one pair of keys per machine (and change it regularly).

### Generate a pair of SSH keys

To generate a pair of keys linked to an email address (to better identify the connected user) you have to type the following line:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Then, follow the instructions.
You can let the default key path if you do not have a default pair of keys.
You will also set a password to unlock your private key, be sure to remember it.
**Be careful to not revel your private key (the `id_rsa` file) in any case.**

### Start ssh-agent and add your private key

To let your system remember your private key for your session you can use the ssh-agent as follows:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Now we have a pair of private and public key and the ssh-agent configured with it.
We are ready to automatize our SSH identifications to servers or Git servers.

## Automatize your login on servers using SSH

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

## Automatize your authentifications on Git servers

To automatize your identification (typing your login and password) after using a `pull` or `push` command, you can add your public key to your Git server (via their web interface).
To copy your public key on a website (such as GitHub or GitLab) you may want to add your key to the clipboard (to use Ctrl+V inside your web browser).
For this purpose, you need to install `xclip`:

```bash
sudo apt install xclip
```

or if you are on manjaro:

```bash
sudo pamac install xclip
```

Then, it is as simple as this:

```bash
xclip -sel clip < ~/.ssh/id_rsa.pub
```

After, follow the steps from this [link](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account) for GitHub and that [link](https://docs.gitlab.com/ee/ssh/#add-an-ssh-key-to-your-gitlab-account) for GitLab.

Now your configuration is ready for your Git server.
The next subsection will be about testing this configuration without modifying your repositories.

### Test your configuration

To test your new configuration on your Git server, you can use the ssh protocol.
It will avoid doing modifications to one of your Git repository.
The next subsections show you how to do it for different Git servers.

#### GitHub

```bash
ssh -T git@github.com
```

#### GitLab

```bash
ssh -T git@gitlab.com
```

## Keeping ssh-agent identities on Kubuntu 22.04 after restart

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
echo 'ssh-add < /dev/null' >> ~/.config/autostart-scripts/ssh-add.sh
chmod +x ~/.config/autostart-scripts/ssh-add.sh
```

For the next step, type the following command and **check the remember checkbox**:

```bash
~/.config/autostart-scripts/ssh-add.sh
```

It will let the KDE wallet retain the password for your private key and unlocks it after each login.

## Keeping ssh-agent identities on Manjaro after a reboot

After my switch to Manjaro linux ([see my post about my migration](/blog/2022/09/03/my-switch-to-manjaro.html)), I realized that the method for Kubuntu didn't work (and might not work on next LTS). Here is my solution, which is a combination of two approaches that you can find in my sources.

First of all, make sure you have the necessary tools:

```bash
sudo pamac install kwallet ksshaskpass kwalletmanager
```

Next, let's configure our system and zsh to use the appropriate socket for the ssh agent:

```bash
sudo echo '#!/bin/sh' > /etc/profile.d/ssh-askpass.sh
sudo echo 'export SSH_ASKPASS=/usr/bin/ksshaskpass' >> /etc/profile.d/ssh-askpass.sh
echo 'export SSH_AUTH_SOCK="$XDG_RUNTIME_DIR"/ssh-agent.socket' >> ~/.zshrc
```

Next, we create the user directory for systemd:

```bash
mkdir -p ~/.config/systemd/user
```

Next, create the file `~/.config/systemd/user/ssh-agent.service` and fill it with the following content:

```bash
[Unit]
Description=SSH agent (ssh-agent)

[Service]
Type=simple
Environment=SSH_AUTH_SOCK=%t/ssh-agent.socket
Environment=DISPLAY=:0
Environment=KEY_FILE=/home/%u/.ssh/id_rsa
ExecStart=ssh-agent -D -a $SSH_AUTH_SOCK
ExecStartPost=/bin/sleep 3
ExecStartPost=/usr/bin/ssh-add $KEY_FILE
ExecStop=kill -15 $MAINPID

[Install]
WantedBy=default.target
```

This service starts the ssh agent at each login on your machine and adds the private key you created at the beginning of this article.
We will now enable it and run it:

```bash
systemctl --user daemon-reload
systemctl --user enable ssh-agent.service
```

Now you can restart your machine and everything should work 😄. I hope this was helpful 😉.

## Sources and inspirations

- [GitHub Official instructions for SSH keys](https://help.github.com/en/github/authenticating-to-github)
- [GitLab Official instructions for SSH keys](https://docs.gitlab.com/ee/user/ssh.html)
- [Configure ssh-agent on Ubuntu](http://www.linuxproblem.org/art_9.html)
- [Kubuntu and ssh-agent](https://wiki.csnu.org/index.php/Kubuntu_/_KDE_:_login_ssh_automatique_par_cl%C3%A9)
- [Manjaro and ssh-agent (1/2)](https://forum.manjaro.org/t/configuring-ssh-agent-to-autostart-and-automatically-add-ssh-keys-to-it/99715)
- [Manjaro and ssh-agent (2/2)](https://forum.manjaro.org/t/howto-use-kwallet-as-a-login-keychain-for-storing-ssh-key-passphrases-on-kde/7088)

Hope it helps some of you.

Cheers, Vincent
