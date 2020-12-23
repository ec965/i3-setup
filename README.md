# The i3 Window Manager Guide
- [The i3 Window Manager Guide](#the-i3-window-manager-guide)
- [Setting Up i3](#setting-up-i3)
  - [Choosing A Distribution](#choosing-a-distribution)
    - [Ubuntu](#ubuntu)
    - [Manjaro](#manjaro)
- [Installing i3](#installing-i3)
    - [i3 Default Keybinds with mod](#i3-default-keybinds-with-mod)
    - [i3 Default Keybinds with mod+Shift](#i3-default-keybinds-with-modshift)
  - [Preliminary Dependencies](#preliminary-dependencies)
  - [Setting up i3lock](#setting-up-i3lock)
- [Getting Hardware Working](#getting-hardware-working)
  - [Display](#display)
    - [xrandr](#xrandr)
    - [autorandr](#autorandr)
    - [Configuring HiDPI Screens and Scaling Applications](#configuring-hidpi-screens-and-scaling-applications)
  - [Brightness](#brightness)
  - [light](#light)
  - [Audio](#audio)
    - [Pulse Audio](#pulse-audio)
      - [pactl](#pactl)
    - [pavucontrol](#pavucontrol)
    - [playerctl](#playerctl)
  - [Wi-Fi](#wi-fi)
    - [nmtui](#nmtui)
    - [nm-applet](#nm-applet)
  - [Bluetooth](#bluetooth)
    - [bluetoothctl](#bluetoothctl)
    - [blueman](#blueman)
  - [Mouse Sensitivity](#mouse-sensitivity)
  - [i3 Config File Update](#i3-config-file-update)
- [Making i3 Pretty](#making-i3-pretty)
  - [Compton](#compton)
  - [Rofi](#rofi)
    - [Replacing dmenu with Rofi](#replacing-dmenu-with-rofi)
  - [Polybar](#polybar)
    - [Installing](#installing)
    - [Cofiguring](#cofiguring)
    - [Using Polybar with i3](#using-polybar-with-i3)
    - [Debugging Missing Unicode Characters](#debugging-missing-unicode-characters)
  - [GTK Themes](#gtk-themes)
    - [lxappearance](#lxappearance)
  - [Wallpaper](#wallpaper)
    - [nitrogen](#nitrogen)
  - [Nightlight](#nightlight)
    - [redshift](#redshift)
  - [i3lock-color](#i3lock-color)
    - [Installing](#installing-1)
    - [Configuring](#configuring)
    - [Using i3lock-color with i3](#using-i3lock-color-with-i3)
  - [i3 Config File Update](#i3-config-file-update-1)
- [Theming](#theming)
  - [Using .Xresources For a Consistent Color Theme](#using-xresources-for-a-consistent-color-theme)
    - [Using .Xresources with i3](#using-xresources-with-i3)
    - [Using .Xresources with polybar](#using-xresources-with-polybar)
    - [Using .Xresources with Rofi](#using-xresources-with-rofi)
- [FAQ](#faq)
  - [I want to use my HiDPI screen with another monitor.](#i-want-to-use-my-hidpi-screen-with-another-monitor)
  - [My bluetooth headphones sound like crap when I use the microphone. How can I make them sound better?](#my-bluetooth-headphones-sound-like-crap-when-i-use-the-microphone-how-can-i-make-them-sound-better)
# Setting Up i3
## Choosing A Distribution
If you frequent [r/unixporn](https://www.reddit.com/r/unixporn/),
a very common setup tends to be i3 with Arch linux.
That being said, Arch takes a lot of time and patients to set up.
The aim of this guide is to get i3 working as quickly as possible.
Therefore, the recommended distributions are either Ubuntu or Manjaro.

### Ubuntu
* Pros: very popular with a large community meaning that many questions have already been answered.
* Cons: Somewhat bloated as there is no minimal installation.
### Manjaro
* Pros: provides a minimal installation ISO as well as access to the arch user repository (AUR) which makes installing some programs easier.
* Cons: Not as stable as Ubuntu, but good enough.

For this guide, I will be using Ubuntu. If you are using Manjaro, most of the installation commands can be replaced with `pacman -S <name of package>`.

# Installing i3
Now that you've installed a distribution, install i3 by running `sudo apt install i3`. 
**Log out** and click on the circle in the lower right corner. 
(If you don't see a circle, click to enter your password.) 
**Select i3** from the window managers menu and then log in.

You will be greeted by the i3 setup wizard. 
Pick your mod key (typically `Win`) and continue.

### i3 Default Keybinds with mod

![i3 default keybinds](https://i3wm.org/docs/keyboard-layer1.png)

### i3 Default Keybinds with mod+Shift

![i3 default keybinds](https://i3wm.org/docs/keyboard-layer2.png)

Take note of the following shortcuts
* mod+Enter: opens the terminal
* mod+Shift+q: closes the current window
* mod+d: opens dmenu to open other applications
* mod+Shift+r: reloads i3 after changing the i3 config file
* mod+Shift+e: exit i3

The i3 setup wizard will also create a config file at `~/.config/i3/config`. 
Throughout this guide, we will update this config file at the end of each section.

## Preliminary Dependencies
`sudo apt install build-essentials git cmake python3`

This might not cover everything, if you get a dependency error, you can usually fix it by installing the dependecy via `sudo apt install <dependency>`.

## Setting up i3lock
i3lock is included when installing i3. 
It allows you to lock your screen by running `i3lock`.
There is no default keybind for i3lock in the i3 config file.
I prefer to bind it to mod+Shift+w as it is next to the exit hotkey.

Add the following to the i3 config file at `~/.config/i3/config:
```
# lock the screen
bindsym $mod+Shift+w exec --no-startup-id i3lock
```

# Getting Hardware Working

## Display
### xrandr
xrandr is a display manager that is used to change monitor settings.
It usually comes preinstalled on most distributions.
### [autorandr](https://github.com/phillipberndt/autorandr)
`sudo pip install autorandr`

Autorandr is a script that can automatically configure different displays. More information can be found in the link.

### Configuring HiDPI Screens and Scaling Applications
A HiDPI screen is a relativley small screen with a high resolution. These generally appear on ultra books and high end laptops.
1. Create a file in the home directory with `touch ~/.Xresources`.
2. Edit `~/.Xresources` and add `Xft.dpi = 192`. Generally, dpi is a scalar of 96, 192 is 2x of this base dpi.
3. Log out (mod+Shift+e) and log back in.

## Brightness
## [light](https://github.com/haikarainen/light)
Follow the installation instructions in the repository.
Light controls the backlight brightness of the screen.
After installing, run `light -P 5` to set the minimum brightness to 5.
If you don't set a minimum brightness, you may turn the backlight off accientally and won't be able to see anything.

## Audio
### Pulse Audio
Once again, depending on your distribution, you probably already have pulse audio installed.
#### pactl
Controls a running Pulse Audio server. This will be needed for changing volume using the keyboard.
### pavucontrol
`sudo apt install pavucontrol`

pavucontrol stands for PulseAudio Volume Control and it does exactly that. It provides a gui interface for controling sound.

### playerctl
`sudo apt install playerctl`

Controls media keys such as play/pause, skip, etc. 


## Wi-Fi
### nmtui
nmtui stands for network-manager terminal user interface. It allows you to configure your wifi settings in the terminal. It's probably already installed.
### nm-applet
`sudo apt install nm-applet`

nm-applet is a widget that allows configuration of wifi. After installation, it should appear in the bottom bar's right corner.  

## Bluetooth
### bluetoothctl
Bluetooth can be controlled via the command line using bluetoothctl.

### blueman
`sudo apt install blueman`

Blueman is a gui interface for configuring bluetooth devices. 
It also comes with blueman-applet which provides a bluetooth menu on the bottom bar.

Run blueman using the command `blueman-manager` or from dmenu.

blueman-applet can be started by using `blueman-applet`.

## Mouse Sensitivity
i3 doesn't have any persistent mouse configuration settings so we will have to write a script if we want to change the mouse sensitivity. A TLDR has been provided below but you can read more [here](https://www.reddit.com/r/i3wm/comments/4efbsm/mouse_speed/d2056gj?utm_source=share&utm_medium=web2x&context=3).

1. First, run: `xinput list` and check the output for your mouse.
2. Run `xinput list-props ID` where ID is the name of your mouse. For example if I were using the 'Logitech M510' I would type `xinput list-props "Logitech M510"`.
3. Look for the property called `libinput Accel Speed (312)` or something similar. Take note of the property id, in this case it would be 312.
4. Run `xinput set-prop ID PROPERTY VALUE`. For example, to decrease the mouse sensitivty on the Logitech M510 we would run `xinput set-prop 'Logitech M510' 312 -0.5`
5. Check that the property has been updated by running `xinput list-props ID` again.

The problem with this is that everytime we relog/restart, we would have to run these commands again.
To simplify this, we can write a script to do this for us and put it in i3's startup. 

Example script: logitech-sens.sh
```
#!/bin/bash
if xinput | grep "Logitech" > /dev/null; then
    xinput --set-prop "Logitech M510" "libinput Accel Speed" -0.65
    echo "setting mouse sens"
fi
```
1. Enable the script using `chmod +x SCRIPTNAME.sh`. 
2. Add the following line to your i3 config file:
```
exec --nostartup-id /path/to/script/SCRIPTNAME.sh
```

## i3 Config File Update
Add the following to your i3 config file and refresh i3 with mod+Shift+r. (Some of this may already be in your default i3 config file.)
```
# These apps will be run at startup
# startup network applet
exec --no-startup-id nm-applet
# startup bluetooth manager applet
exec --no-startup-id blueman-applet
# load setup for monitors
exec --no-startup-id autorandr --change

# Use pactl to adjust volume in PulseAudio.
bindsym XF86AudioRaiseVolume exec --no-startup-id pactl set-sink-volume @DEFAULT_SINK@ +10% && $refresh_i3status
bindsym XF86AudioLowerVolume exec --no-startup-id pactl set-sink-volume @DEFAULT_SINK@ -10% && $refresh_i3status
bindsym XF86AudioMute exec --no-startup-id pactl set-sink-mute @DEFAULT_SINK@ toggle && $refresh_i3status
bindsym XF86AudioMicMute exec --no-startup-id pactl set-source-mute @DEFAULT_SOURCE@ toggle && $refresh_i3status

# use playerctl to control media keys
bindsym XF86AudioPlay exec playerctl play-pause
bindsym XF86AudioPause exec playerctl play-pause
bindsym XF86AudioNext exec playerctl next
bindsym XF86AudioPrev exec playerctl previous

# use light to control birightness
bindsym XF86MonBrightnessUp exec light -A 20 # increase screen brightness
bindsym XF86MonBrightnessDown exec light -U 20 # decrease screen brightness
```

# Making i3 Pretty

## [Compton](https://github.com/chjj/compton)
`sudo apt install compton`

Compton is a compositor. It allows things like transparency.
The config file should be at `~/.config/i3/compton.conf`.
If it is not there, you can create it.
A [default config file](https://github.com/chjj/compton/blob/master/compton.sample.conf) is provided on github.

## [Rofi](https://github.com/davatorium/rofi)
`sudo apt install rofi`

Rofi is a replacement for dmenu. It looks nicer and has some extra features. Rofi can be configured from `~/.config/rofi/config`. 
Rofi's theme can be changed using the `~/.config/rofi/config.rasi`.
My Rofi config file looks like this:
```
rofi.lines:10
rofi.modi:run,window,ssh,drun
rofi.font: Gill Sans MT 14
```
There are many different config options and themes. You can read more about them on github or the [Rofi manpages](https://www.mankier.com/1/rofi).

### Replacing dmenu with Rofi
In your i3 config file, replace 
```
# start dmenu (a program launcher)
bindsym $mod+d exec dmenu_run
```
with
```
# start rofi (a program launcher)
bindsym $mod+d exec rofi -show drun -show-icons
```
* -show drun: runs Rofi using drun
* -show-icons: shows icons in the Rofi menu

After reloading i3, pressing mod+d should start rofi instead of dmenu.

## [Polybar](https://github.com/polybar/polybar)
Up till now, the bottom bar has been controlled by a script called i3bar.
Polybar is a replacement for i3 bar that is easily configurable and customizable.

### Installing
The [compiling page](https://github.com/polybar/polybar/wiki/Compiling) has detailed instructions for installing polybar, I've provided a tldr below:

Run the follow commands in order: 
```
apt install build-essential git cmake cmake-data pkg-config python3-sphinx libcairo2-dev libxcb1-dev libxcb-util0-dev libxcb-randr0-dev libxcb-composite0-dev python3-xcbgen xcb-proto libxcb-image0-dev libxcb-ewmh-dev libxcb-icccm4-dev
```
```
apt install libxcb-xkb-dev libxcb-xrm-dev libxcb-cursor-dev libasound2-dev libpulse-dev i3-wm libjsoncpp-dev libmpdclient-dev libcurl4-openssl-dev libnl-genl-3-dev
```  
```
git clone --recursive https://github.com/polybar/polybar
cd polybar
```
```
mkdir build
cd build
cmake ..
make -j$(nproc)
# Optional. This will install the polybar executable in /usr/local/bin
sudo make install
```
```
make userconfig #generates the config file
```
### Cofiguring
All config settings are stored in `~/.config/polybar/config`.
Configuration options can be found in the [github wiki](https://github.com/polybar/polybar/wiki).

### Using Polybar with i3
1. Place the following script in `~/.config/polybar/` and name it `launch.sh`. Give it permissions with `chmod +x ~/.config/polybar/launch.sh`.
```
#!/bin/bash
# https://wiki.archlinux.org/index.php/Polybar

# Terminate already running bar instances
killall -q polybar

# Wait until the processes have been shut down
while pgrep -u $UID -x polybar >/dev/null; do sleep 1; done

# Launch Polybar, using default config location ~/.config/polybar/config
polybar example &

echo "Polybar launched..."
```
2. Disable i3 bar by commenting out or deleting the following line from the i3 config file.
```
# Start i3bar to display a workspace bar (plus the system information i3status
# finds out, if available)
bar {
        status_command i3status
}
```
3. Still in the i3 config file, add:
```
# launch polybar
exec_always --no-startup-id $HOME/.config/polybar/launch.sh
```
4. Restart i3 (mod+Shift+r), the example bar should now be in place.

### Debugging Missing Unicode Characters
Make sure that the font you are using for polybar supports all the unicode characters in your config file.
[Read more here](https://github.com/polybar/polybar/wiki/Fonts#icon-fonts).
## GTK Themes
### lxappearance
`sudo apt install lxappearance`

GTK themes control how most gui applications will look.
lxappearance provides a gui for configuring GTK themes and icons.
Lots of free GTK themes can be found online. 
* GTK themes should be placed in `~/.themes/` directory. 
* Icons should be placed in `~/.icons/`. 
* Fonts should be placed in `~/.fonts/`

## Wallpaper
### [nitrogen](https://github.com/l3ib/nitrogen)
`sudo apt install nitrogen`

I have copied the instructions from the [ArchWiki](https://wiki.archlinux.org/index.php/nitrogen) below.
Set a wallpaper:
```
nitrogen /path/to/image/directory/
```
Restore a wallpaper:
```
nitrogen --restore &
```

## Nightlight
### [redshift](https://github.com/jonls/redshift)
`sudo apt-get install redshift`

Redshift decreases blue light exposure at night.
Many other desktop enviroments have this function built in.
Redshift can be configured via a config file but I find that the default config is enough for practical usage.
## [i3lock-color](https://github.com/Raymo111/i3lock-color)
i3lock-color enables some customization for i3lock.
### Installing
1. Install dependencies:
```
sudo apt install autoconf gcc make pkg-config libpam0g-dev libcairo2-dev libfontconfig1-dev libxcb-composite0-dev libev-dev libx11-xcb-dev libxcb-xkb-dev libxcb-xinerama0-dev libxcb-randr0-dev libxcb-image0-dev libxcb-util-dev libxcb-xrm-dev libxkbcommon-dev libxkbcommon-x11-dev libjpeg-dev
```
2. Clone the repository
```
git clone https://github.com/Raymo111/i3lock-color.git
cd i3lock-color
```
3. Build
```
chmod +x build.sh
./build.sh
```
4. Install
```
chmod +x build.sh
./build.sh
```
### Configuring
There are two example scripts in the repository called `lock.sh` and `lock_bar.sh`. You may need to enable them with `chmod +x lock.sh` and `chmod +x lock_bar.sh`.
You may need to edit the scripts and change this line:
```
./x86_64-pc-linux-gnu/i3lock \
```
to
```
i3lock \
```

### Using i3lock-color with i3
After you have a script you're happy with, place it somewhere safe. Edit the your i3 config file at `~/.config/i3/config`

If you followed the instructions for i3lock above, replace:
```
# lock the screen
bindsym $mod+Shift+w exec --no-startup-id i3lock
```
with
```
bindsym $mod+Shift+w exec --no-startup-id /path/to/script/lock.sh
```
Otherwise use add the above line to your i3config.

## i3 Config File Update
We've added some configuration to the i3 config file earlier in this section, 
but there are still a few things we need to add, namely startup programs.
```
# load wallpaper manager
exec --no-startup-id nitrogen --restore &
# red shift to decrease blue night at light
exec --no-startup-id redshift &
# launch polybar
exec_always --no-startup-id $HOME/.config/polybar/launch.sh
```
Reload your config with mod+Shift+r to see the changes take place.

You now have some basic configuration for i3!

# Theming

## Using .Xresources For a Consistent Color Theme
Various programs can pull color codes from the `~/.Xresources` file.
This allows a unified color scheme with minimal configuration.

1. If it doesn't exist, create the `.Xresources` file in the home directory.
2. Create a color scheme, an example is given below for the Material Palenight theme:
```
#define MATERIAL_PALENIGHT

#ifdef MATERIAL_PALENIGHT
#define BLACK       #292d3e
#define BLACKALT     #434758
#define RED         #f07178
#define REDALT       #ff8b92
#define GREEN       #c3e88d
#define GREENALT     #ddffa7
#define YELLOW      #ffcb6b
#define YELLOWALT    #ffe585
#define BLUE        #82aaff
#define BLUEALT      #9cc4ff
#define MAGENTA     #c792ea
#define MAGENTAALT   #e1acff
#define CYAN        #89ddff
#define CYANALT      #a3f7ff
#define WHITE       #d0d0d0
#define WHITEALT     #ffffff
#endif

*foreground: WHITE
*background: BLACK
*color0: BLACK
*color1:RED
*color2:GREEN
*color3:YELLOW
*color4:BLUE
*color5:MAGENTA
*color6:CYAN
*color7:WHITE
*color8: BLACKALT
*color9: REDALT
*color10:GREENALT
*color11:YELLOWALT
*color12:BLUEALT
*color13:MAGENTAALT
*color14:CYANALT
*color15:WHITEALT
```
3. Source the file with `xrdb -load ~/.Xresources`.
4. Verify the changes with `xrdb -query`.

### [Using .Xresources with i3](https://i3wm.org/docs/userguide.html#xresources)

### Using .Xresources with polybar
Colors can be defined as:
```
background = ${xrdb:color0:#292d3e}
```
The hex color code at the end is a fallback color.

### Using .Xresources with Rofi
The old (deprecated) themeing method supports Xresources.
The theme should be put directly in the `~/.Xresoureces` file.
The colors can be configured directly by using the macros in the `~/.Xresources` file.

For example:
```
rofi.color-window: BLACK,    WHITE,  BLACKALT,   YELLOW,    BLACK 
rofi.color-normal: BLACKALT, WHITE,  BLACK,      RED,       BLACK 
rofi.color-urgent: MAGENTA,  BLACK,  BLACKALT,   YELLOW,    BLACK 
rofi.color-active: YELLOW,   BLACK,  YELLOWALT,   BLACK,      GREEN 
```

The new method of theme is to use `config.rasi`. This may or may not support using xrdb variables.

# FAQ

## I want to use my HiDPI screen with another monitor.
If you've followed the instructions [above for configuring a HiDPI screen](#configuring-hidpi-screens-and-scaling-applications), you'll probably get some weird stuff happening when connecting a second monitor.

One possible solution is to increase the scaling of the second monitor to match the scaling of the HiDPI monitor.
1. Run `xrandr` and take note of the name of the second monitor. It should be something like VGA-1, HDMI-1, or DP-1.
2. Run the follow command to incrase the scaling of the second monitor. This will artificially increase the resolution to match the HiDPI monitor.
```
xrandr --output <name of monitor> --scale 2x2
```
3. Set the monitors relative location. For example, I have a laptop monitor called `eDP-1` and an external monitor called `DP-1`. I want to put `DP-1` on the right side of `eDP-1`. For more options run `xrandr --help`.
```
xrandr --output DP-1 --right-of eDP-1
``` 
4. If you are using autorandr, save the config using `autorandr --save dual-setup`. In the future, you can load this setup using `autorandr dual-setup`.

Note: It is possible to write scripts to change the Xft.dpi variable in ~/.Xresources,
 but in-order for these changes to take full effect on the entire desktop, you must relog or restart the computer.

## My bluetooth headphones sound like crap when I use the microphone. How can I make them sound better?
This [article](https://askubuntu.com/questions/1004712/how-to-keep-the-audio-profile-at-a2dp-while-using-a-mic-with-bluetooth-headset) gives a very good overview of the issue.

TLDR:
Bluetooth only allows 2 modes:
* Crappy audio, microphone on
* High quality audio, no microphone

Github users OndraZizka and artem328 created a [script](https://gist.github.com/OndraZizka/2724d353f695dacd73a50883dfdf0fc6) for swapping between the two modes depending on what you're doing. 

