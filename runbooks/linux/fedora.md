# Fedora environment

Last tested on Fedora 33.

## Installation

Load Live CD and select "Try Fedora".

Open Settings -> Mouse & Touchpad and enable Tap to Click.

Connect to WIFI.

Open terminal.

Optionally delete previous linux bootloader:

```bash
sudo efibootmgr -v
sudo efibootmgr -b xxxx -B
```

Optionally remove/shrink partitions:

```bash
sudo dnf install gparted
sudo gparted
```

Run "Install to Hard drive".

Select custom partitioning, change "LVM" to "Standard Partition" and click "Click here to create them automatically".

## Basic Terminal

Open Terminal -> Preferences. Set:

- General
  - Enable the menu accelerator = off
- Profiles -> Unnamed
  - Text
    - Custom Font -> size = 14 (15 on laptop)
  - Colors
    - Use transparent background ~ 15%

## System update

```bash
sudo dnf update --refresh
sudo dnf autoremove
```

## Host name

Set host name:

```bash
sudo hostnamectl set-hostname NEW_HOSTNAME
```

## Remove bloatware

```bash
sudo dnf remove cheese orca rhythmbox
```

## Package managers

Install `fedora-workstation-repositories` to enable 3rd party repos:

```bash
sudo dnf install fedora-workstation-repositories
```

Enable RPM Fusion (free):

```bash
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
```

Enable additional repositories:

```bash
sudo dnf config-manager --enable rpmfusion-nonfree-steam
sudo dnf config-manager --enable google-chrome
```

Optionally enable RPM Fusion repos:

```bash
sudo dnf config-manager --enable rpmfusion-nonfree-nvidia-driver
```

Install flatpak and add Flathub:

```bash
sudo dnf install flatpak
flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

## System tools

Install CLI utilities:

```bash
sudo dnf install curl mc ripgrep tmux vim wget
```

Install system diagnostic tools:

```bash
sudo dnf install htop iotop neofetch net-tools qdirstat strace stress sysstat
sudo pip3 install s-tui
```

## Advanced Terminal

- Profiles -> default -> General:
  - font = Monospace Regular 14 (15 on laptop)
  - copy on selection = on

Run `ibus-setup`, go to Emoji and delete keyboard shortcuts.

## Basic Gnome

Open settings:

- Privacy
  - File History & Trash
    - Automatically Delete Trash Content = on
    - Automatically Delete Temporary Files = on
  - Screen Lock
    - Show Notifications = off
- Power
  - Automatic suspend = on
    - Plugged In = on
  - Power Button Action = suspend
- Displays -> Night Light
  - Schedule = Manual
  - Times = 22:00 - 06:00
  - Color Temperature = 1/3
- Mouse & Touchpad
  - Touchpad Speed = 75%
  - Tap to Click = on
- Keyboard Shortcuts
  - Settings = Super + I
  - Hide all normal windows = Super + D
  - Home Folder = Super + E
  - Switch windows = Alt + Tab
  - Switch applications = Super + Tab
  - Add
    - Launch terminal
    - `ptyxis --new-window`
    - Ctrl+Alt+T
- Region & Language
  - Formats -> select Russian
  - Input sources -> Add Russian

Install Gnome Tweaks (in Software).

- Appearance -> Themes -> Applications -> Adwaita-dark
- Keyboard & Mouse -> Additional Layout Options
  - Switching to another layout -> Caps Lock to first layout; Shift+Caps Lock to last layout
  - Key to choose the 3rd level -> disable Right Alt
- Top Bar
  - Activities Overview Hot Corner -> Off

Open Preferences in file manager:

- Sort folder before files = on

## Basic Visual Studio Code

Download and install Visual Studio Code.

Add the following to `.bashrc`, if vscode is installed using flatpack:

```bash
alias code='flatpak run com.visualstudio.code'
```

Setup it using [this manual](../vscode.md).

## Firefox

Open Preferences in Firefox:

- General
  - Restore previous session = on
  - Default zoom = 120% (on HiDPI screen only)
- Home
  - Homepage and new windows = Firefox Home (Default)
- Search
  - Default search engine = Google
- Privacy & Security
  - Ask to save logins and passwords = off

## Git

```bash
sudo dnf install git git-credential-libsecret
```

Configure git:

```bash
git config --global user.name "Sergei Kogan"
git config --global user.email foo@gmail.com
git config --global credential.helper libsecret
```

## Environment Repo

```bash
mkdir ~/projects
cd ~/projects
git clone https://github.com/sekogan/environment.git
code environment
```

## OneDrive

```bash
sudo dnf install onedrive
mkdir -p ~/.config/onedrive/accounts/svkogan78@gmail.com
echo /secrets >~/.config/onedrive/accounts/svkogan78@gmail.com/sync_list
```

If there is one account:

```bash
onedrive --synchronize --resync
systemctl --user enable onedrive
systemctl --user start onedrive
journalctl --user-unit=onedrive -f
```

If there are multiple accounts:

```bash
onedrive --confdir=/home/sekogan/.config/onedrive/accounts/svkogan78@gmail.com --synchronize --resync
onedrive --confdir=/home/sekogan/.config/onedrive/accounts/svkogan78@gmail.com --monitor -v
```

```bash
sudo cp /usr/lib/systemd/user/onedrive{,-svkogan78}.service
sudo vi /usr/lib/systemd/user/onedrive-svkogan78.service
```

```config
ExecStart=/usr/bin/onedrive --monitor --confdir=/home/sekogan/.config/onedrive/accounts/svkogan78@gmail.com
```

```bash
systemctl --user enable onedrive-svkogan78
journalctl --user-unit=onedrive-svkogan78 -f
echo alias onedrivelog=\'journalctl --user-unit=onedrive-svkogan78 -f\' >>~/.bashrc
source ~/.bashrc
```

## KeePass

```bash
sudo dnf install keepassxc
```

Start KeePassXC. Open the password database in `~/OneDrive/secrets/`.

## Github access

Login into github.com in the browser.
Create a new PAT on GitHub (Avatar -> Settings -> Developer settings -> Personal access tokens
-> Generate new token).

```bash
cd ~/projects/environment
git push
user: sekogan
password: PAT (use special button in Github UI to copy)
```

Test that git doesn't ask password anymore:

```bash
git push
```

Open this file and fix encountered mishaps.

## Install essential packages

```bash
sudo dnf install gimp meld telegram-desktop transmission vlc
```

Open Telegram and enable Night mode. Then go to Settings:

- Interface scale = 250% (on laptop only)
- Notifications
  - Include muted chats in unread count = off
- Advanced
  - Launch Telegram = on
  - Launch minimized = on

Sometimes telegram doesn't start. Run Tweaks and add it to startup applications.

Open VLC, go to Preferences:

- Interface:
  - Show systray icon = off
  - Pause on the last frame = on
- Hotkeys:
  - Short backward jump = Left
  - Short forward jump = Right
  - Cycle subtitle track = s
  - Cycle audio track = a

## Audio

```bash
sudo dnf install pavucontrol
flatpak install --user flathub com.spotify.Client
```

## Google Chrome

```bash
sudo dnf install google-chrome-stable
```

Open Settings:

- Passwords
  - Offer to safe = off
- Appearance
  - Page zoom = 125% (on HiDPI screen only)

## Advanced Gnome

Run Clocks and add world clocks.

Run Weather and select current location.

Optionally add online accounts:

- Online accounts -> Add Google

Remove `Ctrl+Alt+Up` and `Ctrl+Alt+Down` shortcuts:

```bash
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-up "['<Super>Page_Up']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-down "['<Super>Page_Down']"
```

Install helpers that allows to install extensions in the browser:

- Native connector:

  ```bash
  sudo dnf install chrome-gnome-shell
  ```

- [Firefox extension](https://addons.mozilla.org/en/firefox/addon/gnome-shell-integration/).

Install Extensions app:

```bash
sudo dnf install gnome-extensions-app
```

Install essential extensions:

- [caffeine](https://extensions.gnome.org/extension/517/caffeine/)
  - open settings and disable everything except "Show Caffeine in top panel"
- [gravatar](https://extensions.gnome.org/extension/1015/gravatar/)
  - open settings and enter email
- [Hide Keyboard Layout](https://extensions.gnome.org/extension/2848/hide-keyboard-layout/)
- [hide-activities-button](https://extensions.gnome.org/extension/744/hide-activities-button/)
- [remove-alttab-delay](https://extensions.gnome.org/extension/1403/remove-alttab-delay/)
- [Guillotine](https://extensions.gnome.org/extension/3981/guillotine/)
  - `cp ~/projects/environment/runbooks/linux/fedora/.config/guillotine.json ~/.config/`
- [Primary Input on LockScreen](https://extensions.gnome.org/extension/4727/primary-input-on-lockscreen/)

Install optional extensions:

- [autohide-battery](https://extensions.gnome.org/extension/595/autohide-battery/) (removes battery icon when on AC power)
- [bing-wallpaper-changer](https://extensions.gnome.org/extension/1262/bing-wallpaper-changer/)
- [block-caribou](https://extensions.gnome.org/extension/1326/block-caribou/) (blocks on-screen keyboard)
- [cpu-power-manager](https://extensions.gnome.org/extension/945/cpu-power-manager/)
- [hide-top-bar](https://extensions.gnome.org/extension/545/hide-top-bar/)
- [icon-hider](https://extensions.gnome.org/extension/351/icon-hider/) (removes any item from the top bar including its own icon)
- [impatience](https://extensions.gnome.org/extension/277/impatience/) (set 0.30)
- [remove-audio-device-selection-dialog](https://extensions.gnome.org/extension/1482/remove-audio-device-selection-dialog/)
- [remove-dropdown-arrows](https://extensions.gnome.org/extension/800/remove-dropdown-arrows/)
- [sound-output-device-chooser](https://extensions.gnome.org/extension/906/sound-output-device-chooser/)
- [transparent-top-bar](https://extensions.gnome.org/extension/1708/transparent-top-bar/) (makes the top bar transparent if there are no windows near it)
- [workspace-grid](https://extensions.gnome.org/extension/484/workspace-grid/)

Remove unwanted applications from Dock.

## Monitoring tools

Install lm-sensors (required by freon):

```bash
sudo dnf install lm_sensors
sudo sensors-detect --auto
```

Install [freon](https://extensions.gnome.org/extension/841/freon/).

TODO: install GPU monitoring tools: nvtop and intel gpu tools.

## Undervolting

Install undervolting tool:

```bash
sudo pip3 install undervolt
```

Create file:

```bash
sudo vi /etc/systemd/system/undervolt.service
```

```ini
[Unit]
Description=undervolt
After=suspend.target
After=hibernate.target
After=hybrid-sleep.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/undervolt -v --gpu -120 --core -120 --cache -120 --uncore -120 --analogio -120 -t 97

[Install]
WantedBy=multi-user.target
WantedBy=suspend.target
WantedBy=hibernate.target
WantedBy=hybrid-sleep.target
```

Check that the script works and enable service:

```bash
sudo systemctl enable undervolt
sudo systemctl start undervolt
```

## AMD GPU fan control

Only if an AMD GPU is present:

```bash
lspci | grep VGA
```

Install the fan controller:

```bash
sudo pip3 install git+https://github.com/sekogan/amdgpu-fan.git
```

Create configuration file `/etc/amdgpu-fan.yml`:

```bash
sudo vi /etc/amdgpu-fan.yml
```

```yaml
# Fan Control Matrix. [<Temp in C>,<Fanspeed in %>]
speed_matrix:
- [0, 0]
- [50, 0]
- [55, 40]
- [65, 60]
- [75, 100]

temp_drop: 5
```

Create systemd service `/etc/systemd/system/amdgpu-fan.service`:

```bash
sudo vi /etc/systemd/system/amdgpu-fan.service
```

```ini
[Unit]
Description=amdgpu fan controller

[Service]
ExecStart=/usr/local/bin/amdgpu-fan
Restart=always

[Install]
WantedBy=default.target
```

Enable and start:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now amdgpu-fan
```

## Intel HD Audio

Only if an Intel HD Audio card is present:

```bash
lspci | grep Audio
```

Disable power saving mode immediately:

```bash
echo 0 | sudo tee /sys/module/snd_hda_intel/parameters/power_save
```

Make it persistent by creating `/etc/modprobe.d/sound.conf`:

```bash
sudo vi /etc/modprobe.d/sound.conf
```

```text
options snd-hda-intel power_save=0
```

## Fan control on Dell laptop

NOTE: not tested on Fedora!

Disable fan controller in BIOS (Dell laptops only):

```bash
cd ~/projects
git clone https://github.com/TomFreudenberg/dell-bios-fan-control.git
cd dell-bios-fan-control
make
sudo ./dell-bios-fan-control 1
sudo ./dell-bios-fan-control 0
sudo cp dell-bios-fan-control /usr/local/bin
```

Create file:

```bash
sudo vi /etc/systemd/system/bios_fan_control.service
```

```bash
[Unit]
Description=undervolt
After=suspend.target
After=hibernate.target
After=hybrid-sleep.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/dell-bios-fan-control 0
ExecStop=/usr/local/bin/dell-bios-fan-control 1

[Install]
WantedBy=multi-user.target
WantedBy=suspend.target
WantedBy=hibernate.target
WantedBy=hybrid-sleep.target
```

Check that the script works and enable service:

```bash
sudo systemctl start bios_fan_control
sudo systemctl enable bios_fan_control
```

Install and start i8k fan control (Dell laptops only):

```bash
sudo dnf install i8kutils
```

Edit configuration file:

```bash
sudo vi /etc/i8kmon.confi8kmon
```

```bash
set config(poll_ac)     0
set config(poll_fans)   0
set config(0)   {{0 0}  -1  55  -1  55}
set config(1)   {{0 1}  50  60  50  60}
set config(2)   {{1 1}  55  65  55  65}
set config(3)   {{2 1}  60  70  60  70}
set config(4)   {{2 2}  65 128  65 128}
```

Patch i8kmon to fix [sound issue](https://github.com/vitorafsr/i8kutils/issues/15).

```bash
cd ~/projects
git clone https://github.com/sekogan/i8kutils.git
sudo mv /usr/bin/i8kmon /usr/bin/i8kmon.original
sudo cp ~/projects/i8kutils/i8kmon /usr/bin/
```

Test that it works:

```bash
sudo i8kmon -v
```

Enable i8kmon service:

```bash
sudo systemctl enable i8kmon
sudo systemctl start i8kmon
```

## Fix issues with Intel Wifi

Not sure it helped, but done this so far:

`/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf`

```ini
[connection]
wifi.powersave = 2
```

`/etc/modprobe.d/iwlwifi.conf`

```text
options iwlwifi 11n_disable=8
options iwlmvm power_scheme=1
```

## VPN

Install eToken and OpenConnect dependencies:

```bash
sudo dnf install nss-tools pcsc-lite gnutls-utils openssl openconnect vpnc-script NetworkManager-openconnect-gnome
```

Remove opensc (makes Firefox non-responsive):

```bash
sudo dnf remove opensc
```

## Remmina

If Remmina was installed as a flatpak, remove it first:

```bash
sudo flatpak remove org.remmina.Remmina
flatpak remove --user org.remmina.Remmina
```

Install via dnf:

```bash
sudo dnf install remmina
```

## Screen grabbers

```bash
sudo dnf install flameshot ffmpeg peek
```

Start and configure flameshot (`flameshot config`):

- Configuration
  - General
    - Launch at startup = off
    - Close after capture = on

Configure keyboard shortcuts:

- Go to Settings -> Keyboard Shortcuts.
- Remove Ctrl + Print shortcut.
- Add new shortcut:
  - Name: Make screenshot with flameshot
  - Command: flameshot gui
  - Shortcut: Ctrl + Print

Start peek, go to Preferences and enable "Open file manager after saving".

- Go to Settings -> Keyboard Shortcuts.
- Remove Ctrl + Shift + Print shortcut.
- Add new shortcut:
  - Name: Record screen with peek
  - Command: peek
  - Shortcut: Ctrl + Shift + Print

## Printer

Go to Canon website and download driver for linux (google "LPB613Cdw driver").

```bash
tar xzvf downloaded_file.tgz
cd linux-UFRII-drv-v520-uken
sudo ./install.sh
```

Go to Settings -> Printers. Delete the printer if already exists and then add it again.
