## Multiple Wi-Fi Adapter Guide

This guide will help you configure your Raspberry Pi with two wireless interfaces as a travel router.  As in one interface acts as a wireless client, and the other an access point so the internet connection, and anything else running on your Pi, can be shared with all of your wireless devices. 

Once you have Raspap-Webgui installed, performing the following adjustments to your configuration. 

## Adjust Sudoers File for Additional Wi-Fi Interface
 
 Add the following to the end of `/etc/sudoers` for your additional wireless interface substituting your wireless interface for `wlan1` if needed: 

```sh
www-data ALL=(ALL) NOPASSWD:/sbin/ip link set wlan1 down
www-data ALL=(ALL) NOPASSWD:/sbin/ip link set wlan1 up
www-data ALL=(ALL) NOPASSWD:/sbin/ip -s a f label wlan1
www-data ALL=(ALL) NOPASSWD:/sbin/wpa_cli -i wlan1 scan_results
www-data ALL=(ALL) NOPASSWD:/sbin/wpa_cli -i wlan1 scan
www-data ALL=(ALL) NOPASSWD:/sbin/wpa_cli -i wlan1 reconfigure
www-data ALL=(ALL) NOPASSWD:/sbin/ifdown wlan1
www-data ALL=(ALL) NOPASSWD:/sbin/ifup wlan1

```

## Ensure Wi-Fi Interface Names are Static

If you have multiple Wi-Fi adapters the friendly interface names, such as wlan0 and wlan1, may swap physical interface adapters upon reboot. This will lead to headaches as you wonder why your configuration suddenly stopped working as expected.  To fix this, follow these steps to permanently assign interface names to a specific interface based on its MAC address: 

Create a file in `/etc/udev/rules.d/` called `72-net.rules`

Set the content to the following: 
```sh
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="adapter mac address", NAME="wlan1"
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="adapter mac address", NAME="wlan0"
```


