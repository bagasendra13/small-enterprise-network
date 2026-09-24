# Network Troubleshooting

Troubleshooting was performed using an evidence-based approach rather than immediately changing the configuration.

## Troubleshooting Methodology

```text
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
```

The main goal was to identify the fault domain before making configuration changes.

---

## Scenario 1 — Incorrect VLAN Assignment

### Symptom

The user reported that PC-FIN-01 could not ping its Finance gateway.

### Expected

```text
PC-FIN-01
192.168.10.x
      ↓
VLAN 10
      ↓
192.168.10.1
Finance Gateway
```

### Investigation

The first verification command was:

```cisco
show vlan brief
```

The output showed that Fa0/1 was assigned to VLAN 20 instead of VLAN 10.

### Observed

```text
VLAN 10 → Fa0/2
VLAN 20 → Fa0/1, Fa0/3, Fa0/4
```

### Root Cause

Fa0/1 had an incorrect access VLAN assignment.

The port was configured for VLAN 20 even though PC-FIN-01 belonged to Finance and should have been connected to VLAN 10.

### Resolution

Fa0/1 was assigned back to VLAN 10.

### Verification

PC-FIN-01 was then able to successfully reach:

```text
192.168.10.1
```

### Lesson Learned

`show vlan brief` is an important first-level verification command for identifying incorrect VLAN membership on access ports.

---

## Scenario 8 — DNS Configuration

### Symptom

A PC could reach the server by IP address but could not resolve:

```text
server.company.local
```

### Investigation

The DHCP configuration was inspected using:

```cisco
show running-config | section dhcp
```

The DHCP configuration was initially providing:

```text
dns-server 8.8.8.8
```

### Root Cause

The clients were receiving an external DNS server instead of the DNS server deployed in the lab.

The intended DNS server was:

```text
203.0.113.6
```

### Resolution

The DHCP pools were updated to provide:

```text
dns-server 203.0.113.6
```

The clients then renewed their DHCP leases.

### Verification

The client received:

```text
DNS: 203.0.113.6
```

DNS resolution was tested with:

```text
ping server.company.local
```

The hostname successfully resolved to:

```text
203.0.113.6
```

### Lesson Learned

When troubleshooting hostname connectivity, IP connectivity and DNS resolution should be tested separately.

A successful ping to an IP address does not automatically prove that DNS resolution is working.

---

## Scenario 10 — End-to-End Connectivity Investigation

### Symptom

A user reported that PC-IT-01 could not access:

```text
server.company.local
```

### Investigation

The investigation started at the access layer and moved toward the server.

### 1. VLAN Membership

Command:

```cisco
show vlan brief
```

Finding:

```text
Fa0/3 → VLAN 20
```

This was correct because PC-IT-01 is connected to Fa0/3.

### 2. Switch Port Status

Command:

```cisco
show interfaces status
```

Finding:

```text
Fa0/3 → connected
VLAN 20
```

The access port was operational and correctly assigned.

### 3. Trunk

Command:

```cisco
show interfaces trunk
```

Finding:

```text
G0/1 → trunking
802.1Q
VLAN 20 → allowed
VLAN 20 → active
VLAN 20 → forwarding
```

The VLAN was successfully traversing the trunk between SW1 and R1.

### 4. Router Interface

Command:

```cisco
show ip interface brief
```

Finding:

```text
G0/0.20
192.168.20.1
up/up
```

The VLAN 20 gateway was operational.

### 5. Routing

Command:

```cisco
show ip route
```

The routing table contained:

```text
192.168.20.0/24 → connected
203.0.113.0/30  → connected
0.0.0.0/0       → 203.0.113.1
```

The required routes were present.

---

## End-to-End Testing

### Gateway Connectivity

From PC-IT-01:

```text
ping 192.168.20.1
```

Result:

```text
Successful
```

### Server Connectivity

From PC-IT-01:

```text
ping 203.0.113.6
```

Result:

```text
Successful
```

### DNS Resolution

From PC-IT-01:

```text
ping server.company.local
```

Result:

```text
Successful
```

### HTTP Access

HTTP access was also tested using:

```text
http://203.0.113.6
```

The server web page was successfully displayed.

### Finding

No network fault was identified.

The VLAN, access port, trunk, Router-on-a-Stick gateway, routing, DNS resolution, and HTTP service were functioning correctly.

### Lesson Learned

A troubleshooting engineer should not assume that every reported problem is caused by a network failure.

The correct approach is to collect evidence, test each layer, and determine whether the reported fault can actually be reproduced.

---

## NAT Verification

NAT/PAT was also verified using:

```cisco
show ip nat statistics
show ip nat translations
```

Initially, the translation table contained no entries because there was no active NAT traffic.

After generating traffic from a Finance client to the server, NAT translations appeared.

### Example

| Inside Local | Inside Global |
|---|---|
| 192.168.10.3 | 203.0.113.2 |

This demonstrated that R1 was translating the private client address to the WAN interface address using NAT overload.

---

## Troubleshooting Commands Used

| Command | Purpose |
|---|---|
| `show vlan brief` | Verify VLAN membership |
| `show interfaces status` | Verify switch port status and VLAN |
| `show interfaces trunk` | Verify trunk operation |
| `show ip interface brief` | Verify router interfaces |
| `show ip route` | Verify routing |
| `show running-config` | Inspect current configuration |
| `show running-config \| section dhcp` | Inspect DHCP configuration |
| `show ip dhcp binding` | Verify DHCP leases |
| `show ip nat translations` | Verify active NAT translations |
| `show ip nat statistics` | Verify NAT statistics |

---

## Troubleshooting Approach

The troubleshooting process emphasized:

- Evidence collection before configuration changes
- Layer-by-layer investigation
- Hypothesis testing
- Verification after remediation
- Distinguishing configuration faults from non-network faults
- End-to-end validation
