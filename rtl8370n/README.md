# Realtek RTL8370N Ethernet Switch Chip

The RTL8370N is a very common networking chip used on a variety of
multi-node carrier boards and consumer networking equipment.

Its use on the DeskPi Super6C by default is as an unmanaged switch
which connects all 6 nodes to each other and 2 external LAN ports.
By default all ports can talk to all ports and there is no security.

Interestingly, the RTL8370N can be reprogrammed using it's EEPROM chip.
This can enable or disable the functionality in the network processing
application from Realtek.

Potential features I want to explore are:-

- Making 1 LAN port a Management VLAN only port, and the other an incoming Workload VLAN only port
- Restricting which nodes can talk to the Kubernetes control plane node(s) API server at layers 2/3/4
- Using Linux on the SOQuartz nodes to 'hide' the main eth0 interface, but allow use of VLAN virtual
Ethernet interfaces to be used by specific users/groups/processes
- Restricting egress traffic on certain VLAN interfaces from leaving the DeskPi Super6C board's network
- Alternatively, using both LAN uplinks with Link Aggregation, as [mentioned by Jeff in his Super6C video](https://www.youtube.com/watch?v=ecdm3oA-QdQ&t=606s)
- Setting up a Ceph cluster on the same nodes as the K8S cluster, and prioritising storage VLAN traffic

## Investigation

17 Aug 2022 - Tried to find more information on the RTL8370N and its 8051 chip. Found a (Chinese, translated) GitHub reference 
[for the chip](https://github-com.translate.goog/libc0607/Realtek_switch_hacking/blob/master/RTL8370N-Demo.md?_x_tr_sl=auto&_x_tr_tl=en&_x_tr_hl=en-US&_x_tr_pto=wapp)
 and 
[for a rom burner](https://github-com.translate.goog/libc0607/Yakigani?_x_tr_sl=auto&_x_tr_tl=en&_x_tr_hl=en-US&_x_tr_pto=wapp)

It certainly looks like the 8370N does support VLANs and has the same register layout as the 8367N which is supported by linux. Interestingly, the
linux device driver information in my Manjaro Arm boot for board #1 does mention an 8051 chip. I wonder if it has an I2C / SPI connection
in the DeskPi Super6C to the RTL8370N Chip? If so it's possible the first board could use this to change EEPROM settings and restart the Realtek
RTL8370N chip. The 8370(M) DataSheet - which is for the 8370 and 8370M - mentions that both support Link Aggregation (the TRUNK hash map registers)
and so this chip can probably do more than people realise.

## Further reading

- [Pine64 discussion thread on this chip (for a different board)](https://forum.pine64.org/showthread.php?tid=13181)
- [Renze Nicolai's GitHub repository investigating this chip](https://github.com/renzenicolai/uboot-pine64-clusterboard-instructions/tree/master/Ethernet%20switch)
