# bb_mac_wifi_diagnostics
A bash bunny payload to collect Wi-Fi related info from a Mac.

## Objective
The goal of this payload is to create an easy way to collect wifi diagnostics from a Mac OSX computer for the aid of troubleshooting.  This is not a security payload, nor is this meant to be a stealth package.  It's just intended to ease collection of data from a machine to expedite troubleshooting.

## What it does
In an effort to be completely transparent, this payload does 4 things.
1. Attaches to the machine as ECM_ETHERNET.  Gets the host name through the GET TARGET_HOSTNAME
2. Attaches to the machine as a HID device and USB Flash Storage
3. Opens a terminal and runs the following commands which save the command output to the bashbunny's loot folder.
   1.```/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport --scan --xml | tee /Volumes/BashBunny/Loot/${TARGET_HOSTNAME}-wifidiag.xml```
   2. ```/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -I --xml| tee /Volumes/BashBunny/Loot/${TARGET_HOSTNAME}-wificurrent.xml```
   3. ```/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport --scan | tee /Volumes/BashBunny/Loot/${TARGET_HOSTNAME}-wifidiag.txt```
   4. ```defaults read /Library/Preferences/SystemConfiguration/com.apple.airport.preferences | grep LastConnected -A 7 | tee /Volumes/BashBunny/Loot/${TARGET_HOSTNAME}-roaming-info.txt```
   5. ```/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -I | tee /Volumes/BashBunny/Loot/${TARGET_HOSTNAME}-wificurrent.txt```
4. Ejects the bash bunny as a USB Storage device and quits the terminal
