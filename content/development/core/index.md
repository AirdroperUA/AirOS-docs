+++
title = "AirOS-core"
description = "AirOS-core development documentation."
date = 2024-04-10T01:30:00+10:00
template = "docs/page.html"
sort_by = "weight"
weight = 30
draft = false

[extra]
lead = ''
toc = true
top = false
+++

## Function

AirOS-core is the body of AirOS, and runs all of the built in [services](../../usage/advanced/#available-services), along with the main web interface. It contains the key components for running, configuring, and operating a robotic vehicle, as well as a variety of convenience features to aid with system introspection and development.

## Codebase

[AirOS-core](https://github.com/airdroperua/AirOS/tree/master/core) is open source, and lives within the broader [AirOS](https://github.com/airdroperua/AirOS) GitHub repository. [Issues](https://github.com/airdroperua/AirOS/issues) can be used to report bugs or suggest features, and [Pull Requests](https://github.com/airdroperua/AirOS/pulls) fixing bugs or adding new features are welcomed.

AirOS is set up with a [GitHub Action](https://docs.github.com/en/actions) that [automatically builds and deploys](https://github.com/airdroperua/AirOS/blob/master/.github/workflows/test-and-deploy.yml#L90) a AirOS-core image when changes are pushed to the GitHub repository.
If you want to make use of that functionality you'll need a [DockerHub](https://hub.docker.com) account, and will need to specify your DockerHub username (`DOCKER_USERNAME`) and password (`DOCKER_PASSWORD`) in your fork's [GitHub secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets).

The [AirOS Version](../../usage/advanced/#airos-version) chooser can be used to install a custom image by either
1. Changing the "Remote" to your DockerHub repository, then installing like you would normally
2. Doing a "Manual Upload" of a `.tar` compressed `AirOS-core` Docker image

### Structure

1. [Dockerfile](https://github.com/airdroperua/AirOS/tree/master/core/Dockerfile) (and corresponding `.dockerignore` file) for building the AirOS-core Docker image
1. `**/install-*.sh` scripts that the Dockerfile uses to install the tools, libraries, and services
1. [tools](https://github.com/airdroperua/AirOS/tree/master/core/tools) scripts used to install the underlying programs used by the service backends
1. [configuration](https://github.com/airdroperua/AirOS/tree/master/core/configuration) files that the Dockerfile moves to appropriate locations for the programs they apply to
1. [start-airos-core](https://github.com/airdroperua/AirOS/tree/master/core/start-airos-core) script that runs when the AirOS-core container gets started
    - Responsible for configuring and starting the services
    - Supports disabling a comma-separated list of core services via the `AIROS_DISABLE_SERVICES` environment variable `(New in 1.2)`
1. [libs](https://github.com/airdroperua/AirOS/tree/master/core/libs) code libraries of shared functionality available to the service backends
1. [services code](https://github.com/airdroperua/AirOS/tree/master/core/services) for running [the services](#services)
    - Mostly Python backend code, often wrapped around / making use of a program installed by `tools`
    - Some frontend code, for services that have a frontend that opens in its own window
1. [frontend](https://github.com/airdroperua/AirOS/tree/master/core/frontend) web code for displaying the AirOS web interface, including the main interface elements for the services
    - `/public`: browser tab icons and the like
    - `/src`:
        - Mostly [Vuetify](https://vuetifyjs.com/en/) visual components and [Typescript](https://www.typescriptlang.org) interface code
        - `/assets`: styling, images, and models used throughout the web interface

### Services

AirOS provides automatic detection of [Available Services](../../usage/advanced/#available-services),
whereby any HTTP server with a [title tag](https://www.w3schools.com/tags/tag_title.asp) is found and listed.
This is particularly helpful for testing out API calls, both for development and debugging purposes.

Documentation can also be parsed if

- it follows the Swagger / OpenAPI specification, and
- is available at `/docs` or `/v1.0/ui`

The services built into AirOS are as follows:

| Service Name | Purpose | Webpage(s)? | Tool(s)? | Frontend? |
| --- | --- | --- | --- | --- |
| [ArduPilot Manager](https://github.com/airdroperua/AirOS/tree/master/core/services/ardupilot_manager) | Handles ArduPilot firmware connection, flashing, and MAVLink routing. | - [Autopilot Firmware](../../usage/advanced/#autopilot-firmware)<br> - [MAVLink Endpoints](../../usage/advanced/#mavlink-endpoints) | - [tools/ardupilot_tools](https://github.com/airdroperua/AirOS/tree/master/core/tools/ardupilot_tools) | - [views/Autopilot.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/Autopilot.vue)<br> - [views/EndpointView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/EndpointView.vue)<br> - [components/autopilot](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/autopilot)<br> - [store/autopilot.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/autopilot.ts)<br> - [store/autopilot_manager.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/autopilot_manager.ts)<br> - [types/autopilot.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/autopilot.ts)<br> - [types/autopilot/parameter*.ts](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/types/autopilot) |
| Available Services | Finds and lists available HTTP servers and their API documentation. | - [Available Services](../../usage/advanced/#available-services) | -- | - [views/AvailableServicesView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/AvailableServicesView.vue)<br> - [components/scanner](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/scanner)<br> - [store/servicesScanner.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/servicesScanner.ts) |
| [Bag of Holding](https://github.com/airdroperua/AirOS/tree/master/core/services/bag_of_holding) | A simple key-value storage API, used for storing and retrieving data as JSON objects through HTTP requests. | - [Bag Editor](../../usage/advanced/#bag-editor) | -- | - [views/BagEditorView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/BagEditorView.vue)<br> - [store/bag.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/bag.ts) |
| [Beacon Service](https://github.com/airdroperua/AirOS/tree/master/core/services/beacon) | Handles mDNS domain advertisement and local network vehicle identification | -- | - [tools/dnsmasq](https://github.com/airdroperua/AirOS/tree/master/core/tools/dnsmasq) | - [components/beacon](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/beacon)<br> - [store/beacon.ts](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/store/beacon.ts)<br> - [types/beacon.ts](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/types/beacon.ts) |
| AirOS | The main AirOS interface | - [Dashboard](../../usage/advanced/#dashboard) | - [tools/nginx](https://github.com/airdroperua/AirOS/tree/master/core/tools/nginx) | - [views/MainView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/MainView.vue)<br> - [components/app](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/app)<br> - [components/notifications](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/notifications)<br> - [store/frontend.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/frontend.ts)<br> - [store/settings.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/settings.ts)<br> - [types/notifications.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/notifications.ts) |
| [Bridget](https://github.com/airdroperua/AirOS/tree/master/core/services/bridget) | Manages serial bridges. | - [Serial Bridges](../../usage/advanced/#serial-bridges) | - [tools/bridges](https://github.com/airdroperua/AirOS/tree/master/core/tools/bridges) | - [views/BridgesView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/BridgesView.vue)<br> - [components/bridges](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/bridges)<br> - [store/bridget.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/bridget.ts)<br> - [types/bridges.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/bridges.ts) |
| [Cable-guy](https://github.com/airdroperua/AirOS/tree/master/core/services/cable_guy) | Manages ethernet IP(s) and DHCP server configuration | -- | -- | - [components/ethernet](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/ethernet)<br> - [store/ethernet.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/ethernet.ts)<br> - [types/ethernet.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/ethernet.ts) |
| [Commander](https://github.com/airdroperua/AirOS/tree/master/core/services/commander) | Provides access to the operating system, and allows running arbitrary bash commands in the core docker container. | -- | -- | -- |
| File Browser | Provides a graphical interface to the file system. | - [File Browser](../../usage/advanced/#file-browser) | - [tools/filebrowser](https://github.com/airdroperua/AirOS/tree/master/core/tools/filebrowser) | - [views/FileBrowserView](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/FileBrowserView.vue)<br> - [types/filebrowser.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/filebrowser.ts) |
| [helper](https://github.com/airdroperua/AirOS/tree/master/core/services/helper) | Lists available webpages | - Sidebar | -- | - [types/helper.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/helper.ts) |
| [Kraken](https://github.com/airdroperua/AirOS/tree/master/core/services/kraken) | Manages extensions and the extension store. | - [Extensions Manager](../../usage/advanced/#extensions-manager) | -- | - [views/ExtensionView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/ExtensionView.vue)<br>- [views/ExtensionManagerView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/ExtensionManagerView.vue)<br> - [components/kraken](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/kraken)<br> - [types/kraken.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/kraken.ts) |
| [log_zipper](https://github.com/airdroperua/AirOS/tree/master/core/services/log_zipper) | GZips old log files to reduce space usage, and deletes them if space runs out. | -- | -- | -- |
| Log Browser | Allows browsing, downloading, and viewing autopilot log files. | - [Log Browser](../../usage/advanced/#log-browser) | - [tools/logviewer](https://github.com/airdroperua/AirOS/tree/master/core/tools/logviewer) | - [views/LogView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/LogView.vue)<br> - [components/logs](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/logs) |
| MAVLink Camera Manager | Manages camera and video stream pipelines, and presents them over MAVLink.  | - [Video Streams](../../usage/advanced/#video-streams) | - [tools/ mavlink_camera_manager](https://github.com/airdroperua/AirOS/tree/master/core/tools/mavlink_camera_manager) | - [views/VideoManagerView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/VideoManagerView.vue)<br> - [components/video-manager](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/video-manager)<br> - [store/video.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/video.ts)<br> - [types/video.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/video.ts) |
| MAVLink2Rest | A REST-based interface to the MAVLink network | - [MAVLink Inspector](../../usage/advanced/#mavlink-inspector)<br> - [Autopilot Parameters](../../usage/advanced/#autopilot-parameters)<br> - [Vehicle Setup](../../usage/advanced/#vehicle-setup) | - [tools/mavlink2rest](https://github.com/airdroperua/AirOS/tree/master/core/tools/mavlink2rest) | - [views/MavlinkInspectorView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/MavlinkInspectorView.vue)<br> - [components/mavlink](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/components/mavlink/)<br> - [components/mavlink-inspector](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/components/mavlink-inspector)<br> - [store/mavlink.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/mavlink.ts)<br> - [types/mavlink.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/mavlink.ts)<br> - [views/ParameterEditorView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/ParameterEditorView.vue)<br> - [components/parameter-editor](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/parameter-editor)<br> - [types/parameter_repository.d.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/parameter_repository.d.ts)<br> - [views/VehicleSetupView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/VehicleSetupView.vue)<br> - [components/vehiclesetup](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/vehiclesetup) |
| [NMEA Injector](https://github.com/airdroperua/AirOS/tree/master/core/services/nmea_injector) | Injects NMEA GPS position messages into the MAVLink stream. | - [NMEA Injector](../../usage/advanced/#nmea-injector) | -- | - [views/NMEAInjectorView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/NMEAInjectorView.vue)<br> - [components/nmea-injector](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/nmea-injector)<br> - [store/nmea-injector.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/nmea-injector.ts)<br> - [types/nmea-injector.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/nmea-injector.ts) |
| [Pardal](https://github.com/airdroperua/AirOS/tree/master/core/services/pardal) | Helps to perform network speed and latency tests. | - [Network Test](../../usage/advanced/#network-test) | -- | - [views/NetworkTestView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/NetworkTestView.vue)<br> - [components/speedtest](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/speedtest) |
| [Ping Service](https://github.com/airdroperua/AirOS/tree/master/core/services/ping) | Detects and manages Ping family sonar devices. | - [Ping Sonar Devices](../../usage/advanced/#ping-sonar-devices) | - [tools/bridges](https://github.com/airdroperua/AirOS/tree/master/core/tools/bridges) | - [views/Pings.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/Pings.vue)<br> - [components/ping](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/ping)<br> - [store/ping.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/ping.ts)<br> - [types/ping.ts](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/types/ping.ts) |
| System Information | Provides access to and information about the system status and operating system. | - [System Information](../../usage/advanced/#system-information) | - [tools/linux2rest](https://github.com/airdroperua/AirOS/tree/master/core/tools/linux2rest) | - [views/SystemInformationView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/SystemInformationView.vue)<br> - [components/system-information](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/system-information)<br> - [store/system-information.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/system-information.ts)<br> - [types/system-information](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/system-information)<br> - [widgets](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/widgets) |
| ttyd - Terminal | Provides a web-based terminal. | - [Terminal](../../usage/advanced/#terminal) | - [tools/ttyd](https://github.com/airdroperua/AirOS/tree/master/core/tools/ttyd)<br> - [tools/scripts](https://github.com/airdroperua/AirOS/tree/master/core/tools/scripts) | - [views/TerminalView.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/TerminalView.vue) |
| [Version Chooser](https://github.com/airdroperua/AirOS/tree/master/core/services/versionchooser) | Manages AirOS version selection and updates. | - [AirOS Version](../../usage/advanced/#airos-version) | -- | - [views/VersionChooser.vue](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/views/VersionChooser.vue)<br> - [components/version-chooser](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/version-chooser)<br> - [types/version-chooser.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/version-chooser.ts) |
| [Wifi Manager](https://github.com/airdroperua/AirOS/tree/master/core/services/wifi) | Manages wifi detection and hotspot configuration. | -- | - [tools/hotspot](https://github.com/airdroperua/AirOS/tree/master/core/tools/hotspot) | - [components/wifi](https://github.com/airdroperua/AirOS/tree/master/core/frontend/src/components/wifi)<br> - [store/wifi.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/store/wifi.ts)<br> - [types/wifi.ts](https://github.com/airdroperua/AirOS/blob/master/core/frontend/src/types/wifi.ts) |

## Contributions

### Adding a Flight Controller

Adding USB detection support for a new type of [flight controller board](@/integrations/hardware/required/flight-controller/index.md) is reasonably straightforward, but does require a few different steps:

#### Find the USB device information

1. Connect your flight controller board via USB[^2] to a computer running AirOS
1. Go to the AirOS [Terminal](../../usage/advanced/#terminal) page
1. Open a Python console (run the `python3` command)
1. Run `from serial.tools.list_ports import comports`
1. Run `from pprint import pprint`
1. Run `[pprint(port.__dict__) for port in comports()]`
1. Get the `"product"` and `"manufacturer"` values for your flight controller board

[^2]:Non-USB serial connections are [not yet supported](https://github.com/airdroperua/AirOS/issues/1615).

#### Find the flight controller hardware information

1. Go to [the ArduPilot hardware definition files](https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_HAL_ChibiOS/hwdef)
1. Find the folder corresponding to your flight controller board
1. Find the `APJ_BOARD_ID` variable in the `hwdef.dat` or `hwdef.inc` file

#### Add the board, and confirm it works

1. Fork the [AirOS](https://github.com/airdroperua/AirOS) repository
    - You'll need a GitHub account to do this
1. Click where it says `master`, and create a new branch (e.g. `add-pixhawk-6C`)
1. Navigate to `core/services/ardupilot_manager/typedefs.py` and add your board's name and type to the `Platform` class
    - Use `PlatformType.Serial` for USB/serial connections - `Linux` is reserved for sensor/peripheral expansion boards that run the autopilot firmware directly on the [Onboard Computer](@/integrations/hardware/required/onboard-computer/index.md)
1. Navigate to `core/services/ardupilot_manager/flight_controller_detector/board_identification.py` and add appropriate `SerialBoardIdentifier` instances to the `identifiers` list (using the "product" and "manufacturer" values from earlier)
    - The "product" is more important/useful, because one "manufacturer" can make multiple different board types
1. Navigate to `core/services/ardupilot_manager/firmware/FirmwareInstall.py` and add your board to the `get_board_id` function (using the `APJ_BOARD_ID` value from earlier)
1. If you did your code modifications in GitHub skip to the next step - if you're editing the files in a local clone make sure to `git commit` and `git push` back up to your GitHub fork of the repository
1. GitHub should prompt you that there are recent changes in your new branch - click the prompt to submit a Pull Request to the upstream Airdroper repository
    - The "files" tab of your pull request should look similar to [this example](https://github.com/airdroperua/AirOS/pull/1067/files)
1. Wait for the GitHub actions to complete (at the bottom of your pull request), then find [the build action](https://github.com/airdroperua/AirOS/actions/workflows/test-and-deploy.yml) corresponding to your branch, and download the `AirOS-core-docker-arm-v7` artifact from it
1. Go to the AirOS [Version Chooser](../../usage/advanced/#airos-version), scroll down to the bottom, and "manual upload" the artifact you downloaded
1. Once AirOS has restarted, see whether the flight controller board is being detected as an autopilot in the [Autopilot Firmware](../../usage/advanced/#autopilot-firmware) page
