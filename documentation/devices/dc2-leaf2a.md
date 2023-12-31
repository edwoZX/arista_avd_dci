# dc2-leaf2a

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router General](#router-general)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 192.168.1.15/24 | 192.168.1.254 |

##### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 192.168.1.15/24
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

#### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| admin | 15 | network-admin | False | - |
| ansible | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin nopassword
username ansible privilege 15 role network-admin secret sha512 <removed>
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 192.168.1.200:9910 | MGMT | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=192.168.1.200:9910 -cvauth=token,/tmp/token -cvvrf=MGMT -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| DC2_L3_LEAF2 | Vlan4094 | 10.255.1.5 | Port-Channel3 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id DC2_L3_LEAF2
   local-interface Vlan4094
   peer-address 10.255.1.5
   peer-link Port-Channel3
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 4096
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 200 | A_DC2_VLAN200 | - |
| 210 | B_DC2_VLAN210 | - |
| 220 | C_DC2_VLAN220 | - |
| 3099 | MLAG_iBGP_A_DC2 | LEAF_PEER_L3 |
| 3109 | MLAG_iBGP_B_DC2 | LEAF_PEER_L3 |
| 3219 | MLAG_iBGP_C_DC2 | LEAF_PEER_L3 |
| 4093 | LEAF_PEER_L3 | LEAF_PEER_L3 |
| 4094 | MLAG_PEER | MLAG |

### VLANs Device Configuration

```eos
!
vlan 200
   name A_DC2_VLAN200
!
vlan 210
   name B_DC2_VLAN210
!
vlan 220
   name C_DC2_VLAN220
!
vlan 3099
   name MLAG_iBGP_A_DC2
   trunk group LEAF_PEER_L3
!
vlan 3109
   name MLAG_iBGP_B_DC2
   trunk group LEAF_PEER_L3
!
vlan 3219
   name MLAG_iBGP_C_DC2
   trunk group LEAF_PEER_L3
!
vlan 4093
   name LEAF_PEER_L3
   trunk group LEAF_PEER_L3
!
vlan 4094
   name MLAG_PEER
   trunk group MLAG
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet3 | MLAG_PEER_dc2-leaf2b_Ethernet3 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 3 |
| Ethernet4 | MLAG_PEER_dc2-leaf2b_Ethernet4 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 3 |
| Ethernet8 | DC1_HOST2_ALL_VLANS_G0/0 | *trunk | *200,210,220 | *4092 | *- | 8 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_LINK_TO_DC2-SPINE1_Ethernet3 | routed | - | 10.255.0.9/31 | default | 1500 | False | - | - |
| Ethernet2 | P2P_LINK_TO_DC2-SPINE2_Ethernet3 | routed | - | 10.255.0.11/31 | default | 1500 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_LINK_TO_DC2-SPINE1_Ethernet3
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.0.9/31
!
interface Ethernet2
   description P2P_LINK_TO_DC2-SPINE2_Ethernet3
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.0.11/31
!
interface Ethernet3
   description MLAG_PEER_dc2-leaf2b_Ethernet3
   no shutdown
   channel-group 3 mode active
!
interface Ethernet4
   description MLAG_PEER_dc2-leaf2b_Ethernet4
   no shutdown
   channel-group 3 mode active
!
interface Ethernet8
   description DC1_HOST2_ALL_VLANS_G0/0
   no shutdown
   channel-group 8 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel3 | MLAG_PEER_dc2-leaf2b_Po3 | switched | trunk | - | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |
| Port-Channel8 | DC1_HOST2_ALL_VLANS_PortChannel DC2_HOST2_ALL_VLANS | switched | trunk | 200,210,220 | 4092 | - | - | - | 8 | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel3
   description MLAG_PEER_dc2-leaf2b_Po3
   no shutdown
   switchport
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
!
interface Port-Channel8
   description DC1_HOST2_ALL_VLANS_PortChannel DC2_HOST2_ALL_VLANS
   no shutdown
   switchport
   switchport trunk allowed vlan 200,210,220
   switchport trunk native vlan 4092
   switchport mode trunk
   mlag 8
   spanning-tree portfast
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 3.3.3.5/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 4.4.4.5/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |


#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 3.3.3.5/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 4.4.4.5/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan200 | A_DC2_VLAN200 | A_DC2 | - | False |
| Vlan210 | B_DC2_VLAN210 | B_DC2 | - | False |
| Vlan220 | C_DC2_VLAN220 | C_DC2 | - | False |
| Vlan3099 | MLAG_PEER_L3_iBGP: vrf A_DC2 | A_DC2 | 1500 | False |
| Vlan3109 | MLAG_PEER_L3_iBGP: vrf B_DC2 | B_DC2 | 1500 | False |
| Vlan3219 | MLAG_PEER_L3_iBGP: vrf C_DC2 | C_DC2 | 1500 | False |
| Vlan4093 | MLAG_PEER_L3_PEERING | default | 1500 | False |
| Vlan4094 | MLAG_PEER | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan200 |  A_DC2  |  -  |  10.200.0.1/24  |  -  |  -  |  -  |  -  |
| Vlan210 |  B_DC2  |  -  |  10.210.0.1/24  |  -  |  -  |  -  |  -  |
| Vlan220 |  C_DC2  |  -  |  10.220.0.1/24  |  -  |  -  |  -  |  -  |
| Vlan3099 |  A_DC2  |  10.255.2.4/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan3109 |  B_DC2  |  10.255.2.4/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan3219 |  C_DC2  |  10.255.2.4/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.255.2.4/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.255.1.4/31  |  -  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan200
   description A_DC2_VLAN200
   no shutdown
   vrf A_DC2
   ip address virtual 10.200.0.1/24
!
interface Vlan210
   description B_DC2_VLAN210
   no shutdown
   vrf B_DC2
   ip address virtual 10.210.0.1/24
!
interface Vlan220
   description C_DC2_VLAN220
   no shutdown
   vrf C_DC2
   ip address virtual 10.220.0.1/24
!
interface Vlan3099
   description MLAG_PEER_L3_iBGP: vrf A_DC2
   no shutdown
   mtu 1500
   vrf A_DC2
   ip address 10.255.2.4/31
!
interface Vlan3109
   description MLAG_PEER_L3_iBGP: vrf B_DC2
   no shutdown
   mtu 1500
   vrf B_DC2
   ip address 10.255.2.4/31
!
interface Vlan3219
   description MLAG_PEER_L3_iBGP: vrf C_DC2
   no shutdown
   mtu 1500
   vrf C_DC2
   ip address 10.255.2.4/31
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   no shutdown
   mtu 1500
   ip address 10.255.2.4/31
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 1500
   no autostate
   ip address 10.255.1.4/31
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 200 | 20200 | - | - |
| 210 | 20210 | - | - |
| 220 | 20220 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| A_DC2 | 100 | - |
| B_DC2 | 110 | - |
| C_DC2 | 220 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description dc2-leaf2a_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 200 vni 20200
   vxlan vlan 210 vni 20210
   vxlan vlan 220 vni 20220
   vxlan vrf A_DC2 vni 100
   vxlan vrf B_DC2 vni 110
   vxlan vrf C_DC2 vni 220
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

##### Virtual Router MAC Address: 00:1c:73:00:00:10

#### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:00:10
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| A_DC2 | True |
| B_DC2 | True |
| C_DC2 | True |
| MGMT | False |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf A_DC2
ip routing vrf B_DC2
ip routing vrf C_DC2
no ip routing vrf MGMT
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| A_DC2 | false |
| B_DC2 | false |
| C_DC2 | false |
| MGMT | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| MGMT | 0.0.0.0/0 | 192.168.1.254 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 192.168.1.254
```

### Router General

#### VRF Route leaking

| VRF | Source VRF | Route Map Policy |
|-----|------------|------------------|
| A_DC2 | C_DC2 | RM_LEAK_ROUTES_C_DC2 |
| B_DC2 | C_DC2 | RM_LEAK_ROUTES_C_DC2 |
| C_DC2 | A_DC2 | RM_LEAK_ROUTES_A_DC2 |
| C_DC2 | B_DC2 | RM_LEAK_ROUTES_B_DC2 |

#### Router General configuration

```eos
!
router general
   vrf A_DC2
      leak routes source-vrf C_DC2 subscribe-policy RM_LEAK_ROUTES_C_DC2
      exit
   !
   vrf B_DC2
      leak routes source-vrf C_DC2 subscribe-policy RM_LEAK_ROUTES_C_DC2
      exit
   !
   vrf C_DC2
      leak routes source-vrf A_DC2 subscribe-policy RM_LEAK_ROUTES_A_DC2
      leak routes source-vrf B_DC2 subscribe-policy RM_LEAK_ROUTES_B_DC2
      exit
   !
   exit
```

### Router BGP

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65202|  3.3.3.5 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

##### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65202 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- |
| 3.3.3.1 | 65002 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 3.3.3.2 | 65002 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 10.255.0.8 | 65002 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.255.0.10 | 65002 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.255.2.5 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - |
| 10.255.2.5 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | A_DC2 | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - |
| 10.255.2.5 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | B_DC2 | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - |
| 10.255.2.5 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | C_DC2 | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Encapsulation |
| ---------- | -------- | ------------- |
| EVPN-OVERLAY-PEERS | True | default |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 200 | 3.3.3.5:20200 | 20200:20200 | - | - | learned |
| 210 | 3.3.3.5:20210 | 20210:20210 | - | - | learned |
| 220 | 3.3.3.5:20220 | 20220:20220 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| A_DC2 | 3.3.3.5:100 | connected |
| B_DC2 | 3.3.3.5:110 | connected |
| C_DC2 | 3.3.3.5:220 | connected |

#### Router BGP Device Configuration

```eos
!
router bgp 65202
   router-id 3.3.3.5
   maximum-paths 4 ecmp 4
   no bgp default ipv4-unicast
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS password 7 <removed>
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 <removed>
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65202
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description dc2-leaf2b
   neighbor MLAG-IPv4-UNDERLAY-PEER password 7 <removed>
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor 3.3.3.1 peer group EVPN-OVERLAY-PEERS
   neighbor 3.3.3.1 remote-as 65002
   neighbor 3.3.3.1 description dc2-spine1
   neighbor 3.3.3.2 peer group EVPN-OVERLAY-PEERS
   neighbor 3.3.3.2 remote-as 65002
   neighbor 3.3.3.2 description dc2-spine2
   neighbor 10.255.0.8 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.0.8 remote-as 65002
   neighbor 10.255.0.8 description dc2-spine1_Ethernet3
   neighbor 10.255.0.10 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.0.10 remote-as 65002
   neighbor 10.255.0.10 description dc2-spine2_Ethernet3
   neighbor 10.255.2.5 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.255.2.5 description dc2-leaf2b
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 200
      rd 3.3.3.5:20200
      route-target both 20200:20200
      redistribute learned
   !
   vlan 210
      rd 3.3.3.5:20210
      route-target both 20210:20210
      redistribute learned
   !
   vlan 220
      rd 3.3.3.5:20220
      route-target both 20220:20220
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf A_DC2
      rd 3.3.3.5:100
      route-target import evpn 100:100
      route-target export evpn 100:100
      router-id 3.3.3.5
      neighbor 10.255.2.5 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
   !
   vrf B_DC2
      rd 3.3.3.5:110
      route-target import evpn 110:110
      route-target export evpn 110:110
      router-id 3.3.3.5
      neighbor 10.255.2.5 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
   !
   vrf C_DC2
      rd 3.3.3.5:220
      route-target import evpn 220:220
      route-target export evpn 220:220
      router-id 3.3.3.5
      neighbor 10.255.2.5 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 1200 | 1200 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 1200 min-rx 1200 multiplier 3
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 3.3.3.0/27 eq 32 |
| 20 | permit 4.4.4.0/27 eq 32 |

##### PL_LEAK_ROUTES_A_DC2

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.200.0.0/24 |

##### PL_LEAK_ROUTES_B_DC2

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.210.0.0/24 |

##### PL_LEAK_ROUTES_C_DC2

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.220.0.0/24 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 3.3.3.0/27 eq 32
   seq 20 permit 4.4.4.0/27 eq 32
!
ip prefix-list PL_LEAK_ROUTES_A_DC2
   seq 10 permit 10.200.0.0/24
!
ip prefix-list PL_LEAK_ROUTES_B_DC2
   seq 10 permit 10.210.0.0/24
!
ip prefix-list PL_LEAK_ROUTES_C_DC2
   seq 10 permit 10.220.0.0/24
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

##### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

##### RM_LEAK_ROUTES_A_DC2

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL_LEAK_ROUTES_A_DC2 | - | - | - |

##### RM_LEAK_ROUTES_B_DC2

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL_LEAK_ROUTES_B_DC2 | - | - | - |

##### RM_LEAK_ROUTES_C_DC2

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL_LEAK_ROUTES_C_DC2 | - | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
!
route-map RM_LEAK_ROUTES_A_DC2 permit 10
   match ip address prefix-list PL_LEAK_ROUTES_A_DC2
!
route-map RM_LEAK_ROUTES_B_DC2 permit 10
   match ip address prefix-list PL_LEAK_ROUTES_B_DC2
!
route-map RM_LEAK_ROUTES_C_DC2 permit 10
   match ip address prefix-list PL_LEAK_ROUTES_C_DC2
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| A_DC2 | enabled |
| B_DC2 | enabled |
| C_DC2 | enabled |
| MGMT | disabled |

### VRF Instances Device Configuration

```eos
!
vrf instance A_DC2
!
vrf instance B_DC2
!
vrf instance C_DC2
!
vrf instance MGMT
```
