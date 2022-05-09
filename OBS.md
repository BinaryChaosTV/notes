tags: #streaming, #twitch, #recording

# Open Broadcast Software (OBS)
[OBS Website](https://obsproject.com) 

## Installation
To install OBS, refer to the installation instructions for Linux which can be found [here](https://obsproject.com/wiki/install-instructions#linux). It can either be installed through the PPA (Ubuntu-based only) or as a Flatpak. FFMPEG is necessary to run OBS on Linux. It will be installed at the same time as OBS if you follow the instructions from the website.

What makes OBS a fantastic streaming application is its' modularity through Plugins. At this point, some plugins are installed by default on Linux (such as Browser source), but here is a short list of some I like to add.

## Plugins
### OBS Captions Plugin
[OBS Captions Plugin Repo](https://github.com/ratwithacompiler/OBS-captions-plugin#installation-linux)

Installation for this plugin is as simple as downloading it from the Releases section and drag & drop in the `~/.config/obs-studio/plugins` directory. Make sure you place the entire folder in there, and not just the files. Restart OBS, and it should load on startup.

## Configuration
OBS is an powerful streaming application which can encode a wide range of resolutions and bitrates without using too much CPU power if you have the right hardware. In my case, my PC is approximately 5-6 years old with a Nvidia GTX 980 and an Intel i7 5930K. I can stream at 1080p 30 FPS depending on the game, but 720p30/60FPS is much more comfortable. Here are the settings I use:

### General
- Toggle `Automatically record when streaming` ON.

### Stream
OBS allows the user to stream to a wide variety of services, including Twitch, Youtube, Twitter, Facebook, Restream and more! Depending on the service you choose, you can also log in in OBS to retrieve some of the Docks for that service (I know this is the case for Twitch at least).

For myself, I usually stream on Twitch. Log in, then select the server of your choice (usually the one closest to you), or leave it on Automatic.

I usually enable the Twitch Chat Add-ons, as they're quite popular nowadays.

### Output
- Output Mode to `Advanced`.
- Audio Track to `1`, and Twitch VOD track to `2`. This allows the Twitch VODs to cut out Copyrighted music from the livestreams.
- Rate Control to `CBR` (Constant BitRate).
- Video Bitrate to about 1/3 of your upload, or to the recommended bitrate for the resolution you're planning to stream at. If you're wondering what the recommended bitrate is, [check this website from Twitch](https://stream.twitch.tv/encoding/). In my case, it's about `4500 Kbps`.
- Leave `Keyframe Intervals` at 0, or increase to 2.
- Preset to `Quality`.
- Profile to `High`.
- GPU to `0`.
- Max B-frames to `2`.

### Audio
- Sample Rate to `48 kHz`.
- Channels to `Stereo`.
- If you followed the instructions on how to set up [Pipewire](POP_OS.md#pipewire), then you can set the Desktop Audio to `pipe_sink`, `WebMusic_OUT`, and the Mic to your selected Microphe.

### Video
- Base Resolution to the resolution on your main monitor. In my case, it's `1920x1080`.
- Output Resolution to `1280x720`, or whatever you'll be streaming at. If you're streaming at 1080p, then leave at the original resolution
- Downscale Filter to `Bicubic`.
- FPS to `30` or `60`.

### Hotkeys
I'm not going to share my hotkeys as this can change per person and per setup. However, it's a good idea to set some hotkeys to hide/show your overlay, to (un)mute your microphone and music, and some more hotkeys to change scenes.

### Advanced
- Color Format to `NV12`.
- Color Space to `709`.
- Color Range to `Full`.
- I do not recommend using a Stream Delay as this can impact your experience to communicate with your chat.
- Automatic Reconnect Enabled to 10s and 20 retries.

## Scenes/Sources
The Linux version of OBS differs in some ways to the Windows version.

### Adding Sinks
If you're like me and use Voicemeeter Banana on Windows, then you might realise that you can't simply use the Cable Output as a "microphone" in the Linux version. In fact, it's almost always preferable to add the device you want as a Source in OBS. 

If you followed the instructions for [Pipewire](POP_OS.md#pipewire), then add Discord as a Source in your scenes and set it as `Discord_OUT` so your viewers can listen to the conversation on Discord. It's always a good idea to set a hotkey to (un)mute your Discord conversation.

## Alerts
**TODO: Create Streamelements.md**
For Alerts, the two major services who offer alerts for donations, subscriptions, hosts, follows, etc. are [Streamlabs](https://streamlabs.com) & [StreamElements](https://streamelements.com). I personally use StreamElements, but the choice is ultimately yours. Streamlabs is more "feature-rich" and have their own version of OBS which integrates with their own features if you prefer (albeit a little bloated for my taste).

To start, pick one of the services and sign-in to your Twitch account. Then, set up your alerts as you desire.

### Triggerfyre
A fun type of alert which uses the Twitch Channel Points system to make audible rewards. Simply add the sound effect of your choice, and link it to the Channel Point reward of your choice. Then, add a `Browser Source` in OBS to Triggerfyre so people can hear the sound effect when they redeem a Channel Point reward.