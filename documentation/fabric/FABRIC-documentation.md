# FABRIC

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| FABRIC | l3leaf | dc1-borderleaf1a | 192.168.1.7/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc1-borderleaf1b | 192.168.1.8/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc1-leaf1a | 192.168.1.3/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc1-leaf1b | 192.168.1.4/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc1-leaf2a | 192.168.1.5/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc1-leaf2b | 192.168.1.6/24 | vEOS-lab | Provisioned | - |
| FABRIC | spine | dc1-spine1 | 192.168.1.1/24 | vEOS-lab | Provisioned | - |
| FABRIC | spine | dc1-spine2 | 192.168.1.2/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc2-borderleaf1a | 192.168.1.17/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc2-borderleaf1b | 192.168.1.18/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc2-leaf1a | 192.168.1.13/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc2-leaf1b | 192.168.1.14/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc2-leaf2a | 192.168.1.15/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc2-leaf2b | 192.168.1.16/24 | vEOS-lab | Provisioned | - |
| FABRIC | spine | dc2-spine1 | 192.168.1.11/24 | vEOS-lab | Provisioned | - |
| FABRIC | spine | dc2-spine2 | 192.168.1.12/24 | vEOS-lab | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | dc1-borderleaf1a | Ethernet1 | spine | dc1-spine1 | Ethernet5 |
| l3leaf | dc1-borderleaf1a | Ethernet2 | spine | dc1-spine2 | Ethernet5 |
| l3leaf | dc1-borderleaf1a | Ethernet3 | mlag_peer | dc1-borderleaf1b | Ethernet3 |
| l3leaf | dc1-borderleaf1a | Ethernet4 | mlag_peer | dc1-borderleaf1b | Ethernet4 |
| l3leaf | dc1-borderleaf1a | Ethernet7 | l3leaf | dc2-borderleaf1a | Ethernet7 |
| l3leaf | dc1-borderleaf1b | Ethernet1 | spine | dc1-spine1 | Ethernet6 |
| l3leaf | dc1-borderleaf1b | Ethernet2 | spine | dc1-spine2 | Ethernet6 |
| l3leaf | dc1-borderleaf1b | Ethernet8 | l3leaf | dc2-borderleaf1b | Ethernet8 |
| l3leaf | dc1-leaf1a | Ethernet1 | spine | dc1-spine1 | Ethernet1 |
| l3leaf | dc1-leaf1a | Ethernet2 | spine | dc1-spine2 | Ethernet1 |
| l3leaf | dc1-leaf1a | Ethernet3 | mlag_peer | dc1-leaf1b | Ethernet3 |
| l3leaf | dc1-leaf1a | Ethernet4 | mlag_peer | dc1-leaf1b | Ethernet4 |
| l3leaf | dc1-leaf1b | Ethernet1 | spine | dc1-spine1 | Ethernet2 |
| l3leaf | dc1-leaf1b | Ethernet2 | spine | dc1-spine2 | Ethernet2 |
| l3leaf | dc1-leaf2a | Ethernet1 | spine | dc1-spine1 | Ethernet3 |
| l3leaf | dc1-leaf2a | Ethernet2 | spine | dc1-spine2 | Ethernet3 |
| l3leaf | dc1-leaf2a | Ethernet3 | mlag_peer | dc1-leaf2b | Ethernet3 |
| l3leaf | dc1-leaf2a | Ethernet4 | mlag_peer | dc1-leaf2b | Ethernet4 |
| l3leaf | dc1-leaf2b | Ethernet1 | spine | dc1-spine1 | Ethernet4 |
| l3leaf | dc1-leaf2b | Ethernet2 | spine | dc1-spine2 | Ethernet4 |
| l3leaf | dc2-borderleaf1a | Ethernet1 | spine | dc2-spine1 | Ethernet5 |
| l3leaf | dc2-borderleaf1a | Ethernet2 | spine | dc2-spine2 | Ethernet5 |
| l3leaf | dc2-borderleaf1a | Ethernet3 | mlag_peer | dc2-borderleaf1b | Ethernet3 |
| l3leaf | dc2-borderleaf1a | Ethernet4 | mlag_peer | dc2-borderleaf1b | Ethernet4 |
| l3leaf | dc2-borderleaf1b | Ethernet1 | spine | dc2-spine1 | Ethernet6 |
| l3leaf | dc2-borderleaf1b | Ethernet2 | spine | dc2-spine2 | Ethernet6 |
| l3leaf | dc2-leaf1a | Ethernet1 | spine | dc2-spine1 | Ethernet1 |
| l3leaf | dc2-leaf1a | Ethernet2 | spine | dc2-spine2 | Ethernet1 |
| l3leaf | dc2-leaf1a | Ethernet3 | mlag_peer | dc2-leaf1b | Ethernet3 |
| l3leaf | dc2-leaf1a | Ethernet4 | mlag_peer | dc2-leaf1b | Ethernet4 |
| l3leaf | dc2-leaf1b | Ethernet1 | spine | dc2-spine1 | Ethernet2 |
| l3leaf | dc2-leaf1b | Ethernet2 | spine | dc2-spine2 | Ethernet2 |
| l3leaf | dc2-leaf2a | Ethernet1 | spine | dc2-spine1 | Ethernet3 |
| l3leaf | dc2-leaf2a | Ethernet2 | spine | dc2-spine2 | Ethernet3 |
| l3leaf | dc2-leaf2a | Ethernet3 | mlag_peer | dc2-leaf2b | Ethernet3 |
| l3leaf | dc2-leaf2a | Ethernet4 | mlag_peer | dc2-leaf2b | Ethernet4 |
| l3leaf | dc2-leaf2b | Ethernet1 | spine | dc2-spine1 | Ethernet4 |
| l3leaf | dc2-leaf2b | Ethernet2 | spine | dc2-spine2 | Ethernet4 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.254.0.0/27 | 32 | 24 | 75.0 % |
| 10.255.0.0/27 | 32 | 24 | 75.0 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| dc1-borderleaf1a | Ethernet1 | 10.254.0.17/31 | dc1-spine1 | Ethernet5 | 10.254.0.16/31 |
| dc1-borderleaf1a | Ethernet2 | 10.254.0.19/31 | dc1-spine2 | Ethernet5 | 10.254.0.18/31 |
| dc1-borderleaf1a | Ethernet7 | 172.100.100.0/31 | dc2-borderleaf1a | Ethernet7 | 172.100.100.1/31 |
| dc1-borderleaf1b | Ethernet1 | 10.254.0.21/31 | dc1-spine1 | Ethernet6 | 10.254.0.20/31 |
| dc1-borderleaf1b | Ethernet2 | 10.254.0.23/31 | dc1-spine2 | Ethernet6 | 10.254.0.22/31 |
| dc1-borderleaf1b | Ethernet8 | 172.100.100.2/31 | dc2-borderleaf1b | Ethernet8 | 172.100.100.3/31 |
| dc1-leaf1a | Ethernet1 | 10.254.0.1/31 | dc1-spine1 | Ethernet1 | 10.254.0.0/31 |
| dc1-leaf1a | Ethernet2 | 10.254.0.3/31 | dc1-spine2 | Ethernet1 | 10.254.0.2/31 |
| dc1-leaf1b | Ethernet1 | 10.254.0.5/31 | dc1-spine1 | Ethernet2 | 10.254.0.4/31 |
| dc1-leaf1b | Ethernet2 | 10.254.0.7/31 | dc1-spine2 | Ethernet2 | 10.254.0.6/31 |
| dc1-leaf2a | Ethernet1 | 10.254.0.9/31 | dc1-spine1 | Ethernet3 | 10.254.0.8/31 |
| dc1-leaf2a | Ethernet2 | 10.254.0.11/31 | dc1-spine2 | Ethernet3 | 10.254.0.10/31 |
| dc1-leaf2b | Ethernet1 | 10.254.0.13/31 | dc1-spine1 | Ethernet4 | 10.254.0.12/31 |
| dc1-leaf2b | Ethernet2 | 10.254.0.15/31 | dc1-spine2 | Ethernet4 | 10.254.0.14/31 |
| dc2-borderleaf1a | Ethernet1 | 10.255.0.17/31 | dc2-spine1 | Ethernet5 | 10.255.0.16/31 |
| dc2-borderleaf1a | Ethernet2 | 10.255.0.19/31 | dc2-spine2 | Ethernet5 | 10.255.0.18/31 |
| dc2-borderleaf1b | Ethernet1 | 10.255.0.21/31 | dc2-spine1 | Ethernet6 | 10.255.0.20/31 |
| dc2-borderleaf1b | Ethernet2 | 10.255.0.23/31 | dc2-spine2 | Ethernet6 | 10.255.0.22/31 |
| dc2-leaf1a | Ethernet1 | 10.255.0.1/31 | dc2-spine1 | Ethernet1 | 10.255.0.0/31 |
| dc2-leaf1a | Ethernet2 | 10.255.0.3/31 | dc2-spine2 | Ethernet1 | 10.255.0.2/31 |
| dc2-leaf1b | Ethernet1 | 10.255.0.5/31 | dc2-spine1 | Ethernet2 | 10.255.0.4/31 |
| dc2-leaf1b | Ethernet2 | 10.255.0.7/31 | dc2-spine2 | Ethernet2 | 10.255.0.6/31 |
| dc2-leaf2a | Ethernet1 | 10.255.0.9/31 | dc2-spine1 | Ethernet3 | 10.255.0.8/31 |
| dc2-leaf2a | Ethernet2 | 10.255.0.11/31 | dc2-spine2 | Ethernet3 | 10.255.0.10/31 |
| dc2-leaf2b | Ethernet1 | 10.255.0.13/31 | dc2-spine1 | Ethernet4 | 10.255.0.12/31 |
| dc2-leaf2b | Ethernet2 | 10.255.0.15/31 | dc2-spine2 | Ethernet4 | 10.255.0.14/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 1.1.1.0/27 | 32 | 8 | 25.0 % |
| 3.3.3.0/27 | 32 | 8 | 25.0 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| FABRIC | dc1-borderleaf1a | 1.1.1.7/32 |
| FABRIC | dc1-borderleaf1b | 1.1.1.8/32 |
| FABRIC | dc1-leaf1a | 1.1.1.3/32 |
| FABRIC | dc1-leaf1b | 1.1.1.4/32 |
| FABRIC | dc1-leaf2a | 1.1.1.5/32 |
| FABRIC | dc1-leaf2b | 1.1.1.6/32 |
| FABRIC | dc1-spine1 | 1.1.1.1/32 |
| FABRIC | dc1-spine2 | 1.1.1.2/32 |
| FABRIC | dc2-borderleaf1a | 3.3.3.7/32 |
| FABRIC | dc2-borderleaf1b | 3.3.3.8/32 |
| FABRIC | dc2-leaf1a | 3.3.3.3/32 |
| FABRIC | dc2-leaf1b | 3.3.3.4/32 |
| FABRIC | dc2-leaf2a | 3.3.3.5/32 |
| FABRIC | dc2-leaf2b | 3.3.3.6/32 |
| FABRIC | dc2-spine1 | 3.3.3.1/32 |
| FABRIC | dc2-spine2 | 3.3.3.2/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 2.2.2.0/27 | 32 | 6 | 18.75 % |
| 4.4.4.0/27 | 32 | 6 | 18.75 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| FABRIC | dc1-borderleaf1a | 2.2.2.7/32 |
| FABRIC | dc1-borderleaf1b | 2.2.2.7/32 |
| FABRIC | dc1-leaf1a | 2.2.2.3/32 |
| FABRIC | dc1-leaf1b | 2.2.2.3/32 |
| FABRIC | dc1-leaf2a | 2.2.2.5/32 |
| FABRIC | dc1-leaf2b | 2.2.2.5/32 |
| FABRIC | dc2-borderleaf1a | 4.4.4.7/32 |
| FABRIC | dc2-borderleaf1b | 4.4.4.7/32 |
| FABRIC | dc2-leaf1a | 4.4.4.3/32 |
| FABRIC | dc2-leaf1b | 4.4.4.3/32 |
| FABRIC | dc2-leaf2a | 4.4.4.5/32 |
| FABRIC | dc2-leaf2b | 4.4.4.5/32 |
