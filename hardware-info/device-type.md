# Device Type

## CHI@Edge Supported Device Types

This page details the Single Board Computers (SBCs) available for edge computing experiments. These devices serve as the primary compute nodes and hosts for peripherals.

| machine\_name (for reservation) | Description                                  |
| ------------------------------- | -------------------------------------------- |
| raspberrypi5                    | Raspberry Pi 5, all variants                 |
| raspberrypi4-64                 | Raspberry Pi 4, all variants, including CM4  |
| jetson-nano                     | Nvidia Jetson Nano                           |
| jetson-xavier-nx-devkit-emmc    | Nvidia Jetson Xavier NX                      |
| jetson-orin-nano-devkit-nvme    | Nvidia Jetson Orin Nano Developer Kit (NVMe) |
| jetson-agx-orin-devkit-64gb     | Nvidia Jetson AGX Orin 64GB Developer Kit    |

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

The latest Nvidia software supported on the Jetson Nano is:

* [JetPack 4.6.6](https://developer.nvidia.com/jetpack-sdk-466)
* [L4T 32.7.6](https://developer.nvidia.com/embedded/linux-tegra-r3276)

This is based on Ubuntu 18.04, Linux Kernel 4.9, and [**CUDA 10.2**](https://docs.nvidia.com/cuda/archive/10.2/cuda-toolkit-release-notes/index.html#title-new-features)

**You must use a container image targeting CUDA10.2 to work on these devices.**

The NVIDIA SDK for these devices is unfortunately end-of-life, and we are rolling out support for the replacement "Orin Nano" devices, described below.

### Nvidia Jetson Xavier NX

<div align="left"><figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure></div>

* [https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-xavier-series/](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-xavier-series/)

The latest Nvidia software supported on the Xavier NX is:

* [JetPack 4.6.6](https://developer.nvidia.com/jetpack-sdk-466)
* [L4T 32.7.6](https://developer.nvidia.com/embedded/linux-tegra-r3276)

This is based on Ubuntu 18.04, Linux Kernel 4.9, and [**CUDA 10.2**](https://docs.nvidia.com/cuda/archive/10.2/cuda-toolkit-release-notes/index.html#title-new-features).

**You must use a container image targeting CUDA 10.2 to work on these devices.**

Support for these is limited, as NVIDIA considers them EoL. The replacement is the AGX Orin and Orin Nano, described below.

### Jetson Orin Nano Developer Kit

<div align="left"><figure><img src="../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure></div>

* [https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/)
* [https://nvdam.widen.net/s/zkfqjmtds2/jetson-orin-datasheet-nano-developer-kit-3575392-r2](https://nvdam.widen.net/s/zkfqjmtds2/jetson-orin-datasheet-nano-developer-kit-3575392-r2)

The Orin Nano is Nvidia's current offering, replacing the Jetson Nano. CHI@Edge images target the NVMe variant of the developer kit (`jetson-orin-nano-devkit-nvme`);

The Nvidia software supported on the Orin Nano is:

* JetPack 6.x
* L4T r36.4
* **CUDA 12.6**

This is based on Ubuntu 22.04. The Orin Nano is currently running in the original (non-Super) power profile.

Container images must target **CUDA 12.6** (or compatible) and `linux/arm64`.

### Jetson AGX Orin 64GB Developer Kit

<div align="left"><figure><img src="../.gitbook/assets/image (6).png" alt="" width="375"><figcaption></figcaption></figure></div>

* [https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/)
* [Jetson AGX Orin Developer Kit datasheet](https://developer.nvidia.com/embedded/downloads)

The AGX Orin is the high-end Orin-series board. CHI@Edge currently supports the 64GB developer kit (`jetson-agx-orin-devkit-64gb`).

The Nvidia software supported on the AGX Orin is:

* JetPack 6.x
* L4T r36.4
* **CUDA 12.6**

This is based on Ubuntu 22.04.

Container images must target **CUDA 12.6** (or compatible) and `linux/arm64`.

## Hardware Summary Table

<table data-full-width="true"><thead><tr><th>Device Name</th><th width="117.265625">CPU</th><th>Memory</th><th>GPU Arch</th><th>AI Perf [1]</th><th>Power (TDP)</th></tr></thead><tbody><tr><td><strong>Raspberry Pi 4B</strong></td><td>4c A72 <br>@ 1.8GHz</td><td>8GB LPDDR4<br>13.3 GB/s</td><td>VideoCore VI</td><td></td><td>3W - 7W</td></tr><tr><td><strong>Raspberry Pi 5</strong></td><td>4c A76<br>@ 2.4GHz</td><td>8GB LPDDR4x<br>27.3 GB/s</td><td>VideoCore VII</td><td></td><td>5W - 12W</td></tr><tr><td><strong>Jetson Nano</strong></td><td>4c A57<br>@ 1.43GHz</td><td>4GB LPDDR4<br>25.6 GB/s</td><td>Maxwell (128c)</td><td>472 GFLOPS</td><td>5W - 10W</td></tr><tr><td><strong>Jetson Xavier NX</strong></td><td>6c Carmel v8.2</td><td>8GB LPDDR4x<br>59.7 GB/s</td><td>Volta (384c+48T)</td><td>21 TOPS (Dense, Int8)</td><td>10W - 20W</td></tr><tr><td><strong>Jetson Orin Nano (Dev Kit)</strong></td><td>6c A78AE<br>@ 1.5GHz</td><td>8GB LPDDR5<br>68 GB/s</td><td>Ampere (1024c + 32T)</td><td>40 TOPS (Sparse, Int8)</td><td>7W - 15W</td></tr><tr><td><strong>Jetson AGX Orin 64GB</strong></td><td>12c A78AE<br>@ 2.2GHz</td><td>64GB LPDDR5<br>204.8 GB/s</td><td>Ampere (2048c + 64T)</td><td>275 TOPS (Sparse, Int8)</td><td>15W - 60W</td></tr></tbody></table>
