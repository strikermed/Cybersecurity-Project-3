# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:
- ELK
  - **Operating System**: Linux: ubuntu V18.04.4 LTS (Bionic Beaver)
  - **Purpose**: Elastic Stack
  - **IP Address**: 192.168.1.100/24
- Capstone
  - **Operating System**: Linux: Ubuntu 18.04.1 LTS (Bionic Beaver)
  - **Purpose**: Attack Target/Alert Testing
  - **IP Address**: 192.168.1.105/24
- Kali
  - **Operating System**: Linux: Kali-rolling 2020.1
  - **Purpose**: Pen Test machines
  - **IP Address**: 192.168.1.90/24
- Target 1
  - **Operating System**: Linux
  - **Purpose**: Attack Target 1/Vuln Word Press
  - **IP Address**: 192.168.1.110
- Target 2
  - **Operating System**: Linux: Ubuntu
  - **Purpose**: Attacke Targetr 2/Vuln Apache server
  - **IP Address**: 192.168.1.105

### Description of Targets

The target of this attack was: `Target 1` (192.168.1.110).

Target 1 is an Apache web server running wordpress and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Name of Alert 1

Excessive HTTp Errors is implemented as follows:
  - **Metric**: packetbeat - http.response.status_code
  - **Threshold**: More than 400 for 5 min
  - **Vulnerability Mitigated**: Brute Force Attempts
  - **Reliability**: Medium - It could throw some false positives if a large number of users input the wrong passwords within a 5 minute period.
#### Name of Alert 2
Alert 2 is implemented as follows:
  - **Metric**: Packetbeat - http.request.bytes
  - **Threshold**: over 3500 bytes in the last minute
  - **Vulnerability Mitigated**: Brute force attempts, DOS attacks, excessive http requests
  - **Reliability**: Medium, depending on if you have high traffic flow or not this will alert for a high volume of requests.  If legitimate traffic grows the threshold will likely need to be adjusted.
#### Name of Alert 3
Alert 3 is implemented as follows:
  - **Metric**: metricbeat - system.process.cpu.total.protect
  - **Threshold**: .5 (50%) for 5 minutes
  - **Vulnerability Mitigated**: DOS attacks, brute force attacks, database attacks
  - **Reliability**: Low, it's likely that the threshold won't be met unless your traffic volume grows, in that case you likely will want to expand hardware to accommodate the traffic.

### Suggestions for Going Further (Optional)

- Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks. It is not necessary to explain _how_ to implement each patch.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:
- Vulnerability 1
  - **Patch**: _install `Bitdefender` with `sudo apt-get update; sudo apt-get install bitdefender-scanner-gui`_
  - **Why It Works**: _`Bitdefender` scans the system for viruses every day_
- Vulnerability 2
  - **Patch**: _implement `Two Step Authentication` with `wordpress`_
  - **Why It Works**: _`Two Step Authentication` adds an extra layer of security that prevents credential stuffing, bruteforce, and even dictionary attacks on your user accounts_
- Vulnerability 3
  - **Patch**: _Clost/Restrict `SSH` with `Firewall Rules`_
  - **Why It Works**: _`Firewall Rules` can reduce your attack surface by blocking ssh, or by restricting it to specific IP's like the office or Developer Home IP addresses._
  - Vulnerability 4
    - **Patch**: _Secure `SSH login` with `SSH Keys`_
    - **Why It Works**: _`SSH Keys` are much more secure than passwords.  The asymmetrical (public and Private) key pair allow for only the creator of the keys pair to connect.  These keys are incredibly hard to crack especially when salting is involved_
