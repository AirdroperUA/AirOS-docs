+++
title = "AirOS-bootstrap"
description = "AirOS-bootstrap development documentation."
date = 2023-12-04T19:30:00+11:00
template = "docs/page.html"
sort_by = "weight"
weight = 20
draft = false

[extra]
lead = ''
toc = true
top = false
+++

## Function

AirOS-bootstrap is responsible for making sure [AirOS-core](../core) is running as expected, as well as gracefully restarting core during AirOS updates and if it is detected to have unexpectedly stopped/crashed. 
For an update the current core image gets shut down and the newly installed image gets started in its place, whereas in the case of a crash bootstrap reverts to running a known working core image, which is currently the one tagged as `factory` (which is whatever it was first flashed with), so that it's at least possible to access the interface.

## Codebase

[AirOS-bootstrap](https://github.com/airdroperua/AirOS/tree/master/bootstrap) is open source, and lives within the broader [AirOS](https://github.com/airdroperua/AirOS) GitHub repository. [Issues](https://github.com/airdroperua/AirOS/issues) can be used to report bugs or suggest features, and [Pull Requests](https://github.com/airdroperua/AirOS/pulls) fixing bugs or adding new features are welcomed.

AirOS is set up with a [GitHub Action](https://docs.github.com/en/actions) that [automatically builds and deploys](https://github.com/airdroperua/AirOS/blob/master/.github/workflows/test-and-deploy.yml#L90) a AirOS-bootstrap image when changes are pushed to the GitHub repository.
If you want to make use of that functionality you'll need a [DockerHub](https://hub.docker.com) account, and will need to specify your DockerHub username (`DOCKER_USERNAME`) and password (`DOCKER_PASSWORD`) in your fork's [GitHub secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets).

## Updating

{% warning() %}
AirOS-bootstrap is a critical component of running AirOS, and can significantly affect the system stability. It should only be updated when necessary, and preferably at times where the onboard computer hardware is accessible in case something goes wrong. For normal users it is strongly recommended to only update AirOS-bootstrap to match stable releases of AirOS, and even then only if there is a known issue an update is expected to fix or improve.
{% end %}

AirOS-bootstrap versions are built at the same time as AirOS-core versions, and they get bundled together in the Raspberry Pi images that can be flashed onto an SD card to install AirOS onto it. For official AirOS releases it is possible to update the AirOS-bootstrap image to match the AirOS release through the [AirOS Version](../../usage/advanced/#airos-version) chooser, and is the recommended process.

Manually updating to a non-matched and/or custom bootstrap image requires using the [Terminal](../../usage/advanced/#terminal):

```sh
# drop down from airos-core into the underlying operating system:
red-pill
# get the running bootstrap container id
CURRENT_BOOTSTRAP_CONTAINER=$(docker ps -aq --filter name=airos-bootstrap)
# stop the bootstrap container (takes 10 seconds)
docker stop $CURRENT_BOOTSTRAP_CONTAINER
# remove the container from local memory
# (the underlying image remains on the system, unused)
docker container rm $CURRENT_BOOTSTRAP_CONTAINER
# specify the Docker image source (use your account if testing a change)
BOOTSTRAP_REPO=airdroperua
BOOTSTRAP_IMAGE=airos-bootstrap
# specify the new version to use (e.g. 1.1.0-beta.27, or master)
NEW_BOOTSTRAP_VERSION=1.1.0-beta.27
# start running the new version
# (will automatically download if it's not already available locally)
docker run \
    -d -t \
    --restart unless-stopped \
    --name airos-bootstrap \
    --net=host \
    -v /root/.config/airos/bootstrap:/root/.config/bootstrap \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -e AIROS_CONFIG_PATH=/root/.config/airos \
    $BOOTSTRAP_REPO/$BOOTSTRAP_IMAGE:$NEW_BOOTSTRAP_VERSION
# view the logs, to check for any error or progress messages
docker logs -f $BOOTSTRAP_IMAGE
# press ctrl+c to return to the terminal, and you're done
# if you want, type 'exit' or 'logout' to return to the AirOS-core container
```
