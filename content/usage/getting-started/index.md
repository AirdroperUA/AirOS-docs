+++
title = "Getting Started"
description = "AirOS getting started instructions."
date = 2023-12-04T20:00:00+11:00
template = "docs/page.html"
sort_by = "weight"
weight = 20
draft = false

[extra]
lead = ''
toc = true
top = false
+++
## Network Configuration

Your topside computer’s network configuration should be the same as for the previous Companion software.
To configure it, you can follow our [network setup instructions](https://www.ardusub.com/quick-start/installing-companion.html#network-setup).

## Web Interface

AirOS is designed as a modular collection of services, which are accessed and configured via a combined web interface.

The web interface monitors the autopilot and other main software components. It also listens for and displays connections from other HTTP servers (on TCP ports), which allows extensions and custom integrations to provide an interface through AirOS while remaining independent from the main AirOS release/update cycle.

### Interface Access

- By default you can access AirOS via [airos.local](http://airos.local/)
    - This applies if the AirOS device is connected to via a direct ethernet connection, or [USB-OTG](#usb-otg)
    - On an ethernet connection you can also access AirOS via its static IP address ([192.168.2.2](http://192.168.2.2/))
- When AirOS is connected to the same wifi network as your device you can also connect with it using [airos-wifi.local](http://airos-wifi.local/)
- By default if AirOS does not have a wifi connection configured within 5 minutes of booting, it will start its own wifi hotspot which, when connected to, allows accessing the AirOS interface via [airos-hotspot.local](http://airos-hotspot.local/)
    - The hotspot SSID is `AirOS (******)`, with password `airdroper`

### Wizard

When AirOS is newly installed the interface provides a configuration wizard to help get things set up. 

The Welcome section allows skipping the wizard if AirOS and your vehicle have already been configured as desired:
{{ easy_image(src="wizard-welcome", width=450, center=true) }}

To support AirOS and autopilot firmware updates, it is recommended for AirOS to be connected to the internet:
{{ easy_image(src="wizard-wifi", width=450, center=true) }}

AirOS supports multiple vehicle types, and allows selecting a quick-setup option for the most common ones:
{{ easy_image(src="wizard-vehicle", width=450, center=true) }}

Vehicle quick-setup involves setting appropriate parameters for the selected vehicle type and frame, as well as choosing a name for your vehicle, and changing the mDNS hostname if you would prefer to connect with something other than [http://airos.local](http://airos.local):
{{ easy_image(src="wizard-parameters", width=450, center=true) }}

Progress is displayed for any selected configuration changes, and an up to date autopilot firmware is downloaded and installed (if using a standard vehicle type):
{{ easy_image(src="wizard-progress", width=450, center=true) }}

A completion window is shown once all configuration is done:
{{ easy_image(src="wizard-complete", width=450, center=true) }}

### Interface Features

When you first open the web interface, you'll see a page that looks like this:
{{ easy_image(src="../advanced/interface-overview", width="600") }}

As a brief overview,
- the header contains system health indicators and notifications, and some network and display configuration options
- the sidebar allows navigating between pages, and allows restarting, freeing up space, and reporting issues
- most pages show their content within the AirOS interface sections, but some extensions open as full pages in a separate tab

For more details see the Advanced Usage [Interface Overview](../advanced/#interface-overview) section.

## Updating / Releases

AirOS supports [multiple release types](../overview/#release-types) - we recommend the latest stable version for most people. Releases and change-logs are available on the [GitHub releases page](https://github.com/airdroperua/airos-docker/releases).

### Connect Internet

When starting out, it's important to connect your vehicle to the internet so you can update to the latest suitable release.
Internet connectivity is possible via either [wifi](#connect-wifi) or [passed through a tether](#tether-passthrough).

#### Connect Wifi

1. First, click the wifi indicator to scan for available wifi networks

   ![Wifi Scan](wifi-scan.png)
1. Select the desired network, type in the password, and click connect

   ![Wifi Login](wifi-login.png)
1. Once connected, the wifi icon will change to show the signal strength, and the connected wifi network will be selected on the menu

   ![Wifi Confirm](wifi-confirm.png)

#### Tether Passthrough

- [macOS instructions](https://support.apple.com/en-au/guide/mac-help/mchlp1540/mac)
- [Windows instructions](https://pureinfotech.com/share-internet-connection-windows-10/)
   - [Windows 10 troubleshooting](https://discuss.airdroper.org/t/windows-10-cellular-while-flying-rov/14816)
- [Ubuntu (Linux) instructions](https://unix.stackexchange.com/questions/575178/sharing-wifi-internet-through-ethernet-interface)
- [Arch (Linux) instructions](https://wiki.archlinux.org/title/Internet_sharing)

Once the internet passthrough has been configured, the AirOS header should 
[show that it has internet connectivity](../advanced/#internet-status-and-management).

### Select Version

Now that your AirOS has an internet connection, you can perform the update to the latest available version.

1. Click on the hamburger menu (if the sidebar is not already open)

   ![Version Menu](version-menu.png)
1. Under **Settings**, select [**AirOS Version**](../advanced/#airos-version)

   ![Version Chooser](version-chooser.png)
1. If you're already on the latest version, the right side of your Local Version will be blank. If not, you should see a blue **Update** button.

   <img src="../advanced/version-chooser-simple.png" width="550">

1. Once the update button is clicked the update process will run.
   Please wait until it finishes - it will automatically reload the webpage for you.

## Camera Streams

AirOS is capable of configuring and streaming multiple cameras simultaneously. The first time it boots, it will automatically detect any connected H264-capable cameras and start streaming them. If not, make sure your camera is properly connected, and that AirOS is on the latest available version. Reset settings and restart AirOS if necessary.

Additional information is available in the Advanced Usage [Video Streams](../advanced/#video-streams) section.

## USB OTG
It is possible to connect with a Raspberry Pi 4 through its USB-C port[^1]. Once it's connected, the interface should be available though [airos.local](http://airos.local)[^2].

[^1]:A Raspberry Pi draws more power than many computer USB ports can provide, so a USB-C connection should generally only be used for data transfer, with a separate [power supply](@/integrations/hardware/required/power-supply/index.md) (or through a powered USB hub).

[^2]:The `usb0` network interface is configured to use a DHCP server at `192.168.3.1` by default, which does **not** provide access via the [192.168.2.2](http://192.168.2.2) static IP address.

### Mac configuration
If you are using MacOS, make sure to allow the RNDIS/ethernet gadget:
{{ easy_image(src="allow-rndis", width="300") }}

MacOS will consider a USB-OTG connection as a valid source of internet, which can be avoided by reordering your network interfaces, making sure that wifi and your actual wired internet connection interface are used with higher priority.

{{ easy_image(src="network-settings", width="600") }}

{{ easy_image(src="network-service-order", width="400") }}
