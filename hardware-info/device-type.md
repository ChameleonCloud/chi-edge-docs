# Device Type

## CHI@Edge Supported Device Types

This page details the Single Board Computers (SBCs) available for edge computing experiments. These devices serve as the primary compute nodes and hosts for peripherals.

| machine\_name (for reservation) | Description                                 |
| ------------------------------- | ------------------------------------------- |
| raspberrypi5                    | Raspberry Pi 5, all variants                |
| raspberrypi4-64                 | Raspberry Pi 4, all variants, including CM4 |
| jetson-nano                     | Nvidia Jetson Nano                          |
| jetson-xavier-nx-devkit-emmc    | Nvidia Jetson Xavier NX                     |

## Hardware Details

### Raspberry Pi 5

<div align="left"><figure><img src="../.gitbook/assets/image (2).png" alt="" width="375"><figcaption></figcaption></figure></div>

* 8gb and 16gb variants
* [https://www.raspberrypi.com/products/raspberry-pi-5/](https://www.raspberrypi.com/products/raspberry-pi-5/)



#### Compute Module 5 (coming soon)

* [https://www.raspberrypi.com/products/compute-module-5/?variant=cm5-104032](https://www.raspberrypi.com/products/compute-module-5/?variant=cm5-104032)
* Lower power draw, wide temperature range support, suitable for embedding into outdoor or rugged applications.

### Raspberry Pi 4

#### Model B, 8gb

* [https://www.raspberrypi.com/products/raspberry-pi-4-model-b/specifications/](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/specifications/)

#### compute module 4, 8gb

<div align="left"><figure><img src="../.gitbook/assets/image (3).png" alt="" width="300"><figcaption></figcaption></figure></div>

* [https://www.raspberrypi.com/products/compute-module-4/?variant=raspberry-pi-cm4001000](https://www.raspberrypi.com/products/compute-module-4/?variant=raspberry-pi-cm4001000)
* Lower power draw, wide temperature range support, suitable for embedding into outdoor or rugged applications.

### Nvidia Jetson Nano

<div align="left"><figure><img src="../.gitbook/assets/image (4).png" alt="" width="375"><figcaption></figcaption></figure></div>

* [https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-nano/product-development/](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-nano/product-development/)

Please note! The latest Nvidia software supported on the Jetson Nano is:

* [JetPack 4.6.6](https://developer.nvidia.com/jetpack-sdk-466)
* [L4T 32.7.6](https://developer.nvidia.com/embedded/linux-tegra-r3276)

This is based on Ubuntu 18.04, Linux Kernel 4.9, and [**CUDA 10.2**](https://docs.nvidia.com/cuda/archive/10.2/cuda-toolkit-release-notes/index.html#title-new-features)

**You must use a container image targeting CUDA10.2 to work on these devices.**

The NVIDIA SDK for these devices is unfortunately end-of-life, and we are rolling out support for the replacement "Orin Nano" devices, described below.

### Nvidia Jetson Xavier NX

<div align="left"><figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure></div>

* [https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-xavier-series/](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-xavier-series/)

Please note! The latest Nvidia software supported on the Xavier NX is:

* [JetPack 5.1.6](https://developer.nvidia.com/embedded/jetpack-sdk-516)
* [L4T 35.6.4](https://developer.nvidia.com/embedded/jetson-linux-r3564)

This is based on Ubuntu 20.04, and Linux Kernel 5.10.

They currently package **CUDA 11.8**. Please use containers targeting this release for now.

We are investigating whether they can be upgraded to CUDA12.2, but this is not yet supported on CHI@Edge.

Support for these is limited, as NVIDIA considers them EoL.

### Jetson AGX Orin (coming soon)

<div align="left"><figure><img src="../.gitbook/assets/image (6).png" alt="" width="375"><figcaption></figcaption></figure></div>

* [https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-xavier-series/](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-xavier-series/)

The AGX Orin is currently suported by NVIDIA, we are working to roll out access to a 64GB dev kit.

### Jetson Orin Nano (coming soon)

<div align="left"><figure><img src="../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure></div>

* [https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/)
* [https://nvdam.widen.net/s/zkfqjmtds2/jetson-orin-datasheet-nano-developer-kit-3575392-r2](https://nvdam.widen.net/s/zkfqjmtds2/jetson-orin-datasheet-nano-developer-kit-3575392-r2)

The Orin Nano is the current offering from Nvidia, replacing the Jetson Nano.

We have two of these under test, and will update once they're generally available.

We are working to get other Orin variants in the pipeline, but don't have an ETA at this time.

## Hardware Summary Table

<table data-full-width="true"><thead><tr><th>Device Name</th><th width="117.265625">CPU</th><th>Memory</th><th>GPU Arch</th><th>AI Perf [1]</th><th>Power (TDP)</th></tr></thead><tbody><tr><td><strong>Raspberry Pi 4B</strong></td><td>4c A72 <br>@ 1.8GHz</td><td>8GB LPDDR4<br>13.3 GB/s</td><td>VideoCore VI</td><td></td><td>3W - 7W</td></tr><tr><td><strong>Raspberry Pi 5</strong></td><td>4c A76<br>@ 2.4GHz</td><td>8GB LPDDR4x<br>27.3 GB/s</td><td>VideoCore VII</td><td></td><td>5W - 12W</td></tr><tr><td><strong>Jetson Nano</strong></td><td>4c A57<br>@ 1.43GHz</td><td>4GB LPDDR4<br>25.6 GB/s</td><td>Maxwell (128c)</td><td>472 GFLOPS</td><td>5W - 10W</td></tr><tr><td><strong>Jetson Xavier NX</strong></td><td>6c Carmel v8.2</td><td>8GB LPDDR4x<br>59.7 GB/s</td><td>Volta (384c+48T)</td><td>21 TOPS (Dense, Int8)</td><td>10W - 20W</td></tr></tbody></table>

