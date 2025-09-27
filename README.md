# Installation Script

## Basic Installation

- `setup-alpine`
  - Keymap: `br br`
  - Best repo: `http://mirror.uepg.br/alpine/v3.22/main`, with `community` enabled
  - Time: `America/Sao_Paulo`
  - `none` for all disk installation questions

- `SWAP_SIZE=0 setup-disk -m sys /dev/mmcblk1`

## Primary post-installation

### Video:
```bash
doas setup-wayland-base wayland-protocols
doas apk add xf86-video-intel mesa-dri-galium mesa-va-gallium mesa-vdpau-gallium
```
  - `setup-wayland-base`: Installation script to install Wayland dependences automatically;
  - `wayland-protocols`: HDMI doesnt work without it
  - `xf86-video-intel mesa-dri-galium mesa-va-gallium mesa-vdpau-gallium`: according Alpine's documentation for pre-Broadwell Intel chips.

### Audio:
```bash
doas apk add sof-firmware alsa-utils alsa-lib pipewire pipewire-alsa pipewire-pulse wireplumber
doas apk echo "options snd_sof sof_debug=1" >> /etc/modprobe.d/alsa-base.conf
```
  - `sof-firmware`: Basic audio firmware;
  - `alsa-lib alsa-utils`: Basic ALSA installation with basic lib and basic audio control tools;
  - `pipewire pipewire-alsa pipewire-pulse`: Baisic Pipewire installation, with basic lib, and interaction tools with ALSA and PulseAudio-only apps;
  - alsa-base.conf is a workaround to fix pre-Broadwell Intel chips bug (buzz after some time playing audio)

### Connectivity:
```bash 
doas apk add networkmanager-bluetooth pipewire-spa-bluez networkmanager-wifi
``` 
  - `networkmanager-wifi`: basic Wifi lib
  - `networkmanager-bluetooth`: basic Bluetooth lib
  - `pipewire-spa-bluez`: Needed to audio devices work with via Bluetooth


### DE:
```sh
doas apk add gnome gnome-shell gnome-bluetooth gnome-keyring font-liberation
```
  - `gnome gnome-shell`: Basic GNOME libs
  - `gnome-bluetooth`: to GNOME control Bluetooth
  - `gnome-keyring`: to GNOME control wifi passwords
  - `font-liberation`: default fonts

- Keyboard: `gsettings set org.gnome.desktop.input-sources xkb-model 'chromebook'`

- Essential applications: `doas apk add gnome-console chromium`

- Swap file:
```sh
doas mkswap -U clear --size 2G --file /swapfile
doas fallocate -l 1G /swapfile
doas chmod 600 /swapfile
doas mkswap /swapfile 
doas nvim /etc/fstab
doas rc-service swap start
doas rc-update add swap
```

- Initialization:
```sh
doas rc-update add gdm
doas rc-update add bluetooth
doas rc-update add networkmanager boot
doas rc-update del networking boot
doas rc-update del wpa_supplicant boot
```

## Secondary post-installation


### Basic GNOME APPS suite
```sh
doas apk add gnome-tweaks gnome-extensions-app decibels amberol resources baobab evince loupe
```
- `gnome-tweaks gnome-extensions-app`: to manage GNOME extensions
- `decibel amberol`: basic audio players
- `resources`: system monitor
- `boabab`: disk usage analyzer
- `loupe`: image viewer
- `evince`: file reader

### Basic development suite
```sh
doas apk add curl git python3 nodejs
```
- `curl`: CLI download app
- `git`: source code versioning
- `python3 nodejs`: language interpreters

### Terminal apps
```sh
doas apk add zsh && sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Neovim
```sh
doas apk add font-nerd-fonts-symbols build-base gcompat gcc python3-dev musl-dev linux-headers neovim
git clone https://github.com/LazyVim/starter ~/.config/nvim && rm -rf ~/.config/nvim/.git
```

## Next implementations
- To create an [answer file](https://wiki.alpinelinux.org/wiki/Using_an_answerfile_with_setup-alpine).
- To automatize apps installation. Maybe coupling it with de installation process (IDK if its possible) or to creating an [APK BUILD](https://wiki.alpinelinux.org/wiki/APKBUILD_examples) 
- To delete root user
- To create /home partition with LUKS cryptography