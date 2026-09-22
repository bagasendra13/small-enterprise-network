Device	Interface	IP Address	Subnet	Purpose
R1	G0/0.10	192.168.10.1	/24	Finance Gateway
R1	G0/0.20	192.168.20.1	/24	IT Gateway
R1	G0/1	203.0.113.2	/30	WAN
ISP	G0/0	203.0.113.1	/30	R1-facing interface
ISP	G0/1	203.0.113.5	/30	Server-facing interface
Server	NIC	203.0.113.6	/30	DNS / HTTP
PC-FIN-01	NIC	DHCP	/24	Finance client
PC-FIN-02	NIC	DHCP	/24	Finance client
PC-IT-01	NIC	DHCP	/24	IT client
PC-IT-02	NIC	DHCP	/24	IT client
