##### `yum`이 안될 때

- `dhclient`

### GUI로 바꾸기

- `yum groupinstall "x window system" "gnome desktop environment"`
- `startx`

- 재부팅
- `ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target`

- `reboot`

- 아니면 그냥 깔때 GUI 넣고 깔기

