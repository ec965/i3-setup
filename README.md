# Setting Up an i3 Desktop Environment
## Table of Contents
- [Setting Up an i3 Desktop Environment](#setting-up-an-i3-desktop-environment)
  - [Table of Contents](#table-of-contents)
- [Intoduction](#intoduction)
  - [Choosing A Distribution](#choosing-a-distribution)
- [Installing i3](#installing-i3)
  - [The i3 Config File](#the-i3-config-file)
- [Setting Up the Desktop Environment](#setting-up-the-desktop-environment)
  - [Preliminary Dependencies](#preliminary-dependencies)
  - [Setting up i3lock](#setting-up-i3lock)
  - [Display](#display)
  - [Brightness](#brightness)
  - [Audio](#audio)
  - [Wi-Fi](#wi-fi)
  - [Bluetooth](#bluetooth)
  - [Mouse Sensitivity](#mouse-sensitivity)
  - [Section Summary](#section-summary)
- [Making i3 Pretty](#making-i3-pretty)
  - [Picom](#picom)
  - [Rofi](#rofi)
  - [Polybar](#polybar)
  - [LXappearance](#lxappearance)
  - [Wallpaper](#wallpaper)
  - [Redshift](#redshift)
  - [i3lock-color](#i3lock-color)
  - [Section Summary](#section-summary)
- [Theming](#theming)
  - [Using .Xresources For a Consistent Color Theme](#using-xresources-for-a-consistent-color-theme)
- [FAQ](#faq)
  - [I want to use my HiDPI screen with another monitor.](#i-want-to-use-my-hidpi-screen-with-another-monitor)
  - [My bluetooth headphones sound like crap when I use the microphone. How can I make them sound better?](#my-bluetooth-headphones-sound-like-crap-when-i-use-the-microphone-how-can-i-make-them-sound-better)

# Intoduction
This guides goal is to get the i3 window manager working in a similar way to a regular desktop environment.
The topics that will be covered include:
* choosing a distribution
* installing i3
* setting up a desktop environment in i3
* customization and theming

This guide requires an intermediate understanding of linux.
For new Linux users, I recommend [Regolith Linux](https://regolith-linux.org/).

## Choosing A Distribution
If you frequent [r/unixporn](https://www.reddit.com/r/unixporn/),
a very common setup tends to be i3 with Arch Linux.
You may be tempted to choose Arch but it takes time and patients to learn.
The aim of this guide is to get i3 working as quickly as possible;
therefore, I recommend using either Manjaro or Ubuntu.
Both distributions have its pros and cons:
* Ubuntu
    - Pros: Ubuntu is popular with a large community. Any issues will likely be easily answered with a simple web search. 
    - Cons: Ubuntu is not on a rolling release like other distributions. Some software may need to be manually installed.
* Manjaro
    - Pros: Manjaro provides a minimal installation ISO as well as access to the Arch user repository (AUR) which makes installing some programs easier.
    - Cons: Not as stable as Ubuntu, but still fairly stable.

For this guide, I will be using Ubuntu. 
If you are using Manjaro, most of the installation commands can be replaced with Manjaro's package manager, pacman. 
For example: `sudo apt install i3` would become `sudo pacman -S i3`.

# Installing i3
1. Open the terminal and install i3 by running `sudo apt install i3`. 
2. Log out and click on the circle in the lower right corner. 
(If you don't see a circle, click to enter your password.) 
3. Select i3 from the window managers menu and then log in. 
You will be greeted by the i3 setup wizard. 
4. Pick your mod key (typically the Windows/Super key) and continue.
The mod key will be the main modifier for shortcuts in i3.

i3 Default Keybinds with mod

![i3 default keybinds](https://i3wm.org/docs/keyboard-layer1.png)

i3 Default Keybinds with mod+Shift

![i3 default keybinds](https://i3wm.org/docs/keyboard-layer2.png)

*Please note that these images use the ALT key as the mod key.

Listed below are commonly used shortcuts:
* **mod+Enter**: open the terminal
* **mod+Shift+q**: close the current window
* **mod+d**: open dmenu, an application launcher
* **mod+Shift+r**: reload i3, typically run after changing the i3 configuration
* **mod+Shift+e**: exit i3

## The i3 Config File
The i3 setup wizard will also create a config file at `~/.config/i3/config`. 
You will update this config file throughout this guide.

# Setting Up the Desktop Environment
In this section you will set up the following components:
* screen locker
* display manager
* backlight adjuster
* audio controller
* WiFi manager
* bluetooth manager
* mouse sensitivity

I recommend using the command line programs but a GUI option will also be mentioned.

## Preliminary Dependencies
`sudo apt install build-essentials git cmake python3 python3-pip`

Git, CMake, Python and it's package manager pip will be used to download and build programs.

You will also install other dependencies as needed throughout the guide.
Dependencies issues can be solved by install the dependency with `sudo apt install <DEPENDENCY>`.

Another useful command for resolving dependencies is `sudo apt install -f`. This command tells the package manager to attempt to resolve any dependency issues.


## Setting up i3lock
i3lock allows you to lock your screen with the command `i3lock`.
There is no default keybind for i3lock in the i3 config file.
I prefer to bind it to mod+Shift+w as it is next to the default exit hotkey (mod+Shift+e).
You may use any hotkey you prefer.

Add your hotkey to the i3 config file by appending the following text:
```
# lock the screen
bindsym $mod+Shift+w exec --no-startup-id i3lock
```

## Display
### xrandr
`sudo apt install xrandr`

Xrandr is a command line display manager that is used to change monitor settings.
Xrandr is used in most X11 based distributions.
You may already have xrandr installed.

### arandr
`sudo apt install arandr`

Arandr provides a GUI for xrandr.

### [autorandr](https://github.com/phillipberndt/autorandr)
`sudo apt install autorandr`

Autorandr is a script that can automatically configure different displays.
If you opt to use autorandr, configure the program and then add the following line to your i3 config:
```
# load setup for monitors
exec --no-startup-id autorandr --change
```

### Configuring HiDPI Screens and Scaling Applications
A HiDPI screen is a relativley small screen with a high resolution. 
These generally appear on ultra books and high end laptops.

To configure a HiDPI screen:
1. Create a file in the home directory with `touch ~/.Xresources`.
2. Edit `~/.Xresources` and add `Xft.dpi = 192`. 
(Generally, dpi is a scalar of 96, 192 is 2x of this base dpi.)
3. Log out (mod+Shift+e) and log back in.

## Brightness
### [light](https://github.com/haikarainen/light)
Follow the installation instructions in the repository.

Light controls the backlight brightness of the screen.
After installing, run `light -P 5` to set the minimum brightness to 5.
Setting a minimum brightness prevents you from accedentally turning off the backlight.

Add the following to your i3 config to enable brightness keys:
```
# use light to control birightness
bindsym XF86MonBrightnessUp exec light -A 20 # increase screen brightness
bindsym XF86MonBrightnessDown exec light -U 20 # decrease screen brightness
```

## Audio

### pactl
Pactl stands for [Pulse Audio](https://www.freedesktop.org/wiki/Software/PulseAudio/) Controller. 
Pactl will be needed for changing volume using keyboard hotkeys.

Add the following to your i3 config to settup volume keys:
```
# Use pactl to adjust volume in PulseAudio.
bindsym XF86AudioRaiseVolume exec --no-startup-id pactl set-sink-volume @DEFAULT_SINK@ +10% && $refresh_i3status
bindsym XF86AudioLowerVolume exec --no-startup-id pactl set-sink-volume @DEFAULT_SINK@ -10% && $refresh_i3status
bindsym XF86AudioMute exec --no-startup-id pactl set-sink-mute @DEFAULT_SINK@ toggle && $refresh_i3status
bindsym XF86AudioMicMute exec --no-startup-id pactl set-source-mute @DEFAULT_SOURCE@ toggle && $refresh_i3status
```

### pavucontrol
`sudo apt install pavucontrol`

Pavucontrol provides a gui interface for controling Pulse Audio.

### playerctl
`sudo apt install playerctl`

Controls media keys such as play/pause, skip, etc. 

Add the following to your i3 config to settup media keys:
```
# use playerctl to control media keys
bindsym XF86AudioPlay exec playerctl play-pause
bindsym XF86AudioPause exec playerctl play-pause
bindsym XF86AudioNext exec playerctl next
bindsym XF86AudioPrev exec playerctl previous
```

## Wi-Fi

### nmtui
Nmtui stands for network-manager terminal user interface. Nmtui allows you to configure your wifi settings in the terminal. 
Nmtui comes preinstalled on most distributions.

### nm-applet
`sudo apt install nm-applet`

Nm-applet is a widget that allows configuration of wifi. After installation, it should appear in the bottom bar's right corner.  

Add the following lines to your i3 config:
```
# startup network applet
exec --no-startup-id nm-applet
```

## Bluetooth
### bluetoothctl
Bluetooth can be controlled via the command line using bluetoothctl.

### blueman
`sudo apt install blueman`

Blueman is a gui interface for configuring bluetooth devices. 
Blueman comes with blueman-applet, which provides a bluetooth menu on the bottom bar similar to nm-applet.

Run blueman using the command `blueman-manager` or from dmenu.

blueman-applet can be started by using `blueman-applet`.

Add the following to your i3 config:
```
# startup bluetooth manager applet
exec --no-startup-id blueman-applet
```

## Mouse Sensitivity
i3 doesn't have any persistent mouse configuration settings so we will have to write a script if we want to change the mouse sensitivity. 
The follow instructions for writing a mouse sensitivity script were extracted from this [detailed explanation](https://www.reddit.com/r/i3wm/comments/4efbsm/mouse_speed/d2056gj?utm_source=share&utm_medium=web2x&context=3).

In the terminal:
1. Run: `xinput list` and check the output for your mouse.
2. Run `xinput list-props ID` where ID is the name of your mouse. For example if I were using the 'Logitech M510' I would type `xinput list-props "Logitech M510"`.
3. Look for the property called `libinput Accel Speed (312)` or something similar. Take note of the property id, in this case it would be 312.
4. Run `xinput set-prop ID PROPERTY VALUE`. For example, to decrease the mouse sensitivty on the Logitech M510 we would run `xinput set-prop 'Logitech M510' 312 -0.5`
5. Check that the property has been updated by running `xinput list-props ID` again.

These commands will configure the mouse sensitivity, but you will have to run them on every startup.
You can write a script to do this automatically during startup. 

Example script: `logitech-sens.sh`
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

## Section Summary
Congratulations, most common hardware should now be easily controllable.





# Making i3 Pretty
Stock i3, while nice, is not the prettiest thing to look at.
You can spice it up with a few programs:
* Picom: a compositor
* Rofi: a replacement for dmenu
* Polybar: a replacement for i3's stock i3bar
* LXAppearance: a manager for GTK skins
* Feh and Nitrogen: a wallpaper manager
* Redshift: a blue light reducer
* i3lock-color: a patch for i3lock that gives more customization options

## [Picom](https://github.com/yshui/picom)
`sudo apt install picom`

Picom is a compositor which allows things like transparency.
The config file should be at `~/.config/picom/picom.conf`.
If the picom config file is not there, you can create it.
A [sample config file](https://github.com/yshui/picom/blob/next/picom.sample.conf) is provided in the repository.

*You may read about another compositor named Compton. 
Picom is a fork of Compton as Compton was no longer being maintained.

## [Rofi](https://github.com/davatorium/rofi)
`sudo apt install rofi`

Rofi is a replacement for dmenu. Rofi looks nicer and has some extra features. Rofi can be configured from `~/.config/rofi/config`. 
Rofi's theme can be changed using the `~/.config/rofi/config.rasi`.
A simple Rofi config file could look like this:
```
rofi.lines:10
rofi.modi:run,window,ssh,drun
rofi.font: Gill Sans MT 14
```
There are many different config options and themes. You can read more about them on the [Rofi manpages](https://www.mankier.com/1/rofi).

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
* -show drun: runs Rofi using drun (drun is a mode similar to dmenu)
* -show-icons: shows icons in the Rofi menu

After reloading i3, pressing mod+d should start rofi instead of dmenu.

## [Polybar](https://github.com/polybar/polybar)
Up till now, the bottom bar has been controlled by a script called i3bar.
Polybar is a replacement for i3 bar that is easily configurable and customizable.

### Installing
Polybar's wiki has a [compiling page](https://github.com/polybar/polybar/wiki/Compiling) that has detailed instructions for installing polybar.
I've provided a TLDR below:

Run the follow commands in order: 
1. Install dependencies:
```
apt install build-essential git cmake cmake-data pkg-config python3-sphinx libcairo2-dev libxcb1-dev libxcb-util0-dev libxcb-randr0-dev libxcb-composite0-dev python3-xcbgen xcb-proto libxcb-image0-dev libxcb-ewmh-dev libxcb-icccm4-dev
```
2. Install more dependencies:
```
apt install libxcb-xkb-dev libxcb-xrm-dev libxcb-cursor-dev libasound2-dev libpulse-dev i3-wm libjsoncpp-dev libmpdclient-dev libcurl4-openssl-dev libnl-genl-3-dev
```  
3. Clone the repository:
```
git clone --recursive https://github.com/polybar/polybar
cd polybar
```
4. Build Polybar:
```
mkdir build
cd build
cmake ..
make -j$(nproc)
# Optional. This will install the polybar executable in /usr/local/bin
sudo make install
```
5. Generate the example config file:
```
make userconfig #generates the config file
```
### Configuring
All config settings are stored in `~/.config/polybar/config`.
Configuration options can be found in the [Polybar wiki](https://github.com/polybar/polybar/wiki).

### Using Polybar with i3
After you've installed Polybar, you'll want to replace i3bar with polybar.

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
3. Add to the i3 config file:
```
# launch polybar
exec_always --no-startup-id $HOME/.config/polybar/launch.sh
```
4. Restart i3 (mod+Shift+r), the example bar should now be in place.

### Debugging Missing Unicode Characters
You may get an error that you are missing unicode characters in your font.
Make sure you are using a font that has unicode icons.

Popular fonts with unicode icons include:
* [Nerd Fonts](https://www.nerdfonts.com/)
* [Material icons](https://github.com/google/material-design-icons)
* [Font Awesome](https://fontawesome.com/)

Fonts placed in `~/.fonts/` will be recognized by the system.

[The wiki page on fonts](https://github.com/polybar/polybar/wiki/Fonts#icon-fonts) also directly addresses fonts.

## LXappearance
`sudo apt install lxappearance`

LXappearance is a GUI program to apply GTK themes, icons, and fonts.
GTK themes control how most gui applications will look.
LXappearance provides a gui for configuring GTK themes and icons.
Lots of free GTK themes can be found on github and [gnome-look](https://www.gnome-look.org/browse/cat/135/order/latest/).
* GTK themes should be placed in `~/.themes/`.
* Icons should be placed in `~/.icons/`. 
* Fonts should be placed in `~/.fonts`.

## Wallpaper
### [feh](https://wiki.archlinux.org/index.php/Feh)
`sudo apt install feh`

Feh is a command line program that sets the wallpaper.
To set the wallpaper run the command:
```
feh --bg-scale /path/to/image.file

```
To restore the wallpaper on the next startup run:
```
~/.fehbg &
```
You will also want to put this command in your i3 config:
```
# startup feh to set the wallpaper
exec --no-startup-id ~/.fehbg &
```

### [nitrogen](https://github.com/l3ib/nitrogen)
`sudo apt install nitrogen`

Nitrogen is a GUI wrapper for feh.

To set a wallpaper run:
```
nitrogen /path/to/image/directory/
```
To restore a wallpaper on the next startup run:
```
nitrogen --restore &
```
You will also want to put this command in your i3 config:
```
# startup feh to set the wallpaper
exec --no-startup-id nitrogen --restore &
```

## [Redshift](https://github.com/jonls/redshift)
`sudo apt-get install redshift`

Redshift decreases blue light at night.
Redshift can be configured via a config file but I find that the default config is enough for practical usage.

Add the following to your i3 config to start redshift at startup:
```
# start redshift
exec --no-startup-id redshift &
```

## [i3lock-color](https://github.com/Raymo111/i3lock-color)
i3lock-color enables some customization for i3lock.

### Installing
1. Install dependencies:
```
sudo apt install autoconf gcc make pkg-config libpam0g-dev libcairo2-dev libfontconfig1-dev libxcb-composite0-dev libev-dev libx11-xcb-dev libxcb-xkb-dev libxcb-xinerama0-dev libxcb-randr0-dev libxcb-image0-dev libxcb-util-dev libxcb-xrm-dev libxkbcommon-dev libxkbcommon-x11-dev libjpeg-dev
```
2. Clone the repository:
```
git clone https://github.com/Raymo111/i3lock-color.git
cd i3lock-color
```
3. Build:
```
chmod +x build.sh
./build.sh
```
4. Install:
```
chmod +x build.sh
./build.sh
```

### Configuring
There are two example scripts in the repository called `lock.sh` and `lock_bar.sh`, which provide a basic config.
Give these scripts run permissions with `chmod +x lock.sh` and `chmod +x lock_bar.sh`.

*You may need to edit the scripts and change this line:
```
./x86_64-pc-linux-gnu/i3lock \
```
to
```
i3lock \
```

### Using i3lock-color with i3
After you have a script you're happy with, place it somewhere safe. 

Edit the your i3 config file at `~/.config/i3/config` and add:
```
bindsym $mod+Shift+w exec --no-startup-id /path/to/script/lock.sh
```
Also delete any i3lock settings you have in your i3lock config.
The i3lock settings might look like this: 
```
# lock the screen
bindsym $mod+Shift+w exec --no-startup-id i3lock
```

## Section Summary
Reload your config with mod+Shift+r to see the changes take place!







# Theming

## Using .Xresources For a Consistent Color Theme
Various programs can pull color codes from the `~/.Xresources` file.
This allows a unified color scheme with minimal configuration.

To configure Xresources:
1. Create the `.Xresources` file in the home directory:
```
cd ~
touch ~/.Xresources
```
2. Edit `.Xresources` and create a color scheme. An example is given below for the Material Palenight theme:
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

The new method of theme is to use `config.rasi`.
This method does not support Xresources.
`config.rasi` is the new preferred way to theme Rofi.

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

Github users OndraZizka and artem328 created a [script for swapping between the two modes](https://gist.github.com/OndraZizka/2724d353f695dacd73a50883dfdf0fc6) depending on what you're doing. 

