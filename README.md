# Network-Segmentation-Monitoring-

In this project, I simulated an enterprise network in Cisco Packet Tracer to practice secure network design. I implemented VLANs, ACLs, and port security to enforce role-based access control, then used Wireshark and SNMP to monitor the environment and analyze traffic anomalies.

## Tools Used

- Cisco Packet Tracer (network simulation)
- Wireshark (packet analysis)
- SNMP utilities (via Linux Mint)
- draw.io (optional for diagrams)

## Network Architecture

### Diagram 1: Segmented Enterprise Network

- Layer 2 and Layer 3 switches
- Multiple VLANs: HR, IT, Finance
- Inter-VLAN routing via router-on-a-stick
- Access Control Lists (ACLs) on router
- Port security on switch edge ports
- Monitoring simulated using SPAN port for Wireshark and SNMP polling

## Network Segments & Roles

| VLAN | Department | IP Range         |
|------|------------|------------------|
| 10   | HR         | 192.168.10.0/24  |
| 20   | IT         | 192.168.20.0/24  |
| 30   | Finance    | 192.168.30.0/24  |

## Setup Steps

### 1. Created Network Topology in Cisco Packet Tracer

- Added 3 switches, 1 router, and multiple PCs
- Connected switches to simulate core, distribution, and access layers
- Connected PCs and servers to appropriate VLAN access ports

### 2. Configured VLANs on Switches

Switch> enable
Switch# config t
Switch(config)# vlan 10
Switch(config-vlan)# name HR
Switch(config)# vlan 20
Switch(config-vlan)# name IT
Switch(config)# vlan 30
Switch(config-vlan)# name Finance

graphql
Copy code

- Assigned access ports and trunk links

### 3. Configured Inter-VLAN Routing (Router-on-a-Stick)

Router(config)# interface g0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0

Router(config)# interface g0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0

Router(config)# interface g0/0.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.30.1 255.255.255.0

shell
Copy code

### 4. Implemented Access Control Lists (ACLs)

- Denied HR access to Finance

access-list 100 deny ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255
access-list 100 permit ip any any

interface g0/0.10
ip access-group 100 in

shell
Copy code

### 5. Applied Port Security

Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 1
Switch(config-if)# switchport port-security mac-address sticky
Switch(config-if)# switchport port-security violation restrict

vbnet
Copy code

- Limited each port to a single device

### 6. Simulated Network Anomalies

- Added rogue device on guest VLAN
- Simulated port scan from IT to Finance
- Used to validate segmentation and ACLs

### 7. Captured Traffic with Wireshark

- Mirrored switch port in Packet Tracer to simulate SPAN
- Exported traffic to Wireshark on Linux Mint

#### Wireshark Filters Used

vlan
ip.addr == 192.168.10.10
tcp.port == 80

shell
Copy code

### 8. Enabled SNMP Monitoring

Router(config)# snmp-server community public RO

graphql
Copy code

On Linux Mint:

sudo apt install snmp
snmpwalk -v2c -c public <router IP>

markdown
Copy code

- Polled routing and interface data

## Outcome

- Simulated a fully segmented enterprise network
- Enforced role-based access with VLANs, ACLs, and port security
- Captured and analyzed traffic in Wireshark
- Monitored router performance using SNMP
- Reinforced core network security principles and troubleshooting skills
