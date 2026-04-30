---
description: >-
  If you want to launch a container on CHI@Edge using a private image, follow
  these instructions
---

# Using Private Registries

This guide will show you how to setup and use a private registry with CHI@Edge. This allows you to use a private image behind authentication.

In order to setup a private registry, you first must add the registry and credentials to via the CLI.&#x20;

Install the Openstack Zun client&#x20;

```
pip install python-zunclient
```

Next, you will need to download the OpenRC file from the CHI@Edge site. This process is the same as from the main Chameleon sites, and is documented [here](https://chameleoncloud.readthedocs.io/en/latest/technical/cli.html#the-openstack-rc-script). After sourcing this file, run the following command:

```
openstack appcontainer registry create \
    --username USERNAME \
    --password PASSWORD \
    --domain DOMAIN
```

The domain here should be the domain used when tagging your image. For example, Docker Hub would use the domain `registry.hub.docker.com`.

After the registry is setup, you can use CHI@Edge as normal. To use a private image, include the domain in the image name, for example `registry.hub.docker.com/mppowers/my_private_image`. This will then automatically load your stored credentials for the matching domain before the container is created, and use them to pull the image.
