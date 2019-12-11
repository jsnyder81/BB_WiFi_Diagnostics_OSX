# BB_WiFi_Diagnostics_OSX
A bash bunny payload to collect Wi-Fi related info from a Mac.

## Objective
The goal of this payload is to create an easy way to collect wifi diagnostics from a Mac OSX computer for the aid of troubleshooting.  This is not a security payload, nor is this meant to be a stealth package.  It's just intended to ease collection of data from a machine to expedite troubleshooting.

## What it does
In an effort to be completely transparent, this payload does 4 things.
1. Attaches to the machine as ECM_ETHERNET.  Gets the host name through the GET TARGET_HOSTNAME
2. Attaches to the machine as a HID device and USB Flash Storage
3. Opens a terminal and runs the following commands which save the command output to the bashbunny's loot folder
   1. ```/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport --scan --xml | tee /Volumes/BashBunny/Loot/${TARGET_HOSTNAME}-wifidiag.xml```
   
   2. ```/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -I --xml| tee /Volumes/BashBunny/Loot/${TARGET_HOSTNAME}-wificurrent.xml```
   
   3. ```/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport --scan | tee /Volumes/BashBunny/Loot/${TARGET_HOSTNAME}-wifidiag.txt```
   
   4. ```defaults read /Library/Preferences/SystemConfiguration/com.apple.airport.preferences | grep LastConnected -A 7 | tee /Volumes/BashBunny/Loot/${TARGET_HOSTNAME}-roaming-info.txt```
   
   5. ```/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -I | tee /Volumes/BashBunny/Loot/${TARGET_HOSTNAME}-wificurrent.txt```
   
4. Ejects the bash bunny as a USB Storage device and quits the terminal

## Example Output:

Files:
1. HOSTNAME-wificurrent.txt:
   This file contains the current state of the Wi-Fi including RSSI, SNR, state, TX Rate, Authentication, BSSID, SSID, MCS and Channel.

2. HOSTNAME-wificurrent.xml
   This is a duplicate of the previous file, but formatted in XML output.  There are some different elements in this output, and some missing.  But it is the same command.
  
3. HOSTNAME-wifidiag.txt
   This file is a current scan result from the wifi chipset in text format.

4. HOSTNAME-wifidiag.xml
   This is the same command but run with the --xml flag.  Not exactly the same data, but similar and in structured format.

5. HOSTNAME-roaming-info.txt
   This file gives you the roaming history of the device.
   
