# VLAN Design

## VLAN Overview

The network uses VLANs to provide logical segmentation between Finance and IT users.

| VLAN | Name | Network | Gateway |
|---|---|---|---|
| 10 | FINANCE | 192.168.10.0/24 | 192.168.10.1 |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 |

## VLAN 10 — Finance

- VLAN ID: `10`
- Name: `FINANCE`
- Network: `192.168.10.0/24`
- Default Gateway: `192.168.10.1`

### Access Ports

| Port | Device |
|---|---|
| Fa0/1 | PC-FIN-01 |
| Fa0/2 | PC-FIN-02 |

## VLAN 20 — IT

- VLAN ID: `20`
- Name: `IT`
- Network: `192.168.20.0/24`
- Default Gateway: `192.168.20.1`

### Access Ports

| Port | Device |
|---|---|
| Fa0/3 | PC-IT-01 |
| Fa0/4 | PC-IT-02 |

## Trunk

SW1 `GigabitEthernet0/1` connects to R1 and operates as an IEEE 802.1Q trunk.

The trunk carries:

- VLAN 10 — FINANCE
- VLAN 20 — IT

## Inter-VLAN Routing

Inter-VLAN communication is provided by R1 using Router-on-a-Stick.

R1 uses two subinterfaces:

```text
G0/0.10
encapsulation dot1Q 10
192.168.10.1/24

```

```text
G0/0.20
encapsulation dot1Q 20
192.168.20.1/24
```
Each subinterface acts as the default gateway for its respective VLAN.

## Design Purpose
VLAN 10 and VLAN 20 provide logical network segmentation between Finance and IT users.

The separation creates distinct Layer 2 broadcast domains while allowing controlled communication between the networks through R1.
