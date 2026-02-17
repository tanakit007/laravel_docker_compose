# Laravel 12 Docker

โปรเจค Laravel 12 ที่รันบน Docker พร้อมด้วย MySQL, Nginx, Redis, phpMyAdmin และ MailHog

## เทคโนโลยีที่ใช้

- **PHP**: 8.2-fpm
- **Laravel**: 12.x
- **MySQL**: 8.0
- **Nginx**: 1.19.8-alpine
- **Redis**: 6.2.1-buster
- **phpMyAdmin**: 5.1.0-apache
- **MailHog**: v1.0.1
- **Node.js**: 20.x
- **Composer**: latest

## โครงสร้างโปรเจค

```
laravel-12-Docker/
├── Dockerfile              # PHP 8.2-fpm configuration
├── docker-compose.yaml     # Docker services configuration
├── nginx/
│   └── conf/
│       └── app.conf        # Nginx configuration
├── mysql/
│   └── data/               # MySQL data (ignored in git)
├── redis/
│   └── data/               # Redis data (ignored in git)
└── src/                    # Laravel application
```

## Ports ที่ใช้งาน

| Service    | Port | URL                      |
|------------|------|-------------------------|
| Nginx      | 8100 | http://localhost:8100   |
| phpMyAdmin | 8200 | http://localhost:8200   |
| MailHog UI | 8025 | http://localhost:8025   |
| MySQL      | 3308 | localhost:3308          |

## การติดตั้งและใช้งาน

### 1. Clone โปรเจค

```bash
git clone https://github.com/thanakorn03/Laravel12-Docker.git
cd Laravel12-Docker
```

### 2. สร้างไฟล์ .env

```bash
cp src/.env.example src/.env
```

### 3. ตั้งค่า .env สำหรับ Docker

แก้ไขไฟล์ `src/.env` ให้มีค่าดังนี้:

```env
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel12db
DB_USERNAME=admin
DB_PASSWORD=1234

REDIS_HOST=redis
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailhog
MAIL_PORT=1025
```

### 4. สร้างและรัน Docker Containers

```bash
docker compose up -d
```

### 5. เข้าไปใน Container และติดตั้ง Dependencies

```bash
docker compose exec app bash
composer install
php artisan key:generate
php artisan migrate
exit
```

### 6. เปิดเว็บไซต์

เปิดเบราว์เซอร์และเข้า: http://localhost:8100

## คำสั่งที่ใช้บ่อย

### เข้าสู่ Container

```bash
# เข้า PHP Container
docker compose exec app bash

# เข้า MySQL Container
docker compose exec db bash
```

### Laravel Artisan Commands

```bash
# Run migrations
docker compose exec app php artisan migrate

# Clear cache
docker compose exec app php artisan cache:clear
docker compose exec app php artisan config:clear
docker compose exec app php artisan view:clear

# Create controller
docker compose exec app php artisan make:controller YourController

# Create model
docker compose exec app php artisan make:model YourModel -m
```

### Composer Commands

```bash
# Install package
docker compose exec app composer require package-name

# Update dependencies
docker compose exec app composer update
```

### Docker Commands

```bash
# เริ่มต้น containers
docker compose up -d

# หยุด containers
docker compose down

# ดูสถานะ containers
docker compose ps

# ดู logs
docker compose logs -f

# Rebuild containers
docker compose up -d --build
```

## ทดสอบฟีเจอร์

### ทดสอบ Redis

```bash
# บันทึกข้อมูลลง Redis
http://localhost:8100/store

# ดึงข้อมูลจาก Redis
http://localhost:8100/retrieve
```

### ทดสอบ Email (MailHog)

```bash
# ส่ง test email
http://localhost:8100/send-email

# ดู email ที่ MailHog UI
http://localhost:8025
```

### เข้าใช้งาน phpMyAdmin

```
URL: http://localhost:8200
Server: db
Username: admin
Password: 1234
```

## การตั้งค่า Database

### ข้อมูลการเชื่อมต่อ MySQL

- **Host** (ภายใน Docker): `db`
- **Host** (จากเครื่อง): `localhost` หรือ `127.0.0.1`
- **Port** (จากเครื่อง): `3308`
- **Database**: `laravel12db`
- **Username**: `admin`
- **Password**: `1234`
- **Root Password**: `1234`

## การแก้ปัญหา

### ไม่สามารถเชื่อมต่อ Database

ตรวจสอบว่าไฟล์ `.env` ใช้:
```env
DB_HOST=db
```
**ไม่ใช่** `DB_HOST=127.0.0.1`

### Redis Connection Refused

ตรวจสอบว่าไฟล์ `.env` ใช้:
```env
REDIS_HOST=redis
```
**ไม่ใช่** `REDIS_HOST=127.0.0.1`

### Port ถูกใช้งานอยู่แล้ว

แก้ไขไฟล์ `docker-compose.yaml` เปลี่ยน port ที่ต้องการ:
```yaml
ports:
  - "8100:80"  # เปลี่ยนเป็น port อื่นได้
```

### Git Error: mysql.sock

ไฟล์ `.gitignore` ได้ตั้งค่าไว้แล้วให้ ignore:
```
/mysql/data/*
/redis/data/*
```

## License

Open-source software licensed under the [MIT license](https://opensource.org/licenses/MIT).

## สิ่งที่ได้จากการเรียน Laravel

- เข้าใจสถาปัตยกรรม MVC และการจัดการ routing, controllers, views
- การใช้งาน Eloquent ORM สำหรับการทำงานกับฐานข้อมูล
- การเขียน migration, seeding และการจัดการ schema
- การใช้งาน Artisan CLI เพื่อเพิ่มประสิทธิภาพการพัฒนา
- การตั้งค่า environment และการจัดการ config
- การทำงานร่วมกับ Redis, MailHog และบริการภายนอกผ่าน queue, mail, cache
- การดีพลอยและการตั้งค่าใน Docker เพื่อสร้างสภาพแวดล้อมที่สอดคล้องกัน

## ผู้พัฒนา

Nitiphon664259010
