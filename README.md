
# Inter-VLAN Routing Lab

A small inter-VLAN routing project using a router (Router-on-a-Stick) to connect four VLANs.

## VLAN Design

| VLAN ID | Name   | Network         | Gateway       |
|---------|--------|-----------------|---------------|
| 10      | HR     | 192.168.1.0/24 ; | 192.168.1.1   |
| 20      | IT     | 192.168.2.0/24 ; | 192.168.2.1   |
| 30      | SALES  | 192.168.3.0/24 ; | 192.168.3.1   |
| 40      | SERVER | 192.168.4.0/24 ; | 192.168.4.1   |

## Topology

```

[HR] ---+
[IT] ---+--- [Switch] --- Trunk --- [Router] (sub-interfaces)
[SALES]-+
[SERVER]+

```

- The switch carries all VLANs over a single trunk link to the router.
- The router's physical interface (e.g., `GigabitEthernet0/0`) is divided into logical sub-interfaces, one per VLAN.

## Router Configuration (Cisco IOS example)

```cisco
interface GigabitEthernet0/0
no ip address
no shutdown

interface GigabitEthernet0/0.10
encapsulation dot1Q 10
ip address 192.168.1.1 255.255.255.0

interface GigabitEthernet0/0.20
encapsulation dot1Q 20
ip address 192.168.2.1 255.255.255.0

interface GigabitEthernet0/0.30
encapsulation dot1Q 30
ip address 192.168.3.1 255.255.255.0

interface GigabitEthernet0/0.40
encapsulation dot1Q 40
ip address 192.168.4.1 255.255.255.0
```

Switch Configuration (Access Ports)

```cisco
vlan 10
name HR
vlan 20
name IT
vlan 30
name SALES
vlan 40
name SERVER

interface GigabitEthernet0/1
switchport mode access
switchport access vlan 10
! repeat for other VLANs

interface GigabitEthernet0/24
switchport mode trunk
```

Testing

· Assign IP addresses to devices in their respective subnets (e.g., HR PC: 192.168.1.10/24, gateway 192.168.1.1).
· Ping between VLANs – traffic is routed by the router.

Notes

· All devices can now communicate across VLANs through the router.
· For a real deployment, add ACLs to control access between departments.

```
