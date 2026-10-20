*This project has been created as part of the 42 curriculum by jcamarer.*
 
# NetPractice
 
## Description
 
**NetPractice** is a project that introduces the fundamentals of **computer networking** through a set of 10 practical exercises. The goal is to learn how to configure small-scale networks so that they work properly: assigning **IP addresses** and **subnet masks**, connecting devices through **routers** and **switches**, and understanding the role of the **default gateway** and of **routing tables**.
 
The networks used are **simulated** (they are not real networks) and are displayed in a web-based training interface. Each level presents a non-functioning network diagram and one or more communication goals (for example, "host A must be able to communicate with host C" or "host B must be able to reach the Internet"). The editable fields (IP, mask and routes) must be modified until all goals are met. The architecture and addresses used are fictitious and are not connected to real-world configurations.
 
The result of the project is **10 exported configuration files** (one per level), which are submitted at the root of the Git repository.
 
## Instructions
 
### Requirements
 
- A web browser.
- `python3` (for the local web server).
- The project file attached to the project's page (`net_practice.1.9.tgz`), together with the subject (`subject.pdf`).
### Launching the training interface
 
1. Download the file attached to the project's page and extract its contents into any folder.
2. In that folder, run the script:
```bash
   ./run.sh
```
 
   This script launches a local web server and opens the browser on the NetPractice page.
 
3. If `run.sh` does not work, the project can be accessed manually. First, in the project folder, start a local web server (the port number can be changed):
```bash
   python3 -m http.server 49242
```
 
   Then, open the address `http://localhost:49242` (or the chosen port) in the browser.
 
   A local web server is required because of technical design and security constraints in several web browsers.
 
### Using the interface
 
- **Training mode:** enter the **intranet login** in the field provided and click `Start!`. This loads a personal configuration. **Entering the login is very important**, since the level configuration and the grading are based on it.
- **Evaluation mode:** generates a random configuration, suitable for training and for evaluations.
- Each level shows one or more **goals** at the top and a diagram with **editable** fields (unshaded) and **fixed** fields (shaded). The **logs** at the bottom of the page explain why a configuration is incorrect (for example, if a route is missing or an IP is invalid).
- Buttons:
  - `Check again`: checks whether the current configuration meets the goals.
  - `Get my config`: downloads the level's configuration once it is completed.
  - `Next level`: appears when the level is passed and allows moving on to the next one.
### Exporting the configurations
 
1. With the login entered, solve the level until all of its goals show an OK status.
2. Before moving on to the next level, click **`Get my config`** to download that level's configuration file.
3. Repeat for all 10 levels.
### Submission details
 
- **10 exported configuration files, one per level**, are submitted and placed at the **root of the Git repository**.
- In addition, the repository contains this `README.md` at its root, written in English.
- Only the content of the repository is evaluated. It is advisable to check that the 10 files are present, are not empty and have the correct names.
### Evaluation (defense)
 
- During the defense, the **Evaluation** tab of the interface is used. **3 random levels (from levels 6 to 10)** are presented automatically, and they must be solved correctly in a maximum of about **15 minutes**.
- The reasoning must be explained to the evaluator throughout the evaluation.
- The Internet and external resources may not be used. A simple calculator such as `bc` is tolerated, but that is the limit.
## Networking Concepts Studied
 
This section summarizes the concepts needed to understand the project.
 
### TCP/IP addressing and IPv4
 
An **IP address** identifies a **network interface** (not the whole device: a router has one IP per interface). In IPv4 it consists of **32 bits** written as **4 octets** in decimal (from 0 to 255) separated by dots, for example `192.168.1.10`.
 
Every address is divided into a **network part** (which identifies the network) and a **host part** (which identifies the device within it). The boundary between the two is set by the subnet mask.
 
### Subnet mask and CIDR notation
 
The **subnet mask** is another sequence of 32 bits: the bits set to **1** mark the network part and the bits set to **0** mark the host part (always ones first, followed by zeros). It is written in decimal (`255.255.255.0`) or in **CIDR notation** with the number of ones (`/24`).
 
Two devices are on the same network if, by applying the same mask to their IPs (bitwise AND operation), they obtain the same **network address**. If so, they communicate directly; if not, the traffic must go through a router.
 
The block size determines the increment between subnet network addresses.
 
| | | | | | | | | |
|---|---|---|---|---|---|---|---|---|
| **Block Size** | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| **Subnet Mask** | 128 | 192 | 224 | 240 | 248 | 252 | 254 | 255 |
| **CIDR (4th octet)** | /25 | /26 | /27 | /28 | /29 | /30 | /31 | /32 |
| **CIDR (3rd octet)** | /17 | /18 | /19 | /20 | /21 | /22 | /23 | /24 |
| **CIDR (2nd octet)** | /9 | /10 | /11 | /12 | /13 | /14 | /15 | /16 |
| **CIDR (1st octet)** | /1 | /2 | /3 | /4 | /5 | /6 | /7 | /8 |
 
For normal IPv4 subnets:
 
- /30 → 4 addresses, 2 usable host addresses
- /29 → 8 addresses, 6 usable host addresses
- /28 → 16 addresses, 14 usable host addresses
- /27 → 32 addresses, 30 usable host addresses
- /26 → 64 addresses, 62 usable host addresses
- /25 → 128 addresses, 126 usable host addresses
- /24 → 256 addresses, 254 usable host addresses
- /31 and /32 are special cases and should not be treated as normal subnets.
 
Within each subnet there are two addresses that cannot be assigned to devices: the **network address** (host bits set to 0) and the **broadcast** address (host bits set to 1). The number of usable hosts is `2^n − 2`, where `n` is the number of host bits. The **block size** (`256 − the mask value` in the octet where the mask is cut) makes it possible to quickly work out which subnet an IP falls into without converting to binary.
 
### Public, private and special addresses
 
- Private ranges (RFC 1918): `10.0.0.0/8`, `172.16.0.0/12` and `192.168.0.0/16`. They are not routed over the Internet.
- `127.0.0.0/8`: loopback (the device itself). `0.0.0.0/0`: "any destination" (default route).
### Routers and switches
 
- **Switch:** connects devices on the **same network** and works at layer 2 (MAC addresses). It needs no IP and no configuration in the exercises: everything connected to the same switch belongs to the same subnet.
- **Router:** connects **different networks** and works at layer 3 (IP addresses). It has several interfaces, each with its own IP and **each one in a different subnet**. It forwards packets according to its routing table.
### Default gateway
 
It is the IP of the router to which a device sends all traffic whose destination is **not** in its own subnet. It must be the IP of a router interface located **in the same subnet** as the device.
 
### Routing tables
 
Each route has the format `destination network/mask => next hop`: "packets destined for this network are delivered to this IP". Key rules:
 
- Networks directly connected to an interface do not need a route.
- The **next hop** must be the IP of a neighboring device that is directly reachable (never one of the device's own interfaces).
- The **default route** `0.0.0.0/0` is used when no other route matches; if several routes match, the one with the longest mask (the most specific) wins.
- Communication needs a **round trip**: intermediate routers must also know how to send the traffic back to its source.
### OSI model
 
| Layer | Name | Examples |
|---|---|---|
| 7 | Application | HTTP, DNS, SSH |
| 6 | Presentation | Encryption, encoding |
| 5 | Session | Session management |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, ICMP, routers |
| 2 | Data link | Ethernet, MAC, ARP, switches |
| 1 | Physical | Cables, signals |
 
This project mainly takes place at **layer 3 (network)**: IP addresses, masks, gateways and routing. Switches belong to layer 2.
 
## Resolution Methodology (Technical Choices)
 
The same procedure was followed for every level:
 
1. Read the goals and pay attention to the fixed (shaded) fields, which constrain the design.
2. Identify the **subnets**: every point-to-point cable, or every switch with its devices, forms one subnet, and two interfaces of the same router never share a subnet.
3. Choose the masks and calculate, for each subnet, its network address, its broadcast address and its range of usable hosts.
4. Assign unique, valid IPs with the same mask on both sides of each link.
5. Configure the hosts' gateways and the routers' routes (outbound and return).
6. Click `Check again`, read the logs and fix one value at a time.
## Resources
 
### Project resources
 
- NetPractice training interface (`net_practice.1.9.tgz`, launched with `run.sh`).
- Project subject (`subject.pdf`).
- The interface's own logs, used to debug the configurations.
### Networking concepts studied
 
- TCP/IP addressing (IPv4)
- Subnet masks and CIDR notation (subnetting)
- Network addresses, broadcast addresses and usable hosts
- Private and public addresses
- Default gateways
- Routers and switches
- Routing tables and default routes
- OSI model (and TCP/IP model)
### References
 
- RFC 791: Internet Protocol (IP).
- RFC 950: Internet Standard Subnetting Procedure.
- RFC 1918: Address Allocation for Private Internets.
- RFC 4632: Classless Inter-domain Routing (CIDR).
- RFC 792: Internet Control Message Protocol (ICMP).
- RFC 826: Address Resolution Protocol (ARP).
- ISO/IEC 7498-1: OSI reference model.
## AI Usage
 
Artificial intelligence was used as a learning support tool, not to generate the solutions:
 
- **Understanding concepts:** explanations from scratch of IP, subnet masks, CIDR, gateways, routing and the OSI model, and study notes on these topics.
- **Practicing with exercises:** generation of exercises on subnet calculation, gateways and routes, and correction of the answers to detect reasoning errors.
- **Resolving doubts:** help interpreting the interface's logs and guidance with hints when a level did not work out, without handing over the final configuration.
- **Documentation:** translation of the subject and help with the draft of this README.
The level configurations were solved and checked directly in the NetPractice interface, and the content generated by the AI was cross-checked against the interface itself to make sure it is understood and can be explained during the defense.