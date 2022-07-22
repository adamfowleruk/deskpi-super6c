# deskpi-super6c
A repo to hold useful information and files for my exploration of the DeskPi Super6C CM4 carrier board.

The DeskPi Super6C is a mini-itx form factor (17cm x 17cm) board that hosts up to 6 compute boards
that are compatible with the RaspberryPi Compute Module 4's (CM4) form factor. These compute
boards can be mixed and matched. Each board can have an SD card and M2 2280 SSD drive on the reverse of
the carrier board. The carrier board also has a gigabit Ethernet switch built in. This makes
the Super6C board a very interesting cluster computing and edge computing board.

![DeskPi Super6C with the SOQuartz board running Manjaro Arm](images/deskpi-super6c-soquartz-manjaro-arm.jpg)

Details on this board can be found here:-
- Manufacturer website - https://deskpi.com/collections/deskpi-super6c/products/deskpi-super6c-raspberry-pi-cm4-cluster-mini-itx-board-6-rpi-cm4-supported
- Geerling Guy's GitHub Issue - https://github.com/geerlingguy/raspberry-pi-pcie-devices/issues/336

I aim to explore this very interesting board and document my findings, ideas, code and links
to external sites on this repository.

## Project ideas

- [Initial node #1 OS installation](soquartz/README.md) and configuration using Manjaro Arm and a Pine64 SOQuartz board and a spare Samsung NVMe SSD
- SSD partitioning - Can I install the OS and boot from the SSD, and use another partition for shared storage? (E.g. for a future Ceph cluster)
- [Realtek RTL8370N chip](rtl8370n/README.md) - Can I segment the network between nodes for 'physical' VLAN separation? Can I prioritise storage VLAN traffic?
- Kubernetes on Arm - Can I get microk8s or similar installed on one or more nodes? (Tanzu K8s on Arm later!)
- [Ceph on Arm](ceph/README.md) - Can I do both K8S and Ceph, so I have a general shared storage service which can also be exposed to K8S for persistent volumes?
- Bang for my buck - Can I expose the SOQuartz's Neural Processing Unit (NPU) and GPU to Linux to speed Machine Learning operations?

## License and Copyright

Any documentation in this repository is distributed under the Creative Common's ShareAlike licenses v4.
All code samples are distributed under the Apache-2.0 license.
Copyright Adam Fowler 2022, all rights reserved.
See individual files for specific copyright statements.
