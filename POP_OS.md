tags: #distributions, #debian-based
# Setting up Pop_OS!

This is a quick guide on programs to install on a fresh install of Pop_OS! At the time of writing, Pop_OS! is currently on version 22.04.

In some cases, I will share some links to the installation instructions. In other cased, I will simply paste the command to input in the CLI, for simplicity. I will also try my best to divide the instructions in categories, in a logical order so you're not missing a dependency when you reach that step.

## GNOME
Pop_OS! 22.04 uses GNOME version 42 as its default. It comes with many features, but we install some more to make it more personal. 

### Settings
Open Settings (Use the Super Key, and type `settings` in the box).

1. Disable Bluetooth
2. Desktop:
	1. General -- toggle the Workspace and Applications button OFF. Toggle the Minimize and Maximize buttons ON.
	2. 1. Pick a background, set the appearance to **Dark**.
	3. 2. Disable Dock.
3. Notifications -- *Do Not Disturb*.
4. Screen Display -- Activate Night Light to a manual schedule. Set monitors to the way you like.
5. Keyboard -- Input Source set to *English (US, intl. with dead keys)*. This makes it easy to add accents while you're typing.
8. Language and Region -- Language & Formats to English (Canada).
9. Set Default Applications (This can be done later once you've
   installed your other applications).
10. Date & Time -- Automatically detect time zone, and set the clock to 24-hours.
11. Update the Recovery Partition.

### GNOME Extensions
[Gnome Extensions Website](https://extensions.gnome.org)

Pop_OS installs quite a few extensions by default. Here are some of my personal favourites that I can't go without.

#### GNOME-tweaks

For some reason, the GNOME desktop doesn't include these settings out of the box. However, some of the configuration I like to have in GNOME would be impossible without gnome-tweaks. To get it, run the command below:

	sudo apt install gnome-tweaks

Hit the super key, and type in `Tweaks` to open the extension settings.

1. Keyboard & Mouse > Additional Layout Options > Caps Lock Behavior > Caps Lock is also a Ctrl
2. Top Bar -- Toggle Weekday, Date & Seconds ON.

#### Dash to Panel
[Dash to Panel](https://extensions.gnome.org/extension/1160/dash-to-panel/)

Moves your top bar to the bottom of the screen and acts like a traditional Windows taskbar. It's really fantastic.

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
	1. Show favorite applications, on secondary panels and show running applications.

#### Sound Input & Output Device Chooser
[Sound Input & Output Device Chooser](https://extensions.gnome.org/extension/906/sound-output-device-chooser/)

Adds the ability to select your audio device from the taskbar. You can hide any devices you don´t want displayed to clean up the selector.

## Adding extra HDDs & SSDs

If you have any additional drives to add, you can do so from the `Disks` utility. To do so, Hit Super, and type `Disks`.

1. Select the disk drive and format a partition to `Ext4` if needed.
2. Mount it so it's available on system startup:
	1. Select the partition, then click the gears > Edit Mount Options
	2. Toggle User Session Defaults OFF.
	3. Mount Point -- Change to `/home/<username>/extradrive`
	4. Click OK.

If you have multiple partitions to add, change *extradrive* to another name.

## Drivers & Updates

Whenever you want to install updates, run the following commands in a terminal:

	sudo apt update
	sudo apt upgrade

You should perform these regularly whenever you want to install software to ensure everything is up to date.

In Pop_OS!, the NVIDIA driver is installed by default. However, you can install a newer version by visiting the `Pop!_Shop`. Simply hit Super, and type `Pop!_Shop`. Then, go to `Installed`. Once loaded, the first section should be `Drivers`. Install the latest one (or the one you prefer). Once installed, reboot your PC.

Once rebooted, hit Super, and type `NVIDIA X server settings`. On the first tab, you can find the driver version. If it's the same as the one you last installed, the driver installed properly.

## Password management
### Bitwarden
[Bitwarden](https://bitwarden.com/download/)

Personally, I install the application through the .deb file. However, you cannot receive automatic updates via the .deb file. Hence, it may be preferable to install as an .AppImage, or through Snap. Once installed, sign in, as you're going to need your passwords for many of the other applications and configurations.

2. Sign in
3. Settings:
	1. Check Enable Tray Icon, Close to Tray, Start to Tray, Start automatically on login, and Enable browser integration.

## Browsers
### Brave
[Brave](https://brave.com/linux/#release-channel-installation)

Despite Firefox being the default and privacy-focused browser, I really like Brave for the Brave Rewards and for being privacy focused.

#### Settings:
  1. Sync -- Restore the sync code via [Bitwarden](#Bitwarden). This will sync most of the Brave settings, including your bookmarks and extensions.
  2. Appearance -- Set your theme and colours. I like to have the wide address bar.
  3. New tab page -- Set to **Blank Page**
  4. Shields -- Advanced View.
  5. Brave Rewards -- Disable Auto-contribute, number of ads to 10. Sign in to Uphold.
  6. Social media blocking -- toggle LinkedIn ON, toggle Facebook OFF.
  7. Security and privacy -- Turn daily usage ping and diagnostic reports OFF. Under Site and Shield settings, toggle Location and Notifications to **Don't allow**.
  8. Search engine -- [DuckDuckGo](https://duckduckgo.com), [Startpage](https://startpage.com) or [Brave Search](https://search.brave.com).
  9. Extensions -- Enable Widevine if planning on using the browser [Spotify](#Spotify) or Netflix.
  10. Wallet -- Sign in via [Bitwarden](#Bitwarden). Change default base currency to CAD. 
  11. Auto-fill -- Turn off "Offer to save..." for Passwords, Payments and Addresses.
  12. System -- Turn off Brave in the background. Depending on needs, toggle Hardware acceleration OFF.

## Terminal
Follow the preferences found in my [Terminal Documentation](TERMINAL.md).

## Audio Settings
As of 22.04, [PipeWire](#pipewire) is now the default sound driver. We'll use [PulseAudio](#pulseaudio) modules to route our sound device connections in PipeWire to provide us the flexibility that [JACK](JACK.md) would have provided in the past.

### PulseAudio
#### PAVUControl
Additionally to the [Sound Input Output Device Chooser](#sound-input--output-device-chooser), PulseAudio Volume Control (PAVUControl) is a great tool which allows you to select which device is used for each application (Just not two at the same time). You can also disable some unused devices in this application, such as Digital Cables or unused HDMI sound cables. PAVUControl is a great tool to change the default audio device for some applications which don't allow you to pick your device within the application itself (such as Firefox or [Spotify](#Spotify)). To install, simply type the following in a terminal:

	sudo apt install pavucontrol

### PipeWire
[PipeWire website](https://pipewire.org/)
As of Pop_OS 22.04, PipeWire is now the default audio driver for the distribution. PipeWire is a happy middle between the simplicity of PulseAudio and the complexity of [JACK](JACK.md), built for the average user.

The best way I've found to configure PipeWire is to set it up similarly to [JACK](JACK.md), minus some of the extra fluff required by [JACK](JACK.md) to function properly. As such, the only tools I use for my audio is [QJackCTL](JACK.md#qjackctl) and [PAVUControl](#pavucontrol).

In order to set up PipeWire for my needs, we need to create a script to add new virtual cables each time we boot our PC (they don't persist on reboot). Create a new script file called `pipepulse.sh`, and paste the following inside:

```
pactl load-module module-null-sink media.class=Audio/Sink sink_name=pipe_sink
pactl load-module module-null-sink media.class=Audio/Sink sink_name=WebMusic_OUT channel_map=stereo
pactl load-module module-null-sink media.class=Audio/Sink sink_name=Discord_OUT channel_map=stereo
pactl set-default-sink pipe_sink
```

Notice how we use `pactl` as a command instead of `pacmd`, and the slightly different syntax for loading PulseAudio modules. All these commands will load Sinks. If we wanted to load a Source, we'd change the `media.class` to `Audio/Sink/Source`. You can also set it as a Sink/Source by changing the media.class to `Audio/Duplex`, if needed.

Have your new script launch on startup through `Startup Applications`. Afterwards, follow the instructions for [QJackCTL](JACK.md#qjackctl) on how to make persistant connections through the Patchbay. You'll notice that with the removal of [Jack_Mixer](JACK.md#jack-mixer), it's much simpler.

PipeWire creates new audio devices whenever a new application launches and requires audio. For example, [Discord](#Discord) will use the WebRTC (which you can set up in the Patchbay to connect to the `Discord_OUT` and your speakers), and [Spotify](#Spotify) / [Brave](#Brave) will also have new devices created when they're playing sound (Simply connect these to the `WebMusic_OUT` Sink).

## Gaming
### Steam
[Steam Website](https://store.steampowered.com/about/)

Click on `Install Steam`, place the .deb file your downloads folder, then install. As I have an additional HDD, I like to install my games in that disk drive. Once installed and logged in:

1. Settings:
	1. Interface -- Run Steam when PC starts, Display web address toggled ON.
	2. Downloads -- Add HDD to the Steam Library Folders
	3. Shader Pre-Caching -- Disable Pre-caching. It causes some games to take a long time to load, or not load at all.
	4. Steam Play -- Enable Steam Play for all titles. Set to latest version of Proton.

## Social Media & Chat Applications
### Discord
[Discord Website](https://discord.com/)

Click on `Download for Linux`, select .deb and place in your downloads folder. Then install.

You'll want to configure the Audio for Discord. If you followed the documentation I provided in the [PipeWire](#Pipewire) section, you can set the Input Device to `pip_sink`, and Output Device  `Discord_OUT`. I also recommend disabling automatic input sensitivity and increasing the threshold to ~45dB, unless your microphone already has good sound suppression.

Under Linux Settings, toggle Open Discord on system startup and Minimize to tray to ON.

Discord runs as a Chromium browser, and just like Brave, you can disable Hardware Acceleration if necessary under the Advanced tab.

### Signal Messaging
[Signal Website](https://www.signal.org/download/)

Signal is an end-to-end encrypted messaging app for mobile devices. However, it also comes with a desktop app for Linux. To install, simply click on `Download for Linux` and follow the instructions there.

## Entertainment
### Spotify
[Spotify Website](https://www.spotify.com/ca-en/download/linux/)

Follow the instructions for installing Spotify in the website above. I prefer installing it through the .deb package at the bottom of the page. At the moment, the Startup behaviour for Spotify doesn´t work on Linux. In the off-chance it works however, you can toggle Spotify to start Minimized and to minimize when you click the close button.

### VLC
[VLC website](https://www.spotify.com/ca-en/download/linux/)

VLC is probably THE best media player to play local videos and music on Linux. It supports multiple codecs that the default video and music player on Linux doesn't. Just get it. Click on the website above, and install it through the CLI.

## Streaming
### OBS
[OBS Website](https://obsproject.com/download)

Follow the instructions in the website above to install OBS. FFMPEG is necessary to run OBS on Linux. It will be installed at the same time as OBS if you follow the instructions from the website.

What makes OBS a fantastic streaming application is its' modularity through Plugins. At this point, some plugins are installed by default on Linux (such as Browser source), but here is a short list of some I like to add.

#### OBS Captions Plugin
[OBS Captions Plugin](https://github.com/ratwithacompiler/OBS-captions-plugin#installation-linux)

Installation for this plugin is as simple as downloading it from the Releases section and drag & drop in the `~/.config/obs-studio/plugins` directory. Make sure you place the entire folder in there, and not just the files. Restart OBS, and it should load on startup.
