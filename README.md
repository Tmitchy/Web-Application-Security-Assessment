# Web-Application-Security-Access_log-Assessment
Web applications are crucial interfaces that connect users to various services over the internet. Due to their widespread use across organizations, these applications are susceptible to exploitation by malicious actors. Such vulnerabilities pose significant risks, including unauthorized access to devices, theft of sensitive personal data, and disruptions in service continuity. These negative outcomes can result in substantial financial losses for affected entities.
 
To mitigate these risks, it is essential to implement robust detection mechanisms that can identify and respond to web-based attacks. This involves a combination of preventive measures, monitoring tools, and analytical techniques designed to safeguard web applications and their users. Understanding the various types of web attacks, along with their methodologies, is vital for developing effective defense strategies and ensuring the integrity and security of web services.

---

📌 **Overview**<br>

This project documents a comprehensive security assessment of common web application vulnerabilities, expertly analyzed from the perspective of a SOC Analyst, including:

- XSS Detection<br>
- SQL Injection Detection<br>
- Command Injection<br>
- Open Redirection<br>
- Brute Force Attacks<br>
- XXE (XML) Attacks<br>
- File Inclusion (RFI/LFI)<br>
- IDOR<br>
- Directory Traversal Attacks<br>

The objective was to identify attack patterns, trace malicious activities in access log files, and suggest implementations to mitigate risks and improve overall application security.

---

📚 **Table of Contents**<br>

- [SQL Injection Detection](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%201%3A%20Identifying%20an%20SQL%20Attack)<br>
- [XSS Detection](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%202%3A%20Identifying%20an%20XSS%20Attack)<br>
- [Command Injection](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%203%3A%20Identifying%20a%20Command%20Injection%20Attack)<br>
- [Open Redirection](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%208%3A%20Identifying%20an%20Open%20Redirection%20Attack)<br>
- [Brute Force Attacks](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%207%3A%20Identifying%20a%20Brute%20Force%20Attack)<br>
- [XXE (XML) Attacks](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%206%3A%20Identifying%20an%20XML%20External%20Entity%20(XXE)%20Attack)<br>
- [File Inclusion (RFI/LFI)](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%206%3A%20Identifying%20Remote%20%26%20Local%20File%20Inclusion%20(RFI/LFI))<br>
- [IDOR](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%204%3A%20Identifying%20an%20Insecure%20Direct%20Object%20Reference%20(IDOR))<br>
- [Directory Traversal Attacks](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%205%3A%20Identifying%20a%20Directory%20Traversal%20Attack)<br>
- [🧠 Key Takeaways](https://github.com/Tmitchy/Web-Application-Security-Assessment#:~:text=of%20dynamic%20includes-,%F0%9F%A7%A0%20Key%20Takeaways,-Most%20attacks%20rely)<br>

---

**Part 1: Identifying an SQL Attack**

<img width="1413" height="275" alt="image" src="https://github.com/user-attachments/assets/8d429e79-ef61-48d7-b2a0-abdb60c9f036" /><br>

To detect SQL injection attempts, I followed a structured analysis process on an access log:

1 - Inspected user inputs for SQL keywords:<br>
I began by reviewing input fields and request parameters for common SQL injection indicators such as SELECT, AND, and UNION, which attackers frequently use to manipulate database queries.

2 - Checked for encoded payloads:<br>
I analyzed inputs for encoding techniques like percent encoding (e.g., %27 for ') that can be used to obfuscate malicious SQL statements and bypass basic filters.

3 - Monitored request patterns and timing:<br>
I tracked the frequency and timing of requests, focusing on repeated or abnormal query patterns that could indicate automated injection attempts.

4 - Correlated activity with a specific source:<br>
I identified a suspicious IP address (192.168.31.167) and monitored its behavior across multiple requests to determine whether it was attempting exploitation.

5 - Analyzed user-agent data:<br>
By reviewing the user-agent string, I assessed the browser and operating system being used. This helped determine whether the activity was likely automated (e.g., scripts/tools) or manual.

6 - Evaluated potential impact:<br>
Based on the payload patterns and responses, I assessed whether the attacker was attempting to extract, modify, or bypass access to database records.

7 - Took mitigation steps:<br>
To address the risk, I recommended/implemented:<br>
   - Input validation and parameterized queries (prepared statements)<br>
   - Web application firewall (WAF) rules to filter malicious patterns<br>
   - Blocking or rate-limiting suspicious IP activity<br>
   - Continuous monitoring for similar injection attempts<br>
     
---

**Part 2: Identifying an XSS Attack**

<img width="1420" height="363" alt="image" src="https://github.com/user-attachments/assets/44c6b56f-18d7-4b6f-97cb-dd8f8aaa37c6" /><br>


To detect XSS code injection attempts, I followed a structured analysis process on an access log:

1 - Inspected request parameters:<br>
I analyzed incoming requests for suspicious input, focusing on common XSS indicators such as <script> tags and JavaScript functions like alert().

2 - Analyzed request patterns and timing:<br>
I reviewed timestamps and request frequency to identify repeated attempts or unusual behavior from specific sources.

3 - Investigated encoding and special characters:<br>
I checked for encoded payloads (e.g., %3Cscript%3E) and unusual character patterns used to bypass input filters.

4 - Correlated activity with source IP:<br>
I identified a suspicious IP address (192.168.31.183) and tracked its interaction with the application.

5 - Confirmed successful exploitation<br>
As shown in Figure 2, the attacker successfully executed an XSS payload, gaining access to browser cookies, which may expose session data.

6 - Recommended mitigation steps:<br>

   - Enforce input validation and output encoding
   - Implement Content Security Policy (CSP)
   - Block or restricted malicious IP activity
   - Enable continuous monitoring for similar patterns

---

**Part 3: Identifying a Command Injection Attack**

<img width="1412" height="269" alt="image" src="https://github.com/user-attachments/assets/82b7f538-e53d-4f15-8c05-4ae0eac9e38b" /><br>

To detect command injection attempts, I followed a structured analysis process on an access log:

1 - Inspected inputs for command patterns:<br>
I reviewed request parameters for OS command indicators such as ;, &&, |, and keywords like whoami, ls, dir, or cat, which are often used to chain or execute system commands.

2 - Checked for encoded payloads:<br>
I analyzed encoded inputs (e.g., %3B, %26%26) to detect obfuscated command injection attempts.

3 - Monitored abnormal application behavior:<br>
I looked for unexpected system-level responses (e.g., command output in responses, delays, or errors) that could indicate command execution.

4 - Correlated activity with source IP:<br>
I tracked repeated suspicious requests from a specific IP to identify a potential attacker.

5 - Assessed execution success:<br>
I evaluated whether the payload resulted in command execution or system-level interaction.

6 - Recommended mitigation steps:<br>

   - Avoide direct system command execution from user input
   - Implemente strict input validation and sanitization
   - Use safe APIs instead of shell commands
   - Apply least-privilege principles on the server

---

**Part 4: Identifying an Insecure Direct Object Reference (IDOR)**

<img width="1377" height="323" alt="image" src="https://github.com/user-attachments/assets/f633444a-f33e-46f3-8035-53319e79a9e7" /><br>


To detect ID0R injection attempts, I followed a structured analysis process on an access log:

 1 - Analyzing the parameters :<br>
I analyzed the request parameters for user IDs.

2 - Analyzed the pages:<br>
I Looked at the number of requests made to the same page, trying to find a pattern.

3 - Analyzed the IP adress:<br>
I conducted an analysis of the IP address to ascertain whether it had requested multiple pages through parameter manipulation. My findings revealed that the same IP address, 192.168.31.174, requested and successfully accessed various user IDs (object Identifiers) by altering the numerical value within the parameters. This observation highlights potential security vulnerabilities related to user authentication and access controls, suggesting a need for stricter validation mechanisms to prevent unauthorized data access.

4 -Recommended mitigation steps:<br>

   - Enforce server-side access control checks
   - Use indirect references (e.g., UUIDs)
   - Validate user permissions on every request

---

**Part 5: Identifying a Directory Traversal Attack**

<img width="1410" height="217" alt="image" src="https://github.com/user-attachments/assets/3b5ecfff-fe3e-4d5d-91da-d05a78896d5e" /><br>


To detect Directory Traversal Attack, I followed a structured analysis process on an access log:

1 - Inspected file path inputs
I analyzed parameters for traversal patterns like ../ used to access restricted directories.

2 - Checked encoded variations
I reviewed encoded payloads (e.g., %2E%2E%2F) to detect bypass attempts.

3 - Monitored file access behavior
I looked for attempts to access sensitive files (e.g., system configs).

4 - Correlated suspicious requests
I tracked repeated attempts from specific IPs targeting file paths.

5 - Assessed access success
I evaluated whether unauthorized files were exposed.

6 - Recommended mitigation steps:<br>

   - Validate and sanitize file paths
   - Restrict access to specific directories
   - Use secure file handling mechanisms
   - Use a secure regular expression to detect payloads e.g /^.*"GET.*\?.*=(.+?(?=%2e%2e%2fetc%2f)).+?.*HTTP\/.*".*$/gm
   - look out for unicode encode characters e.g  / = %c0af

---

**Part 6: Identifying an XML External Entity (XXE) Attack**

<img width="1400" height="292" alt="image" src="https://github.com/user-attachments/assets/bacda878-4402-4529-8e94-4ddc7bb5a44b" /><br>


To detect XXE Attack, I followed a structured analysis process on an access log:

1 - Inspected XML inputs:<br>
I analyzed XML-based requests for the presence of DOCTYPE declarations and <!ENTITY> definitions, which are commonly used in XXE attacks.

2 - Checked for external entity references:<br>
I looked for references to local or remote resources (e.g., file:///, http://) that could indicate attempts to access sensitive files or external systems.

3 - Analyzed encoded and obfuscated payloads:<br>
I reviewed encoded XML inputs to detect hidden or obfuscated entity definitions designed to bypass validation.

4 - Monitored application responses:<br>
I examined responses for signs of sensitive data exposure, such as system file contents or unexpected output from parsed entities.

5 - Correlated activity with source IP:<br>
I tracked repeated XML-based requests from specific IP addresses attempting to exploit parsing behavior.

6- Assessed exploitation impact:<br>
I evaluated whether the attack could lead to data exfiltration, server-side request forgery (SSRF), or denial of service.

7 -  Recommended mitigation steps:<br>

   - Disable external entity processing in XML parsers
   - Use secure parsing libraries and configurations
   - Validate and sanitize XML input
   - Replace XML with safer formats like JSON where possible

---

**Part 7: Identifying a Brute Force Attack**

<img width="1411" height="647" alt="image" src="https://github.com/user-attachments/assets/e70255ce-5c32-4bae-836e-002827d5ba06" /><br>


To detect Brute Force Attack, I followed a structured analysis process on an access log:

1 - Monitored authentication requests:<br>
I analyzed login endpoints (IP address) for repeated authentication attempts, focusing on high-frequency login requests targeting the same or multiple accounts.

2 - Tracked failed vs. successful attempts:<br>
I reviewed logs to identify patterns of multiple failed logins followed by a potential success, which can indicate password guessing.

3 - Analyzed request timing and rate:<br>
I examined the speed and volume of requests to detect automated attacks, as brute force attempts typically occur in rapid succession.

4 - Correlated activity with source IP:<br>
I identified suspicious IP addresses making repeated login attempts across accounts or targeting a single account aggressively.

5 - Reviewed user-agent data:<br>
I checked the user-agent strings to determine whether the activity was likely automated (scripts/tools) or manual.

6 - Assessed account impact:<br>
I evaluated whether any accounts were successfully compromised due to weak credentials or lack of protection mechanisms. My analysis confirmed that the IP address 146.24.173.240 accessed the login page for victim.com and successfully gained access to the site through postID = 3 after several brute-force attempts, as shown in the image above.
 

7 - Recommended mitigation steps:<br>

   - Implement rate limiting and login throttling
   - Enforce account lockout after multiple failed attempts
   - Enable multi-factor authentication (MFA)
   - Monitor and block suspicious IP activity

---

**Part 8: Identifying an Open Redirection Attack**

<img width="1412" height="397" alt="image" src="https://github.com/user-attachments/assets/56234519-8ec9-4eb5-99f4-8313b6332745" />


To detect Open Redirection Attack, I followed a structured analysis process on an access log:

1 - Inspected redirect parameters:<br>
I analyzed request parameters commonly used for redirection (e.g., redirect=, url=, next=) to identify inputs that could be manipulated to send users to unintended destinations.

2 - Checked for external URL patterns:<br>
I looked for full URLs (http://, https://) within these parameters, which may indicate attempts to redirect users to malicious external sites.

3 - Correlated suspicious activity with source IP:<br>
I tracked repeated attempts from specific IP addresses trying different redirect values, indicating potential exploitation.

4 - Techniques Analysis:<br>
I analyzied attack techniques used to bypass the WAF or other middleware products, with crafted payload.<br> e.g http://[::]:25/, http://①②⑦.⓪.⓪.⓪, CDIR:
http://127.0.0.0,
 Decimal Bypass:
http://2130706433/ = http://127.0.0.1,
 Hexadecimal Bypass:
http://0x7f000001/ = http://127.0.0.1


5 - Assessed exploitation risk:<br>
I evaluated whether the vulnerability could be used for phishing attacks, user credential theft, or redirecting users to malicious websites.

6 - Recommended mitigation steps:<br>

   - Validate and restricted redirect URLs to trusted domains only
   - Avoid using user-controlled input for redirects
   - Implement allowlists for approved destinations
   - Display warnings for external redirects when necessary
---

**Part 6: Identifying Remote & Local File Inclusion (RFI/LFI)**

<img width="1446" height="319" alt="image" src="https://github.com/user-attachments/assets/5166e3dd-2bc8-48c0-92e4-b40144f486f2" />


To detect RFI/LFI Attack, I followed a structured analysis process on an access log:


1 - Inspected file inclusion parameters:<br>
I analyzed request parameters that handle file loading (e.g., file=, page=, include=) to identify inputs that could be manipulated to include unintended files.

2- Checked for traversal and remote inclusion patterns:<br>
I looked for indicators such as:<br>

Directory traversal: ../<br>
Remote URLs: http://, https://<br>
These patterns often signal LFI (local access) or RFI (external file inclusion) attempts.

3 - Analyzed encoded payloads:<br>
I reviewed encoded inputs (e.g., %2E%2E%2F, %68%74%74%70) to detect obfuscated inclusion attempts designed to bypass filters.

4 - Monitored application responses:<br>
I examined server responses for signs of file exposure, such as:<br>

System file contents (e.g., /etc/passwd)<br>
Unexpected HTML or script output from external sources

5 - Correlated activity with source IP:<br>
I tracked repeated attempts from specific IP addresses trying different file paths or remote URLs.

6 - Assessed exploitation success:<br>
I evaluated whether the attacker successfully included local or remote files, potentially leading to sensitive data exposure or remote code execution. My analysis confirmed that the IP address 192.168.31.174 successfully gained access to files such as 'index.php' and 'text.php',and a directory '../etc.passwd' as shown in the image above.

7 - Recommended mitigation steps:<br>

   - Restrict file inclusion to a whitelist of allowed files
   - Disable remote file inclusion settings where not required
   - Implement strict input validation and sanitization
   - Use secure file handling methods instead of dynamic includes

---

🧠 **Key Takeaways**
- Most attacks rely on input manipulation + poor validation
- Encoding and obfuscation are common attacker techniques
- Monitoring patterns, timing, and behavior is critical
- Security improves when combining:<br>
      - Secure coding practices<br>
      - Real-time monitoring<br>
      - User-aware defenses<br>

---

🛡️ **Final Thoughts**

This project demonstrates a structured, repeatable approach to identifying and mitigating common web vulnerabilities. By focusing on both technical patterns and attacker behavior, it strengthens overall application resilience against real-world threats.
