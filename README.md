# Web-Application-Security-Assessment
Web applications are crucial interfaces that connect users to various services over the internet. Due to their widespread use across organizations, these applications are susceptible to exploitation by malicious actors. Such vulnerabilities pose significant risks, including unauthorized access to devices, theft of sensitive personal data, and disruptions in service continuity. These negative outcomes can result in substantial financial losses for affected entities.
 
To mitigate these risks, it is essential to implement robust detection mechanisms that can identify and respond to web-based attacks. This involves a combination of preventive measures, monitoring tools, and analytical techniques designed to safeguard web applications and their users. Understanding the various types of web attacks, along with their methodologies, is vital for developing effective defense strategies and ensuring the integrity and security of web services.

---

📌 **Overview**<br>

This project documents a structured security assessment of common web application vulnerabilities, including:

- XSS Detection<br>
- SQL Injection Detection<br>
- Command Injection<br>
- Open Redirection<br>
- Brute Force Attacks<br>
- XXE (XML) Attacks<br>
- File Inclusion (RFI/LFI)<br>
- IDOR<br>
- Directory Traversal Attacks<br>

The objective was to identify attack patterns, trace malicious activity, and implement mitigation strategies to improve overall application security.

---

📚 **Table of Contents**<br>

- [SQL Injection Detection](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%201%3A%20Identifying%20an%20SQL%20Attack)<br>
- [XSS Detection](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%202%3A%20Identifying%20an%20XSS%20Attack)<br>
- [Command Injection](https://github.com/Tmitchy/Web-Application-Security-Assessment/blob/main/README.md#:~:text=Part%203%3A%20Identifying%20a%20Command%20Injection%20Attack)<br>
- Open Redirection<br>
- Brute Force Attacks<br>
- XXE (XML) Attacks<br>
- File Inclusion (RFI/LFI)<br>
- IDOR<br>
- Directory Traversal Attacks<br>
- 🧠 Key Takeaways<br>

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

6 - Mitigation steps<br>

   - Enforced input validation and output encoding
   - Implemented Content Security Policy (CSP)
   - Blocked or restricted malicious IP activity
   - Enabled continuous monitoring for similar patterns

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

6 - Mitigation steps:<br>

   - Avoided direct system command execution from user input
   - Implemented strict input validation and sanitization
   - Used safe APIs instead of shell commands
   - Applied least-privilege principles on the server

---

**Part 5: Identifying an Insecure Direct Object Reference (IDOR)**
Approach

1. Tested object references:<br>
I manipulated parameters such as user IDs in URLs or requests to check for unauthorized access.

2. Analyzed access control behavior:<br>
I verified whether the application properly restricted access to resources based on user roles.

3. Monitored response differences:<br>
I compared responses when accessing authorized vs. unauthorized resources.

4. Identified predictable patterns:<br>
I checked if object identifiers were sequential or guessable.

5. Assessed exposure risk:<br>
I evaluated whether sensitive data could be accessed without proper authorization.

6. Mitigation steps<br>

     - Enforced server-side access control checks
     - Used indirect references (e.g., UUIDs)
     - Validated user permissions on every request
