# K8s Architecture

| ***Date*** | 24-10-2022 |
| --- | --- |
| ***Author*** | Thanakorn P. |

### Worker node
Worker มีไว้เพื่อ run pod ใน 1 worker machine สามารถมีได้หลาย pod ในแต่ละ work machince จะมี 3 processes หลักๆ ดังนี้
* Container runtime เพื่อใช้สำหรับ run container
* Kubelet ใช้สำหรับ ติดต่อระหว่าง container กับ node และ kubelet เป็นคน start pod
* Kube Proxy ใช้สำหรับ forward request จาก service มายัง pod
### Master node
เมื่อ user หรือ admin ต้องการ deploy application ใหม่ จะต้องทำผ่าน Master node ที่ Master node จะมี API server, K8s dashboard และที่ Master node โดย Master node จะประกอบไปด้วย Processes ดังนี้
* API Server 
  * ใช้สำหรับสร้าง component ต่างๆ ของ K8s Cluster ดู health ของ cluster ก็ทำผ่าน API Server
* Scheduler 
    * ใช้สำหรับการสร้าง pod ใหม่ scheduler จะเป็นคนตัดสินใจว่า pod ควรจะไป start ที่ worker node ไหน โดย scheduler จะ check available ของ node และคุยกับ kubelet
* Controller manager 
  * จะคอยเช็ค health ของ pod ว่า crash หรือไม่ ถ้ามี Controller manager จะไปสั่งให้ scheduler ไปสั่ง kubelet ทำการ restart pod ให้กลับมาใช้ได้เหมือนเดิม
* etcd
    * key value store ของ cluster ทำหน้าที่เหมือนสมองของ cluster มีหน้าที่เก็บค่าการเปลี่ยนแปลงทั้งหมดของ cluster และให้ control manager และ scheduler เอาไปใช้ ยกตัวอย่างเช่น pod ตาย etcd จะเก็บ state และ controller manager จะรู้ว่า pod ตาย โดยอ่านข้อมูลจาก etch หรือ Worker node มี available เท่าไหร่ ก็จะเขียนลงใน etcd และ scheduler ก็จะรู้ว่า ควรจะ start pod ที่ worker node ตัวไหน