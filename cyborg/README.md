# Bounty Hacker 🚀

![Cyborg](cyborg.jpeg)


### Setting Up the Environment

To begin, ensure you have the necessary VPN configuration file and OpenVPN installed on your system.

1. **Connect to the THM (TryHackMe) VPN** to access the challenge environment.
   ```bash
   sudo openvpn yourvpnconfig.ovpn
   ```

   It's essential to establish a VPN connection to securely access the challenge network. 🔒

2. **Verify the VPN connection** is established to ensure access to the challenge network.

   This step confirms that you're connected to the challenge environment and can proceed with the tasks. ✔️

## Usage Guide

### Task 1: Reconnaissance 🔍

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
    80/tcp   open  http        Apache httpd 2.4.18 ((Ubuntu))
```

Then, use Gobuster to enumerate directories:

```bash
gobuster dir -u <machine-ip> -w /path/to/wordlist -x php,sh,txt,cgi,html,js,css,py,pdf -o output.txt
```

- `u` <machine-ip>: Specifies the target URL or IP address to scan.
- `w` /path/to/wordlist: Sets the wordlist to be used for directory enumeration.
- `x` php,sh,txt,cgi,html,js,css,py,pdf: Specifies extensions to be appended to discovered directories for further enumeration.
- `o` output.txt: Saves the results to a file named output.txt.

```bash
    /index.html           (Status: 200) [Size: 11321]
    /admin                (Status: 301) [Size: 312] [--> http://10.10.166.87/admin/]
    /etc                  (Status: 301) [Size: 310] [--> http://10.10.166.87/etc/]
```
### Task 2: Gaining Access 📂

1. **Explore Directories**:
   - Navigate to the admin page and inspect its content.
   - A file named `admin.html` is discovered, containing a chat log.
   - The chat log reveals conversations between individuals, potentially containing sensitive information.
   - Notably, mention of a backup named "music_archive" is found, sparking interest. 💬

2. **Utilize Discovered Credentials**:
   - Explore the `/etc` directory to find configuration files and credentials.
   - Discover `squid.conf` explaining configuration details and a `passwd` file containing hashed passwords.
   - Extract the hashed password associated with "music_archive" from the `passwd` file.
   - Save the hashed password in a text file and employ tools like John the Ripper to crack it. 🔐💻

   Command:
   ```bash
   john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
   ```

3. **Examine Downloadable Archive**:
   - Identify a link on the webpage facilitating the download of an archive named `archive.tar`.
   - Upon examining the contents of the downloaded archive, locate a README file.
   - The README file provides instructions to use a tool called `Borg` on the extracted archive. 📦📜

   Command:
   ```bash
   borg extract home/field/dev/final_archive::music_archive
   ```

4. **Exploit Privilege Escalation**:
   - Analyze the `/etc/mp3backups/backup.sh` script for potential vulnerabilities.
   - Attempt to exploit these conditions to gain elevated privileges:

   ```bash
   sudo /etc/mp3backups/backup.sh -c '/bin/bash'
   ```

   Explanation:
   - Execute the `backup.sh` script with the `-c` flag to pass a command to be executed.
   - In this case, attempting to spawn a new shell (`/bin/bash`) with elevated privileges. ⚠️🛠️

5. **Implement SUID Bit on Bash**:
   - Set the SUID (Set User ID) bit on the `/bin/bash` binary to execute with the permissions of its owner (root).

   ```bash
   chmod 4577 /bin/bash
   ```

   Explanation:
   - `chmod 4577`: Sets the permissions of `/bin/bash`, ensuring it runs with the privileges of its owner and with the SUID bit set.
   - After setting the SUID bit, executing `/bin/bash` as user "****" will grant root access. 🛠️🔑🔒

## Conclusion

In conclusion, the "Cyborg" cybersecurity challenge presents a structured opportunity for participants to practice reconnaissance, exploitation, and privilege escalation techniques. By leveraging tools like Nmap and Gobuster, and exploiting vulnerabilities like misconfigured permissions, participants can enhance their penetration testing skills and achieve the goal of gaining full control over the target system. 🛡️