Usage example:  Script (executing on router) monitors a video streaming device (e.g., WD TV Live, Roku, Ceton Echo).  When the device is turned on, the script (router) wakes a media server that stores media files for the device.  Script is low overhead on router, so multiple scripts can be run for different devices or combinations of different devices.

```
#!/bin/sh

#scan IP address of source, when active send  WOL to IP address of target
#Source Device Description:  Description Here
#Target Device Description:  Description Here


INTERVAL=5
NUMP=3
# Target is internal IP static address you want to wake
TARGET=PUT YOUR INTERNAL IP ADRESS HERE!
# Source is internal IP static address you want to scan if awake
SRC=PUT YOUR INTERNAL IP ADDRESS HERE!
IFACE=br0
# MAC is the MAC address of the device you want to wake
MAC=PUT YOUR MAC ADRESS HERE!
WOL=/usr/bin/ether-wake
LOGFILE="/var/log/ether-wake.log"

while sleep $INTERVAL;do

ping -q -c $NUMP $SRC > /dev/null

if [ $? -eq 0 ]; then
#       remove comments from echo commands for debug logging
#  echo "$SRC is awake and responding to pings" `date`>> $LOGFILE
	ping -q -c $NUMP $TARGET > /dev/null
	if [ $? -eq 0 ]; then
#		echo "DO NOT WAKE because Target $Target is already awake" `date`>> $LOGFILE
		sleep 1
	else
#		echo "WAKE Target $TARGET at $MAC because $SRC has awaken but Target is still asleep" `date`>> $LOGFILE
		$WOL -i $IFACE $MAC
		sleep 1
	fi
else
#	echo "DO NOT WAKE Target because Source $SRC is asleep" `date`>> $LOGFILE
	sleep 1
fi

done
```