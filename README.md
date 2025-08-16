# Small Office Network using Cisco Packet Tracer

A simple **small office network** designed for the Cisco Networking Academy Packet Tracer curriculum.  It follows the structure of the **"Exploring Networking with Cisco Packet Tracer"** course: you will build and test a branch-office network that mixes wired and wireless devices, learn how to verify connectivity and observe packet flows, and experiment with network management concepts.

## Overview

The network mimics a branch office with both wired and wireless segments.  A single router connects the office to an upstream network (e.g. ISP) and provides NAT and DHCP services.  A layer-2 switch feeds several wired hosts and a server.  A wireless router connects wireless clients such as a laptop and an IoT sensor.  The design draws on the course modules:

- **Set up your small office network:** add routers, switches, PCs, a server and a wireless access point; connect them with appropriate cables or wireless associations; assign IP addresses and verify device connectivity.
- **Manage and monitor your branch office network:** enable DHCP and NAT on the router so devices get addresses and can reach external networks; explore the simulation mode in Packet Tracer to watch packets traverse the network; experiment with a network controller for monitoring (optional).

## Topology

![Network Diagram](network_diagram.png)

### Devices

- **BranchRouter** – Cisco ISR router providing NAT and DHCP.  Connected to the ISP on `GigabitEthernet0/0` and the internal LAN on `GigabitEthernet0/1`.
- **Switch** – Layer-2 switch connecting the BranchRouter to wired hosts and the server.
- **WirelessRouter** – Wireless access point/router providing Wi-Fi access for wireless clients.  Its WAN interface connects to BranchRouter `GigabitEthernet0/2` (or FastEthernet0).  The LAN side uses the `192.168.20.0/24` subnet.
- **Server** – Provides services (web/file) to LAN hosts.  Static IP `192.168.10.10`.
- **PC1/PC2/PC3** – Wired hosts that receive addresses from the BranchRouter via DHCP in `192.168.10.0/24`.
- **Laptop** – Wireless client that receives an address from the WirelessRouter via DHCP in `192.168.20.0/24`.
- **IoT Device** – Example sensor connected via Wi-Fi to the WirelessRouter.

### IP Addressing

| Interface | Device | Address | Notes |
|---|---|---|---|
| `Gig0/0` | BranchRouter | `198.51.100.2/30` | Upstream/ISP side |
| `Gig0/1` | BranchRouter | `192.168.10.1/24` | LAN subnet |
| `Gig0/2` | BranchRouter → WirelessRouter WAN | `192.168.20.1/24` | Wireless subnet gateway |
| Server | Static | `192.168.10.10/24` | Set manually on the server |
| PCs 1–3 | DHCP | `192.168.10.x` | Assigned via DHCP pool |
| WirelessRouter LAN | DHCP | `192.168.20.2/24` onwards | Wireless clients |

## Getting Started in Packet Tracer

1. **Add devices.** In Packet Tracer, drag a **2901 router** for BranchRouter, a **2960 switch**, a **wireless router**, three **PCs**, one **server**, and optionally a **laptop** and an **IoT sensor**.
2. **Connect the devices.** Use copper straight-through cables for router–switch and switch–host connections.  Use a straight-through cable between BranchRouter `Gig0/2` and the wireless router’s WAN port.  The wireless clients associate via Wi-Fi.
3. **Configure BranchRouter.** Use the commands in [`router_config.txt`](router_config.txt) to set hostnames, interface IP addresses, DHCP, NAT and the default route.  You can paste these into the CLI.
4. **Configure the server.** Assign the IP `192.168.10.10`, subnet mask `255.255.255.0`, default gateway `192.168.10.1` and a DNS server (e.g. `8.8.8.8`).
5. **Configure the wireless router.** Set the WAN IP address to `192.168.20.2/24` with default gateway `192.168.20.1`.  Configure a DHCP pool for the wireless LAN (e.g. `192.168.20.100`–`192.168.20.150`) and set an SSID (e.g. `BranchWiFi`) with WPA2 encryption.
6. **Test connectivity.** From each PC, use the **ping** command to test connectivity to other devices and to the BranchRouter’s upstream interface (`198.51.100.1` can be simulated by another router or cloud).  In Simulation mode, observe packets flowing through the network.

## Router Configuration

The file [`router_config.txt`](router_config.txt) contains a minimal configuration script for the BranchRouter.  After adding the router in Packet Tracer, open the CLI and paste the contents.  Adjust interface names if your router has different port names.  This configuration sets IP addresses, DHCP, NAT and a static default route.

Feel free to expand on this design—add VLANs, additional switches, or experiment with a network controller (such as Cisco DNA Center) as introduced in the Packet Tracer course.  This repository provides a starting point aligned with the Networking Academy’s small office network scenario.
