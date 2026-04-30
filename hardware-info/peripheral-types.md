# Peripheral Types

## Devices with Peripherals

Note: This table will be moving into the [dynamic hardware table](https://www.chameleoncloud.org/edge/hardware/) soon, but lives here for the moment.

| Name             | Peripheral Type         | Device Profile         |
| ---------------- | ----------------------- | ---------------------- |
| iot-rpi-cm4-02   | Waveshare Sense Hat (B) | pi\_gpio               |
| iot-rpi4-picam2  | Raspberry Pi Sense HAT  | pi\_sensehat           |
| iot-rpi4-picam2  | Pi Camera v2.1          | pi\_camera             |
| iot-rpi4-picam3  | Raspberry Pi Sense HAT  | pi\_sensehat           |
| iot-rpi4-picam3  | Pi Camera v3            | pi\_camera,pi\_camera3 |
| iot-rpi4-ov5647  | Wide Angle 160 OV5647   | pi\_camera             |
| Iot-rpi5-nvme-01 | Pi Camera v3            | pi\_camera,pi\_camera3 |

## CHI@Edge Peripheral Types

Peripherals require explicit `device_profiles` during container creation to map physical hardware paths into the container namespace.

For example, a camera might require /dev/video0. The device profile "pi\_camera" makes /dev/video0 (among others), get passed from the host device into your container.

## Vision and Imaging

### Raspberry Pi Camera

Support for official Raspberry Pi Camera modules (v2, v3, and HQ).

* Device Profil&#x65;**:** `pi_camera`
* Driver Stac&#x6B;**:** Uses the **libcamera** stack. Legacy `raspistill`/`raspivid` tools are deprecated in favor of `libcamerastill` and related. The following artifact and git repo provide a docker image with necessary drivers pre-installed.
* Artifact&#x73;**:**
  * Trov&#x69;**:** [Camera Peripheral Example](https://trovi.chameleoncloud.org/dashboard/artifacts/7d35f884-68d8-4b91-b42b-207717c9b742)
  * GitHu&#x62;**:** [edge-picamera-image](https://github.com/ChameleonCloud/edge-picamera-image)
* Spec&#x73;**:** [Raspberry Pi Camera Documentation](https://www.raspberrypi.com/documentation/accessories/camera.html)
* Device Nodes mappe&#x64;**:**
  * `/dev/video0` - `/dev/video31`
  * `/dev/media0-4`
  * `/dev/vchiq`
  * `/dev/dma_heap`

#### Pi Camera 3

<div align="left"><figure><img src="../.gitbook/assets/image (8).png" alt="" width="307"><figcaption></figcaption></figure></div>

* Sensor: Sony IMX708
* Autofocus
* 12MP
* Specs: [https://www.raspberrypi.com/products/camera-module-3/](https://www.raspberrypi.com/products/camera-module-3/)

Note: The Pi Camera3 needs both the `pi_libcamera` and the `pi_camera3` device profiles.\
This is because it exposes additional interfaces under /dev for things like autofocus.\
\
Upcoming work on peripheral management will remove the need to do this.

#### Pi Camera 2.1

<div align="left"><figure><img src="../.gitbook/assets/image (9).png" alt="" width="375"><figcaption></figcaption></figure></div>

* Sensor: Sony IMX219
* No Autofocus
* 8MP
* Specs: [https://www.raspberrypi.com/products/camera-module-v2/](https://www.raspberrypi.com/products/camera-module-v2/)

#### Wide Angle 160 OV5647

* Sensor: OV5647
* 5MP
* No Autofocus
* Datasheet: [https://files.seeedstudio.com/wiki/OV5647\_Series\_Camera\_Module/OV5647-160\_FOV\_IR\_Camera\_module.pdf](https://files.seeedstudio.com/wiki/OV5647_Series_Camera_Module/OV5647-160_FOV_IR_Camera_module.pdf)

## Environmental and Motion Sensors

### Official Raspberry Pi Sense HAT

{% embed url="https://www.raspberrypi.com/documentation/accessories/sense-hat.html" %}

<figure><img src="../.gitbook/assets/image (10).png" alt="" width="563"><figcaption></figcaption></figure>

* Device Profile: `pi_sensehat`
* Sensors/Interfaces:
  * I2C: **Humidity** (HTS221), **Pressure**/**Temp** (LPS25H), **IMU** (LSM9DS1).
  * **Framebuffer**: 8x8 RGB LED Matrix (`/dev/fb0`).
  * **Input**: 5-button Joystick (`/dev/input/event0-4`).
* Artifacts:
  * Trovi: [CHI@Edge Sensors and GPIO Tutorial](https://trovi.chameleoncloud.org/dashboard/artifacts/1b8cdcba-e10c-4cae-8b1e-145037bc4cda)
  * GitHub: [edge\_sensehat\_image](https://github.com/ChameleonCloud/edge_sensehat_image)
* Docker Image: `chameleoncloud/edge_sensehat_image:latest`&#x20;

### Waveshare Sense HAT (B)

{% embed url="https://www.waveshare.com/wiki/Sense_HAT_(B)" %}

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

* Sensors/Interfaces:
  * I2C: Temp/Humidity (SHTC3), Pressure (LPS22HB), IMU (ICM20948), Color (TCS34725).
  * ADC: SGM58031 (4-ch 16-bit) for external analog inputs.
  * GPIO: Extended pins for additional peripheral integration.
* Artifacts:
  * Trovi (Primary): [CHI@Edge Sensors and GPIO Tutorial](https://trovi.chameleoncloud.org/dashboard/artifacts/1b8cdcba-e10c-4cae-8b1e-145037bc4cda) (includes Waveshare-specific notebook).
  * GitHub: [edge\_sensehat\_image](https://github.com/ChameleonCloud/edge_sensehat_image)
* Docker Image: `chameleoncloud/edge_sensehat_image:latest` (Packages `adafruit-blinka` and `rpilgpio` for Waveshare hat support).
* Device Profile: `pi_gpio` (Shared profile providing I2C and GPIO access).
