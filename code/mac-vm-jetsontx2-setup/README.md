This is an awkward setup. With the Virtualbox required for running the jetpack install manager (flash and package transfer), a lot of hangups with connecting the host vm, host and jetson. Several different stories of success are out there, but my experience had even more problems, so try those first and then mine if problems persist

Start here: https://github.com/KleinYuan/tx2-flash

If you can only flash but the package transfer quits because no ip address is found, try here: https://devtalk.nvidia.com/default/topic/1002081/jetson-tx2/jetpack-3-0-install-with-a-vm/1

If you cannot ping or ssh the jetson (should be "ping 192.168.55.1" and "ssh nvidia@192.168.55.1", though check README.txt on l4t removable device (connected by ethernet and microusb) and run "ifconfig" on jestson to confirm)..

..then there is likely a mixup between the three operating systems. Try running "arp -an" "nmcli g" "route -n" on each device to verify (or divine) which have each other's addresses and which don't

If you're like me, the jetson can be ssh'd by the Mac but not the vm on the mac. How I solved this was to remove the l4t connection from the network preferences in Mac. Power down the jetson, open the VM and then power on the jetson again. The l4t should dock now in the VM instead of in the Mac os. Set a new usb ethernet connection (do every time) to 192.168.55.2 subnet mask 255.255.255.0 (as described in the l4t README.txt). Though it may still not appear in the arp table, it should be pingable from this connection, and vice versa from the jetson. 

Now, we can feasibly transfer the package by running the jetpack installer but not flashing or installing partitions and etc. as these were successful. But, supposedly we need to do this while in force recovery mode, and placing the device like so causes the device to no longer be pingable from the vm. Well, I simply tried to transfer the package (not the flash and partitioning) without being in force recovery mode and found it worked. 

Afterwards, I use Mac os to ssh through the ethernet connection.

Python and tensorflow can be setup  here:

But anaconda can't run on the aarm64 so packages must be installed oneself. 

Pytorch can be installed here:

I had a problem though as Cmake wasn't installed, so check version and/or install: sudo apt-get --assume-yes install cmake
