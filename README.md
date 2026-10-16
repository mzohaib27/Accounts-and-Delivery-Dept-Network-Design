# Accounts and Delivery Department Network Design

## 1. Project Overview

This project demonstrates the design and configuration of a small office network connecting two departments: **Accounts** and **Delivery**.

The network is designed using one router, two switches, four PCs, and two printers. The network address `192.168.40.0/24` is divided into two `/25` subnets to provide separate IP address ranges for each department.

The router provides communication between the two departmental networks, allowing PCs in the Delivery department to ping PCs in the Accounts department.

**Project Type:** Small Office Network Design  
**Platform:** Cisco Packet Tracer  
**Network Address:** `192.168.40.0/24`  
**Subnet Mask:** `255.255.255.128` (`/25`)

## 2. Project Objectives

- Design a network connecting Accounts and Delivery departments.
- Use one router and two switches to connect all end devices.
- Divide the given network into two subnets using `/25` subnetting.
- Configure appropriate IP addresses, subnet masks, and default gateways.
- Connect PCs, printers, switches, and the router using appropriate cables.
- Enable inter-subnet communication through router interfaces.
- Verify connectivity by pinging PCs in the Accounts department from PCs in the Delivery department.

## 3. Network Topology

The network consists of two departmental LANs connected through a router.

- **Accounts Department:** One switch, two PCs, and one printer.
- **Delivery Department:** One switch, two PCs, and one printer.
- **Router:** One router with two Gigabit Ethernet interfaces connecting the two departmental networks.

Each switch connects its department's PCs and printer to the router.

_Insert your Cisco Packet Tracer topology screenshot here._

![Network Topology](topology/network-topology.png)

## 4. Network Subnetting

The given network address is `192.168.40.0/24`. It is divided into two equal-sized subnets, each using a `/25` prefix.

| Parameter          | Accounts Department | Delivery Department |
| ------------------ | ------------------- | ------------------- |
| Network Address    | 192.168.40.0/25     | 192.168.40.128/25   |
| Subnet Mask        | 255.255.255.128     | 255.255.255.128     |
| First Usable IP    | 192.168.40.1        | 192.168.40.129      |
| Last Usable IP     | 192.168.40.126      | 192.168.40.254      |
| Broadcast Address  | 192.168.40.127      | 192.168.40.255      |
| Default Gateway    | 192.168.40.1        | 192.168.40.129      |
| Total Usable Hosts | 126                 | 126                 |

Each `/25` subnet provides 128 total addresses, including one network address and one broadcast address, leaving 126 usable host addresses.

## 5. IP Addressing Scheme

### Accounts Department

| Device           | Interface          | IP Address   | Subnet Mask     | Default Gateway |
| ---------------- | ------------------ | ------------ | --------------- | --------------- |
| Router           | GigabitEthernet0/0 | 192.168.40.1 | 255.255.255.128 | N/A             |
| Accounts PC1     | FastEthernet0      | 192.168.40.2 | 255.255.255.128 | 192.168.40.1    |
| Accounts PC2     | FastEthernet0      | 192.168.40.3 | 255.255.255.128 | 192.168.40.1    |
| Accounts Printer | Ethernet           | 192.168.40.4 | 255.255.255.128 | 192.168.40.1    |

### Delivery Department

| Device           | Interface          | IP Address     | Subnet Mask     | Default Gateway |
| ---------------- | ------------------ | -------------- | --------------- | --------------- |
| Router           | GigabitEthernet0/1 | 192.168.40.129 | 255.255.255.128 | N/A             |
| Delivery PC1     | FastEthernet0      | 192.168.40.130 | 255.255.255.128 | 192.168.40.129  |
| Delivery PC2     | FastEthernet0      | 192.168.40.131 | 255.255.255.128 | 192.168.40.129  |
| Delivery Printer | Ethernet           | 192.168.40.132 | 255.255.255.128 | 192.168.40.129  |

_Note: The end-device IP addresses above are an example addressing plan. Adjust them if your Packet Tracer devices use different addresses._

## 6. Hardware Requirements

The following network devices are used in this design:

| Device   | Quantity | Purpose                                       |
| -------- | -------: | --------------------------------------------- |
| Router   |        1 | Routes traffic between the two subnets        |
| Switch   |        2 | Connects departmental devices within each LAN |
| PCs      |        4 | Provides end-user network access              |
| Printers |        2 | Provides network printing for each department |

## 7. Cabling and Connections

Appropriate Ethernet cables are used to connect the network devices.

| Connection                                   | Cable Type              |
| -------------------------------------------- | ----------------------- |
| Router GigabitEthernet0/0 to Accounts Switch | Copper straight-through |
| Router GigabitEthernet0/1 to Delivery Switch | Copper straight-through |
| Accounts PCs and printer to Accounts Switch  | Copper straight-through |
| Delivery PCs and printer to Delivery Switch  | Copper straight-through |

The router interfaces and connected switch ports must be enabled and operational for successful communication.

## 8. Router Configuration

The router is configured with one IP address from each subnet to act as the default gateway for its respective department.

Example Cisco IOS configuration:

```text
enable
configure terminal

interface gigabitEthernet0/0
 ip address 192.168.40.1 255.255.255.128
 no shutdown
 exit

interface gigabitEthernet0/1
 ip address 192.168.40.129 255.255.255.128
 no shutdown
 exit

end
write memory
```

**Note:** This example assumes the router uses the interface names `GigabitEthernet0/0` and `GigabitEthernet0/1`. Adjust the interface names if your Packet Tracer router uses different names.

Both subnets are directly connected to the router, so no static routes are required for communication between them.

## 9. Connectivity Testing

Connectivity is tested to verify that devices in the Delivery department can communicate with devices in the Accounts department.

### Example Ping Tests

From Delivery PC1 (`192.168.40.130`), open the Command Prompt and run:

```text
ping 192.168.40.2
ping 192.168.40.3
ping 192.168.40.4
```

These tests check connectivity to Accounts PC1, Accounts PC2, and the Accounts printer.

Additional checks:

```text
ping 192.168.40.129
ping 192.168.40.131
ping 192.168.40.132
```

These verify connectivity to the Delivery gateway, Delivery PC2, and the Delivery printer.

### Expected Results

| Test                             | Expected Result                     |
| -------------------------------- | ----------------------------------- |
| Delivery PC1 to Accounts PC1     | Successful ping                     |
| Delivery PC1 to Accounts PC2     | Successful ping                     |
| Delivery PC1 to Accounts printer | Successful ping, if ICMP is enabled |
| Delivery PC2 to Accounts PCs     | Successful ping                     |
| Accounts PCs to Delivery PCs     | Successful ping                     |

_The expected results assume correct IP addressing, default gateways, active interfaces, proper cabling, and no ACL or host firewall rules blocking ICMP traffic. Actual results must be verified in Packet Tracer._

## 10. Key Networking Concepts Demonstrated

This project demonstrates practical understanding of:

- IPv4 addressing and subnetting
- `/25` subnet masks and usable host ranges
- Network and broadcast addresses
- Default gateway configuration
- Router interfaces and inter-subnet routing
- Ethernet cabling and switch connectivity
- ICMP ping testing and basic network troubleshooting

## 11. Project Files

The repository can include the following files:

```text
accounts-delivery-network/
├── README.md
├── accounts-delivery-network.pkt
├── topology/
│   └── network-topology.png
├── configs/
│   └── router-config.txt
└── tests/
    ├── delivery-to-accounts-ping.png
    └── router-interfaces.png
```

## 12. Conclusion

This project demonstrates how a small office network can connect two departments using separate IP subnets and a router.

By configuring the router interfaces, assigning appropriate IP addresses and default gateways, and testing connectivity, the network enables communication between Accounts and Delivery devices.

The project provides a practical foundation for understanding IPv4 subnetting, routing, LAN connectivity, and network troubleshooting.

---

**Disclaimer:** This is a personal educational networking project developed in a simulated environment using Cisco Packet Tracer. It does not represent a production network deployment.
