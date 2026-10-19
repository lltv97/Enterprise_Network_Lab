# Enterprise Network Design & Troubleshooting Lab

**Type:** Personal networking lab  
**Environment:** PNETLab, Cisco IOS, Ubuntu Server 24.04, Nginx  
**Focus:** Routing & switching, redundancy, access control, and troubleshooting

## 1. Project Overview

This project simulates a small enterprise network with multiple VLANs, redundant gateways, redundant WAN paths, and an internal web server. It was built as a hands-on learning project while preparing for Cisco CCNA and practicing network troubleshooting.

The lab focuses on configuration and verification rather than production deployment.
## Network Topology

![Enterprise Network Topology](images/enterprise-network-topology.png)

## Key Implementations

- **VLANs and Inter-VLAN Routing:** Segmented the network into Accounting, HR, IT, Guest, and Server VLANs.
- **VTP and Trunking:** Configured VLAN propagation and trunk links between switches.
- **EtherChannel:** Bundled switch links using LACP for link redundancy.
- **VRRP:** Configured redundant default gateways across VLANs.
- **Routing:** Implemented static routing and OSPF.
- **WAN Redundancy:** Configured dual ISP paths, floating static routes, IP SLA, and object tracking.
- **DHCP:** Configured dynamic IP address assignment for user VLANs.
- **Extended ACLs:** Restricted inter-VLAN traffic and controlled access to the internal web server.
- **Linux Web Server:** Deployed Ubuntu Server 24.04 with Nginx.

## Testing and Verification

- Verified DHCP address assignment for client VLANs.
- Tested inter-VLAN access policies using ping and TCP port checks.
- Verified HTTP access to the internal web server on TCP port 80.
- Tested VRRP gateway failover.
- Tested WAN failover using IP SLA and floating static routes.
- Used Cisco `show` commands and ACL counters to troubleshoot connectivity.

## Troubleshooting Experience

- **VRRP Failover:** Verified gateway availability after changing the active router state.
- **WAN Failover:** Simulated primary ISP failure and verified backup routing using IP SLA and floating static routes.
- **ACL Troubleshooting:** Used ACL hit counters and connectivity tests to identify permitted and denied traffic.
- **Server Connectivity:** Verified HTTP access to the Ubuntu Nginx server and investigated routing and access-control issues.
- **Return-Path Analysis:** Learned to check both forward and return traffic paths when troubleshooting connectivity across VLANs.

## Tools and Technologies

- Cisco IOS
- PNETLab
- Ubuntu Server 24.04
- Nginx
- VLAN, VTP, 802.1Q trunking, LACP
- VRRP, OSPF, static routing, IP SLA
- DHCP, Extended ACLs

## 2. Objectives

- Segment users and servers into separate VLANs.
- Configure inter-VLAN routing and DHCP.
- Provide gateway redundancy with VRRP.
- Practice routing and WAN failover with OSPF, IP SLA tracking, and floating static routes.
- Apply extended ACLs to restrict inter-VLAN traffic and control access to the internal web server.
- Configure an Ubuntu Server running Nginx and verify HTTP connectivity.
- Troubleshoot connectivity using Cisco IOS show commands, ACL counters, and end-to-end tests.

## 3. VLAN and IP Addressing Plan

| VLAN | Purpose | Subnet | Default Gateway (VRRP VIP) |
|---|---|---|---|
| 10 | Accounting | `192.168.10.0/24` | `192.168.10.1` |
| 20 | HR | `192.168.20.0/24` | `192.168.20.1` |
| 30 | IT | `192.168.30.0/24` | `192.168.30.1` |
| 40 | Guest | `192.168.40.0/24` | `192.168.40.1` |
| 50 | Server | `192.168.50.0/24` | `192.168.50.1` |

**Ubuntu Web Server:** `192.168.50.10/24`  
**WAN/transit links:** Separate point-to-point subnets are used between routers and ISP simulation routers.

## 4. Technologies Practiced

- **Layer 2:** VLANs, 802.1Q trunking, STP/RSTP, LACP EtherChannel, port security, BPDU Guard, VTP.
- **Layer 3:** Router-on-a-Stick, OSPF, static/default/floating static routes, VRRP.
- **Network services:** DHCP, IP SLA tracking.
- **Traffic control:** Extended ACLs for inter-VLAN restrictions and web-server access.
- **Linux services:** Ubuntu Server and Nginx HTTP service.
- **Verification:** `show ip interface brief`, `show ip route`, `show vrrp brief`, `show access-lists`, `show etherchannel summary`, `ping`, and TCP port testing.

## 5. Access-Control Policy

The lab uses extended ACLs to practice least-privilege access between VLANs. Examples include:

- Guest traffic is restricted from reaching internal user and server VLANs while internet-simulation access is tested.
- Accounting and HR are allowed to access the Ubuntu server over HTTP/HTTPS by ACL policy; other traffic to the server subnet is restricted according to the configured rules.
- IT is allowed to reach the Ubuntu server on TCP ports 80 and 443, while other traffic from IT to the server subnet is denied by the lab ACL.

**Note:** ACLs are stateless. Return traffic and management protocols must be considered when designing and testing policies. HTTPS access was permitted by ACL, but the lab verification confirmed the Nginx HTTP service on TCP port 80; HTTPS service operation was not confirmed.

## 6. Validation and Troubleshooting

The following behaviors were tested during the lab:

- DHCP address assignment for client VLANs.
- Inter-VLAN reachability according to the ACL policy.
- HTTP access from an authorized client VLAN to `192.168.50.10`.
- Blocking of selected traffic using extended ACLs and verification of ACL hit counters.
- VRRP gateway failover when the primary router's LAN interface path was taken down.
- WAN path failover using IP SLA tracking and floating static routing.
- Connectivity to the simulated ISP loopback destinations.

### Example troubleshooting workflow

1. Check interface and protocol state with `show ip interface brief`.
2. Check gateway redundancy state with `show vrrp brief`.
3. Inspect route selection with `show ip route`.
4. Check ACL order and hit counters with `show access-lists`.
5. Test gateway, server, and WAN reachability separately.
6. Compare permitted TCP traffic with blocked ICMP traffic to distinguish policy filtering from service failure.

## 7. Key Learning Outcomes

- Practiced combining routing, switching, redundancy, and ACLs in one multi-VLAN topology.
- Learned to verify failover behavior rather than relying on configuration alone.
- Investigated how independent VRRP groups can have different Master routers and how this may affect return paths.
- Used ACL counters and targeted connectivity tests to narrow down network issues.

## 8. Lab Scope and Limitations

- This is a personal simulation lab, not a production enterprise deployment.
- The ISP routers and public-looking loopback addresses are simulated inside the lab.
- HTTP was tested on TCP port 80. HTTPS service operation was not verified.
- Configuration details may vary depending on the Cisco IOS image and PNETLab node type.

## 9. Suggested Evidence to Add

Add your own screenshots before publishing:

- Full PNETLab topology.
- `show etherchannel summary`
- `show vrrp brief` before and during failover.
- `show ip route` before and after WAN failover.
- `show access-lists` showing relevant ACL counters.
- Successful TCP/80 test from an authorized client.

## 10. Disclaimer

All addresses, devices, and services in this repository are used for an isolated educational lab. Do not upload passwords, private keys, real customer configurations, or sensitive information.

## VRRP Gateway Redundancy Verification

VRRP was configured to provide redundant default gateways across multiple VLANs.

**Test scenario:** The R1 subinterface for VLAN 10 was shut down to simulate an interface failure.

**Expected result:** R2 transitions from Backup to Master for VLAN 10 while retaining the virtual gateway address `192.168.10.1`.

**Result:** Successful. The `show vrrp brief` output confirmed that R2 became Master for VLAN 10.

### Before Failover

![VRRP before failover](images/verification/vrrp-before-failover-r1-r2.png)

### After Failover

![VRRP after failover](images/verification/vrrp-after-failover.png)
