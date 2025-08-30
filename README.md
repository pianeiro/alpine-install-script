# Installation Script

## Basic Installation

- `setup-alpine`
  - Keymap: `br br`
  - Best repo: `http://mirror.uepg.br/alpine/v3.22/main`, with `community` enabled
  - Time: `America/Sao_Paulo`
  - `none` for all disk installation questions

- `SWAP_SIZE=0 setup-disk -m sys /dev/mmcblk1`

## Primary post-installation

- Video:
```sh
doas setup-wayland-base
doas apk add xf86-video-intel mesa-dri-galium mesa-va-gallium mesa-vdpau-gallium
```

- Audio:
`doas apk add sof-firmware alsa-utils alsa-lib pipewire pipewire-alsa pipewire-pulse wireplumber`
`doas apk echo "options snd_sof sof_debug=1" >> /etc/modprobe.d/alsa-base.conf`

- Connectivity: `doas apk add networkmanager-bluetooth networkmanager-wifi`

- DE: `doas apk add gnome gnome-shell gnome-bluetooth gnome-keyring font-liberation`

- Keyboard: `gsettings set org.gnome.desktop.input-sources xkb-model 'chromebook'`

- Essential applications: `doas apk add gnome-console chromium`

- Swap file:
```sh
doas mkswap -U clear --size 1G --file /swapfile
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
Insert on `/etc/gdm/custom.conf`:
```ini
[daemon]
WalandEnable=false
```

## Secondary post-installation

- `doas apk add gnome-tweaks gnome-extensions-app decibels amberol gnome-system-monitor evince loupe fastfetch curl git python3 nodejs`

- `doas apk add zsh && sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

- `doas apk add font-nerd-fonts-symbols build-base gcompat gcc python3-dev musl-dev linux-headers neovim
git clone https://github.com/LazyVim/starter ~/.config/nvim && rm -rf ~/.config/nvim/.git`

- To delete root user
- To create /home partition with LUKS cryptography

