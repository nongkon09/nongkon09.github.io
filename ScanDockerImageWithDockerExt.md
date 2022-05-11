# Docker image Vulnerability scan by Docker desktop

| ***Date*** | 11-05-2022 |
| --- | --- |
| ***Author*** | Thanakorn P. |

เนื่องจากวันนี้ได้ทำการ update docker desktop เป็น version 4.8.1 พบว่ามีการ update feature ใหม่เข้ามาคือ Extensions beta

![](img/DocScanEXT/Screen%20Shot%202565-05-11%20at%2010.22.30.png)

และก็ได้ไปสะดุดตากับ Extensions ที่ชื่อว่า Aqua Trivy

![](img/DocScanEXT/Screen%20Shot%202565-05-11%20at%2011.00.14.png)

หลังจากทำการ install เราจะได้หน้าจอแบบนี้

![](img/DocScanEXT/Screen%20Shot%202565-05-11%20at%2011.10.46.png)

ลองทำการ Scan docker image ที่มีอยู่ในเครื่องดู

![](img/DocScanEXT/Screen%20Shot%202565-05-11%20at%2011.15.24.png)

เราก็จะพบว่า Docker image ที่เราใช้งานมีช่องโหว่อะไรบ้าง ลองไปใช้กันดูนะครับ

![](img/DocScanEXT/Screen%20Shot%202565-05-11%20at%2011.16.17.png)