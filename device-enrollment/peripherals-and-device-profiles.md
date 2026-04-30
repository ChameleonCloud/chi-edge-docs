---
description: >-
  How to ensure physically-attached peripherals (via, e.g., USB or serial
  interface) can be made available to containers launched on the device.
---

# Peripherals and Device Profiles

Peripherals are an essential part of edge-computing. Facilitating the ability to add sensors and actuators that act on a device's environment was and remains, one of the core ideas behind the CHI@Edge research testbed. In contrast to our previous peripheral support approach where we maintained a limited subset of peripherals on a best-effort basis; this updated documentation aims to provide a hollistic approach for contributing peripheral support on CHI@Edge.&#x20;

To integrate peripheral devices into the CHI@Edge platform, follow these three essential steps:

* **Setting the Right Kernel Boot Options and Device Tree Overlays**
* **Exposing the Right Devices Under `/dev` Using Whitelisted Device Profiles**
* **Installing the Right Software in the User Container to drive the peripheral**

## **Setting the Right Kernel Boot Options and Device Tree Overlays**

Kernel boot options and device tree overlays are critical for enabling and configuring hardware peripherals at the device level. These settings ensure that the hardware is recognized and properly managed by the edge device OS.

### Example: Enabling a Pi Camera Module 3 on Raspberry Pi 4

The following are just some of the required boot options for enabling a Pi Camera Module 3 on a Raspberry Pi 4.&#x20;

```
start_x=1 # Directive to enable legacy camera stack
camera_auto_detect=1 # Directive to detect Pi camera modules on a raspberry pi
dtoverlay=imx708 # Sensor driver for the Pi Camera module 3
```

Unfortunately, due to security reasons, we can not expose our device kernel boot options to direct modification from our users (We aim to support a framework for manually adding boot options to self-hosted user devices in the future).  For now, to add boot options to our own hosted CHI@Edge devices, please open a ticket through our helpdesk with the following information:

* Targeted type of peripheral support
* The concerned boot option.
* Documentation to support the purpose of the boot option and its addition

## **Exposing the Right Devices Under `/dev` Using Whitelisted Device Profiles**&#x20;

By default, the testbed will auto-discover most devices available under `/dev`. Currently this works against a list of known/allowed device names, which differs by platform:

| Device platform | Linux device patterns                                                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Raspberry Pi    | <p><code>^snd$</code></p><p><code>^gpiomem$</code></p><p><code>^gpiochip[0-9]*$</code></p><p><code>^hci[0-9]*$</code></p><p><code>^i2c-[0-9]*$</code></p><p><code>^rtc0$</code></p><p><code>^video[0-9]*$</code></p><p><code>^vchiq$</code></p><p><code>^vcsm.*$</code></p><p><code>^ttyUSB[0-9]*$</code></p><p><code>^ttyACM[0-9]*$</code></p><p><code>^ttyTHS[0-9]*$</code></p><p><code>^ttyS[0-9]*$</code></p>                            |
| NVIDIA Jetson   | <p><code>^snd$</code></p><p><code>^gpiomem$</code></p><p><code>^gpiochip[0-9]*$</code></p><p><code>^hci[0-9]*$</code></p><p><code>^i2c-[0-9]*$</code></p><p><code>^rtc0$</code></p><p><code>^video[0-9]*$</code></p><p><code>^vchiq$</code></p><p><code>^vcsm.*$</code></p><p><code>^ttyUSB[0-9]*$</code></p><p><code>^ttyACM[0-9]*$</code></p><p><code>^ttyTHS[0-9]*$</code></p><p><code>^ttyS[0-9]*$</code><br><code>nvidia-gpu</code></p> |

When launching a container on the testbed, end-users can specify a "device profile", which maps to some set of detected and available devices. The containers will then have permission to access these devices.

For security reasons, we currently whitelist the following `device profiles`, which map a subset of the above devices to a key that can be specified during container launch.

### Currently defined device profiles

| Key                                    | Mapped Devices                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| jetson\_camera (currently unsupported) | <ul><li><code>/dev/video0</code></li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| pi\_camera                             | <p></p><ul><li><code>/dev/dma_heap</code>: required for contiguous memory allocation used for capture buffers</li><li><code>/dev/media0:4</code></li><li><code>/dev/v4l-subdev0</code> and <code>/dev/v4l-subdev1</code>, both of which are only available after specifying the <code>imx-708</code> Raspberry Pi device tree overlay in boot configs (<code>imx-708</code> refers to the driver specific to the pi camera module 3)</li><li><code>vchiq</code></li><li><code>vcsm-cma</code></li><li><code>video0, video1, video10:16, video18:23, and video31</code></li></ul> |
| pi\_gpio                               | <ul><li><code>/dev/gpiomem</code></li><li><code>/dev/i2c-1</code></li><li><code>/dev/gpiochip0</code></li><li><code>/dev/gpiochip1</code></li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                              |
| pi\_serial                             | <ul><li><code>/dev/ttyACM0</code></li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| pi\_meter                              | <ul><li><code>/dev/ttyUSB0</code></li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

### **Adding a device profile to CHI@Edge**

Once again, due to security reasons, we refrain from providing free access to mounting devices under /dev on user containers. To package a subset of /dev devices into a device profile, contact us through the helpdesk with the following required information to support your device profile addition request:

* Targeted type of peripheral support
* /dev devices required
* Documentation to support the various required /dev devices requested

## **Installing the Right Software in the User Container to drive the peripheral**

Often times, peripherals require additional software to function as intended, in this section, we showcase our recent development effort to enable Pi Camera Module 3 support on our raspberry pi 4 devices as an example of a `peripheral support container image`&#x20;

### Example: Dockerfile for the Pi Camera Module 3 peripheral support image

```
FROM debian:bullseye

# Install necessary dependencies and tools
RUN apt-get update && \
    apt-get install -y \
    dkms \
    build-essential \
    git \
    v4l-utils \
    python3-dev \
    python3-pip \
    python3-yaml \
    python3-ply \
    cmake \
    libboost-dev \
    libgnutls28-dev openssl libtiff5-dev libjpeg-dev libpng-dev pybind11-dev \
    qtbase5-dev libqt5core5a libqt5gui5 libqt5widgets5 \
    libboost-program-options-dev libdrm-dev libexif-dev \
    && apt-get clean

# Install the required Python modules using pip
RUN pip3 install meson ninja jinja2 
#ply pyyaml

# Clone and build libcamera
RUN git clone https://github.com/raspberrypi/libcamera.git /libcamera && \
    cd /libcamera && \
    meson setup build --buildtype=release -Dpipelines=rpi/vc4,rpi/pisp -Dipas=rpi/vc4,rpi/pisp -Dv4l2=true -Dgstreamer=disabled -Dtest=false -Dlc-compliance=disabled -Dcam=disabled -Dqcam=disabled -Ddocumentation=disabled -Dpycamera=enabled && \
    ninja -C build && \
    ninja -C build install

# Clone and build rpicam-apps
RUN git clone https://github.com/raspberrypi/rpicam-apps.git /rpicam-apps && \
    cd /rpicam-apps && \
    sed -i 's/platform = options_->GetPlatform();/platform = Platform::VC4;/' core/rpicam_app.cpp && \
    meson setup build -Denable_libav=disabled -Denable_drm=enabled -Denable_egl=disabled -Denable_qt=disabled -Denable_opencv=disabled -Denable_tflite=disabled && \
    meson compile -C build && \
    meson install -C build

RUN usermod -a -G video root

# Ensure the container has access to video devices
ENV UDEV=on
ENV LD_LIBRARY_PATH=/usr/local/lib/aarch64-linux-gnu:/libcamera/build/src/libcamera:${LD_LIBRARY_PATH}

CMD ["sleep", "infinity"]
```

The above Dockerfile specifies an image that packages 2 essential dependencies for the Pi Camera Module3: `libcamera` and the `rpicam-apps` . Both of which we compile manually.&#x20;

## Example: Launching a container on CHI@Edge with access to the Pi Camera Module 3

In summary, to launch a `peripheral support container image`  with fully featured support for the Pi Camera Module 3. Ensure the following pre-requisites are satisfied

1.  The device to which the camera is connected is supplied with the appropriate boot options. In this case:

    <pre><code>start_x=1 # Directive to enable legacy camera stack
    <strong>camera_auto_detect=1 # Directive to detect Pi camera modules on a raspberry pi
    </strong>dtoverlay=imx708 # Sensor driver for the Pi Camera module 3
    </code></pre>
2.  The `pi_camera` device profile is included in the `create_container()` call as follows &#x20;

    <pre><code>my_container = container.create_container(
        container_name,
        image="ghcr.io/chameleoncloud/edge-picamera-image:latest",
        workdir="/home",
        <a data-footnote-ref href="#user-content-fn-1">device_profiles=["pi_camera"],</a>
        reservation_id=lease.get_device_reservation(lease_id),
        platform_version=2,
    )  
    </code></pre>
3. The appropriate `peripheral support container image` is used when launching the container. In this case, we supply `ghcr.io/chameleoncloud/edge-picamera-image:latest` as shown in the above `create_container()` call code snippet.&#x20;
4.  The right devices appear under `/dev` once the container is running                                                                                                                                       &#x20;

    ```
    # ls /dev
    dma_heap  media3  random           tty          video0   video14  video21                                                                             
    fd        media4  shm              urandom      video1   video15  video22                                                                             
    full      mqueue  stderr           v4l-subdev0  video10  video16  video23                                                                             
    media0    null    stdin            v4l-subdev1  video11  video18  video31                                                                             
    media1    ptmx    stdout           vchiq        video12  video19  zero                                                                                
    media2    pts     termination-log  vcsm-cma     video13  video20
    ```
5.  We then use the `rpicam-apps` utility tools for capturing pictures and videos with the Pi Camera Module 3&#x20;

    ```
    # rpicam-still -o image.png --width 1920 --height 1080
    [1:29:40.425770158] [42] INFO Camera camera_manager.cpp:284 libcamera v0.2.0+120-eb00c13d
    [1:29:40.556087776] [43] WARN RPiSdn sdn.cpp:40 Using legacy SDN tuning - please consider moving SDN inside rpi.denoise
    [1:29:40.559500703] [43] INFO RPI vc4.cpp:446 Registered camera platform/soc/fe205000.i2c/i2c-22/i2c-10/10-001a imx708 to Unicam device /dev/media2 a nd ISP device /dev/media3
    [1:29:40.559686831] [43] INFO RPI pipeline_base.cpp:1102 Using configuration file '/usr/local/share/libcamera/pipeline/rpi/vc4/rpi_apps.yaml'
    Preview window unavailable
    Mode selection for 2304:1296:12:P
    SRGGB10_CSI2P,1536x864/0 - Score: 3400
    SRGGB10_CSI2P,2304x1296/0 - Score: 1000
    SRGGB10_CSI2P,4608x2592/0 - Score: 1900
    Stream configuration adjusted
    [1:29:40.666479417] [42] INFO Camera camera.cpp:1183 configuring streams: (0) 2304x1296-YUV420 (1) 2304x1296-SBGGR10_CSI2P
    [1:29:40.667590017] [43] INFO RPI vc4.cpp:621 Sensor: platform/soc/fe205000.i2c/i2c-22/i2c-10/10-001a imx708 - Selected sensor format: 2304x1296-SBGG R10_1X10 - Selected unicam format: 2304x1296-pBAA
    ## Frame capturing output...
    Mode selection for 1920:1080:12:P                                                                                                                     
        SRGGB10_CSI2P,1536x864/0 - Score: 2200                                                                                                            
        SRGGB10_CSI2P,2304x1296/0 - Score: 1150                                                                                                           
        SRGGB10_CSI2P,4608x2592/0 - Score: 2050                                                                                                           
    [1:30:54.605246722] [49]  INFO Camera camera.cpp:1183 configuring streams: (0) 1920x1080-YUV420 (1) 2304x1296-SBGGR10_CSI2P                           
    [1:30:54.618294956] [50]  INFO RPI vc4.cpp:621 Sensor: platform/soc/fe205000.i2c/i2c-22/i2c-10/10-001a imx708 - Selected sensor format: 2304x1296-SBGG
    R10_1X10 - Selected unicam format: 2304x1296-pBAA                                                                                                     
    Still capture image received 
    ```

## Contributing Peripheral support to CHI@Edge:

With the recent effort to revive peripheral support on CHI@Edge, we aim to brew an environment where our community members can contribute on all the different layers involved in enabling peripherals on the platform.

If you developped support for a peripheral and would like to share it with the community, please refer to the following resources:

* [Chameleon Helpdesk](https://chameleoncloud.readthedocs.io/en/latest/user/help.html#help-desk) for submitting special device configuration requests (boot options and device profiles)
* [How to use Trovi](https://app.gitbook.com/o/Zkch89lL259lOhVcksT8/s/QcsY7v2L1BHb6PdHVA31/), our reproducible artifact sharing tool to share peripheral support container images and tutorials
* Chameleon PowerUsers CHI@Edge [slack channel](https://chameleoncloud-users.slack.com/archives/C025RJL25L6), please first request access to the power users slack and motivate your request. (**Note:** this slack channel is **not** a support channel but rather a collaborative environment for our power users to contribute to Chameleon)

[^1]: Device profile keyword argument
