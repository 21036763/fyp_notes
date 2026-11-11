Meeting Date: 5 November 2026
Attendees: Mr. Gibbsen Omar, Edbert Lee, Calissa Soh, Shi Difen (Stefanie)
Absent: N/A

Items Presented:
- Current progress of the vulnerable web server which compromised of the following:
	- SQLi Attack
	- XSS Attack
	- Local File Inclusion
	- Remote Code Execution
	- Path Traversal Attack 
	- Broken Access Control (IDOR)

Items Discussed: 
- Feasibility of the attack chain sequence of the web server.
- What is the objective of the SQLi attack? 
- How did the attacker managed to obtain a low-privilege shell on the web server? This can be achieved via LFI followed by RCE.
- Automated tools to automate the various attacks to automate certain attacks to reduce human demonstration fatigue. 
- Lab Network Configuration on the various zones. 
- Discussed about the feasibility of using LDAP on the web server.

Actionable Items: 

- Cross Site Request Forgery (CSRF) to be implemented and worked on by Stefanie. 
- The current web setup with the SQLi attack chain has no objective behind it. Instead of showing that the web server is vulnerable to SQLi attack, it needs a objective of why the SQLi attack is conducted.
- Explore/research on tools to automate the process of web exploitations such as "Selenium Browser Access" to automate the process of web exploitation instead of having the user to conduct this repetitive step.
- Redesign the lab architecture in a manner that the AD users shouldn't be on the same zone as the DMZ since the DMZ is accessible via the WAN network.
- Decouple LDAP from the DMZ as it would cause a lot of issues in the long run as we get further into the lab. 
- Remote LDAP binding from with DMZ and instead of using LDAP to authenticate and login to the web server, use a separate database instead like MySQL or MariaDB. 
- Check with ST Engineering on their availability for us to visit ST to configure our laptops and VPN's to access the AI model. 
- Redesign the attack flow to not use the same credentials on ALL the attack boxes.

Open questions: 
- Is the team graded more on the vulnerable lab or the agent?
- Is the team currently still on track? 
- What are the requirements of the attack? 