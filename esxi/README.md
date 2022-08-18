# VMware ESXi on Arm cluster

VMware ESXi and vSphere is the leading platform for creating private
virtualised datacentres. Pretty much every medium or large organisation
uses VMware ESXi and vSphere extensively for all their private cloud
computing needs.

For the last couple of years there's been an Early Access
[ESXi on Arm Fling](https://flings.vmware.com/esxi-arm-edition)
that enables you to install ESXi on a Raspberry Pi and other similar
devices. This lets you virtualise your Raspberry Pi's and run mutual
virtual machines (VMs) across a cluster.

You still need to run the vSphere controller VM on an Intel ESXi server
(or VMware Fusion or VMware Workstation if you like - in development only!)
but you can use that to administer a purely Arm64 based cluster like
the DeskPi Super6C running Raspberry Pi CM4s or 
[SOQuartz CM4 modules](../soquartz/README.md)

Things to investigate:-

- See if I can install the ESXi fling on a SOQuartz module on the Super6C
and administer it from an external vSphere Virtual Machine (VM)
- See if the Gigabit Ethernet, HDMI, storage controller, 
USB controller, and so on works OOTB
- See if I can access a shared iSCSI storage folder on my home synology from ESXi
- Get this running across 6 baords in a cluster
- Test vMotion - the live moving of a Virtual Machine from one device to 
another! - across SOQuartz modules (It should work fine but it'll 
be cool to see in action!)

