| Bagian | Evidence |
|---|---|
| DHCP | `ip dhcp pool FINANCE`, `ip dhcp pool IT` |
| Router-on-a-Stick | `G0/0.10`, `G0/0.20` |
| 802.1Q | `encapsulation dot1Q 10/20` |
| Inter-VLAN gateway | `192.168.10.1`, `192.168.20.1` |
| WAN | `G0/1 203.0.113.2/30` |
| NAT inside | `G0/0.10`, `G0/0.20` |
| NAT outside | `G0/1` |
| NAT/PAT | `ip nat ... overload` |
| Default route | `0.0.0.0/0 → 203.0.113.1` |
| NAT ACL | `ACL 1` untuk Finance + IT |
| Unused interface | `G0/2 shutdown` |
