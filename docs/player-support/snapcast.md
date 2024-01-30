# Snapcast ![Preview image](../assets/icons/snapcast-icon.svg){ width=70 align=right }

Music Assistant supports Snapcast, a powerful solution for synchronized multi-room audio streaming. Snapcast enables seamless playback across various devices, creating an immersive audio experience.
Whether you're using Snapcast-compatible speakers or devices like the Raspberry Pi, you can enjoy synchronized audio playback effortlessly. This component was contributed and maintained by [SantiagoSotoC](https://github.com/Santiagosotoc)

In order to use this provider you need to also have a Snapcast server running on your network. The diagram below shows a possible combination of outputs. In the diagram a Raspberry Pi runs the server which communicates to MA and all of the clients. The server running Pi is also running Snapclient and is connected to a set of speakers. Then there is another Pi running Snapclient in another room, a phone running Snapdroid and a laptop running Snapweb,

![Preview image](../assets/snapcast.png){ width=800 }

## Features

- Synchronized playback across all Snapcast devices.
- Lossless audio quality with options for 48 kHz /16bits PCM.

## Known Issues / Notes

- The Snapcast provider is not enabled by default. When enabled the Snapcast server IP and port must be entered. From this point the workings of the server are transparent and the clients appear in the MA UI. 
- Music Assistant currently implements Snapcast version 0.27.0 only; future updates may include support for newer versions.
- Snapcast players don´t support crossfading of audio by default. For full gapless support and enhanced crossfading, "enable crossfade" in the player's advanced settings.
- If it is necessary to adjust the latency of a client, it must be done from another interface such as snapdroid or snapweb
- Pausing the player is NOT supported. If you try and do that you will get weird behaviour.
- The Snapcast app for iOS is broken as it uses an old version of Snapclient. Using it brings problems with this provider.
- Ensure the server is launched with the command "snapserver -v".
- Ensure that the ports on the Snapserver host 1704, 1705 are open. Also ensure that for each client a port equal to or greater than 4953 is open.
- Try the default Snapcast settings and then make changes as necessary.
- Client names for all clients can be adjusted in Snapweb and Snapdroid via their respective UIs

## Install Snapserver

<h3>On Linux</h3>

Snapcast packages are available for several Linux distributions:

- [Debian](#debian)
- [OpenWrt](#openwrt)
- [Alpine Linux](#alpine-linux)
- [Gentoo Linux](#gentoo-linux)
- [Archlinux](#archlinux)
- [Void Linux](#void-linux)

<h3>Debian</h3>

For Debian (and Debian-based systems, such as Ubuntu, Linux Mint, elementary OS), download the package for your CPU architecture from the [latest release page](https://github.com/badaix/snapcast/releases/latest).

For example, for Raspberry Pi `snapserver_0.27.0-1_armhf.deb`, for laptops `snapserver_0.27.0-1_amd64.deb`.

<h4>Using apt 1.1 or later:</h4>

```bash
sudo apt install </path/to/snapserver_0.27.0-1_[arch].deb>
```

If you encounter the error `E: Unable to locate package` then do this in two steps:
```
sudo wget https://github.com/badaix/snapcast/releases/download/v0.27.0/snapserver_0.27.0-1_armhf.deb
sudo apt install ./snapserver_0.27.0-1_armhf.deb
```

<h4>Using dpkg:</h4>

Install the package:

```bash
sudo dpkg -i </path/to/snapserver_0.27.0-1_[arch].deb>
```

Install missing dependencies:

```bash
sudo apt-get -f install
```

<h3>Alpine Linux</h3>

On Alpine Linux, use:

```bash
apk add snapcast-server
```

<h3>Gentoo Linux</h3>

On Gentoo Linux, use:

```bash
emerge --ask media-sound/snapcast
```

<h3>Archlinux</h3>

On Archlinux, Snapcast is available through the AUR. To install, use your favorite AUR helper, or do:

```bash
git clone https://aur.archlinux.org/snapcast
cd snapcast
makepkg -si
```

<h3>Void Linux</h3>

To install the server:

```bash
# xbps-install snapserver
```

## Enable SnapWeb

This will enable MA to stream directly to any browser.

1. **Locate the Snapcast Configuration File:**
   - Find the configuration file for Snapcast on your system. It is often named `snapserver.conf`. The location of this file is normally found in /etc but may vary depending on your installation method and operating system.

2. **Edit the Configuration File:**
   - Open the `snapserver.conf` file in a text editor.

3. **Enable Snapweb:**
   - Look for a section in the configuration file related to http. There might be a line that looks like `doc_root = /usr/share/snapserver/snapweb` or similar. If this line is commented out (starts with `#`), remove the `#` to uncomment it and enable Snapweb. 
4. **[Optional]: Change web port**
     - Look for a section in the configuration file related to http. There might be a line that looks like `webport = 1780` or similar. If this line is commented out (starts with `#`), remove the `#` to uncomment it and enable Snapweb. You may also need to adjust the port number if desired.

5. **Save the Changes:**
   - Save the changes to the configuration file.

6. **Restart Snapcast:**
   - Restart the Snapcast server to apply the changes to the configuration. This can usually be done by restarting the Snapcast service or process.

7. **Access Snapweb:**
   - Open a web browser and navigate to the specified port (e.g., `http://localhost:1780` if the default port is used). This should allow you to access the Snapweb interface.

## Install Snapclient

<h3>Linux</h3>

Snapcast provides packages for various Linux distributions to install the Snapclient.

<h4>Debian-based systems (e.g., Ubuntu, Linux Mint)</h4>

1. Download the Snapclient package for your CPU architecture from the [latest release page](https://github.com/badaix/snapcast/releases/latest).

   - For Raspberry Pi: `snapclient_0.27.0-1_armhf.deb`
   - For laptops: `snapclient_0.27.0-1_amd64.deb`

2. Using `apt`:

   ```bash
   sudo apt install </path/to/snapclient_0.27.0-1_[arch].deb>
   ```

3. Alternatively, using `dpkg`:

   ```bash
   sudo dpkg -i </path/to/snapclient_0.27.0-1_[arch].deb>
   sudo apt-get -f install
   ```
<h4>Alpine Linux</h4>

On Alpine Linux, use:

```bash
apk add snapcast-client
```

<h4>Gentoo Linux</h4>

On Gentoo Linux, use:

```bash
emerge --ask media-sound/snapcast
```

<h4>Archlinux</h4>

On Archlinux, Snapcast is available through the AUR. To install, use your favorite AUR helper, or do:

```bash
git clone https://aur.archlinux.org/snapcast
cd snapcast
makepkg -si
```

<h4>Void Linux</h4>

To install the client:

```bash
# xbps-install snapclient
```

## Install Snapdroid

Snapdroid is an Android app designed for use with Snapcast. It can be found on the Google Play Store.
