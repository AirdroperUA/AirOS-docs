+++
title = "Installation"
description = "AirOS installation instructions."
date = 2023-08-25T07:10:00+11:00
template = "docs/page.html"
sort_by = "weight"
weight = 10
draft = false
aliases = ['/software/onboard/AirOS-latest/installation', '/airos/latest/installation']

[extra]
toc = true
top = false
+++
## Download

AirOS is a ground-up rewrite software to replace Companion. To use it you'll need to download and flash an SD card.
It is compatible with both **Raspberry Pi 3**, **Raspberry Pi 4** and **Raspberry Pi 5**.

The latest available versions are:

[![Latest Stable](https://img.shields.io/github/v/release/airdroperua/airos.svg?label=Latest%20Stable)
![Date](https://img.shields.io/github/release-date/airdroperua/airos?label=Date)](https://github.com/airdroperua/airos/releases/latest/download/AirOS-raspberry.zip)

[![Latest Beta](https://img.shields.io/github/v/tag/airdroperua/airos.svg?label=Latest%20Beta)
![Date](https://img.shields.io/github/release-date-pre/airdroperua/airos?label=Date)](https://github.com/airdroperua/AirOS/releases)

## Flash

We recommend using a fresh SD card, with at least 4GB capacity.

1. Download and install [Balena Etcher](https://www.balena.io/etcher/)
1. Insert the SD card to your computer (you may need an SD card reader)
1. Open Etcher, select the image you just downloaded, and flash it onto the SD card

## Run

1. Eject your SD card with the new AirOS software
1. Insert it into your Raspberry Pi, and power it up!
   - The first boot may take a couple of minutes, as it expands the filesystem to the new SD card capacity
      - It should take around 2 minutes for a 16GB class 10 SD card
1. AirOS is a _headless_ operating system, and uses a web interface rather than HDMI to a monitor
   - See the [Getting Started](../getting-started/) section for how to connect


## Updates

Once AirOS is installed, updating to a different version is simple via the [Version Chooser](../advanced/#airos-version).

## Manual Installation

For developers with alternative hardware, or who would rather install over a pre-installed base operating system / image, AirOS provides an [install directory](https://github.com/airdroperua/AirOS/tree/master/install) with utilities to help perform manual/software-based installations.
