树莓派开始，需要准备一张高速的SD卡，用来刷img。

* #### RaspberryPi img下载

https://www.raspberrypi.org/downloads/

* #### Win32DiskImager下载及使用。

https://raspberry-projects.com/pi/pi-operating-systems/win32diskimager

* #### 树莓派打开串口

由于没有显示器，所以最开始阶段打算使用串口与树莓派通信。

树莓派默认没有打开串口，需要在SD卡的config.txt文件，最后添加`enable_uart=1`。

https://blog.csdn.net/w_z_z_1991/article/details/52670047