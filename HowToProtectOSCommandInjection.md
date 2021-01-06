# Prevent OS command injection vulnerabilities in Python by shlex.quote()

ปกติการเขียนโปรแกรมนั้น สิ่งแรกที่คนเขียนโปรแกรมจะคิดถึงเป็นส่วนใหญ่ ก็คือ โปรแกรมต้องทำงานตาม requirement ที่ได้รับเป็นอันดับแรก เช่นโปรแกรมตามด้านล่างนี้ requirement ก็คือการรับค่า domain name จากผู้ใช้งาน และนำไปหาค่า IP address ของ Domain นั้นๆ มาแสดง

***Source code***

```python
import subprocess

domain_name = input("Enter your domain name: ")

command = f'nslookup {domain_name}'

response = subprocess.check_output(command, shell=True, encoding='UTF-8')
print(response)
```

***Program result***

```sh
Enter your domain name: google.com
Server:		115.178.58.26
Address:	115.178.58.26#53

Non-authoritative answer:
Name:	google.com
Address: 216.58.196.46
```

จะเห็นได้ว่า โปรแกรมนั้นสามารถทำงานได้ตาม requirement อย่างถูกต้อง

แต่การเขียน code ที่นำ input จาก user มาต่อเข้ากับ command ของ OS สามารถทำให้เกิดช่องโหว่ OS command injection ได้ ตามตัวอย่างต่อไปนี้

***Program result (OS Command Injection)***

```sh
Enter your domain name: google.com && ls -la
Server:		115.178.58.26
Address:	115.178.58.26#53

Non-authoritative answer:
Name:	google.com
Address: 172.217.174.174

total 8
drwxr-xr-x  5 korn  staff  160 Jan  6 23:29 .
drwxr-xr-x  6 korn  staff  192 Jan  6 23:13 ..
drwxr-xr-x  8 korn  staff  256 Jan  6 23:29 .idea
-rw-r--r--  1 korn  staff  197 Jan  6 23:29 os_command_injection_sample.py
drwxr-xr-x  6 korn  staff  192 Jan  6 23:14 venv
```

จะเห็นได้ว่า เมื่อทำการเพิ่ม ***"&& ls -la"*** ที่เป็น OS command ต่อจาก domain name ที่เป็น input ปกติ โปรแกรมจะทำการ list directory ออกมาหลังจากที่ทำงาน nslookup เรียบร้อยแล้ว

วันนี้เรามีทางออกมานำเสนอ โดยการใช้ function shlex.quote() โดยเมื่อนำมาใช้แล้ว หน้าตาของ code ก็จะประมาณนี้
และเมื่อทำการทดสอบ 

***Source code (with shlex)***

```python
import subprocess
import shlex

domain_name = input("Enter your domain name: ")

command = f'nslookup {shlex.quote(domain_name)}'

response = subprocess.check_output(command, shell=True, encoding='UTF-8')
print(response)
```

เมื่อทดสอบโปรแกรมดู จะพบว่า สามารถทำงานได้ถาม requirement ปกติ

***Program result(with shlex)***

```sh
Enter your domain name: google.com
Server:		115.178.58.26
Address:	115.178.58.26#53

Non-authoritative answer:
Name:	google.com
Address: 216.58.221.206
```

และเมื่อทดสอบด้วย OS command injection ดูจะพบว่าโปรแกรม ***พัง...*** ไม่สามารถทำงานได้

***Program result (with shlex and OS Command Injection)***

```sh
Enter your domain name: google.com && ls -la
Traceback (most recent call last):
  File "/Users/korn/PycharmProjects/Code4Sec/os_command_injection_sample.py", line 7, in <module>
    response = subprocess.check_output(command, shell=True, encoding='UTF-8')
  File "/usr/local/Cellar/python@3.9/3.9.0_1/Frameworks/Python.framework/Versions/3.9/lib/python3.9/subprocess.py", line 420, in check_output
    return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
  File "/usr/local/Cellar/python@3.9/3.9.0_1/Frameworks/Python.framework/Versions/3.9/lib/python3.9/subprocess.py", line 524, in run
    raise CalledProcessError(retcode, process.args,
subprocess.CalledProcessError: Command 'nslookup 'google.com && ls -la'' returned non-zero exit status 1.

Process finished with exit code 1
```
### อธิบาย ### 

เนื่องจากการใช้ function shlex.quote() จะทำให้ input ที่เข้าสู่โปรแกรมจะถูกใส่ '' (quote) ทำให้ OS คิดว่า input ที่เข้ามาเป็น string ชุดเดียว ***เสมอ***

จากนั้น คำสั่ง nslookup ก็นำ string ที่ได้รับมานั้น มาใช้ในการหา IP address ซึ่งโดยปกติแล้ว domain name จะไม่มีช่องว่าง ทำให้ คำสั่ง nslookup ไม่สามารถทำงานได้นั่นเอง ***จบ...***


| ***author*** | Thanakorn P. |
| --- | --- |
| ***Date*** | xx-01-2021 |
