This is an awkward setup. With the Virtualbox required for running the jetpack install manager (flash and package transfer), a lot of hangups with connecting the host vm, host and jetson. Several different stories of success are out there, but my experience had even more problems, so try those first and then mine if problems persist

Start here: https://github.com/KleinYuan/tx2-flash

If you can only flash but the package transfer quits because no ip address is found, try here: https://devtalk.nvidia.com/default/topic/1002081/jetson-tx2/jetpack-3-0-install-with-a-vm/1

If you cannot ping or ssh the jetson (should be "ping 192.168.55.1" and "ssh nvidia@192.168.55.1", though check README.txt on l4t removable device (connected by ethernet and microusb) and run "ifconfig" on jestson to confirm)..

..then there is likely a mixup between the three operating systems. Try running "arp -an" "nmcli g" "route -n" on each device to verify (or divine) which have each other's addresses and which don't

If you're like me, the jetson can be ssh'd by the Mac but not the vm on the mac. How I solved this was to remove the l4t connection from the network preferences in Mac. Power down the jetson, open the VM and then power on the jetson again. The l4t should dock now in the VM instead of in the Mac os. Set a new usb ethernet connection (do every time) to 192.168.55.2 subnet mask 255.255.255.0 (as described in the l4t README.txt). Though it may still not appear in the arp table, it should be pingable from this connection, and vice versa from the jetson. 

Now, we can feasibly transfer the package by running the jetpack installer but not flashing or installing partitions and etc. as these were successful. But, supposedly we need to do this while in force recovery mode, and placing the device like so causes the device to no longer be pingable from the vm. Well, I simply tried to transfer the package (not the flash and partitioning) without being in force recovery mode and found it worked. 

Afterwards, I use Mac os to ssh through the ethernet connection.

Python and tensorflow can be setup here: http://www.jetsonhacks.com/2017/09/22/install-tensorflow-python-nvidia-jetson-tx-dev-kits/

But anaconda can't run on the aarm64 so packages must be installed oneself. 

Pytorch for gpu-accelerated tensors can be installed here: https://github.com/dusty-nv/jetson-scripts/blob/master/pytorch_jetson_install.sh. Useful issues and links can be found in comments below. 

I had a problem though as Cmake wasn't installed, so check version and/or install: sudo apt-get --assume-yes install cmake

This got through to the "sudo python3 setup.py develop" command which failed. Since pytorch recommends installation with anaconda (not possible with TX2) I figured I should set up dependencies first to see if this would help it through.. and it did.

I installed standard and dev python packages + OpenCV 3 here: https://github.com/Naurislv/NVidia-Jetson-TX2-Install-Guide-Lines and here: https://github.com/Naurislv/NVidia-Jetson-TX2-Install-Guide-Lines

For choosing performance modes: http://www.jetsonhacks.com/2017/03/25/nvpmodel-nvidia-jetson-tx2-development-kit/

install/build tensorflow here: https://github.com/jetsonhacks/installTensorFlowTX2

if you have a jetson package later than 3.1, you will have Cuda 9.0, and the above tensorflow install is for 8.0. You can either install 8.0 or gedit the sh files in the tensorflow build to be for Cuda version 9.0 and Cudnn version 7.0.5. Also, be sure to change the file directory from cuda to cuda-9.0  



