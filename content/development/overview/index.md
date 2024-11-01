+++
title = "Overview"
description = "AirOS development overview."
date = 2023-06-01T19:30:00+11:00
template = "docs/page.html"
sort_by = "weight"
weight = 10
draft = false

[extra]
lead = ''
toc = true
top = false
+++

## Technology Stack

### Structure

{{ easy_image(src="airos-structure", width=450) }}

Conceptually, AirOS is structured around 4 main components:

1. [Bootstrap](#bootstrap)
1. [Core](#core)
1. [Extensions](#extensions)
1. [Cloud](#cloud-infrastructure)

### Development Infrastructure

To understand how AirOS is developed, it's valuable to understand what it's made of, and the ideas it has been designed around:

- Containerised components and services ([Docker](#docker))
    - Containerised, individual services are easier to maintain than a monolithic system
    - Can restart and update containers with very low risk of needing to re-flash SD card
    - Integrators can create their own [Extensions](#extensions), instead of forking the whole system
        - Allows Extension and AirOS version changes without affecting the underlying system
    - Dependencies are isolated, so individual services don’t need to compete for versions
    - Access to system and hardware resources can be limited per Extension
- Web interfaces and APIs
    - Core interface provides consistency, control, and displays active services
    - Services display links to API documentation → simplifies development
    - [Extensions Manager](../../usage/advanced/#extensions-manager) makes it easy to find, install, and manage services from others
        - Includes a web store, where Extensions can be downloaded from
- Frontend (Vue.js, Typescript)
    - Allows for very reactive components (see heartbeats icon)
    - Using reusable Vuetify components to make it look good
    - Relies heavily on [Mavlink2Rest](https://github.com/mavlink/mavlink2rest) (rust-mavlink) for integrating MAVLink with web technologies
- Modern backend technologies
    - Python (3.9) for simple services
    - Rust used for critical services
    - MAVLink-Router for MAVLink routing

#### Docker

[Docker](https://www.docker.com/resources/what-container/) is a lightweight containerisation system that allows packaging one or more software programs into a single executable that can be easily shared across systems. A static package is referred to as a "Docker Image", and when it is being run it takes the form of a "Docker Container".

A Docker Container generally acts like an isolated mini operating system, but it's possible to enter a running Container from the host computer to see (and modify) what the programs inside are doing, and access things like logging output. Importantly, changes in a Container do not affect its [Image](https://docs.docker.com/get-started/overview/#images), so they're not persistent[^1] and it's possible to "start fresh" by simply restarting the Container (which creates a new Container from the Image, without any changes from previous instances).

[^1]:While changes _within_/_to_ the Container are not persistent, it is possible to make persistent changes to the host's file-system if the Container has access to it (usually via `"HostConfig": "Binds"` in the [metadata](../extensions#metadata-dockerfile) `permissions`, or [volumes](https://docs.docker.com/storage/volumes/)).

### Bootstrap

[AirOS-bootstrap](../bootstrap) is responsible for making sure [AirOS-core](#core) is running as expected, as well as gracefully restarting core during AirOS updates and/or if it is detected to have unexpectedly stopped/crashed.

### Core

[AirOS-core](../core) is the body of AirOS, and runs all of the built in [services](../../usage/advanced/#available-services), along with the main web interface.

### Extensions

AirOS has support for [Extensions](../extensions), which are handled by the [Extensions Manager](../../usage/advanced/#extensions-manager) service in core.

Extensions are individual Docker images that run independently of AirOS, but can hook into the core systems and host system, providing access to additional devices and data streams, as well as modifying / adding to the web interface.

### Cloud Infrastructure

While AirOS itself runs locally on the vehicle, there are various cloud-based services involved that allow downloading new AirOS releases, installing new autopilot firmware, finding and installing available extensions, finding information about AirOS, and getting support.

Of note are
1. The [AirOS-docker repository](https://github.com/airdroperua/AirOS-docker), where AirOS-core and AirOS-bootstrap are developed
1. The [Airdroper DockerHub](https://hub.docker.com/u/airdroperua/), where the AirOS-core and AirOS-bootstrap Docker images are deployed/hosted
1. The [AirOS-Extensions-Repository](https://github.com/airdroperua/AirOS-Extensions-Repository), where AirOS extensions are registered and findable from
1. The [ArduPilot Firmware Server](https://firmware.ardupilot.org), where autopilot firmwares are fetched from
1. The [AirOS documentation](https://airos.cloud/docs) (this site)
1. The [Airdroper Forum](https://discuss.airdroper.org/c/airdroperua-software/blue-os/85), where ideas and questions can be discussed

## Developer Presentations

The AirOS development team have done some presentations about what AirOS is for, and how it can be used / integrated with. Bear in mind that presentation recordings are snapshots of history, and the information in them gets outdated over time as development progress is made.

### AirOS Development Features
`ArduPilot Developers Unconference (March 2023)`
<iframe width="560" height="315" src="https://www.youtube.com/embed/61pHgPzhHv8" title="YouTube video player" frameborder="0" allow="encrypted-media;" allowfullscreen></iframe>

### Introduction to AirOS
`ArduPilot Developers Unconference (April 2022)`
<iframe width="560" height="315" src="https://www.youtube.com/embed/eV6oDm6He2U" title="YouTube video player" frameborder="0" allow="encrypted-media;" allowfullscreen></iframe>
