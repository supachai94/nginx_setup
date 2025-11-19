# Nginx Docker Setup สำหรับ Laravel

## โครงสร้างไฟล์

```
nginx/
├── nginx.conf                  # ไฟล์ config หลัก
├── sites/                      # โฟลเดอร์สำหรับ config แต่ละเว็บไซต์
│   ├── laravel.conf            # Config สำหรับ Laravel (ใช้ PHP 8.2)
│   ├── laravel-php74.conf      # Config สำหรับ Laravel (ใช้ PHP 7.4)
│   ├── laravel-php82.conf      # Config สำหรับ Laravel (ใช้ PHP 8.2)
│   └── example.conf            # ตัวอย่าง config สำหรับเว็บไซต์อื่น
├── docker-compose.yml          # Docker Compose (nginx + PHP 7.4 + PHP 8.2)
└── logs/                       # โฟลเดอร์สำหรับเก็บ log files
```

## วิธีใช้งาน

### รัน stack (nginx + PHP 7.4 + PHP 8.2)

```bash
docker-compose up -d
```

Stack นี้จะรัน:
- **nginx**: Web server
- **php74**: PHP 7.4-FPM
- **php82**: PHP 8.2-FPM

## การเพิ่มเว็บไซต์ใหม่

1. สร้างไฟล์ config ใหม่ในโฟลเดอร์ `sites/`
   ```bash
   cp sites/example.conf sites/mywebsite.conf
   ```

2. แก้ไขไฟล์ `sites/mywebsite.conf`:
   - เปลี่ยน `server_name` เป็น domain ของคุณ
   - เปลี่ยน `root` เป็น path ของโปรเจกต์
   - ปรับ `access_log` และ `error_log` ให้เป็นชื่อเฉพาะ
   - เลือก PHP version ที่ต้องการ:
     - `fastcgi_pass php74:9000;` สำหรับ PHP 7.4
     - `fastcgi_pass php82:9000;` สำหรับ PHP 8.2

3. Restart nginx container
   ```bash
   docker-compose restart nginx
   ```

## การเลือกใช้ PHP Version

ในแต่ละเว็บไซต์ config คุณสามารถเลือกใช้ PHP version ที่ต้องการได้:

### ใช้ PHP 7.4
```nginx
location ~ \.php$ {
    fastcgi_pass php74:9000;
    # ... config อื่นๆ
}
```

### ใช้ PHP 8.2
```nginx
location ~ \.php$ {
    fastcgi_pass php82:9000;
    # ... config อื่นๆ
}
```

### ตัวอย่าง Config ที่มีอยู่แล้ว
- `laravel.conf` - ใช้ PHP 8.2 (default)
- `laravel-php74.conf` - ใช้ PHP 7.4
- `laravel-php82.conf` - ใช้ PHP 8.2

## หมายเหตุ

- ไฟล์ config ในโฟลเดอร์ `sites/` จะถูก include อัตโนมัติโดย nginx config หลัก
- ไฟล์ config หลัก (`nginx.conf`) กำหนด global settings
- แต่ละเว็บไซต์มี config แยกในโฟลเดอร์ `sites/`
- แต่ละเว็บไซต์สามารถเลือกใช้ PHP version ที่แตกต่างกันได้
- Log files จะถูกเก็บในโฟลเดอร์ `logs/`

