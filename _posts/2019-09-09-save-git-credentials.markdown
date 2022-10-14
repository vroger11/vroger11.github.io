---
layout: post
comments: true
title:  "Save Git Credentials"
ref: git_credentials
date:   2019-09-09 19:30:00 +0200
categories: blog dev
lang: en
---

# What are git credentials?

Git credentials are the username and password you used to log into a repository on a server.
Saving them allows you to avoid typing your username and password when required (mainly pull and push operations).

# Why saving git credentials?
The use of SSH and GPG keys is available on most popular git servers such as on GitLab or on GitHub websites.
It allows the user to avoid typing their username and password.

Some website does not support this feature (overleaf git server for instance). Also, if you are like me, you use a multitude of git servers (and configuring each SSH keys become a pain).

# Save git credentials

To save the credentials of every repository you log into, just type this:

```bash
git config credential.helper store
```

And you are good to go.
You will be asked only once for each repository.
Afterwards, git will fill automatically the credentials for each repository.

# Cache expiration

For security reasons (especially if you are not on your personal computer), it is better that the stored credentials expire after a certain amount of time.
Let's say you want that delay being 4 hours.
You just have to type:

```bash
git config --global credential.helper 'cache --timeout 14400'
```

14400 is then the time in seconds (our 4 hours).

Hope it helps some of you.

Cheers, Vincent.

