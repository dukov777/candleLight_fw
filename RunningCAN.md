# MacOS Setup for CandleLight and SLCAN Device

This guide will help you install and set up CandleLight and SLCAN device on MacOS.

## Prerequisites

- MacOS

Follow these steps:

1. Install UTM.
2. Create an ARM Ubuntu Server 22.04 LTS (Note: This is important for virtualizing ARM on Apple silicon).
3. Run the Ubuntu Server instance.
4. Install `can-utils`.
5. Connect the CandleLight dongle to the USB port.
6. Verify if it is part of this: 

    ![image](https://github.com/dukov777/candleLight_fw/assets/10469747/aa76b02e-8074-4c31-a336-eb3746b32fe3)
7. Add it as shown in the image.
8. Repeat the same process for the SLCAN device (STM32 STLink).

## Commands

This guide assumes that CandleLight is the only device connected for now, and is referred to as `can0`.

To configure the `can0` to CAN bus bitrate of 125kBits, use the following commands:

```
ip a s can0
sudo ip link set dev can0 up type can bitrate 125000
ifconfig  # Check if the device exists
```

Next, configure the SLCAN to 125kBits. The device UART bitrate is set to 115200:
```
sudo slcand -o -s4 -S 115200 /dev/ttyACM0 
sudo ip link set up slcan0 
ifconfig
```

Open candump in a separate console to check if the device is receiving messages:

```
candump slcan0
```
Send a message on can0:

```
cansend can0 123#11
```
Check if the message is received on slcan0.

**End of Instructions**

## Additional Resources

https://elinux.org/Bringing_CAN_interface_up

https://www.kernel.org/doc/Documentation/networking/can.txt

https://manpages.ubuntu.com/manpages/jammy/man1/slcand.1.html

https://www.linux-automation.com/en/products/candlelight.html
