# group_vars usage

## FABRIC.yml
*** General fabric config accross both DCs ***
- Local username
- Underlay and overlay routing protocol to EBGP
- BGP peer groups passwords
- P2P links MTU
- DCI ports between the border leafs

## DC1_FABRIC.yml
- Building the DC1 underlay & overlay fabric 
- Config on the border leafs related to evpn_gateway

## DC2_FABRIC.yml -> needed?

## EVPN_SERVICES_DC1.yml
## EVPN_SERVICES_DC2.yml
- Defining VRFs and VLANs, L2VNIs, L3VNIs

## DC1_BORDER_LEAFS.yml
## DC2_BORDER_LEAFS.yml
- Specific border-leaf config (DWDM, connections to firewalls, etc)

## CONNECTED_SERVERS_DC1.yml
## CONNECTED_SERVERS_DC2.yml
- Endpoint connections for each server




