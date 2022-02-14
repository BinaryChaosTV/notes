# Installing JACK Audio

This is a guide on how to set up JACK audio on your PC. I am creating this as it's difficult to remember how to configure JACK when it's a 'one and done' type of situation. And these applications have little to no documentation available.

## Install PAVUcontrol

Need to install PAVUcontrol as it will allow us to control which application uses which audio device.

`sudo apt install pavucontrol`

Then, turn off all unnecessary audio device in the 'Configuration' tab.

## Cadence

Cadence is an all-in-one type of application by KXStudio that acts as a Front-end for JACK, ALSA and LADISH.
### Add the Repositories
[KXStudio Repositories](https://kx.studio/Repositories)

```
# Install required dependencies if needed
sudo apt-get install apt-transport-https gpgv

# Remove legacy repos
sudo dpkg --purge kxstudio-repos-gcc5

# Download package file
wget https://launchpad.net/~kxstudio-debian/+archive/kxstudio/+files/kxstudio-repos_10.0.3_all.deb

# Install it
sudo dpkg -i kxstudio-repos_10.0.3_all.deb
```

### Install Cadence

Download the .deb for Cadence. It will include all the tools, carla, catia and claudia.
[Install Cadence](https://kx.studio/Repositories:Applications#cadence)

### Setting Up Cadence

1. Toggle on `Auto-start JACK or LADISH at login`
	- Click the `...` and select the appropriate Studio (once Claudia is configured)
2. Ensure the Bridges are enabled.
	- ALSA Audio Bridge Type: ALSA > PulseAudio > JACK (Plugin)
	- ALSA Midi `Export hardware ports` + `Start with Jack` toggled on.
	- PulseAudio Auto-start at login and bridged to JACK.
3. Select Configure
	Engine:
		- Self Connect Mode to `Ignore all self connect requests`.
	Driver: 
		- Select the Microphone and Speakers under Input/Output. Enable Duplex Mode.
		- Sample Rate may be need to be decreased to 44100 Hz.
		- Buffer Size may need to be increased if too many Xruns.
		- Periods/Buffer left at 2.
4. You may also have to add yourself to the `audio` group. If you see **No** in the `System Checks` area, perform the following command:

`sudo adduser {insert username} audio`

### Catia

Catia is used for the current session only. When you restart your PC, all connections made under Catia are lost. If you want to save your connections, it's best to use Claudia.

### Claudia

Claudia saves your connections as a Studio and can be loaded on Startup. This is what we'll use for our setup.

In order for your Studio to load and not automatically reconnect connections on every boot (This seems to be a bug in Cadence), you need to change a little setting in the config file for your studio. Go to `~/.ladish/studios/`, then open your Studio `.xml` file. Then, add an `a` in the `<parameter path="/engine/self-connect-mode"></parameter>` line. The `a` designates to Cadence to ignore all self-connect requests. **This should only be done when you're done connecting all your sources in Claudia!**


## JACK-MIXER

Jack Mixer is a nifty tool we're going to use to redirect connections we made in Claudia. You can control volume from JACK-MIXER. Again however, it's not exactly simple to configure.

[Jack-Mixer GH](https://github.com/jack-mixer/jack_mixer/wiki/Installing-on-debian---Ubuntu)

### Dependencies and Requirements

Start by checking your Python version using `Python3 --version`. If it's close to the current release, you'll be fine. You may need to also install `pip` if your distro doesn't currently have it:

```
sudo apt update
sudo apt install python3-venv python3-pip
python -m pip install --upgrade pip
```

Then, run the following commands to install dependencies and requirements for Jack-mixer.

```
sudo apt-get install \
    build-essential \
    jackd2 \
    libglib2.0-dev \
    libjack-jackd2-dev \
    meson \
    ninja-build \
    pkgconf \
    python3-gi \
    python3-appdirs \
    python3-xdg
    
sudo apt-get install cython3 git python3-docutils
```

On the documentation for Jack-Mixer, `appdirs` and `ninja-build` are forgotten. If we don't these, we get an error during the build steps.

`git clone https://github.com/jack-mixer/jack_mixer`

I ran into a proble with the next step where I´d get an error where `msgfmt` don´t exist. If that happens, run the command `sudo apt-get install gettext`.

```
sed -i -e \
  's|import sys$|import sys, sysconfig\nsys.path.insert(0, sysconfig.get_path("purelib"))|' \
  jack_mixer/__main__.py
```

```
meson setup builddir --prefix=/usr --buildtype=release
ninja -C builddir
sudo ninja -C builddir install
```

### Setting up Jack_Mixer

Once it's all installed and running, configure `Jack_Mixer` and add the speakers and microphone. We'll add another for Music and Discord later. Save the config as `config.xml` under the path `~/.config/jack_mixer/`

In our `Claudia` Studio, right click on the studio and add the following under command:

`jack_mixer -c ~/.config/jack_mixer/config.xml`

Do **not** toggle the `Run in terminal` option. Leave at Level 0.

This will allow Jack_mixer to open our mixer setup everytime the system boots.
You can then enable the Jack_Mixer. You will see it appear in Claudia.

## Adding Addtional virtual cables.
The following page on the Arch Wiki does a great job explaining how to add audio drivers to PulseAudio: [Arch Wiki PulseAudio Examples](https://wiki.archlinux.org/title/PulseAudio/Examples)

We add some extra sound modules on PulseAudio to add extra customization to go through our Jack_Mixer. This is particularly useful for streaming on OBS. Navigate to `/etc/pulse/` and add the following lines in `default.pa` (You need to run as root):

```
load-module module-jack-sink client_name=WebMusic_OUT
load-module module-jack-sink client_name=Discord_OUT
```

I typically place them after the line saying "Load these modules statically".


