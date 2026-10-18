💃 Hack The Box - Appointment

📌 Room Information

Category
Details
Platform
Hack The Box
Machine
Appointment
Difficulty
Very Easy
Operating System
Linux (Debian)
Status
✅ Completed

🎯 Objectives

- Understand basic web application concepts and HTTP error codes.
- Identify running web services using Nmap.
- Understand SQL Injection (SQLi) vulnerabilities and OWASP Top 10 classifications.
- Learn how MySQL handles line comments (#).
- Bypass authentication using SQL Injection.
- Capture the Machine Flag.

🛠️ Tools Used

- Pwnbox / Kali Linux
- Nmap
- Gobuster
- Web Browser (Firefox)

🌐 Target Information

Target
Description
Machine
Linux (Debian) Target
Open Port
80/TCP
Service
Apache httpd 2.4.38 (Debian)

🔎 Enumeration

Check Connectivity

ping <Target-IP>

Scan the Target

nmap -sV <Target-IP>

Result

Port
State
Service
Version
80/tcp
Open
http
Apache httpd 2.4.38 ((Debian))

Web Directory Enumeration

gobuster dir -u http://<Target-IP>/ -w /usr/share/wordlists/dirb/common.txt

Key Directories Found

Directory / File
Status Code
Description
/index.php
200
Main Login Page
/css/
301
Stylesheets
/js/
301
JavaScript Files

🚀 Exploitation

Authentication Bypass via SQL Injection

The web application contains a login form that does not properly sanitize user input, leaving it vulnerable to SQL Injection.

1. Navigate to the login page in the browser:
   http://<Target-IP>/
2. In the Username field, enter a comment payload to truncate the SQL query and bypass password validation:
   admin' #
3. Leave the Password field empty (or enter any arbitrary input like 123).
4. Click Login.

Query Explanation

Behind the scenes, the backend SQL query is constructed as follows:

SELECT * FROM users WHERE username='admin' # ' AND password='password';

The # symbol comments out the rest of the query in MySQL, ignoring the password check condition entirely and granting full access as admin.

📂 Post-Exploitation & Flag Capture

Upon successful login bypass, the web application renders the admin dashboard containing the flag.

Flag Verification

Congratulations!  
Your flag is: e3d0796d002a446c0e622226f42e9672

🧠 Vulnerability Analysis

SQL Injection (SQLi)

The application directly uses user input inside SQL queries without proper validation or sanitization.
This allows attackers to modify the query logic and bypass authentication without needing valid credentials.
OWASP Top 10: A03:2021 - Injection

📚 What I Learned

- Basic web application enumeration and HTTP response codes.
- Identifying service versions using nmap -sV.
- Directory brute-forcing using gobuster dir.
- Concepts of SQL Injection (OWASP A03:2021-Injection).
- Using comment characters (#) in MySQL to manipulate query execution logic and bypass authentication.

🔐 Key Takeaways

- User input should always be sanitized or handled via Prepared Statements (Parameterized Queries) to prevent SQL Injection.
- Comment characters in database engines (e.g., # or --  in MySQL) can allow attackers to ignore remaining query logic (such as password checks).
- Simple authentication interfaces should never trust raw input concatenated directly into database queries.

💡 Skills Gained

- Nmap
- Gobuster
- SQL Injection (SQLi)
- Authentication Bypass
- Web Reconnaissance
- OWASP Top 10
- Linux Web Services

✅ Machine Completed Successfully 🎉

👨‍💻 Author

Sayed Samy Elbadry

🚀 Web Penetration Testing & Red Team Journey

Platform: Hack The Box
