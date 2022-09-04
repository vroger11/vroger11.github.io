---
layout: post
comments: true
title:  "My switch to Manjaro Linux"
ref: switch-manjaro
date:   2022-09-03 20:00:00 +0200
categories: blog
lang: en
---

I started my KDE adventure with Fedora (for 6 months) where the graphical interface was buggier than KDE Neon where I stayed for a bout a year and finished into Kubuntu LTS with flatpaks apps. It has been three years I was using Kubuntu, while I enjoy some of its aspects (LTS, based on Ubuntu packages, easy to configure and with many resources) my day-to-day experience was inconvenient. Every time I updated my NVIDIA drivers I feared to get unusable GUI (totally black screen with only tty terminals available) because even if all required packages for the NVIDIA driver are not in the repositories, the installation is done by apt. Even worse, those missing files can take 1 week to get in the repositories. Less game changer, but also annoying, snap is forced via apt in Ubuntu distribution (like `apt install firefox` or `apt install chromium` results in a snap install). Finally, having a really outdated KDE environment is not ideal if you want to benefit from last bug fixes of the environment (their team is making good effort to address many bugs and to improve the user experience, you can look into \[Nate’s blog\](https://pointieststick.com/author/pointieststi,ck/) to have a preview of their efforts).

# My requirements

Before telling you why I choose Manjaro KDE, I will expose you my needs:

- a stable distribution that has the necessary applications for all my needs (I can use flatpaks for missing apps), don’t break when I update my drivers and with a minimum of manual maintenance required,

- an easy way to install my system,

- a large community with documentation and wikis,

- I don’t want to reinstall my system each 6 months (it was why I was on Kubuntu LTS),

- a KDE environment up-to-date, but no bleeding-edge without breaks and bugs.

Given these requirements, both Manjaro and openSUSE seem great solutions. I already used Arch/Manjaro documentation and wikis to solve some of my Kubuntu problems (it’s an irony as there was nothing on Ubuntu/Kubuntu forums). Moreover, in 2021 Manjaro seems to be a more popular solution: https://distrowatch.com/index.php?dataspan=2021 (so maybe more people to help in case of problems).

# Experience

As no distribution is perfect, there are great things to take and bad ones to overcome. First the installation process, it is not the best UI out there, but I had no problems or difficulties, everything was smooth. Then comes the first software to install, combining the official packages with flatpak and AUR, I did not install a single package manually. It is a first for me, the software manager look for updates of all my applications (a very handy thing).

Then comes the first troubles to finalize my setup, which I solved with a some research on Arch and Manjaro forums/wikis (I listed them on the last part of this post for anyone curious). Finally, comes the routines and some changes in habits.

Overall, it has been a great experience and I did not regret my switch. I even installed Manjaro on all my computers. It is surely not a beginner distribution (as you have to do little maintenance), but once implemented the user experience is excellent compared to Kubuntu LTS (in terms of performance and stability of the system). If you do not care about the system environment, then PopOS is a great alternative (suitable for beginners and experts). Also I do not have to worry about major upgrades between LTS like with Kubuntu as Manjaro is a rolling release (the downside is the maintenance for some updates). Finally, I am not afraid to update my system, which was my main reason to switch from Kubuntu!

In the two remaining subsections, I expose a more detailed feedback by listing all advantages and disadvantages I have in my day-to-day usage. Then I list the troubles I got and how I solved them (followed by the sources I used to solve those problems).

## Day-to-day usage

### Advantages

- Same interface as in my Kubuntu, but with a more up-to-date plasma (which gave me more up-to-date features from it).

- You can select your kernel and change it easily, with access to last kernels (LTS ones and non-LTS one, which can contain drivers for last pieces of hardware).

- No bleeding-edge plasma version unless it is not stable (compared to KDE Neon), it is a clear win for my needs.

- It runs way smoother on my laptop compared to my Kubuntu with the same battery life!

- The package manager has a similar interface to `apt`: `pamac` (and not the `pacman` manager). Hence, it helps a lot for my transition, plus `pamac` suggests optional packages to install (which can save you time if you miss something).

- Manjaro’s default zsh shell is handy and useful to me out of the box (I even switched from bash to it).

- Large panel of software plus some great out of the box integrations (like the languagetool + texstudio integration if you install both of them).

- NVIDIA updates went all well (I observed it after 10 months of usage).


### Disadvantages

- AUR packages can break on some update. It happens as it is not an official source of packages supported by the Manjaro team (but having access to them is very handy). As I use official packages first, then flatpak apps and as a last resort AUR packages I have few AUR apps, and it is not a dealbreaker for me. Nevertheless, solutions to solve this problem can be:
    
    - Delay an AUR package update to comply with its dependencies from Manjaro’s repositories (as they usually come later in Manjaro than in Arch). I did not need to do this in my usage.
        
    - You may need to manually start a rebuild of an AUR package(`pamac remove <package>` then `pamac build <package>`) after a dependency update from Manjaro’s repositories. It occurred once to me until now.
        
- Some updates can break some of your config files (as format can change with new major releases). It can happen between each Kubuntu LTS upgrade, but now I am on an Arch based distribution, it can occur each update (for each specific config you set on your system). In practice, it only occurred to me once due to a gnome-keyring update (which I hopefully will be able to get rid of with plasma 5.26, as kwallet will implement the missing protocols handled by gnome-keyring). A good habit to solve those kinds of problems is to follow the [official Manjaro releases feed (it is a RSS one)](https://forum.manjaro.org/c/announcements/stable-updates/12). There, they describe the updates with possible problems and most of the time solutions to solve them. As each release is associated to a post on the Manjaro’s forum, you can ask the community for help (if there is not yet a solution 😄).
    
- You can’t use discover to install and update for new applications. I was used to this, and it integrates well in the KDE environment. But on Arch based systems, using it can break your system. Fortunately, there is a similar manager on Manjaro which fills all my needs (search for apps, packages and handle flatpaks). Except it uses a gtk UI instead of a qt one, so it is a minor issue.
    

# Problems I got and solutions:

## Enable Bluetooth devices on the login screen

This feature is quite useful for users of Bluetooth mouses and/or keyboards (to type logins and select users). I think it should be enabled by default, but you can enable it quickly:

```bash
sudo sed -i.back 's/#AutoEnable=false/AutoEnable=true/g' /etc/bluetooth/main.conf
```

## Bluetooth speaker/headphone crackling with poor quality

To address this issue, first disable all power-saving options over Bluetooth devices (only for [tlp users](https://linrunner.de/tlp/index.html)). To do this easily, you can install `tlpui`:

```bash
pamac install tlpui
```

Then, still in tlpui -> audio section, toggle off the “sound power save controller” option. This should improve some crackling, but it was not enough on my side.
You may also look into USB_DENYLIST to add your Bluetooth receptor (even if you are using a laptop or the Bluetooth is integrated with your motherboard).

I combined this solution with replacing pulseaudio by pipewire (which provide low-latency connections, implement more Bluetooth codecs and handle all pulseaudio call as pipewire is compatible with almost all pulseaudio API). More info on pipewire [there](https://pipewire.org/).

To do it under Manjaro, you have to type the following commands:

```bash
pamac remove pulseaudio pulseaudio-jack pulseaudio-lirc pulseaudio-rtp pulseaudio-zeroconf pulseaudio-bluetooth pulseaudio-alsa pulseaudio-ctl manjaro-pulse plasma-pa
pamac install manjaro-pipewire
pamac install plasma-pa
```

As I am writing this, we need to remove `plasma-pa` first to be able to remove pulseaudio. We reinstall it once pipewire is installed to be able to control audio via the KDE interface.

Then restart your computer and it is done. All applications using previously pulseaudio should work with better latency and using better codecs 😄.

## Address Bluetooth devices lags like Bluetooth mouse lags

First, if you are using TLP disable powersave for your device using tlpui (look at the USB_DENYLIST to add your Bluetooth receptor). Then, to address this issue, we’re going to set a service that reduces Bluetooth latency on the kernel level for all devices. Finally, we will discuss Bluetooth codecs for speakers and their interference with other Bluetooth devices.

### Set a service to force low latency
To do so, we need to add the following script in `/etc/systemd/system/fix-mouse-lag.service` to create our service:

```bash
[Unit]
Description=run root script at boot/wake to fix mouse lag
Before=bluetooth.service

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/bin/sleep 2
ExecStart=/usr/local/bin/fix-mouse-lag.sh

[Install]
WantedBy=bluetooth.service
```

Then, add the following script in `/usr/local/bin/fix-mouse-lag.sh` that will be run by our service on every boot/wake:

```bash
#!/bin/sh 

echo 0 > /sys/kernel/debug/bluetooth/hci0/conn_latency
echo 6 > /sys/kernel/debug/bluetooth/hci0/conn_min_interval
echo 7 > /sys/kernel/debug/bluetooth/hci0/conn_max_interval
```

Finally, let us enable and start our service:

```bash
sudo chown root:root /etc/systemd/system/fix-mouse-lag.service
sudo chmod a+rx /usr/local/bin/fix-mouse-lag.sh
sudo systemctl enable fix-mouse-lag.service --now
```

### Selecting a Bluetooth codec for my headset

I own a Bose QC 35 II, and on paper it can only be used with HSP/HFC, AAC and SBC codec. The first one being a low-quality codec that let use the microphone of the headset, and the last ones are for high-quality audio output only. With my testing, I found out it can also handle SBC XQ codec (which is a better codec compared to AAC and SBC, see [this link](http://soundexpert.org/articles/-/blogs/audio-quality-of-sbc-xq-bluetooth-audio-codec) for details).

I tested these three high-quality codecs with a Logitech MX Vertical and I found out that while SBC and SBC XQ codecs do not disrupt my mouse connection, the AAC codec does. Pipewire use the AAC codec by default, as Bose recommend it for this headphone[^1]. To modify that behavior, we have to edit `/usr/share/pipewire/media-session.d/bluez-monitor.conf` file.
In it, I  uncommented the `bluez5.enable-sbc-xq    = true` line and specified the `bluez5.codecs` as follows:

```bluez5.codecs = [ sbc_xq ldac aptx aptx_hd aptx_ll aptx_ll_duplex faststream faststream_duplex ]```

Hence, I removed the SBC and AAC capacity of Pipewire to be sure it will not use those codecs with my devices (the first one is not a problem, but I prefer higher quality for my headset 😛).

[^1]: Note I talk only for the pipewire case. I did not test it for pulseaudio as I have crackling issues with it.

## Fix themes not working with flatpak apps

Most flatpak apps will not have their theme following the system one on Manjaro KDE. Specifically gtk and electron apps. If it is not addressed upstream on Arch, Manjaro devs will not fix it (see my sources for more details). To address the issue, we need to type the following commands:

```bash
flatpak override --filesystem=xdg-config/gtk-3.0:ro
flatpak override --filesystem=xdg-config/gtk-4.0:ro
```

It adds the minimum permissions to flatpak apps needed by them to see the theme used under the KDE environment. Then, you must install the corresponding flatpak GTK theme to the one you are using under your system. For example, if you are using the breeze-dark theme under KDE, you have to install the flatpak theme version for gtk applications like this:

```bash
flatpak -y install org.gtk.Gtk3theme.Breeze-Dark
```

# Sources

- [AUR usage in Manjaro](https://forum.manjaro.org/t/howto-use-aur/116934)
    
- [Enable Bluetooth at startup](https://archived.forum.manjaro.org/t/enable-bluetooth-at-login-screen-system-boot/146842)
    
- [Reduce mouse input lags](https://archived.forum.manjaro.org/t/bluetooth-mouse-lag/99386/43)
    
- [Address flatpak theming issue (by your servant)](https://forum.manjaro.org/t/add-out-of-the-box-flatpak-gtk-application-themes-for-kde-plasma-users/117103)
