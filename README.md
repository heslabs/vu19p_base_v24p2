# HAPS VU19P Prototyping System

https://heslabs.github.io/vu19p_base_v24p2

## Lab Environment Setup  

<img src="https://github.com/user-attachments/assets/1c8c767d-aef3-4d51-a60a-0de7b0845120" width=950>

---

| Device | IP Address | Description | Smart WiFi Plug |
|:-|:-|:-|:-|
| HAPS-VU19P | 192.168.50.2 | HAPS-SX VU19P | 192.168.50.92 |
| HAPS-ZYNQ | 192.168.50.3 | HAPS SMF ZynqMPSoC/CA53 module | 192.168.50.93 |
| HAPS-PC | 192.168.50.5 | Linux PC | 192.168.50.95 |


* The smart WiFi plug is used to reset HAPS-ZYNQ after HAPS-FPGA programmed
   * Tapo P105 | Mini Smart Wi-Fi Plug [[Tapo]](https://www.tapo.com/en/product/smart-plug/tapo-p105/)
     
---
## Quick Start Guide

### 1. Access to HAPS-PC
```
ssh vu19p@59.124.169.195 -X
Password:  
```

### 2. Configure HAPS-FPGA (Skip if already configured)
```
cd labs/xsct
```

### 3. Access to the HAPS-Zynq CPU module 
```
ssh xilinx@192.168.50.3 -X
Password: xilinx
```

### 4. Launch Yolov8 demo on Zynq-HAPS
```
cd ~/haps
make bus
```


 
---
## Remote access to HAPS and ZYNQ module

### Step-1: Configure and program FPGA

* Launch FPGA tool "Confpro-SX" in Linux shell
* Openthe Confpro project file (PASS.csprj) with the FPGA bitstream (PASS.bit)
* Select the HAPS system in the scanned list
* Configure the system and wait few seconds
* Check the successful message.  ```Operation successful!```

```
/home/eda/Confpro-SX_1.1.10/guibin/Confpro-SX-GuiRun.sh &
```

### Step-2: Connect HAPS-PC from remote

* Connect HAPS-ZYNQ via serial console

```
<Remote> $ sshpass -p <password> ssh vu19p@59.124.169.195 -X
```

### Step-3: Connect HAPS-ZYNQ via serial console from HAPS-PC  

* Monitoring the HAPS-ZYNQ Boot Process
* Connect a serial cable to the HAPS-ZYNQ and HAPS-PC. Use a terminal program like PuTTY to establish a connection.
* The boot messages will be displayed in the console, including the IP address assignment
  
```
$ ls /dev/ttyUB*
$ sudo chmod 666 /dev/ttyUSB0
$ putty -serial -sercfg 115200,8,n,1,N /dev/ttyUSB0 -fn "client:Ubuntu Mono 16" &
```

* Enter username and password (xilinx:xilinx)
  
```
pynq login: xilinx
password: xilinx
```

* Note
   * A serial console typically does not display graphics; it's a text-based interface designed for basic system diagnostics and troubleshooting, especially when graphical output is unavailable or the operating system is in a non-bootable state.



### Step-4: Connect HAPS-ZYNQ via Ethernet from HAPS-PC  

* To connect HAPS-ZYNQ from HAPS-PC via ssh:  

```
<HAPS-PC> $ sshpass -p xilinx ssh xilinx@192.169.50.3 -X
```

### Step-5:

* Check the 16GB DDR og HAPS system is in the Linux memory map

```
xilinx@pynq:~/haps$ lsmem
RANGE                                 SIZE  STATE REMOVABLE   BLOCK
0x0000000000000000-0x000000007fffffff   2G online        no    0-15
0x0000000800000000-0x000000087fffffff   2G online        no 256-271
0x0000001000000000-0x00000013ffffffff  16G online        no 512-639

Memory block size:       128M
Total online memory:      20G
Total offline memory:      0B
```
