# How to enable Two-Factor authentication on GitLab.com
***

| ***Date*** | 20-12-2020 |
| ---- | ---- |
| ***Author*** | Thanakorn Prajuabkamon |
|  | Vatcharin Kongsakul |

***

* ทำการ login เข้า Gitlab และไปที่ Setting --> Account จะพบ Button ให้ทำการ enable Two-Factor Authentication

![](img/Enable2factor-1.jpg)

* หลังจากกด button "Enable two-factor authentication แล้ว ระบบจะทำการ Generate QR code มาให้

![](img/Enable2factor-2.jpg)

* ให้นำ application ที่จะในการทำ Multi-factor application (ใน KB นี้ใช้ Application Microsoft Authenticator บน IOS ) มา scan เพื่อจับคู่ Gitlab account และ Multi-factor applicationin เริ่มจากการเปิด Application Microsoft Authenticator และกดปุ่มสัญลักษณ์ "+"

![](img/Enable2factor-3.jpg)

* เลือก Other

![](img/Enable2factor-4.jpg)

* ทำการ scan QR code ที่แสดงใน GitLab.com

![](img/Enable2factor-5.jpg)

* เมื่อทำการจับคู่เรียบร้อย ให้นำ passcode ของ account GitLab ไปใส่ในหน้า setting ของ GitLab.com

![](img/Enable2factor-6.jpg)

* นำ Passcode จาก Microsoft Authenticator มาใส่ และกด "Register with two-factor app"

![](img/Enable2factor-7.jpg)

* ให้ทำการสำรองข้อมูล recovery code เผื่อในกรณีฉุกเฉิน เป็นอันสิ้นสุดวิธีการ Enable Two-Factor Authentication บน GitLab.com

![](img/Enable2factor-8.jpg)