# Ceph shared storage on the DeskPi Super6C

Having an SSD slot per node, and up to 6 nodes, opens up the possiibility
of a great "Poor man's Synology" storage solution, both for general
household use and for Kubernetes persistent volumes.

I plan to install Ceph on each node and create a single shared storage
cluster.

## Storage projects

- Default OOTB performance of the SSDs with Linux
- A basic Ceph cluster set up
- Shared (sharded) store performance with Ceph from outside the Super6C board
- Shared storage for Kubernetes
- Telling k8s to default to the local (same node) Ceph store, and see if that improves performance
- See if a 3 node Ceph cluster and a 3 node k8s cluster performs better or worse
- Prioritising shared storage traffic - [See the RTL8370N README too](../rtl8370n/README.md)
- Act as a camera transcoding endpoint (k8s service) saving video files to storage (Ceph)