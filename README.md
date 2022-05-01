# access-raspberrypi-using-ssh
how to access the RaspberryPi using ssh

sometimes you are trying to connect to you pi using the hostname such as raspberrypi.local
if you faced the problem that you can't connect even you are connected on the same LAN here some solutions

you have to ping search for ip
use the arp -a command

## For Linux 

```
┌──(john㉿JohnHost)-[~]
└─$ arp -a                                                                                                                                                        1 ⨯
h188a (192.168.1.1) at 34:36:56:e6:5f:cb [ether] on wlan0
? (192.168.1.5) at b8:21:eb:23:85:ba [ether] on wlan0
```


as you notice the following is the mac address of my raspberrypi and it's fixed so try to keep it in your notepad
b8:21:eb:23:85:ba so our correponding ip for our mac address is  192.168.1.5

what about if we want to connect using RNDIS/USB
first edit the file 
Step 1. Edit config.txt & cmdline.txt
After burning the SD card, do not eject it from your computer! Use a text editor to open up the config.txt file that is in the SD card post-burn.
Go to the bottom and add dtoverlay=dwc2 the last line:

Save the config.txt file as plain text and then open up cmdline.txt After rootwait (the last word on the first line) add a space and then modules-load=dwc2,g_ether

after connecting the usb go to your network manager and set your ethernet configuration
navigate to IPv4 Settings and set to Shared to other computers instead of Automatic(DHCP)
then do the previous step
 ``` 
┌──(john㉿JohnHost)-[~]
└─$ arp -a
h188a (192.168.1.1) at 34:36:54:e6:5f:cb [ether] on wlan0
? (10.42.0.2) at 82:44:d2:6e:e3:36 [ether] on usb0
```
great our ip address is 10.42.0.2
if you noticed the mac changed 82:44:d2:6e:e3:36 that becuase we use ethernet mac address not wlan and they differ of course

if you tried to ping on the ip, you should find response now
```
john@ubuntu:~$ ping 10.42.0.2
PING 10.42.0.2 (10.42.0.2) 56(84) bytes of data.
64 bytes from 10.42.0.2: icmp_seq=1 ttl=64 time=2.37 ms
64 bytes from 10.42.0.2: icmp_seq=2 ttl=64 time=0.994 ms
64 bytes from 10.42.0.2: icmp_seq=3 ttl=64 time=1.00 ms
64 bytes from 10.42.0.2: icmp_seq=4 ttl=64 time=1.18 ms
64 bytes from 10.42.0.2: icmp_seq=5 ttl=64 time=2.25 ms
64 bytes from 10.42.0.2: icmp_seq=6 ttl=64 time=2.02 ms
64 bytes from 10.42.0.2: icmp_seq=7 ttl=64 time=0.784 ms
64 bytes from 10.42.0.2: icmp_seq=8 ttl=64 time=1.57 ms
64 bytes from 10.42.0.2: icmp_seq=9 ttl=64 time=0.985 ms
64 bytes from 10.42.0.2: icmp_seq=10 ttl=64 time=1.20 ms
64 bytes from 10.42.0.2: icmp_seq=11 ttl=64 time=0.807 ms
64 bytes from 10.42.0.2: icmp_seq=12 ttl=64 time=1.76 ms
64 bytes from 10.42.0.2: icmp_seq=13 ttl=64 time=2.43 ms
  ```
after accessing you pi, I recommend for you to make the ip static for easier further connecting

to make static ip , open the following file
```
sudo nano /etc/network/interfaces 
```
then add the following
```
auto usb0 
allow-hotplug usb0 
# The address, gateway and netmask are necessary. 
iface usb0 inet static 
    address 10.42.0.2 
    netmask 255.255.255.252
    dns-search rpi0.edu
    dns-namesevers 8.8.8.8 128.83.185.40, 128.83.185.41

```










If you cann’t connect on rpi using DHCP

  
  ![image](https://user-images.githubusercontent.com/55362599/166166058-22fa0cd5-374a-49ed-a7f4-f7b7334725ab.png)


Here it’s trying to connect but fails that’s because the dhcp connection

You have to make the connection shared to other computers
Or make it static ip as following

![image](https://user-images.githubusercontent.com/55362599/166166073-9855f741-b0a8-4795-9707-9dd6da1fbf9c.png)






After making the following steps you will find it’s connected



![image](https://user-images.githubusercontent.com/55362599/166166077-a59be138-1335-452c-b4a9-5650d14d1504.png)


## on Windows

use arp -a to find the ip addresses on you local network.
```
C:\Users\Dell>arp -a

Interface: 192.168.1.3 --- 0x3
  Internet Address      Physical Address      Type
  192.168.1.1           34-36-54-e6-5f-cb     dynamic
  192.168.1.5           b8-27-eb-23-85-ba     dynamic
  192.168.1.9           a0-88-69-9c-6a-0d     dynamic
  192.168.1.255         ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static

Interface: 192.168.157.1 --- 0x5
  Internet Address      Physical Address      Type
  192.168.157.254       00-50-56-e6-dd-4d     dynamic
  192.168.157.255       ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static

Interface: 192.168.17.1 --- 0x9
  Internet Address      Physical Address      Type
  192.168.17.254        00-50-56-e3-44-32     dynamic
  192.168.17.255        ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static

C:\Users\Dell>
```

as you notice the 2nd line shows the physical address  b8-27-eb-23-85-ba  and its corresponding ip address 192.168.1.5
now we can ping and connect using putty or cmdline

to connect using the cmdline, just type

ssh <user name>@<ip address>
```
  ssh pi@192.168.1.5
  ```
and insert your password

or you can connect using putty by inserting the ip address only




To connect on windows you have to install RNDIS/USB driver [here](https://evwiki.ensto.technology/display/CHWI/Install+RNDIS+Driver+for+Windows+to+be+able+to+connect+to+charge+controller)



if you tried to ping on the ip, you should find response now
```
C:\Users\Dell>ping 10.42.0.2
PING 10.42.0.2 (10.42.0.2) 56(84) bytes of data.
64 bytes from 10.42.0.2: icmp_seq=1 ttl=64 time=2.37 ms
64 bytes from 10.42.0.2: icmp_seq=2 ttl=64 time=0.994 ms
64 bytes from 10.42.0.2: icmp_seq=3 ttl=64 time=1.00 ms
64 bytes from 10.42.0.2: icmp_seq=4 ttl=64 time=1.18 ms
64 bytes from 10.42.0.2: icmp_seq=5 ttl=64 time=2.25 ms
64 bytes from 10.42.0.2: icmp_seq=6 ttl=64 time=2.02 ms
64 bytes from 10.42.0.2: icmp_seq=7 ttl=64 time=0.784 ms
64 bytes from 10.42.0.2: icmp_seq=8 ttl=64 time=1.57 ms
64 bytes from 10.42.0.2: icmp_seq=9 ttl=64 time=0.985 ms
64 bytes from 10.42.0.2: icmp_seq=10 ttl=64 time=1.20 ms
64 bytes from 10.42.0.2: icmp_seq=11 ttl=64 time=0.807 ms
64 bytes from 10.42.0.2: icmp_seq=12 ttl=64 time=1.76 ms
64 bytes from 10.42.0.2: icmp_seq=13 ttl=64 time=2.43 ms

  ```
Now we can connect using ssh
Using terminal just type
```
ssh pi@10.42.0.2
```
Or using putty

![image](https://user-images.githubusercontent.com/55362599/166166082-6c41efe4-2617-444c-afa8-0beb829e9be5.png)








If you faced that error while trying to connect using terminal
```
[user@hostname ~]$ ssh pi@10.42.0.2
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
6e:45:f9:a8:af:38:3d:a1:a5:c7:76:1d:02:f8:77:00.
Please contact your system administrator.
Add correct host key in /home/hostname /.ssh/known_hosts to get rid of this message.
Offending RSA key in /var/lib/sss/pubconf/known_hosts:4
RSA host key for pong has changed and you have requested strict checking.
Host key verification failed.
```
This shouldn’t happen if you are using putty but to solve this error using terminal , Simply add the ip to known hosts then reconnect
```
ssh-keygen -R <host>
ssh-keygen -R 10.42.0.1
```
now we can connect
```
ssh pi@10.42.0.2
```





youmk lazeez :"
