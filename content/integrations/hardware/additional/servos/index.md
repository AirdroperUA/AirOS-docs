+++
title = "Servos"
description = "Useful when a component needs to be actuated or rotated. They can be inside an enclosure to rotate cameras, or depth-rated and used outside of enclosures."
date = 2022-10-11T17:33:19+11:00
template = "docs/page.html"
sort_by = "weight"
weight = 20
draft = false
[extra]
lead = ""
toc = true
top = false
+++


Servos are a useful addition to any underwater vehicle build where a component needs to be actuated or rotated. Normal servos can be installed inside the watertight enclosures to rotate things such as cameras. Depth rated servos can be used outside of enclosures.

Up to three servos can be operated via joystick button functions when connected to the appropriate signal output.

Most autopilots cannot provide power to the servos so a 5V power supply will need to be connected to the output signal rail.

## Supported Servos

ArduPlane, ArduRover, ArduSub supports either analog or digital PWM controlled servos. The following have been tested:

* [Hitec HS-5055MG Servo](https://hitecrcd.com/products/servos/micro-and-mini-servos/digital-micro-and-mini-servos/hs-5055mg-economy-metal-gear-feather-servo/product) (used in the Airdroper [Camera Tilt System](https://airdroper.org/store/sensors-sonars-cameras/cameras/camera-tilt-mount/))
* [Blue Trail Engineering Waterproof Servo SER-110X](https://www.bluetrailengineering.com/product-page/100-m-underwater-servo-with-low-profile-bulkhead-connector)
