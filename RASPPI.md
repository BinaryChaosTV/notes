# RASPBERRY PI

Some Raspberry Pi don't allow the user to connect automatically to an Enterprise level network through the GUI. You need to set it up manually through the CLI. In our case, we need to set up the RPi to connect to the Board network. I will share the process, without sharing any of the important details.

Specs:
Device: Raspberry Pi4 ModelB 4GB
OS: Raspbian Buster


## Configuring a network on Raspberry Pi 4B on Enterprise

If you're running on Raspbian Buster, there's apparently a bug to connect to WPA2 Enterprise networks. We need to first start in the `dhcpcd.conf`.

    sudo nano /etc/dhcpcd.conf

At the end of the file, add the following lines:

```
interface wlan0
env ifwireless = 1
env wpa_supplicant_driver = wext, nl80211
```
Then save & close `dhcpcd.conf`.

Next, open up `wpa_supplicant.conf`, which is where we set up the network manually.

    sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

Here should be the contents of `wpa_supplicant.conf`. Note that where present, the quotation marks are required.

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=CA

network={
ssid="<your network name>"
proto=RSN
key_mgmt=WPA-EAP
pairwise=CCMP
auth_alg=OPEN
eap=PEAP
identity="<Your login username>"
password=hash:<Hashed password. See below for details.>
phase1="Peaplabel=0"
phase2="auth=MSCHAPV2"
priority=1
}
```

Save & Close, then either `sudo reboot` your Raspberry Pi, or run the following command to restart your network card: `wpa_cli -i wlan0 reconfigure`

### Creating a Hashed Password

By default, `wpa_supplicant.conf` requires the user to input their password for login in plain text, which is not recommended if different users will access your device. For that reason, we want to transform the password into a hash (a series of letters and numbers), which will be read by the Raspberry Pi as your password later. To do so, input the following command in your terminal:

    echo -n <your_password_in_plaintext> | iconv -t utf16le | openssl md4

A 32-character string will show up in your terminal. Copy that string into your `wpa_supplicant.conf` in the password section. Save & Close, then `sudo reboot` your Raspberry Pi, or run the following command to restart your network card: `wpa_cli -i wlan0 reconfigure`. In order for the Raspberry Pi terminal to forget your input, run the following:

    history -c
    rm ~/.bash_history
