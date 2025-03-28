# Troubleshoot FortiGate: Unable to Connect to FortiGuard Servers

### The following error appears under Dashboard -> Status -> Licenses: 
![CHEESE!](/Images/GetImage.png)

### The same message is also shown under System -> FortiGuard -> FortiGuard Updates as below:
![CHEESE!](/Images/GetImage_2.png)

#### The update debug shows 'Failed getting wan ip' as below: 
 
```shell
do_setup[344]-Failed setup 
do_update[632]-UPDATE failed 
do_check_wanip[787]-Failed getting wan ip
```
#### Issue 1: Cloud Communication and Default Servers Disabled in Previous Firmware Version
***Problem***: The issue is due to the 'cloud-communication' and 'include-default-servers' being disabled in the previous firmware version. These need to be enabled in order to allow the FortiGate device to communicate with FortiGuard, located in the internet cloud.
#### Solution: To resolve this issue, enable the necessary settings using the following commands:
```shell
config system global
    set cloud-communication enable
end
```

```shell
config system central-management
    set include-default-servers enable
end
```
#### Scenario 2: PPPoE WAN Interface with Failed WAN IP
***Problem***: In cases where the PPPoE WAN interface fails to receive a WAN IP, the following debug logs may be seen:
```shell
upd_pkg_recv[1721]-Error receiving pkg header len=0 hdr=64
__upd_act_update[303]-Failed receiving update rsp
```
***Solution***: To resolve this issue, try changing the interface MTU to 1300:
```shell
config system interface
    edit pppoe1
        set mtu 1300
    next
end
```
#### FortiGate Commands Differences (v6.2.x or v6.4.x)
The following commands are new or differ from the older versions of FortiGate (v6.2.x or v6.4.x).
***Commands to Run:***
```shell
dia de reset
dia de consol time en
dia de app update -1
dia de en
exe update-now
```
#### Run these commands for five to ten minutes:
```shell
dia de di
dia de reset
dia autoupdate versions
```
