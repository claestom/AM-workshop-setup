### Raspberry PI setup

OS Image: Debian Bookworm 64-bit, no desktop

Connect to device via ssh (terminal/bash):

1) ssh pi@ip-address

    Env: ssh username@ip-address

    password: xxx

    *( if connection does't work: check what IP address of raspberry pi is via command: 
    ```ping raspberrypi.local ```)*

    In case you get an error regarding the hostname, run following command before you try to ssh connect:```ssh-keygen -R ip-address```.

2) Connect to device via vs code:

    1) Install extension: remote development
    2) F1 command pallete
    3) Enter username@ip-address
    4) Select Linux
    5) Enter password: msftiotrp4
    6) Open terminal







