# Small Enterprise Network Infrastructure

A small enterprise network designed and implemented using Cisco Packet Tracer.

The project demonstrates VLAN segmentation, 802.1Q trunking, Router-on-a-Stick inter-VLAN routing, DHCP, NAT/PAT, DNS, HTTP services, and systematic network troubleshooting.

## Network Topology

           <img width="519" height="520" alt="image" src="https://github.com/user-attachments/assets/a1232e8d-3ae3-4006-aeec-596f25d74006" />


## Network Design

| VLAN | Department | Network | Gateway |
|------|------------|---------|---------|
| 10 | Finance | 192.168.10.0/24 | 192.168.10.1 |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 |

### R1 Provides

- Inter-VLAN routing
- DHCP
- NAT/PAT
- Default routing

### External Server-PT Provides

- DNS
- HTTP

### DNS

server.company.local → 203.0.113.6

## Technologies

- Cisco Packet Tracer
- Cisco IOS
- VLAN
- IEEE 802.1Q
- Router-on-a-Stick
- DHCP
- NAT/PAT
- IPv4
- Static/default routing
- DNS
- HTTP
- Network troubleshooting

## Verification

The completed network was verified through:

- VLAN membership
- 802.1Q trunk status
- Router interface status
- Routing table
- DHCP bindings
- NAT translations
- Gateway connectivity
- Server connectivity
- DNS resolution
- HTTP access

### Example

PC-IT-01
   ↓
192.168.20.1       ✓ Gateway
   ↓
203.0.113.6        ✓ Server
   ↓
server.company.local  ✓ DNS
   ↓
HTTP Server        ✓

## Troubleshooting

Troubleshooting followed an evidence-based workflow:

Symptom
   ↓
Collect Evidence
   ↓
Form Hypothesis
   ↓
Test
   ↓
Identify Root Cause
   ↓
Apply Fix
   ↓
Verify

Examples documented in this project include:

- Incorrect VLAN assignment
- DNS configuration issue
- End-to-end connectivity investigation

See documentation/troubleshooting.md for details.

## Repository Structure

├── README.md
├── small-enterprise-network.pkt
├── topology/
├── configs/
├── documentation/
└── screenshots/

## Skills Demonstrated

- IPv4 addressing and subnetting
- VLAN segmentation
- Switching and trunking
- Inter-VLAN routing
- DHCP
- NAT/PAT
- Routing
- DNS and HTTP
- Cisco IOS CLI
- Network troubleshooting
- Technical documentation

## Future Improvements

Potential extensions:

- SSH device management
- Port security
- DHCP snooping
- Dedicated management VLAN
- Network redundancy
- Dynamic routing
- Network monitoring

## Project Status

**Completed**

Detailed addressing, VLAN design, configurations, verification evidence, and troubleshooting documentation are available in the repository.
