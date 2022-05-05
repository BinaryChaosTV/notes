# Setting up an Ubuntu-based distro

This is a quick guide on programs to install on an Ubuntu-based
distribution. This is a personal guide for myself so I don't forget some
steps and applications while configuring a fresh install of Pop_OS or
Mint, etc.

In some cases, I will share some links to the installation instructions.
In other cased, I will simply paste the command to input in the CLI, for
simplicity. I will also try my best to divide the instructions in
categories, in a logical order so you're not missing a dependency when
you reach that step.

These options will differ depending on your distro. On Ubuntu-based
Distros however, these options will look slightly the same. At the time
of writing, I am currently using Pop_OS 21.10.

## GNOME

### Settings
Open Settings (Use the Super Key, and type it in the box).

1. Disable Bluetooth
2. Desktop:
  1. General -- toggle the Workspace and Applications button OFF.
     They tend not to work properly for me. Toggle the Minimize and
     Maximize buttons ON.
  2. Pick a background, set the appearance to **Dark**.
  3. Disable Dock.
3. Notifications -- *Do Not Disturb*.
4. Screen Display -- Activate Night Light to a manual schedule. Set
   monitors to the way you like.
5. Keyboard -- Input Source set to *English (US, intl.
   with dead keys)*. This makes it easy to add accents while you're
   typing.
8. Language and Region -- Language & Formats to English
   (Canada).
9. Set Default Applications (This can be done later once you've
   installed your other applications).
10. Date & Time -- Automatically detect time zone, and
    set the clock to 24-hours.
11. Update the Recovery Partition.

### GNOME Extensions
[Gnome Extensions Website](extensions.gnome.org)

Pop_OS installs quite a few extensions by default. Here are some of my
personal favourites that I can't go without.

#### GNOME-tweaks

Why the GNOME desktop doesn´t include these settings out of the box is
beyond me. However, some of the configuration I like to have in GNOME
would be impossible without gnome-tweaks. To get it, run the command
below:

  sudo apt install gnome-tweaks

Hit the super key, and type in `Tweaks` to open the extension settings.

1. Keyboard & Mouse > Additional Layout Options > Caps Lock Behavior >
   Caps Lock is also a Ctrl
2. Top Bar -- Toggle Weekday, Date & Seconds ON.

#### Dash to Panel
[Dash to Panel](https://extensions.gnome.org/extension/1160/dash-to-panel/)

Moves your top bar to the bottom of the screen and acts like a
traditional Windows taskbar. It's really fantastic.

1. Position:
  1. Display Panels on all monitors.
  2. Panel: Bottom.
  3. Panel Thickness to ~32px.
  4. Hide Applications and Desktop buttons.
  5. Slide Date menu to the complete right.
2. Style:
  1. App Icon Margin to 4px.
  2. App Icon Padding to 6px.
3. Behavior:
  1. Show favorite applications, on secondary panels and show running
     applications.

#### Sound Input & Output Device Chooser
[Sound Input & Output Device Chooser](https://extensions.gnome.org/extension/906/sound-output-device-chooser/)

Adds the ability to select your audio device from the taskbar. You can
hide any devices you don´t want displayed to clean up the selector.

## Adding extra HDDs & SSDs

If you have any additional drives to add, you can do so from the `Disks`
utility. To do so, Hit Super, and type `Disks`.

1. Select the disk drive and format a partition to Ext4 if needed.
2. Mount it so it's available on system startup:
  1. Select the partition, then click the gears > Edit Mount Options
  2. Toggle User Session Defaults OFF.
  3. Mount Point -- Change to /home/<username>/extradrive
  4. Click OK.

If you have multiple partitions to add, change *extradrive* to another
name.

## Drivers & Updates

Whenever you want to install updates, run the following commands in a
terminal:

```
sudo apt update
sudo apt upgrade
```

You should perform these regularly whenever you want to install software
to ensure everything is up to date.

In Pop_OS 21.10, the NVIDIA driver 470.xx is installed by default.
However, you can install a newer version by visiting the `Pop!_Shop`.
Simply hit Super, and type `Pop!_Shop`. Then, go to `Installed`. Once
loaded, the first section should be `Drivers`. Install the latest one
(or the one you prefer). Once installed, reboot your PC.

Once rebooted, hit Super, and type `NVIDIA X server settings`. On the
first tab, you can find the driver version. If it's the same as the one
you last installed, the driver installed properly.

## Password management

1. Install [Bitwarden](https://bitwarden.com/download/)

Personally, I install the application through the .deb file. However,
you cannot receive automatic updates via the .deb file. Hence, it may be
preferable to install as an .AppImage, or through Snap. Once installed,
sign in, as you're going to need your passwords for many of the other
applications and configurations.

2. Sign in
3. Settings:
  1. Check Enable Tray Icon, Close to Tray, Start to Tray, Start
     automatically on login, and Enable browser integration.

## Browsers

1. Install [Brave](https://brave.com/linux/#release-channel-installation)

Despite Firefox being the default and privacy-focused browser, I really
like Brave for the Brave Rewards and for being privacy focused.

2. Settings:
  1. Sync -- Restore the sync code via Bitwarden. This will sync most of the Brave settings, including your bookmarks and extensions.
  2. Appearance -- Set your theme and colours. I like to have the wide
     address bar.
  3. New tab page -- Set to **Blank Page**
  4. Shields -- Advanced View.
  5. Brave Rewards -- Disable Auto-contribute, number of ads to 10. Sign in to Uphold.
  6. Social media blocking -- toggle LinkedIn ON, toggle Facebook OFF.
  7. Security and privacy -- Turn daily usage ping and diagnostic reports OFF. Under Site and Shield settings,
     toggle Location and Notifications to **Don't allow**.
  8. Search engine -- DuckDuckGo or Startpage.
  9. Extensions -- Enable Widevine if planning on using the
     browser Spotify or Netflix.
  10. Wallet -- Sign in via Bitwarden. Change default base currency
      to CAD. 
  11. Auto-fill -- Turn off "Offer to save..." for Passwords,
      Payments and Addresses.
  12. System -- Turn off Brave in the background. Depending on needs,
      toggle Hardware acceleration OFF.

## Terminal

Open the terminal (Super + T)

1. Preferences:
  1. Profile > Colours > Text and Background Color > Solarized Dark
  2. Profile > Colours > Palette > Solarized
2. Install Vim -- `sudo apt install vim`
3. Install [Git](https://git-scm.com/download/linux) -- Add the repo and
   then `sudo apt update; sudo install git`.
4. Install [gh](https://github.com/cli/cli/blob/trunk/docs/install_linux.md) (GitHub CLI)
  1. Sign in by using `gh auth login`, then follow the instructions via
     HTTPS and through a browser.

### Dotfiles

This is a good time to create some directories where we will put all our
repos. Input the following commands in the terminal:

```
mkdir -p ~/github/<username>
cd ~/github/<username>
```

Ideally, <username> should be the same as your github profile as your
.bashrc uses the $USER variable. While staying in the same terminal, paste
the following:

```
gh repo clone ControlShiftEd/dotfiles
cd dotfiles
sh setup
```

The setup file will add a symlink to your `.bashrc`, `.inputrc`,
`.profile`, `.vimrc` and `tmux.conf` files and the `Scripts`
folder from the dotfiles to your $HOME, as well as add the `Scripts`
folder to $PATH. It creates some new aliases for the terminal, which you can view in `.bashrc`. Finally, it
enables auto-completion for `gh`.

It will also install [Ethan Shoover's Solarized for VIM](https://github.com/altercation/vim-colors-solarized) and [Tim Pope's Pathogen](https://github.com/tpope/vim-pathogen).

## Sound Settings
Sound in Ubuntu-based distros are usually managed by PulseAudio. Even
though PA is great for simple configurations or the average user, it's not very flexible if you stream or run a podcast for example. For this reason, I like to route PulseAudio through [JACK](https://jackaudio.org/), a connection kit that allows to route your audio any way you want.

### PulseAudio
#### PAVUControl (PAVUControl)
Additionally to the [Sound Input & Output Device
Chooser](#soundinput&outputdevicechooser), PulseAudio Volume Control
(PAVUControl) is a great little tool which allows you to route different
applications to different devices easily (Just not two at the same
time). You can also disable some unused devices in this application,
such as Digital Cables or unused HDMI sound cables. PAVUControl is a
great tool to change the default audio device for some applications
which don't typically allow you to pick your device within the
application itself (such as Firefox or Spotify). To install, simply type
the following in a terminal:

  sudo apt install pavucontrol

### JACK
[JACK website](https://jackaudio.org/)
I have a whole document explaining how to setup JACK, along with the
KXStudio suite of software. If you prefer something more lightweight, I
also have some instructions on using Qjackctl. Find it [here](JACK.md).

Cadence and Claudia uses LADISH as a back-end to manage sessions.
However, LADISH is now deprecated and has been for several years. The
replacement to it is [New Session Manager](https://github.com/jackaudio/new-session-manager) (NSM). I don't use NSM as I don't make music myself and doesn't meet my current needs.

As of POP_OS 22.04, the default audio driver is now PipeWire. See the
next section with instructions on how to set that up.

### Pipewire
[PipeWire website](https://pipewire.org/)
As of Pop_OS 22.04, PipeWire is now the default audio driver for the
distribution. PipeWire is a happy middle between the simplicity of
PulseAudio and the complexity of JACK, built for the average user.

The best way I've found to set up PipeWire is to set it up similarly to
JACK, minus some of the extra fluff required by JACK to function
properly. As such, the only tools I use for my audio is now QJackCTL and
PAVUControl.

In order to set up PipeWire for my needs, we need to create a quick
script (which is the same as the `JackPulse.sh` script in the
[JACK.md](JACK.md) document). Create a new script file called
`pipepulse.sh`, and paste the following inside:

```
pactl load-module module-null-sink media.class=Audio/Sink sink_name=pipe_sink
pactl load-module module-null-sink media.class=Audio/Sink sink_name=WebMusic_OUT channel_map=stereo
pactl load-module module-null-sink media.class=Audio/Sink sink_name=Discord_OUT channel_map=stereo
pactl set-default-sink pipe_sink
```

Notice how we use `pactl` as a command instead of `pacmd`, and the
slightly different syntax for loading PulseAudio modules.
All these commands are to load Sinks.
If we wanted to load a Source, we'd change the `media.class` to
`Audio/Sink/Source`.
You can also set it as a Sink/Source by changing the media.class to
`Audio/Duplex`, if needed.

Have your new script launch on startup through `Startup Applications`.
Afterwards, follow the instructions for QJackCTL on how to make
persistant connections through the Patchbay. You'll notice that with the
removal of Jack_Mixer, it's much simpler.

PipeWire seems to create new audio devices whenever a new application
launches and requires audio. For example, Discord will use the WebRTC
(which you can set up in the Patchbay to connect to the `Discord_OUT`
and your speakers), and Spotify / Brave will also have new devices
created when they're playing music (Simply connect these to the
`WebMusic_OUT` Sink).

Honestly, I'm very pleased with PipeWire and very surprised there aren't
more Distros or application that take advantage of the simplicity behind
PipeWire.

## Gaming
### Steam
[Steam Website](https://store.steampowered.com/about/)

Click on `Install Steam`, place the .deb file your downloads folder, then install.
As I have an additional HDD, I like to install my games in that disk drive. Once installed and logged in:

1. Settings:
  1. Interface -- Run Steam when PC starts, Display web address toggled
     ON.
  2. Downloads -- Add HDD to the Steam Library Folders
  3. Shader Pre-Caching -- Disable Pre-caching. It causes some games to
     take a long time to load, or not load at all.
  4. Steam Play -- Enable Steam Play for all titles. Set to latest
     version of Proton.

## Social Media & Chat Applications
### Discord
[Discord Website](https://discord.com/)

Click on `Download for Linux`, select .deb and place in your downloads
folder. Then install.

You'll want to configure the Audio for Discord. If you followed the
documentation I provided in the [JACK section](#jack), you can set the
Input Device to `JACK source (Discord_IN)`, and Output Device to the
default (In my case, `JACK sink (PulseAudio JACK Sink)`). I also
recommend disabling automatic input sensitivity and increasing the
threshold to ~45dB, unless your microphone already has good sound
suppresion.

Under Linux Settings, toggle Open Discord on system startup and Minimize
to tray to ON.

Discord runs as a Chromium browser, and just like Brave, you can disable
Hardware Acceleration if necessary under the Advanced tab.

### Signal Messaging
[Signal Website](https://www.signal.org/download/)

Signal is an end-to-end encrypted messaging app for mobile devices.
However, it also comes with a desktop app for Linux. To install, simply
click on `Download for Linux` and follow the instructions there.

## Entertainment
### Spotify
[Spotify Website](https://www.spotify.com/ca-en/download/linux/)

Follow the instructions for installing Spotify in the website above. I
prefer installing it through the .deb package at the bottom of the page.
At the moment, the Startup behaviour for Spotify doesn´t work on Linux.
In the off-chance it works however, you can toggle Spotify to start
Minimized and to minimize when you click the close button.

### VLC
[VLC website](https://www.spotify.com/ca-en/download/linux/)

VLC is probably THE best media player to play local videos and music on
Linux. It supports multiple codecs that the default video and music
player on Linux doesn't. Just get it. Click on the website above, and
install it through the CLI.

## Streaming
### OBS
[OBS Website](https://obsproject.com/download)

Follow the instructions in the website above to install OBS. FFMPEG is
necessary to run OBS on Linux. It will be installed at the same time as
OBS if you follow the instructions from the website.

What makes OBS a fantastic streaming application is its' modularity
through Plugins. At this point, some plugins are installed by default on
Linux (such as Browser source), but here is a short list of some I like
to add.

#### OBS Captions Plugin
[OBS Captions Plugin](https://github.com/ratwithacompiler/OBS-captions-plugin#installation-linux)

Installation for this plugin is as simple as downloading it from the
Releases section and drag & drop in the `~/.config/obs-studio/plugins`
directory. Make sure you place the entire folder in there, and not just
the files. Restart OBS, and it should load on startup.
