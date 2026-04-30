---
description: >-
  How to use the CHI@Edge Python SDK to enroll and manage devices on the
  testbed.
---

# Edge SDK

## Prerequisites

To run the SDK, you'll need a computer with python3 installed, a terminal, and a microsd card writer. The SDK has only been tested with Linux and MacOS, and the Bash and ZSH shells.

## Installing the SDK

We recommend installing the SDK in a python virtualenv. In your chosen installation directory, execute the following:

```
# create a virtualenv
python3 -m venv .venv

# activate it
source .venv/bin/activate

# install the SDK
pip install python-chi-edge
```

## Using the SDK

### Authenticating to CHI@Edge

To use the SDK, you'll need an application credential from CHI@Edge. This will be used both to authenticate the SDK, and to register each device.

Create one here, then download it as an openrc file.\
[https://chi.edge.chameleoncloud.org/identity/application\_credentials/](https://chi.edge.chameleoncloud.org/identity/application_credentials/).

Finally, source the openrc file by executing, e.g.\
`source app-cred-edge-openrc.sh`

### Registering your device

The first step to enrollment is device registration. You can do this even if you don't have the device at-hand; it simply creates a record of a "shadow" device on CHI@Edge, which will then be automatically associated with your target physical device on first boot.

You should have a few pieces of information handy:

{% hint style="info" %}
Device names must be unique across the testbed; this is another reason why prefixing the device name is handy.
{% endhint %}

* **Device name**: what will your device be called? This is designed to be machine-readable (i.e., short, descriptive, no spaces or surprises), and we recommend that you prefix your device with its location or your project or institution. Including an indication of the device type is also helpful for quick scanning. For example:
  * `uc-rpi4-03`: a Raspberry Pi4 (third in a set) physically located at UChicago
  * `pegasus-rpi4-01`: a Raspberry Pi4 (first in a set) managed by a project called "Pegasus."
  * `tacc-nano-10`: a Jetson Nano located at TACC.
* **Device Type**: currently CHI@Edge supports a few specific device types, so ensure yours is supported before continuing with the process. Currently, we support the following:
  * `raspberrypi3-64`
  * `raspberrypi4-64`
  * `raspberrypi5`
  * `jetson-nano`
  * `jetson-xavier-nx-emmc`&#x20;
  * `jetson-orin-nano`
  * `jetson-agx-orin-devkit-64gb`
* **Contact email**: this will be used if testbed operators would like to contact you about your device (e.g., to report power failure or disconnection of significant duration.)



Once you have the above information, registering each device is straightforward. Replace each `<placeholder>` below.

```shell
chi-edge device register \
  --contact-email <contact_email> \
  --machine-name <device_type> \
  <device_name>
```

### Viewing your registered devices

The SDK also supports listing and seeing details for any devices you have registered.

```shell
# List all devices
chi-edge device list
```

This will print a list of any devices registered under your Chameleon project. Notably, the "Health" column will indicate if the device has any issues w/ the testbed. There are multiple health checks that occur on the device and all of them should be up (i.e., it should read "3/3"). Health checks reset whenever you issue an update to the device, but should resolve shortly thereafter, once the changes have propagated through the testbed.

```markdown
              ╷                                      ╷                           ╷        ╷                            
  Name        │ UUID                                 │ Registered at             │ Health │ Last seen                  
╺━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━┿━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━╸
  iot-rpi3-03 │ 9b417c80-81ff-48b7-abdf-39dce659fbc7 │ 2022-02-08T13:29:59-06:00 │ 3/3    │ 2022-02-28T18:06:10-06:00  
  iot-rpi4-01 │ b3437b33-048d-4809-ad7e-7b8d186195a4 │ 2022-02-28T18:34:16-06:00 │ 1/3    │ --                         
  iot-rpi4-02 │ 4ece5ce4-6166-4d19-8de0-73819144b78c │ 2022-02-28T18:36:14-06:00 │ 1/3    │ --                
```

You can view a single device's details, which will display more information regarding the device's configuration and health:

```shell
# View details for a single device
chi-edge device show <uuid|name>
```

This will print a larger view with all user-customizable properties of the device, and the status of all health checks. When a check is in the "STEADY" state, everything is OK. "PENDING" indicates the check has not yet run, "IN\_PROGRESS" similarly indicates the check is running, and failed checks go to an "ERROR" state. There is additionally a "DEFER" state, indicating that the check could not complete due to some dependency not being realized. This should resolve itself once testbed state converges.

```
╭─ iot-rpi3-03 ── 9b417c80-81ff-48b7-abdf-39dce659fbc7 ──────────────────────────────────────────────────────────────────────────────────────╮
│                                 ╷                                                                                                          │
│   Property                      │ Value                                                                                                    │
│ ╺━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╸                                             │
│   application_credential_id     │ 20a809ecf24e4b48be51005f9ea678f2                                                                         │
│   application_credential_secret │ ************                                                                                             │
│   blazar_device_driver          │ k8s                                                                                                      │
│   channels                      │ user:                                                                                                    │
│                                 │   channel_type: wireguard                                                                                │
│                                 │   public_key: ZLflwhnDj/CS3c0j0Dn2smm2Ou/QZP1ZElhCTNOiRzU=                                               │
│   contact_email                 │ jasonanderson@uchicago.edu                                                                               │
│   machine_name                  │ raspberrypi3-64                                                                                          │
│                                 ╵                                                                                                          │
│ Health details                                                                                                                             │
│                 ╷                                       ╷                                        ╷                                         │
│                 │ balena                                │ blazar.device                          │ tunelo                                  │
│ ╺━━━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╸ │
│   state         │ STEADY                                │ STEADY                                 │ STEADY                                  │
│   state_details │ device_api_key:                       │ blazar_resource_id:                    │ channels:                               │
│                 │ ********************************      │ 833e10f0-1e21-48b3-94fe-aa7e9652c680   │   user:                                 │
│                 │ device_id: 5879362                    │ message: Can not make resource         │     endpoint: null                      │
│                 │ fleet_id: 1883023                     │ reservable, as the underlying entity   │     ip: 10.0.3.167                      │
│                 │ last_seen: '2022-03-01T00:06:10Z'     │ could not be found.                    │     peers:                              │
│                 │                                       │ resource_created_at: '2022-02-25       │     - endpoint: 129.114.34.129:51821    │
│                 │                                       │ 23:48:56'                              │       ip: 10.0.3.2                      │
│                 │                                       │                                        │       public_key:                       │
│                 │                                       │                                        │ zgg29Urn5Wwcp9ennCchdGoYnPdWCHUfxzMC…   │
│                 │                                       │                                        │     uuid:                               │
│                 │                                       │                                        │ fb92b276-22be-4fbb-b498-defcfefdb27b    │
│                 ╵                                       ╵                                        ╵                                         │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```

### Imaging your device

Once you have registered the device, you can proceed to downloading an OS image and configuring it for your target device (a process we refer to as "baking" the image.)

{% tabs %}
{% tab title="Raspberry Pi 3/4/5" %}
#### Download an OS image

Depending on what type of device you are enrolling, download one of the Balena OS images here:

* [Raspberry Pi 4: Image version 6.10.24+rev3](https://api.balena-cloud.com/download?deviceType=raspberrypi4-64\&version=6.10.24%2Brev3\&fileType=.zip)
* [Raspberry Pi 5: Version 6.10.24+rev3](https://api.balena-cloud.com/download?deviceType=raspberrypi5\&version=6.10.24%2Brev3\&fileType=.zip)

{% hint style="info" %}
64-bit is required; the service images we will run on the Pi are built for aarch64.
{% endhint %}

{% hint style="warning" %}
make sure you unzip the downloaded image before flashing
{% endhint %}

#### **Bake the image for your target device**

Use the Edge SDK to "bake" the image for your target device. This adjusts a configuration file on the device to inform it to associate itself with the device record you already registered on the testbed.

```shell
chi-edge device bake --image <image> <device_uuid>
```

#### Image the Pi

Use some imaging tool (we recommend [Balena Etcher](https://www.balena.io/etcher/)) to transfer the disk image to a microSD card and insert into the Pi.
{% endtab %}

{% tab title="Nvidia Jetson" %}
#### Download an OS image

Depending on what type of device you are enrolling, download one of the Balena OS images here. We currently only support the following Jetson Nano configurations:

* [Jetson Nano (w/ SD card): Image version 4.0.9+rev2](https://api.balena-cloud.com/download?deviceType=jetson-nano\&version=4.0.9+rev2\&fileType=.zip)
* [Nvidia Jetson Xavier NX Devkit eMMC](https://api.balena-cloud.com/download?deviceType=jetson-xavier-nx-devkit-emmc\&version=6.0.13\&fileType=.zip)
* [Nvidia Jetson Orin Nano](https://chi.hpc.ucar.edu:7480/balena/artifacts/jetson-orin-nano-devkit-nvme/2026-04-21/balena-image-flasher-jetson-orin-nano-devkit-nvme-20260421210704.balenaos-img.xz)
* [Nvidia Jetson AGX Orin Devkit 64gb](https://chi.hpc.ucar.edu:7480/balena/artifacts/jetson-agx-orin-devkit-64gb/2026-04-21/balena-image-flasher-jetson-agx-orin-devkit-64gb.balenaos-img.xz)

#### **Bake the image for your target device**

Use the Edge SDK to "bake" the image for your target device. This adjusts a configuration file on the device to inform it to associate itself with the device record you already registered on the testbed.

```shell
chi-edge device bake --image <image> <device_uuid>
```

#### Image the Jetson Nano (SD card method)

For the Jetson Nano, you can use the same procedure as for Rasbperry Pis, and use an imaging tool (we recommend [Balena Etcher](https://www.balena.io/etcher/)) to transfer the disk image to a microSD card and insert into the Pi.

#### Image the Jetson Xavier NX: eMMC and Jetson-Flash

The Xavier NX is less "user-friendly", we recommend using an external nvme drive with the Xavier NX, and flashing it directly with a m.2 to USB adapter. Advanced users can try [https://github.com/balena-os/jetson-flash](https://github.com/balena-os/jetson-flash)

#### Image the Orin Nano or AGX Orin

For these devices, the base image is a "flasher" type. Use an imaging tool to write the "baked" image to a usb drive. Insert the drive into the Orin device. The Orin should boot off of the USB, and write the image from the USB drive onto internal storage, either eMMC or NVMe.
{% endtab %}

{% tab title="Google Coral (alpha)" %}
#### (Currently not working)

#### Download an OS image

{% embed url="https://api.balena-cloud.com/download?deviceType=coral-dev&version=2.108.26&fileType=.zip" %}

#### **Bake the image for your target device**

Use the Edge SDK to "bake" the image for your target device. This adjusts a configuration file on the device to inform it to associate itself with the device record you already registered on the testbed.

```shell
chi-edge device bake --image <image> <device_uuid>
```

#### Flash the Coral Dev Board

This is more involved than other devices, as the coral dev board runs off an internal eMMC chip, but flashes that chip from the microsd card.

* Use some imaging tool (we recommend [Balena Etcher](https://www.balena.io/etcher/)) to transfer the disk image to a microSD card and insert into the Pi.
* Remove the SD card from the host machine.
* Insert the freshly flashed SD card into the Coral Dev Board.
* Set the BOOT\_SELECT switch to the SD-CARD position&#x20;
* Connect power to the Coral Dev Board
* Wait for the Coral Dev Board to finish flashing and shutdown. Please wait until all LEDs are off.
* Remove the SD card from the Coral Dev Board.
* Set the BOOT\_SELECT switch to the eMMC position,Remove and re-connect power to the Coral Dev Board to boot the device.
{% endtab %}
{% endtabs %}











Once your device boots up, it should download the latest version of the edge services, establish communication with the central testbed control plane, and the health checks should flip over to a healthy state. Use the Edge SDK to investigate reasons why the device may not be coming up, and please reach out to the [users group](https://groups.google.com/g/chameleon-edge-users) if you encounter difficulties.

### Manage which projects can use your devices

By default, newly registered devices can be reserved by any CHI@Edge user. This may not be appropriate, for example, for devices which require physical access to be useful.

You can restrict which projects can reserve a device by using the following options:

<pre><code><strong># --authorized-projects TEXT      A list of Chameleon projects (names or IDs)
</strong>#                                 that will be allowed to reserve and use the
#                                 device. Specify as a comma-separated list.
# --authorized-projects-reason    TEXT
#                                 An optional display reason to explain why
#                                 the device has restrictions.

chi-edge device set \
    --authorized-projects &#x3C;project_1>,&#x3C;project_2> \
    --authorized-projects-reason "special reason here" \
    &#x3C;device_name_or_uuid>
</code></pre>

### Prevent user containers from accessing your local network

We recommend that CHI@Edge devices be connected to a DMZ network, where nothing sensitive is at risk. While we do enforce a usage policy, CHI@Edge fundamentally allows users to execute arbitrary code on these devices.

If you need to prevent users from sending traffic to local IP adresses (say your devices are connected to the same network as classroom computers, smart TVs, printers, etc.), you can enforce this with the following command.

<pre><code># --local-egress [allow|deny]     Can the device contact IPs on its local
<strong>#                                 network
</strong>chi-edge device set \
    --local-egress deny \
    &#x3C;device_name_or_uuid>
</code></pre>

{% hint style="warning" %}
This will not prevent traffic between containers on two CHI@Edge devices, only traffic exiting the device onto your network.&#x20;
{% endhint %}

If set, traffic sent from the device to RFC1918 IP addresses will be blocked: 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16. Traffic can still be sent to public IP addresses via a gateway on your network, e.g reaching 8.8.8.8 via 192.168.0.1

<table><thead><tr><th width="400.3333333333333">Case</th><th align="right">allow</th><th align="right">deny</th></tr></thead><tbody><tr><td>container 1 -> container 2 on same device</td><td align="right">yes</td><td align="right">yes</td></tr><tr><td>container 1 -> container 2 on different devices</td><td align="right">yes</td><td align="right">yes</td></tr><tr><td>container 1 -> local IP (printer, TV, desktop)</td><td align="right">yes</td><td align="right"><strong>no!</strong></td></tr><tr><td>container 1 -> public IP (google, DNS, etc)</td><td align="right">yes</td><td align="right">yes</td></tr></tbody></table>
