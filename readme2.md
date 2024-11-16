# OPC UA deployment on Azure IoT Edge

## Architecture

![Architecture Diagram](./imgs/architecture.png)

*Components being used in the architecture:*

* (Optional) [Raspberry PI Model 4 (4GB ram)](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)
* [Azure IoT Edge](https://learn.microsoft.com/en-us/azure/iot-edge/?view=iotedge-1.5)
* [Azure IoT Edge - EFLOW deployment](https://learn.microsoft.com/en-us/azure/iot-edge/iot-edge-for-linux-on-windows?view=iotedge-1.5)
* [Azure IoT Hub](https://azure.microsoft.com/en-us/products/iot-hub/?msockid=39e7bea7d6b36b3c3f08ad6bd7086a7a)

### Hardware requirements

* [System requirements](https://learn.microsoft.com/en-us/azure/iot-edge/iot-edge-for-linux-on-windows?view=iotedge-1.5#prerequisites)
* [Operating systems that are supported](https://learn.microsoft.com/en-us/azure/iot-edge/iot-edge-for-linux-on-windows-support?view=iotedge-1.5#operating-systems)

## Tutorial

### Raspberry PI

#### Set up Raspberry PI 

[Getting started with Raspberry PI](https://www.raspberrypi.com/documentation/computers/getting-started.html)

Note: make sure to select an OS that is supported by [Azure IoT Edge](https://learn.microsoft.com/en-us/azure/iot-edge/support?view=iotedge-1.5#operating-systems). An example would be the following OS: 

![rpi image](imgs\rpi-os.png)

#### Connect to Raspberry PI

* [Using remote access](https://www.raspberrypi.com/documentation/computers/remote-access.html) - I would recommend this option
* [Using RDP](https://tutorials-raspberrypi.com/raspberry-pi-remote-desktop-connection/)

### Azure IoT Edge

#### Deploy Azure IoT Edge (option Windows OS)

[Make sure to carefully follow the instructions in device requirements](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-on-windows-symmetric?view=iotedge-1.5&tabs=azure-portal#device-requirements)

* [Create and provision an IoT Edge for Linux on Windows device using symmetric keys](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-on-windows-symmetric?view=iotedge-1.5&tabs=azure-portal)
* [Create and provision an IoT Edge for Linux on Windows device using X.509 certificates](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-on-windows-x509?view=iotedge-1.5&tabs=azure-portal)

### Azure IoT Edge (option Linux OS)

* [Create and provision an IoT Edge device on Linux using symmetric keys](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-symmetric?view=iotedge-1.5&tabs=azure-portal%2Cubuntu)
* [Create and provision an IoT Edge device on Linux using X.509 certificates](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-x509?view=iotedge-1.5&tabs=azure-portal%2Cubuntu)

Note: symmetric keys can be used for testing purposes. However, in production, the use of X509 certs is recommended.

### OPC UA Publisher module

First, we need to create a configuration file on the client where the IoT Edge is deployed. We will call this file: `publishednodes.json`.




* [Deploy OPC Publisher using the Azure Portal](https://github.com/Azure/Industrial-IoT/blob/main/docs/opc-publisher/readme.md#deploy-opc-publisher-using-the-azure-portal)
* [Deploy OPC Publisher using Azure CLI](https://github.com/Azure/Industrial-IoT/blob/main/docs/opc-publisher/readme.md#deploy-opc-publisher-using-azure-cli)



