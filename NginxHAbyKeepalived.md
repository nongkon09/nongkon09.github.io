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

ทำการสร้างหน้า web ให้กับ NGINX ด้วยคำสั่ง (ในคำสั่งจะเป็นของเครื่อง A ถ้าทำเครื่อง B ก็ให้ เปลี่ยนจาก A เป็น B นะครับ จะได้สังเกตุได้ง่าย)

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

Keepalived MASTER server config

```
global_defs {
  # Keepalived process identifier
  router_id nginx
} # Script to check whether Nginx is running or not
vrrp_script check_nginx {
  script "/bin/check_nginx.sh"
  interval 1
  weight 50
} # Virtual interface - The priority specifies the order in which the assigned interface to take over in a failover
vrrp_instance VI_01 {
  state MASTER
  interface ens160
  virtual_router_id 151
  priority 110   # The virtual ip address shared between the two NGINX Web Server which will float
  virtual_ipaddress {
    192.168.48.100/24
  }
  track_script {
    check_nginx
  }
  authentication {
    auth_type AH
    auth_pass secret
  }
}
```

Keepalived BACKUP server config

```
global_defs {
  # Keepalived process identifier
  router_id nginx
} # Script to check whether Nginx is running or not
vrrp_script check_nginx {
  script "/bin/check_nginx.sh"
  interval 1
  weight 50
} # Virtual interface - The priority specifies the order in which the assigned interface to take over in a failover
vrrp_instance VI_01 {
  state BACKUP
  interface ens160
  virtual_router_id 151
  priority 100   # The virtual ip address shared between the two NGINX Web Server which will float
  virtual_ipaddress {
    192.168.48.100/24
  }
  track_script {
    check_nginx
  }
  authentication {
    auth_type AH
    auth_pass secret
  }
}
```