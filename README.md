# AM-workshop-setup

This repo consists of the configuration settings for the IoT Edge &amp; OPC UA Publisher workshop for Arcelor Mittal.

The architecture will look as follows.

![Architecture Diagram](./imgs/architecture.png)

* [pn.json](./config-files/pn.json)

This file consists of the "published nodes" of the OPC UA Server. 

This file is basically an array, meaning that this json file can target multiple OPC UA Servers. In other words, there is a many-to-one relationship available between OPC UA Servers & the IoT Edge instance.

More information about the schema & the parameters to be set can be find here: https://github.com/Azure/Industrial-IoT/blob/main/docs/opc-publisher/readme.md#configuration-schema.

For the POC of AM: the OPC UA Server endpoint can be found here: *The base endpoint URL of the Aspen InfoPlus.21 UA server is specified in the 
OPC UA Configuration file tsk_opcua_server.opcua.config.xml.*

Example: opc.tcp://localhost:63500/InfoPlus21/OpcUa/Server

### Resources

[User-guide.pdf](./user-guide/AspenIP21OPCUAServer-V14_3-Usr.pdf)

[Microsoft OPC UA Publisher](https://github.com/Azure/Industrial-IoT/blob/main/docs/opc-publisher/readme.md)

