# K8s Component

| ***Date*** | 24-10-2022 |
| --- | --- |
| ***Author*** | Thanakorn P. |

### Pod
ส่วนย่อยที่เล็กที่สุดของ K8s 1 pod มีได้ 1 container และแต่ละ pod จะมี ip address เป็นของตัวเอง
### Service
ในกรณีที่ pod ตายไป ip address ของ pod จะเปลี่ยน และจะไม่สามารถติดต่อกับ pod ใหม่ได้ จึงต้องมี Service มาคอยทำ permanent IP address
### Ingress
เป็นการ expose url ให้ User หรือ external สามารถ ติดต่อกับ pod ภายใน cluster ผ่าน service ได้ โดยสามารถจำกัดการเข้าถึง ให้สามารถเข้าถึงได้เพียงบาง service เช่น เรามี 2 service App กับ DB เราไม่อยากให้คนนอกเข้าถึง DB ได้ ก็กำหนดที่ ingress ให้เข้าถึงได้เฉพาะ application เท่านั้น
### ConfigMap
เป็นตัวกำหนดค่า Configuration ให้กับ pod เช่น เราจะใช้ pod application เดิม ไปต่อกับ DB instance ใหม่ ก็สามารถเปลี่ยนค่า config ผ่าน ConfigMap ทำให้ไม่ต้องทำ container ใหม่
### Secret
ใช้เก็บค่าที่เป็นความลับที่ใช้งานกับ application เช่น username, password, certificate, database credential เป็นต้น โดยค่าที่เก็บใน Secret จะถูก encode เป็น base64
### Volumes
ในกรณีที่ application มีการเก็บข้อมูลลงไปใน pod ของตัวเอง เมื่อมันตาย และ restart data ที่อยู่ใน pod จะหายไป จึงจำเป็นต้องมี Volumes เป็นที่เก็บ Data ให้กับ pod โดยจะจัดเก็บลงไปบน Disk ของ host ที่ pod นั้น run
### Deployment
เปรียบเสมือน blueprint สำหรับ pod เราสามารถกำหนดให้ application เราจะมีกี่ replica ซึ่งปกติ เราจะทำงานกับ Deployment เป็นส่วนใหญ่ ไม่ได้ทำงานไปที่ pod โดยตรง
### StatefulSet
จะใช้ในกรณีที่ application เป็นแบบ stateful คือต้องมีการ replicate data ตลอดเวลา เราถึงจะเลือกใช้งาน StatefulSet แต่ผู้สอนแนะนำให้นำ application ที่เป็น stateful ออกจาก K8s cluster เพราะการ manage StatefulSet นั้นทำได้ยาก

