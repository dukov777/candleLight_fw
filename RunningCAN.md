## Preconditions
*MacOS*
1. Install UTM
2. Create ARM Ubuntu Server 22.04 LTS - this is important since ARM if virtualized on Apple silicon 
3. Run Ubuntu Server instance
4. Install can utils
5. Plug CandleLight dongle to the USB
6. Check if it is part of 
![image](https://github.com/dukov777/candleLight_fw/assets/10469747/aa76b02e-8074-4c31-a336-eb3746b32fe3)
7. Add it as it shown on the picture
8. do the same for SLCAN device (STM32 STLink) 


## Commands

We are assuming that CandleLight is the only device connected for the time being. It is depicted as *can0*.

`ip a s can0`
Configure the can0 to CAN bus boudrate of 125kBits.

`sudo ip link set dev can0 up type can bitrate 125000`

Check if it exist
`ifconfig`

Configure SLCAN to 125kBits. The device UART boudrate is set to 115200.
```
sudo slcand -o -s4 -S 115200 /dev/ttyACM0 
sudo ip link set up slcan0 
ifconfig
```

On a separate console open candump to check if device receives.

`candump slcan0`

Send on can0 a message:

`cansend can0 123#11`

Check on slcan0 if it is received.

**END**


### Sources:
https://elinux.org/Bringing_CAN_interface_up
https://www.kernel.org/doc/Documentation/networking/can.txt
https://manpages.ubuntu.com/manpages/jammy/man1/slcand.1.html

https://www.linux-automation.com/en/products/candlelight.html
