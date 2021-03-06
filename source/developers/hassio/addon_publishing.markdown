---
layout: page
title: "Publishing your add-on"
description: "Steps on how-to create an add-on for Hass.io."
date: 2017-04-30 13:28
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /hassio/addon_publishing/
---

There are two different ways of publishing add-ons. One is to publish pre-build containers to Docker Hub and the other option is to have users build the containers locally on their Hass.io instance.

#### {% linkable_title Pre-build containers %}

With pre-build containers, the developer is responsible for building the images for each architecture on their machine and push the results out to Docker Hub. This has a lot of advantages for the user. As a user it will only have to download the final container and be up and running once the download finishes. This makes the installation process fast and almost no chance of failure. This is the preferred method.

We have automated the process of building and publishing add-ons. See below for the instructions.

#### {% linkable_title Locally build containers %}

Starting Hass.io 0.26, it is possible to distribute add-ons that will be built on the users machine. The advantage is that as a developer it is easy to test an idea and see if people are interested in your add-ons. This method includes installing and potentially compiling code. This means that installing such an add-on is slow and adds more wear and tear to users SD card/hard drive than the above mentioned pre-build solution. It also has a higher chance of failure if one of the dependencies of the container has changed or is no longer available.

Use this option when you are playing with add-ons and seeing if someone is interested in your work. Once you're an established repository, please migrate to pushing builds to Docker Hub as it greatly improves the user experience. In the future we will mark locally built add-ons in the add-on store to warn users.

## {% linkable_title Build scripts to publish add-ons to Docker Hub %}

All add-ons are simple docker containers. Inside your add-on `config.json` you specify the Docker image that will be installed for your add-on:

```json
{
  …

  "image": "myhub/image-{arch}-addon-name",

  …
}
```

You can use `{arch}` inside the image name to support multiple architectures with 1 configuration file. It will be replaced with the architecture of the user when we load the image.

Hass.io assumes that the `master` branch of your add-on repository matches the latest tag on Docker Hub. When you're building a new version, it's suggested that you use another branch, ie `build`. After you push the add-on to [Docker Hub](https://hub.docker.com/), you can merge this branch to master.

## {% linkable_title Custom Add-ons %}

You need a Docker Hub account to make your own add-ons. Download our [build script][builder] and run one of the following commands.

For a git repository:

```bash
# Test only:
./create_hassio_addon.sh -a amd64 -s addon-slug -r https://github.com/xy/addons -b branchname

# push to docker hub:
./create_hassio_addon.sh -a amd64 -s addon-slug -r https://github.com/xy/addons -b branchname -p

# create for all supported arch:
./create_addon_all.sh -s addon-slug -r https://github.com/xy/addons -b branchname -p
```

For a local repository:

```bash
# Test only:
./create_hassio_addon.sh -a amd64 -s addon-slug -l /home/xy/my_local_repo

# push to docker hub:
./create_hassio_addon.sh -a amd64 -s addon-slug -l /home/xy/my_local_repo -p
```

[builder]: https://github.com/home-assistant/hassio-build/tree/master/build-scripts/addons
