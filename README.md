# Nginx Docker Setup สำหรับ Laravel

## โครงสร้างไฟล์

```
nginx/
├── nginx.conf                  # ไฟล์ config หลัก
├── sites/                      # โฟลเดอร์สำหรับ config แต่ละเว็บไซต์
│   ├── laravel8.conf           # Laravel 8 -> PHP 7.4
│   ├── laravel9.conf           # Laravel 9 -> PHP 8.1
│   └── laravel10.conf          # Laravel 10 -> PHP 8.4
├── php74/                      # PHP-FPM 7.4 image config
├── php81/                      # PHP-FPM 8.1 image config
├── php82/                      # PHP-FPM 8.2 image config
├── php84/                      # PHP-FPM 8.4 image config
├── docker-compose.yml          # Docker Compose stack
└── logs/                       # โฟลเดอร์สำหรับเก็บ log files
```

## วิธีใช้งาน

### รัน stack

```bash
docker compose up -d
```

Stack นี้จะรัน:
- **nginx**: Web server
- **php74**: PHP 7.4-FPM
- **php81**: PHP 8.1-FPM
- **php82**: PHP 8.2-FPM
- **php84**: PHP 8.4-FPM

## การเพิ่มเว็บไซต์ใหม่

1. สร้างไฟล์ config ใหม่ในโฟลเดอร์ `sites/`
   ```bash
   cp sites/laravel10.conf sites/mywebsite.conf
   ```

2. แก้ไขไฟล์ `sites/mywebsite.conf`:
   - เปลี่ยน `server_name` เป็น domain ของคุณ
   - เปลี่ยน `root` เป็น path ของโปรเจกต์
   - ปรับ `access_log` และ `error_log` ให้เป็นชื่อเฉพาะ
   - เลือก PHP version ที่ต้องการ:
     - `fastcgi_pass php74:9000;` สำหรับ PHP 7.4
     - `fastcgi_pass php81:9000;` สำหรับ PHP 8.1
     - `fastcgi_pass php82:9000;` สำหรับ PHP 8.2
     - `fastcgi_pass php84:9000;` สำหรับ PHP 8.4

3. Restart nginx container
   ```bash
   docker compose restart nginx
   ```

## การเลือกใช้ PHP Version

### ใช้ PHP 7.4
```nginx
location ~ \.php$ {
    fastcgi_pass php74:9000;
}
```

### ใช้ PHP 8.1
```nginx
location ~ \.php$ {
    fastcgi_pass php81:9000;
}
```

### ใช้ PHP 8.2
```nginx
location ~ \.php$ {
    fastcgi_pass php82:9000;
}
```

### ใช้ PHP 8.4
```nginx
location ~ \.php$ {
    fastcgi_pass php84:9000;
}
```

## ตัวอย่าง Config ที่มีอยู่แล้ว
- `laravel8.conf` - ใช้ PHP 7.4
- `laravel9.conf` - ใช้ PHP 8.1
- `laravel10.conf` - ใช้ PHP 8.4

## หมายเหตุ

- ไฟล์ config ในโฟลเดอร์ `sites/` จะถูก include อัตโนมัติโดย nginx config หลัก
- ไฟล์ config หลัก (`nginx.conf`) กำหนด global settings
- แต่ละเว็บไซต์มี config แยกในโฟลเดอร์ `sites/`
- แต่ละเว็บไซต์สามารถเลือกใช้ PHP version ที่แตกต่างกันได้
- Log files จะถูกเก็บในโฟลเดอร์ `logs/`

