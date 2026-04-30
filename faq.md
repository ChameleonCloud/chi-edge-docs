---
description: Frequently-Asked Questions
---

# FAQ

#### How do I access my container?

See [Access to your container](getting-started/#access-to-your-container).

You may wish to [install an ssh server](https://stackoverflow.com/questions/18136389/using-ssh-keys-inside-docker-container/43318322#43318322) on your container. If you do so, please ensure that password access is disabled in order to keep your container secure.

#### How do I upload files to my container?

Using the [Chameleon Python API](https://python-chi.readthedocs.io/en/latest/) (pre-installed in the [Chameleon Jupyter environment](https://jupyter.chameleoncloud.org)), you can do:

```python
from chi import container
container.upload(container_uuid, local_path, remote_path)
```

This method is limited to a small file size per each upload, and notably requires the `tar` command be available inside your container.

If your container runs an SSH server, you can copy files using tools like `scp`.

#### Can I use an image on a private Docker registry?

We do not yet support pulling from private Docker registries.

#### Can I run my container in privileged mode or access devices?

For security reasons, we don't support privileged mode. However, you can pass devices into your container to access things like the GPIO, CSI Camera, I2C, serial, or USB interfaces. This is equivalent to `docker run --device ...`

For more information, please see the section on [Device Profiles](device-enrollment/peripherals-and-device-profiles.md#device-profiles).

Support for adding specific capabilities, as in `CAP_ADD ...`, is in progress.

#### How do I run a GPU workload on the Jetsons/Xaviers ?&#x20;

Most GPU workloads on nvidia devices require or take advantage of several libraries in the CUDA ecosystem. For convenience and simplicity, we prepackaged the full Cuda, Tensort, Cudnn, and Visionworks libraries on our Nvidia hosts. To mount these libraries on your container, please include the `runtime="nvidia"`  keyword argument when starting your container with the `create_container()` call.

```
Example usage to run PyTorch:

my_container = container.create_container(
        "container_name", 
        image="nvcr.io/nvidia/l4t-pytorch:r32.7.1-pth1.9-py3",
        command=["/bin/bash", "-c", "--", "while true; do sleep 30; done;"],
        runtime="nvidia",
        reservation_id=lease.get_device_reservation("your-lease-id"),
)
```

Please make sure to specify a reservation\_id with Nvidia devices only as this will not affect containers started on non-Nvidia Edge devices. Lastly, please make sure to use images and software that is compatible with the current L4T (Linux for Tegra) version that we are using, namely L4T 32.7.3.

#### How do I check GPU memory usage on the Jetsons?

This can be done with `tegrastats`.&#x20;

Follow these steps to get the binary, which can be copied to your image.&#x20;

* Get the tegrastats binary from Nvidia which is in the [nvidia-l4t-tools package](https://repo.download.nvidia.com/jetson/t210/pool/main/n/nvidia-l4t-tools/nvidia-l4t-tools_32.7.3-20221122092935_arm64.deb) (version 32.7.3).
* Extract the folder using `dpkg-deb -x <filename>.deb <output_dir>"`, and then you can find the tegrastats binary in `./usr/bin`.

#### My container stops with the status `Exited(1)`

Check the “Logs” tab for more information on what went wrong.

If you see the error `exec user process caused: exec format error`, the issue most likely an architecture issue. Make sure your container is built for the proper CPU type, which is `linux/arm64` on most of our devices.
