# XPS13-Ubuntu16-Env
## Introduction
Personal XPS13-9360 settings and configurations on Ubuntu16.04, also some solutions to touchpad and screen issues. Aim to help XPS13 users to setup Ubuntu environemnt easier.
> XPS13-9360 (8th gen.Kaby Lake-R, QHD touchscreen)


## Shell
### Install [zsh](https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH)
```
sudo apt install zsh
```

### Install [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)
```
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

### Themes

#### [denysdovhan/spaceship-prompt](https://github.com/denysdovhan/spaceship-prompt)

##### Install (oh-my-zsh)
```
git clone https://github.com/denysdovhan/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt"
ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
```

Set `ZSH_THEME="spaceship"` in your `.zshrc`.

##### Fix git symbol display [issue](https://github.com/denysdovhan/spaceship-prompt/issues/230)
Install powerline-font to fix the issue.

```
sudo apt-get install fonts-powerline
```

### plugins
See [.zshrc](.zshrc)

### Terminal Color Scheme
Github: https://github.com/Mayccoll/Gogh
Theme `aco`: `$ wget -O xt  http://git.io/v3Dll && chmod +x xt && ./xt && rm xt`

Screenshots:

![](https://i.imgur.com/n7Jhq33.png)

![](https://i.imgur.com/g159Qkr.png)



### Setup `guake`
```
sudo ln -s /usr/share/applications/guake.desktop /etc/xdg/autostart/
```

## Unity Themes & Icon Packs
### Unity Tweak Tool
#### Install
```
sudo apt-get install unity-tweak-tool gnome-tweak-tool
```
#### Settings 
Lots of settings, you can change the `theme` and `icons`, setup hot-corners and workspace.
To remove the workspace icon on the unity launcer, do the following
* Install dconf-editor `sudo apt install dconf-editor`
* Then open `com` -> `canonical` -> `unity` -> `launcher` and click on `favorites`
* In the field `Custom value` remove `'unity://expo-icon',` and click `Apply`


### Arc theme
#### Install
```
sudo sh -c "echo 'deb http://download.opensuse.org/repositories/home:/Horst3180/xUbuntu_16.04/ /' >> /etc/apt/sources.list.d/arc-theme.list"
sudo apt-get update && sudo apt-get install arc-theme
```
Github: https://github.com/horst3180/arc-theme

Tutorial: [How to Install Arc GTK Theme on Ubuntu](https://www.omgubuntu.co.uk/2016/06/install-latest-arc-gtk-theme-ubuntu-16-04)

### ultra-flat-icons
#### Install
```
sudo add-apt-repository ppa:noobslab/icons
sudo apt-get update
sudo apt-get install ultra-flat-icons
```
Github: https://github.com/anmoljagetia/Flatabulous

## Touchpad

I choose to use `synaptics` + [fusuma](https://github.com/iberianpig/fusuma)



### Fusuma
Works like a charm, smooth like silk.

Github: https://github.com/iberianpig/fusuma
#### Pre-install
Install ruby
```
sudo apt-get install rubygems
```
#### Install
You must be a member of the _input_ group to have permission to read the touchpad device:

```
$ sudo gpasswd -a $USER input
```
Logout and login.

```
$ sudo apt-get install libinput-tools
$ sudo apt-get install xdotool
```
Install Fusuma:
```
$ gem install fusuma
```
#### Post-Install
Add fusuma to startup applications.

#### Configuration
Edit file `~/.config/fusuma/config.yml` to enable multi-gesture control.
Settings at [config.yml](config.yml).

```
3 swipe up: Show Unity launcher
3 swipe down: Close current tab
4 finger gestures: Switch workspace.
```

### Remove Duplicate Touchpad Device

Relative post: https://forums.linuxmint.com/viewtopic.php?t=229932#

This may or may not happen, `$xinput list` to check if there is more than one touchpad under Virtual core pointer. If there is, the synclient commands will never work.

[Solution]

Edit `/etc/modprobe.d/blacklist.conf` by adding the lines: 

```
# remove SynPS/2 Synaptics Touchpad because we want the mouse to work over IC2b
blacklist psmouse
```

then:

```
sudo update-initramfs -u
sudo reboot
```
After the reboot xinput lists should only show one touchpad:

![](https://i.imgur.com/TiVKMwb.png)

This solution saved my day.

### Synclient Settings
#### Enable 3-finger-tap == Middle Key
```
synclient TapButton3=2
```
Add this to startup applications too.

Relative post: [XPS 9560 - setting up multitouch gestures with Ubuntu 16.04](https://www.reddit.com/r/Dell/comments/646y0t/xps_9560_setting_up_multitouch_gestures_with/)

## Touchscreen
### Enable long press screen right clicking
Go to `Settings` > `Universal Access` > `Pointing & Clicking` and enable `Simulated Secondary Click`


## Applications
### Firefox
#### Enable touchscreen scroll 

The solution is to launch firefox using:

```
env MOZ_USE_XINPUT2=1 firefox
```

You can make this permanent by modifying the launcher using the following:

```
sudo sed -i "s|Exec=|Exec=env MOZ_USE_XINPUT2=1 |g" /usr/share/applications/firefox.desktop
```

To undo this change, use:

```
sudo sed -i "s|Exec=env MOZ_USE_XINPUT2=1 |Exec=|g" /usr/share/applications/firefox.de
```
Stackoverflow: https://askubuntu.com/questions/978226/how-to-make-touch-screen-scrolling-work-in-firefox-quantum

### Spotify
#### Install
```
# 1. Add the Spotify repository signing keys to be able to verify downloaded packages
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0DF731E45CE24F27EEEB1450EFDC8610341D9410

# 2. Add the Spotify repository
echo deb http://repository.spotify.com stable non-free | sudo tee /etc/apt/sources.list.d/spotify.list

# 3. Update list of available packages
sudo apt-get update

# 4. Install Spotify
sudo apt-get install spotify-client
```
#### Fix high resolution display issue
You could edit `/usr/share/applications/spotify.desktop` and change the line with Exec= to:
```
Exec=spotify --force-device-scale-factor=1.8 %U
```

Spotify Community: https://community.spotify.com/t5/Desktop-Linux/Linux-client-barely-usable-on-HiDPI-displays/td-p/1067272


## Input
### 新酷音
Install

```
 sudo apt-get install fcitx fcitx-chewing
```

Go to `System Settings` > `Language Support` >  `Language`
Click `Install / Remove Languages` and install `Chinese(traditional)`

Keybord input method: `fctix`

![](https://i.imgur.com/7S80mPU.png)

Then logout and login.

Go to `System Settings` > `Text Entry`

Add `Chewing(fctix)`
![](https://i.imgur.com/YsnR8Z2.png)


## Battery
### TLP- Linux Advanced Power Management
```
sudo add-apt-repository ppa:linrunner/tlp  
sudo apt-get update
sudo apt-get install tlp tlp-rdw
```
Docs: http://linrunner.de/en/tlp/docs/tlp-linux-advanced-power-management.html








