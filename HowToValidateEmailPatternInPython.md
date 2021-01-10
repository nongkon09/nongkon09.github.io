# How to validate E-mail pattern in python

![](img/regex.png)

Credit Img : [Fireship.io](https://fireship.io/)

| ***Date*** | 12-01-2021 |
| --- | --- |
| ***Author*** | Thanakorn P. |

วันนี้จะมาทดลองทำ input validation โดยใช้ regular expression ผ่าน library ของ python ตัวหนึ่งที่ชื่อว่า re

การใช้งาน re module จะเริ่มจากการกำหนด pattern ของ E-mail ในรูปแบบ regular expression ซึ่งโดยปกติแล้ว E-mail pattern จะมีรูปแบบคือ [e-mail user]@[domain name] 

จากนั้นใช้งาน function re.search() ทำการหา pattern ที่เรากำหนดไว้ ตรวจหา input ที่รับเข้ามา ว่าตรงตาม pattern ที่กำหนดไว้หรือไม่ ตาม code ตัวอย่าง ด้านล่าง

***Sample code***

```python
import re
# prepare e-mail pattern
email_regex = '[a-z0-9]+[\._]?[a-z0-9]+[@]\w+[.]\w'


def check_email_pattern(email):
    if re.search(email_regex, email):
        print('E-mail pattern is valid.')
    else:
        print('E-mail pattern is not valid.')


email1 = 'nongkon.github.io'
check_email_pattern(email1)  # check e-mail pattern in email1

email2 = 'abcd1234@test.com'
check_email_pattern(email2)

email3 = 'abcd1234@test.in.th'
check_email_pattern(email3)

injection = "' OR 1 --"
check_email_pattern(injection)
```

***Result***
```python
E-mail pattern is invalid.
E-mail pattern is valid.
E-mail pattern is valid.
E-mail pattern is invalid.
```

จาก Result จะเห็นได้ว่าการใช้ Regular expression นั้นมีประโยชน์หลายด้าน ไม่ว่าจะเป็นในด้านการนำ input ไปใช้งาน ทำให้ได้ input ที่ตรงตามความต้องการของโปรแกรมมากขึ้น ช่วยป้อง input ที่มีความมุ่งร้ายต่อโปรแกรม อธิเช่น การทำ [Injection](https://owasp.org/www-project-top-ten/2017/A1_2017-Injection) ต่างๆ

ref : []()