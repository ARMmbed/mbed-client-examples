# Getting started on LWM2M Client Example

This document describes briefly the steps required to start use of LWM2M Client example application on mbed OS. LWM2M Client example application demonstrates how to perform OMA LWM2M bootstrap with bootstrap server and how to register and unregister to mbed Device Server.

## Required hardware

This demo uses Freescale FRDM-K64F board
* [FRDM-K64F](http://developer.mbed.org/platforms/frdm-k64f/)*
An ethernet connection to the internet
An ethernet cable
A micro-USB cable

## Required software

* Yotta
* [Wireshark](https://www.wireshark.org/) (optional nework debugging tool)
* mbed Device Server

## Setting up the environment

### IP address setup

mbed Client example uses ethernet and IPv4 to communicate with mbed Devce Server.
You must ensure that you have a working IPv4 enabled wired ethernet/internet connection which have DHCP resolving capability plugged directly into RJ45 port of the board so that there is no need to setup static IP address.

### mbed Device Server (mDS)

Demo application will register to mbed Device Server using OMA lwm2m bootstrap. You should install mDS test version with bootstrap server on your local computer. Refer to mbed Device Server documentation for installing instructions.

If you have licensed the commercial version of mbed Device Server, this can be downloaded from [ARM Connect](http://connect.arm.com/).
Otherwise, a free developer version can also be used with this example and can be downloaded [here](https://silver.arm.com/browse/SEN00).
Go to downloads/Evaluation product/SEN00 – Sensinode/Development tools/Nanoservice platform. Download MBED device server 2.0 or newer.

## Build instructions

### Installing and building
1. Connect the frdm-k64f to the internet using the ethernet cable
2. Connect the frdm-k64f to the computer with the micro-USB cable, being careful to use the micro-usb port labled "OpenSDA"
3. Install Yotta. See instructions from http://docs.yottabuild.org/#installing
4. Install needed toolchains (arm-none-eabi-gcc). Refer to the yotta installation page (in step 1 above) for instructions on how do install the toolchains.
5. `cd ` **lwm2m-client-example**
6. Open file main.cpp, edit your mbed Device Server's Ipv4 address and port number in place of  `<xxx.xxx.xxx.xxx>` and `<port>` in `coap://<xxx.xxx.xxx.xxx>:<port>` so that it looks something like this `coap://192.168.0.1:5693`
7. Set up target device, `yotta target frdm-k64f-gcc`
8. Type `yotta build`

Binary file will be created to /build/frdm-k64f-gcc/source/ - folder

### Flashing to target device

You need to plug in the USB cable on J26 port on the K64F borad and other end into  USB port of your computer.
Supported mbed board have drag&drop flashing capability. All you need to do is to copy the binary file to
board's usb mass storage device and it will be automatically flashed to target MCU after reset.
You can find the binary file from `cd lwm2m-client-example/build/frdm-k64f-gcc/test/helloworld-lwm2mclient/` with following name `lwm2m-client-example-test-helloworld-lwm2mclient.bin`

## Testing

### Log network traffic (optional)

1. Start Wireshark in computer where mbed Device Server is running
2. Select your ethernet interface, usually "Local Area Connection"
3. Click start
4. Select "filter" field on top and add filter to correspond your bootstrap server and mbed Device Servers ports
5. Power up your mbed board

You should see endpoint to do OMA LWM2M bootstrap process with bootstrap serve, and after that registering to mbed Device Server.

### Using demo application

Update OMA LWM2M bootstrap server address and port from main.cpp. These values should match your actual server address and configured port.

### Testing OMA lwm2m device with mbed Device Server

Start mbed Device server, bootstrap server and web UI software, for example lighting. See mbed Device Server documentation for more information.
Remember, that you will need to add your end-point name for e.g: `lwm2m-endpoint` as defined in the example application to the accepted list in bootstrap server and provide your mbed Device Server address to the bootstrap server in following format `coap://<xxx.xxx.xxx.xxx>:<Port>` where `<xxx.xxx.xxx.xxx>` is Ipv4 address of mbed Device Server.

The above steps will power up node. Now node application will start bootstrap process with OMA LWM2M bootstrap server. After received needed information application will send CoAP registration message to mbed Device Server.

After registration, you can see your endpoint in mbed Device Server through web UI. Open web UI and select tab "End-points"
You can also see resources registered from that endpoint.
![Node registered](img/registered.jpg)

You can click endpoint name to open view to see your registered resources. However, making request to resources is not implemented in this release.
![Resource list](img/endpoint_resources.jpg)

Pressing button SW2 will cause endpoint to send unregister message to device server. After successful unregistration, led D12 starts blinking indicating that application has successfully completed and endpoint will disappear from endpoint list in web UI.
