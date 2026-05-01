# Lab 04: Enterprise LAN Security Assessment

## Part 1: Network Architecture

For this lab, I built a segmented network for WBMUD that separates the regular IT network from the OT environment and the remote pump station. I also included an attacker node to represent an outside threat. The purpose of separating the network this way is to make sure that one compromised area does not automatically give access to everything else. This is especially important for a water utility because the OT side includes systems connected to monitoring and control operations.

The topology also connects to the OSI model. The cables and device connections show Layer 1 because they represent the physical path for communication. The switches connect to Layer 2 because they move traffic using MAC addresses inside each local network. The routers connect to Layer 3 because they use IP addresses to move traffic between the IT, OT, and remote networks. This helped show how network design and security are connected instead of being two separate things.

### Full Network Topology

![Full Network Topology](topology.png)

### IP Settings and Router Settings

![IP Settings](ip-settings.png)

![Router Settings](router-settings.png)

### Switch MAC Address Table

![Switch MAC Address Table](mac-table.png)

### Ping Test

![Ping Test](ping-test.png)

The ping results show that communication within the same subnet was successful, while communication across different networks did not fully succeed. This is likely because full routing was not configured between all segments. However, the test still demonstrates basic connectivity and confirms that IP addressing and local communication were set up correctly.

---

## Part 2: Protocol Security Analysis

TCP and UDP both work at the transport layer, but they are not equally useful from a security point of view. TCP creates a connection before sending data, which makes it easier to track a session and notice when something looks wrong. UDP does not have that same connection process. It can be faster, but it gives less visibility because traffic is sent without the same type of confirmation.

HTTP and HTTPS show why encryption matters at the application level. HTTP sends information in plain text, so if someone intercepts the traffic, they may be able to read it. HTTPS is more secure because it uses encryption to protect the data while it moves across the network. In an enterprise or critical infrastructure environment, HTTPS is important because even basic web traffic can expose sensitive information.

### HTTP Connection to OT Web Server

![HTTP Connection](http-test.png)

### HTTPS Connection to OT Web Server

![HTTPS Connection](https-test.png)

SSH is also a better option than Telnet for remote access. Telnet is risky because it sends usernames and passwords in clear text. SSH protects the session by encrypting it, which makes it much safer for managing routers, servers, and other important devices. This is why SSH is the standard choice in professional networks.

### SSH Attempt

![SSH Attempt](ssh-attempt.png)

SSH configuration was attempted, but Packet Tracer did not accept the SSH command format from the PC command prompt in this setup. The protocol comparison was still documented because SSH is the secure replacement for Telnet since it encrypts credentials and session traffic, while Telnet sends them in clear text.

---

## Part 3: Shellshock Vulnerability Assessment

Shellshock is a Bash vulnerability that can allow an attacker to run commands on a server remotely. In this lab scenario, the vulnerable system is the OT web server. The attacker sends a specially crafted web request to a CGI script, and Bash incorrectly processes part of that request as a command. Instead of only handling normal web input, the server ends up executing the attacker’s command.

The attack connects to the LAMP stack because Linux, Apache, and Bash are all part of the chain. Apache receives the request, the CGI script passes information to the system, and Bash is where the vulnerability is triggered. This makes the issue more serious because the attacker does not need physical access to the server. They can reach it through the web service.

If this happened in a real OT environment, the impact could be serious. The attacker could steal data, change files, use the server as a starting point to reach SCADA devices, or disrupt monitoring systems. To reduce this risk, WBMUD would need to patch Bash, limit or disable unnecessary CGI scripts, use a web application firewall, and separate the OT network so one compromised server cannot easily reach everything else. CERT/CC also plays an important role by sharing vulnerability information and helping organizations understand how to respond to issues like Shellshock.

Another important aspect of this vulnerability is that it does not require complex tools or advanced access. Attackers can take advantage of it through simple web requests if the system is not patched. This makes it especially dangerous in environments where web servers are exposed to external networks. It also highlights how older vulnerabilities can still be a threat if organizations do not keep systems updated consistently.

---

## Part 4: Incident Response

The most likely attack path started with the attacker reaching the OT web server from the outside. Because the web server was vulnerable to Shellshock, the attacker was able to run commands remotely. After that, the attacker could use the compromised server to move further into the OT network, including toward SCADA devices.

### Attack Path Diagram

![Attack Path Diagram](attack-path.png)

The 2.3 GB data transfer suggests that the attacker had enough access to collect information and send it back out to an external system. The unauthorized SSH activity also shows that the compromise was not limited to one web request. The attacker was likely using the web server as a bridge into more sensitive systems.

Several issues made the incident worse. The web server was not properly patched, the OT network was not isolated strongly enough, access controls were too loose, and monitoring did not catch the activity early. A stronger defense-in-depth approach could have reduced the damage by making each step harder for the attacker.

WBMUD’s first step should be to isolate affected systems, preserve evidence, block the attacker’s IP address, and reset credentials. Short term, the organization should patch vulnerable systems, review SSH access, and tighten firewall rules. Long term, WBMUD should improve network segmentation, use least privilege, apply zero trust principles, and add better monitoring for unusual traffic. Staff should also be trained on patch management and incident response so future attacks can be caught sooner.

### Root Cause Analysis Table

| Vulnerability Category | What Went Wrong | Missing Security Control |
|---|---|---|
| Unpatched Software | The OT web server was vulnerable to Shellshock because Bash was not properly patched. | Patch management and vulnerability scanning |
| Network Segmentation | The attacker was able to move from the web server toward OT/SCADA systems. | Stronger OT isolation and firewall rules |
| Access Controls | The compromised system had too much ability to reach other internal systems. | Least privilege and stricter SSH access |
| Monitoring & Detection | The attack was not caught until after suspicious traffic and data transfer occurred. | Centralized logging, alerts, and intrusion detection |

---

## Part 5: OSI Model Mapping Summary

| OSI Layer | Example from Lab |
|---|---|
| Layer 1: Physical | Cables and physical device connections |
| Layer 2: Data Link | Switches and MAC address communication |
| Layer 3: Network | IP addresses, routers, and routing |
| Layer 4: Transport | TCP, UDP, and port-based communication |
| Layer 7: Application | HTTP, HTTPS, SSH, and the Shellshock web request |

---

## Reflection

This lab helped me see that network security is not just about adding security tools after the network is already built. The design of the network itself matters. By separating IT, OT, remote systems, and the attacker node, I could see how segmentation can help limit the damage if one area is compromised.

The Shellshock scenario also showed how one vulnerability can become much bigger when it exists inside a critical infrastructure environment. A web server issue may seem small at first, but if that server can reach SCADA systems, the risk becomes much more serious. I also understood the OSI model better because each part of the lab connected to a different layer, from physical cabling to application-level protocols.

Overall, the biggest thing I learned is that security has to be layered. Patching, encryption, access control, monitoring, and segmentation all work together. If one control fails, the others can still help slow down or stop an attacker.