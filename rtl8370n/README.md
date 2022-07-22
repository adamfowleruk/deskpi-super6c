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
- Setting up a Ceph cluster on the same nodes as the K8S cluster, and prioritising storage VLAN traffic

## Further reading

- [Pine64 discussion thread on this chip (for a different board)](https://forum.pine64.org/showthread.php?tid=13181)
- [Renze Nicolai's GitHub repository investigating this chip](https://github.com/renzenicolai/uboot-pine64-clusterboard-instructions/tree/master/Ethernet%20switch)
