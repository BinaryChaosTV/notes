# JACK & Cadence

This is a guide on how to set up JACK. JACK is a powerful audio driver for Linux used for media, podcasting, streaming and music production, but can also be for personal use for sound device management. Due to lack of documentation on all involved parties (except perhaps for the Arch Wiki), JACK is sometimes difficult to implement. After scrounging around, I eventually managed to get it working. I'm hoping these notes will help anyone who is having difficulty setting up JACK.

These instructions assume you're using `PulseAudio` by default, as we'll be routing JACK through it.
I am also writing these instructions while using Pop_OS 21.10. However, I've also managed to do this through Linux Mint 20.3 Una, so am assuming this will working on most, if not all Ubuntu-based distros.

You can also configure JACK to not use Cadence / Claudia / Catia and
simply use the light-weight `Qjackctl`. At the time of writing, I opted
to step away from the KXstudio tools as they're using LADISH, which is
now deprecated. I will include instructions on how to configure
`Qjackctl` as well.

## PulseAudio

### PAVUcontrol

Let's install `PAVUcontrol` as it will allow us to direct any application to any audio device. Particularly useful to route Audio Cables to the right place. Unfortunately, PulseAudio doesn't allow us to do this by default.

	sudo apt install pavucontrol

In the `Configuration` tab, turn off all turn off all unnecessary audio devices. We will return here later once we've set up audio modules in PulseAudio.

### PulseAudio-Module-Jack

As we're feeding PulseAudio through JACK, it's important to install the
following PA module: `sudo apt install pulseaudio-module-jack`. The
configuration files for PulseAudio have JACK instructions embedded in
them already. This will allow you to add virtual cables to customize
JACK the way you want.

## Cadence ([Link to Repo](https://github.com/falkTX/Cadence))

Cadence is an all-in-one type of application by KXStudio that acts as a Front-end for JACK, ALSA and LADISH.

### Add the Repositories

Navigate to the [KXStudio Repositories](https://kx.studio/Repositories) and follow the instructions on how to add the necessary repos to install Cadence.

### Installation

Download the .deb for Cadence. It will include all the tools, Carla, Catia and Claudia.
[Install Cadence](https://kx.studio/Repositories:Applications#cadence)

### Configuration

Open Cadence and follow the instructions below to have JACK start when you boot your PC.

1. Toggle on `Auto-start JACK or LADISH at login`
	- Once Claudia is configured, click the `...` and select the appropriate Studio.
2. Ensure the Bridges are enabled.
	- ALSA Audio Bridge Type: ALSA > PulseAudio > JACK (Plugin)
	- ALSA Midi `Export hardware ports` + `Start with Jack` toggled on.
	- PulseAudio Auto-start at login and bridged to JACK.
3. Select `Configure`:
	- Engine:
		- Self Connect Mode to `Ignore all self connect requests`. This option is bugged. See [Claudia](#claudia) on how to circumvent this.
	- Driver: 
		- Select your default Microphone and Speakers under Input/Output. Enable `Duplex Mode`.
		- Select your Sample Rate. Mine is set to 48000Hz. Yours may need to be decreased to 44100 Hz depending on your devices.
		- Buffer Size is 1024 by default. If you're experiencing too many Xruns (as crackling on skipping), then it may need to be increased. Increase/Decrease incrementally until you no longer get Xruns.
		- Periods/Buffer left at 2. You can set this to 4 if you're using USB speakers. Then, test Buffer Size with new Period / Buffer.
4. Under `System Checks`, if you see that your user hasn't been added to the audio grounp automatically (there's a big red **No** sign) you can enter the following command: `sudo adduser {insert username} audio`. On your next reboot, there should be a green checkmark.

## Adding Addtional virtual cables through PulseAudio
The following page on the Arch Wiki does a great job explaining how to add audio drivers to PulseAudio: [Arch Wiki PulseAudio Examples](https://wiki.archlinux.org/title/PulseAudio/Examples)

We add some extra sound modules in PulseAudio to allow us to separate and reroute sound from certain applications through Jack_Mixer and back into our Speakers. This is particularly useful for streaming, podcasting and whatnot. You can now route your music, radio, voice calls, etc., through your Microphone which would otherwise not be possible through PulseAudio alone. Navigate to `/etc/pulse/` and add the following lines in `default.pa` (You may need to run as root):

```
load-module module-jack-sink client_name=WebMusic_OUT
load-module module-jack-sink client_name=Discord_OUT
load-module module-jack-source client_name=Discord_IN
```

I typically place these after the line saying "Load these modules statically".

The `Discord_In` is useful for when you want Discord calls to be able to listen to your music or desktop audio through your microphone.
**Add more as needed. Remember that an Output (Sink) is where the sound would come from, whereas the Input (Source) is where the sound is going into.**

In order for to see if you've created the audio cables properly, run `pulseaudio -k` in your terminal. Check to see if your newly created cables appear in your sound devices menu. Otherwise, reboot your PC.

## JACK-MIXER ([Link to Repo](https://github.com/jack-mixer/jack_mixer))

Jack-Mixer is just that -- A volume mixer for your sound devices. It's a great way to feed your connections' inputs and outputs in Claudia. However, it's not exactly the simplest thing to configure.

Jack-Mixer also appears to be available through the APT repos on Pop_OS.
You can install it easily with the following command:

	sudo apt install jack-mixer

### Dependencies and Requirements

Start by checking your Python version using `Python3 --version`. If it's close to the current release, you'll be fine. You may need to also install `pip` if your distro doesn't currently have it:

```
sudo apt update
sudo apt install python3-venv python3-pip
python -m pip install --upgrade pip
```

Then, follow the instructions to install Jack-Mixer: [For Ubuntu/Debian based distros](https://github.com/jack-mixer/jack_mixer/wiki/Installing-on-debian---Ubuntu)

On the documentation for Jack-Mixer, `python3-appdirs`, `ninja-build`, and `gettext` are forgotten. If we don't install these, we get errors during the build steps. Or at least I did. On top of the dependencies and requirements from the Jack-Mixer repo, I recommend installing the following as well:

`sudo apt-get install python3-appdirs ninja-build gettext`

### Configure Jack_Mixer

Once it's all installed and running, configure `Jack_Mixer` and add inputs for the speakers and microphone. Add additional inputs for your other programs (the ones for which you created modules in you intend on connecting separately (such as Discord, Google Voice, Spotify, etc.). Save the config as `config.xml` under the path `~/.config/jack_mixer/`

## Claudia

Claudia is a visual front-end for LADISH connections. It allows you to connect your audio devices to one another. It also allows you to connect Equalizers and Mixers, such as [JACK-Mixer](#jack-mixer), and saves your connections as a studio so they can be loaded when you start JACK.

In `Claudia`, add a custom application and add the following in the command text box:

	jack_mixer -c ~/.config/jack_mixer/config.xml

Do **not** toggle the `Run in terminal` option. Leave at Level 0. Jack_mixer will now run automatically everytime we start our Studio in Claudia. As our studio also begins on system startup, so will Jack_Mixer.

Connect your inputs and outputs as you see fit. Once all your connections are setup, go ahead and save your Studio. You can then have your studio start automatically when JACK starts ([See Step 1 in Configuration](#configuration)). 

There is currently a bug in Cadence which ignores the `Ignore All Self-Connect Requests` option in the settings. That means that everytime you start up your studio, your connections will be reset and you will see connection all over the place. The current workaround is to modify your Studio `.xml` file. **This workaround gets reset every time you save your studio. It's best to do this once all your connections are finalized.** Navigate to `~/.ladish/studios/`, then open your Studio `.xml` file. Add an `a` in the `<parameter path="/engine/self-connect-mode">a</parameter>` line. The `a` tells Cadence to ignore all self-connect requests. Save and exit the config file.

### Catia

Catia is a similar application to Claudia, but is used for the current session only. When you restart your PC, all connections made under Catia are lost. If you want to save your connections, it's best to use Claudia.

## Qjackctl

As mentioned previous, `Qjackctl` is a light-weight JACK tool which
accomplishes many of the same functions as KXStudio. However, it's not
as feature-rich. At the time of writing, my needs don't require me to
use more than this.

To install, simply run the following command through the APT repos:

	sudo apt install Qjackctl

Qjackctl has a few options: a Connections tab that looks a lot like
Catia (Which only works for the current session), a Patchbay which persists whenever you reboot your PC (this is the one you likely want) and a Session, which is a sort of mix between Connections and the Patchbay.

Once you click on Setup, you'll want to configure it to your liking.
Here is how I've set mine up:

### Settings

Samplerate: 48Khz
Frames/Period: 1024
Periods/Buffer: 2
Realtime: checked
Driver: alsa

Self connect mode: Ignore all self connect requests

Then set your Output/Input devices.

### Misc

I've checked the following boxes:

- Start JACK audio server on application startup
- Enable system tray icon
- Start minimized to system tray
- Save JACK audio server configuration to .jackdrc
- Enable ALSA Sequencer support
- Single application instance
- Replace Connections with Graph button

### Options

Instead of modifying the `/etc/pulse/default.pa`, you can create a bash
script which will create the JACK audio modules in PulseAudio and which
will run at system startup.

Create a bash script:

	vi jackpulse.sh

Then copy the following text:

```
jack_mixer -c ~/.config/jack_mixer/config.xml &
pacmd load-module module-null-sink media.class=Audio/Sink sink_name=pipe_sink
pacmd load-module module-null-sink media.class=Audio/Sink sink_name=WebMusic_OUT channel_map=stereo
pacmd load-module module-null-sink media.class=Audio/Duplex sink_name=DISCORD_IN_OUT channel_map=stereo
pacmd set-default-sink pipe_sink
```

This creates a default sink/source for your default speakers and
microphone. It also creates a few audio cables for my browser / Spotify,
and another for Discord. Finally, it sets the first cables as my default
ones.

If you have a headset like I do, then you may want to be able to route some software solely to your headset (like Discord), and others to your speakers. The `alsa_out` line creates another audio cable to your soundcard found in your headset. Keep in mind that JACK does not "support this" as it may cause some audio to be delayed after a while, but the option is there.

In `Qjackctl`, toggle `Execute script after Startup` and add the script
you just created. If you have a Scripts folder for example, you can
input something that looks like this: `/home/<username>/Scripts/jackpulse.sh`

If you run all your devices through Jack-mixer for example, and you want
it to start with `Qjackctl`, you can toggle the `Execute script on
Startup`, and input something like this (based on the instructions
above in the [JACK-MIXER](#jack-mixer) segment: `jack_mixer -c ~/.config/jack_mixer/config.xml &`

The & is important so you can continue to perform actions w/ Qjackctl.

You'll also want to toggle `Activate Patchbay Persistence` once you've
configured your connections to your liking and saved the session so you
it starts whenever you boot your PC.

## Starting JACK on system boot

If you want JACK to start on system boot, simply add Qjackctl or Cadence (depending on the option you choose) to your startup applications.
