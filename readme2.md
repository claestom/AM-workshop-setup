# OPC UA deployment on Azure IoT Edge

## Architecture

![Architecture Diagram](./imgs/architecture.png)

*Components being used in the architecture:*

* (Optional) [Raspberry PI Model 4 (4GB ram)](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)
* (Optional) Azure Virtual Machine (Linux or Windows)
* [Azure IoT Edge](https://learn.microsoft.com/en-us/azure/iot-edge/?view=iotedge-1.5)
* [Azure IoT Edge - EFLOW deployment](https://learn.microsoft.com/en-us/azure/iot-edge/iot-edge-for-linux-on-windows?view=iotedge-1.5)
* [Azure IoT Hub](https://azure.microsoft.com/en-us/products/iot-hub/?msockid=39e7bea7d6b36b3c3f08ad6bd7086a7a)

Note: If you are using a virtual machine to deploy the IoT Edge runtime, ensure that the operating system supports virtualization. A list of supported operating systems can be found [here](https://learn.microsoft.com/en-us/azure/iot-edge/support?view=iotedge-1.5#operating-systems). 
### Hardware requirements

* [System requirements](https://learn.microsoft.com/en-us/azure/iot-edge/iot-edge-for-linux-on-windows?view=iotedge-1.5#prerequisites)
* [Operating systems that are supported](https://learn.microsoft.com/en-us/azure/iot-edge/iot-edge-for-linux-on-windows-support?view=iotedge-1.5#operating-systems)

## Tutorial

### (Optional) Raspberry PI

#### Set up Raspberry PI 

[Getting started with Raspberry PI](https://www.raspberrypi.com/documentation/computers/getting-started.html)

Note: make sure to select an OS that is supported by [Azure IoT Edge](https://learn.microsoft.com/en-us/azure/iot-edge/support?view=iotedge-1.5#operating-systems). An example would be the following OS: 

![rpi image](imgs\rpi-os.png)

#### Connect to Raspberry PI

* [Using remote access](https://www.raspberrypi.com/documentation/computers/remote-access.html) - I would recommend this option
* [Using RDP](https://tutorials-raspberrypi.com/raspberry-pi-remote-desktop-connection/)

### (Optional) Azure Virtual Machine

* [Connect using RDP](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/connect-rdp)
* [Connect using SSH](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/connect-ssh?tabs=azurecli)
* [Connect using RDP with Azure Bastion](https://learn.microsoft.com/en-us/azure/bastion/bastion-connect-vm-rdp-windows)
* [Connect using SSH with Azure Bastion](https://learn.microsoft.com/en-us/azure/bastion/bastion-connect-vm-ssh-windows)

The latter two, using Azure Bastion is more secure given the fact that the VM doesn't require a client, agent, or additional software. More information [here](https://learn.microsoft.com/en-us/azure/bastion/bastion-overview).

### Azure IoT Edge

#### Deploy EFLOW (Azure IoT Edge for Linux on Windows)

[Make sure to carefully follow the instructions in device requirements](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-on-windows-symmetric?view=iotedge-1.5&tabs=azure-portal#device-requirements)

* [Create and provision an IoT Edge for Linux on Windows device using symmetric keys](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-on-windows-symmetric?view=iotedge-1.5&tabs=azure-portal)
* [Create and provision an IoT Edge for Linux on Windows device using X.509 certificates](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-on-windows-x509?view=iotedge-1.5&tabs=azure-portal)

*Useful resources:*
* [Virtual Switch creation on Windows Server](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-create-virtual-switch?view=iotedge-1.5)
* [Configure nested virualization on WS Server / Windows 10](https://techcommunity.microsoft.com/blog/itopstalkblog/how-to-setup-nested-virtualization-for-azure-vmvhd/1115338)
* [Azure VMs that support nested virtualization](https://www.markou.me/2020/05/which-azure-vm-sizes-support-nested-virtualization/)
* [Shared folders between Guest OS & CLB-Mariner Linux EFLOW VM](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-share-windows-folder-to-vm?view=iotedge-1.5) (optional)

#### Azure IoT Edge (option Linux OS)

* [Create and provision an IoT Edge device on Linux using symmetric keys](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-symmetric?view=iotedge-1.5&tabs=azure-portal%2Cubuntu)
* [Create and provision an IoT Edge device on Linux using X.509 certificates](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-provision-single-device-linux-x509?view=iotedge-1.5&tabs=azure-portal%2Cubuntu)

Note: symmetric keys can be used for testing purposes. However, in production, the use of X509 certs is recommended.

### OPC UA Publisher module

Before jumping into the technical implementation, please refer to following document to get familiar with the OPC UA Publisher we are about the deploy: [how OPC UA Publisher works](https://github.com/Azure/Industrial-IoT/blob/main/docs/opc-publisher/readme.md#how-opc-publisher-works).

First, we need to create a configuration file on the client where the IoT Edge is deployed. We will call this file: `publishednodes.json`.

I would recommend to create this file locally using notepad or another text editor application.

More information on how to configure this file is explained in following [article](https://github.com/Azure/Industrial-IoT/blob/main/docs/opc-publisher/readme.md#configuration-via-configuration-file).

The simplest way to configure OPC Publisher is via a file. A basic configuration file looks like this:

```
[
  {
    "EndpointUrl": "opc.tcp://testserver:62541/Quickstarts/ReferenceServer",
    "UseSecurity": true,
    "OpcNodes": [
      {
        "Id": "i=2258",
        "OpcSamplingInterval": 2000,
        "OpcPublishingInterval": 5000,
        "DisplayName": "Current time"
      }
    ]
  }
]
```
For the Aspentech historian an example can be found [here](./config-files/pninfoplus.json).

Once the configuration file is finished, we move over to the client side, where the IoT Edge runtime will be deployed. 

Please run following commands there:
Connect to the EFLOW runtime
```
Connect-EflowVm
```
Create a new folder called *iiotedge* and next a new file for the configuration file we created above
```
mkdir iiotedge
cd iiotedge
sudo nano publishednodes.json
```
Copy paste the configuration file create a few steps above from the local text edtior 
```
Ctrl + O
Enter
Ctrl + X
```
Now that the client has the required configuration file in the system, we can proceed with installing the OPC UA Publisher module.

Follow the first 6 steps to [deploy OPC Publisher using the Azure Portal](https://github.com/Azure/Industrial-IoT/blob/main/docs/opc-publisher/readme.md#deploy-opc-publisher-using-the-azure-portal).

Next: in the Container Create Options we need to add the followinging file:
```
{
  "HostConfig": {
    "Binds": [
      "/home/iotedge-user/iiotedge:/appdata"
    ]
  },
  "Hostname": "publisher",
  "Cmd": [
    "publisher",
    "--pf=/appdata/publishednodes.json",
    "--aa"
  ]
}
```
Select Add and then Next to continue to the routes section.

Next, in the routes tab, add the following:
```
FROM /messages/modules/publisher/* INTO $upstream
```
Click Review + Create.

You can check whether the module is running correctly:
* Azure: 

    1) Go to your IoT Hub instance
    2) IoT Edge
    3) Click on the relevant IoT Edge device ID
    4) See if the state for the publisher module == running
* IoT Edge (client side)

  1) ``` Connect-EflowVm ```
  2) ```sudo iotedge list```
  3) See if the state for the publisher module == running

Verify whether the data is being transmitted to Azure IoT Hub

First, validate whether the OPC UA Publisher is working properly: ``` sudo iotedge logs publisher ```

Next, verify whether the data is ingested into Azure IoT Hub

*  Using Azure IoT Hub Explorer

    An application you case use to analyze the telemetry being send by the devices to IoT Hub. More information in following documentation on how to install & get started: [Azure IoT Explorer](https://learn.microsoft.com/en-us/azure/iot/howto-use-iot-explorer)

* Azure IoT Hub

    1) Go to your IoT Hub instance
    2) In the overview page, look at the dashboard "Device to cloud messages". 







