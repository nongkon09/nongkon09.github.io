# NGINX HA by Keepalived

| ***Date*** | 17-7-2022 |
| --- | --- |
| ***Author*** | Thanakorn P. |

เนื่องจากได้ยินมาบ่อยๆ ว่า NGINX ทำ HA ไม่ได้ ต้องใช้งานเป็น NGINX+ เลยเกิดความสงสัย มันจะไม่ได้เลยจริงๆ หรอ ผมเลยได้ทำการ research และก็พบว่ามีวิธีทำได้ โดยใช้ keepalived มาช่วย โดยหลักการทำงานของ keepalived จะมาช่วยทำ IP ที่เป็น VIP ให้กับ NGINX ทั้ง 2 เครื่อง ทำให้ เกิดการ HA ได้ วันนี้เลยจะมาลองทำกันดูครับ โดยมีรูปแบบการทำงานตาม Diagram ด้านล่างนี้

![](img/NginxHA/nginxha1.png)

และ Lab ที่จะใช้กันในวันนี้จะมี spec ดังนี้ครับ

| Nginx and Keepalived server spec| |
| --- | --- |
| OS | Rocky Linux 8.6 (minimal install) 
| CPU | 1 core |
| Memory | 2 GB |
| Disk | 20 GB |

และมี fix ip address ดังนี้

| IP Address |  |
| --- | --- |
| VIP IP | 192.168.48.100 |
| NGINX A | 192.168.48.101 |
| NGINX B | 192.168.48.102 |

เริ่มแรก เราจะทำการ install package ที่จำเป็นสำหรับ Lab นี้ ลงไปที่ server NGINX ทั้ง 2 เครื่อง โดยใช้ command ดังนี้


```bash
dnf install net-tools epel-release vim nginx keepalived -y
```

ทำการสร้างหน้า web ให้กับ NGINX ด้วยคำสั่ง (ในคำสั่งจะเป็นของเครื่อง A ถ้าทำเครื่อง B ก็ให้ เปลี่ยนจาก A เป็น B นะครับ จะได้สังเกตุได้ง่ายตอนทดสอบ HA)

```bash
echo "<h1>NginX HA by Keepalived A</h1>" > /usr/share/nginx/html/index.html
```

ทำการ start service nginx และ allow firewall ให้สามารถเข้าดู web ของ NGINX ได้

```bash
systemctl enable nginx
systemctl start nginx
firewall-cmd --permanent --add-service=http
firewall-cmd --reload
```

ลองเข้า web ทดสอบ ด้วย url http://192.168.48.101 ดู ก็จะได้หน้าตาประมาณนี้

![](img/NginxHA/nginxha2.png)

ทำการ backup config เดิมของ keepalived

```bash
mv /etc/keepalived/keepalived.conf /etc/keepalived/keepalived.conf.org
```

สร้าง config ให้กับ keepalived ใหม่

```bash
vim /etc/keepalived/keepalived.conf
```

และใส่ config ด้านล่างนี้เข้าไป สำหรับเครื่องที่เป็น MASTER (เปลี่ยนตรง virtual_ipaddress เป็น ip ที่จะใช้เป็น VIP)

```
vrrp_script chk_nginx {
    script "pidof nginx"
    interval 2
}

vrrp_instance VI_1 {
    interface ens160
    state MASTER
    priority 200

    virtual_router_id 33
    virtual_ipaddress {
      192.168.48.100/24
    }
    authentication {
        auth_type AH
        auth_pass password
    }

    track_script {
        chk_nginx
    }

}
```

และใส่ config ด้านล่างนี้เข้าไป สำหรับเครื่องที่เป็น BACKUP (เปลี่ยนตรง virtual_ipaddress เป็น ip ที่จะใช้เป็น VIP)

```
vrrp_script chk_nginx {
    script "pidof nginx"
    interval 2
}

vrrp_instance VI_1 {
    interface ens160
    state BACKUP
    priority 100

    virtual_router_id 33
    virtual_ipaddress {
      192.168.48.100/24
    }
    authentication {
        auth_type AH
        auth_pass password
    }

    track_script {
        chk_nginx
    }

}
```

ทำการ allow firewall และ start service ให้ keepalived 

```sh
firewall-cmd --add-rich-rule='rule protocol value="vrrp" accept' --permanent
firewall-cmd --reload
systemctl enable keepalived
systemctl start keepalived
```

จากนั้นทำการ ทดสอบเข้าหน้า web ผ่าน VIP จะเห็นว่าตอนนี้ชี้ไปยังเครื่อง NGINX A

![](img/NginxHA/nginxha3.png)

ทดสอบการ HA ด้วยการ หยุดการทำงานของ NGINX

```bash
systemctl stop nginx
```

จะพบว่า VIP ได้ชี้ไปยัง NGINX B

![](img/NginxHA/nginxha4.png)
