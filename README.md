## Intro

Let's directly to the points without emphesizing how usually people get frustrated on flashing JetPack into NVIDIA TX2 without access to an actually Ubuntu machine.

Therefore, this is all about how to flash into TX2 with an VM (VirtualBox) on Mac.

## 1. Pre-requisites

- [X] A computer running VirtualBox with access to the internet (eg. Arch Linux, Mac OS)

- [X] NVIDIA TX2 with _wired_ access to the internet and power (You can't do this locally)

- [X] A monitor, HDMI Cable, Ethernet Cable, one Keyboard (USB plugged in), one mouse (preferrable), one USB hub (preferrable)

## 2. Install Virtual Box and Extensions

#### 1.1 Install Virtual Box

You need VirtualBox _and_ the extension pack. Extension is needed to enable USB-2/USB-3 connection/communications between any physical USB device and the virtual machine.

##### Mac OS
Download Virtual Box for Mac from [here](http://download.virtualbox.org/virtualbox/5.1.28/VirtualBox-5.1.28-117968-OSX.dmg) and install it first; 

Then download Virtual Box extension [here](http://download.virtualbox.org/virtualbox/5.1.28/Oracle_VM_VirtualBox_Extension_Pack-5.1.28-117968.vbox-extpack) and install it;

##### Arch Linux

Everything is in the AUR.

#### 1.2 Spin up an Ubuntu VM

Download Ubuntu 16.04 iso image from [here](http://releases.ubuntu.com/16.04/ubuntu-16.04.5-desktop-amd64.iso). I tested it with Arch Linux (4.19.2-arch1-1-ARCH). This also confirmed works with 14.04 on Mac OS, see the original version of this guide: https://github.com/KleinYuan/tx2-flash

Then, create an ubuntu machine with following settings:

- [X] Storage is larger than 50GB

- [X] Go to Settings --> Network --> Adapter 1, change `Attached to` to `Bridged Adapter`, and name to whatever under Wi-Fi

- [X] Go to Settings --> Ports --> USB, ensure `Enable USB Controller` is under `USB 3.0 (xHCI) Controller`

You don't have to continue in bridge mode, but it is easier (see [below](#####When-the-VM-is-not-in-bridge-mode)).

Last, load the image that you just downloaded and spin up an VM.

#### 1.3 Download NVIDIA JetPack

In the VM, open firefox browser and go to NVIDIA's official [website](https://developer.nvidia.com/embedded/jetpack) and join them as a member so that you can download the JetPack.

I downloaded JetPack 3.3, but this also confirmed works for JetPack 3.1.

You are expected to find a file called `JetPack-L4T-3.3-linux-x64_b39.run` under `Downloads` folder.

#### 1.4 Install JetPack

Open a terminal and navigate to Downloads folder, then change .run file as executable:

```
cd ~/Downloads
chmod +x ./JetPack-L4T-3.3-linux-x64_b39.run
```

Then, run the .run file:

```
./JetPack-L4T-3.3-linux-x64_b39.run

```

Then just follow all the steps (basically from 2 to 12 without 9 in [here](http://docs.nvidia.com/jetpack-l4t/2_1/content/developertools/mobile/jetpack/jetpack_l4t/2.0/jetpack_l4t_install.htm)): choose TX2, L4T 3.1, full-install, then it will take a while to downloads all the dependencies and then a window will pop up to ask you to confirm the installation.

And when download/installation on host machine is finished, you just click all the way to pop up a terminal says : 

![Post Installation](http://docs.nvidia.com/jetpack-l4t/2_1/content/developertools/mobile/jetpack/images/jetpack_l4t_force_recovery_mode.001_600x364.png)


And now, here's the tricky part:

- [X] Plug in the USB with a USB hub, then connect a mouse, keyboard to TX2

- [X] Connect via HDMI (HDMI-HDMI, no VGA adapters ok?) between TX2 and monitor so that you can see what's going on

- [X] Follow the descriptions, to power off TX2, connect to host-machine, power on, press ..... (just follow what they said), but just hold on Press Enter on host machine 

- [X] Before you Press Ener on host machine's terminal, open settings of your VM, and go to Settings --> Ports --> USB, and click `Add new USB filters with all ..... (blahblahblah)`, then add `NVIDIA Corp. APX`

- [X] Then, go to VM, click bottom right corner the button with shape an USB (pbbly forth one), and select `NVIDIA Corp. APX`, (it would be great to unplug any other USB devices from your Mac)

- [X] Last, go back to your VM, do `Press Enter` on the terminal


##### When the VM is not in bridge mode:

The TX2 will boot up after flashing kernel and OS. The install script will wait for the TX2 to report its IP address to the host. When you do not have bridge mode your VM is not routable from the internet and TX2 cannot report its IP.

Thus you have to provide the IP of TX2 manually.

- [X] Figure out the IP of the TX2 (login the booted up TX2 with default password):
  
  $ ip addr show eth0

- [X] Back inside the VM, pass the IP to the install script:

  $ echo <IP-of-TX2-target> | nc 127.0.0.1 33338
  
And, go and grab a coffee, it will take around 30 min to complete the entire process.

#### 1.5 Validation Flash

Go to TX2, open terminal:

```
# Validate NVCC
nvcc -V

# Validate cuda

ls /usr/local | grep cuda

# Run example

# You can find this easily online, just google it.
```

Ta Da!
