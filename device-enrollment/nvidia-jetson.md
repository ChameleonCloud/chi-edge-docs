---
description: >-
  Details about enrolling NVIDIA Jetson devices, notably Jetson Nano and Jetson
  Xavier boards.
---

# NVIDIA Jetson

Flashing board memory

Jetson devices have onboard memory that must be flashed separately from, e.g., memory available on a removable SD card. The following utility can be used to flash devices to the supported SDK version:

{% embed url="https://github.com/balena-os/jetson-flash" %}

{% hint style="info" %}
For best results, you should install and use this tool on an x86 Linux machine running the latest Ubuntu stable release, which has physical connectivity to your target Jetson device over USB. While it is theoretically possible to do some of this with virtualization and USB passthrough, it's not recommended. _Here be dragons_ :dragon::dragon:
{% endhint %}



To run utilize the GPU in your containers and load all the necessary CUDA modules, please see our [FAQ](../faq.md#how-do-i-run-a-gpu-workload-on-the-jetsons-xaviers)

Other resources

{% embed url="https://wiki.seeedstudio.com/Jetson-Mate#getting-started" %}

{% embed url="https://developer.nvidia.com/nvidia-sdk-manager" %}



