# DB SETUP
## 1. ENTER THE MYSQL AS ROOT
```bash
docker exec -it drone-platform-mysql mysql -u root -p
```
## 2. CREATE DB
```bash
CREATE DATABASE drone;
```
## 3. CREATE USER
```bash
CREATE USER IF NOT EXISTS 'YOUR_USERNAME'@'%' IDENTIFIED BY 'YOUR_PASSWORD';
GRANT ALL PRIVILEGES ON drone.* TO 'YOUR_USERNAME'@'%';
FLUSH PRIVILEGES;
```
## 4. ENTER THE MYSQL AS YOUR ACCOUNT
```bash
docker exec -it drone-platform-mysql mysql -u YOUR_MYSQL_USERNAME -p
```
## 1. CREATE DRONE DB
```bash
CREATE DATABASE IF NOT EXISTS drone;
```
---

# DRONE DB SETUP
## 1. USE DB
```bash
USE drone;
```
## 2. CREATE DRONE TABLE
```bash
CREATE TABLE drone (
id BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
serial VARCHAR(50) NOT NULL UNIQUE,
name VARCHAR(50) NOT NULL,
recent_log VARCHAR(255) DEFAULT NULL,
updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
level INT NOT NULL DEFAULT 0,
connecting TINYINT(1) NOT NULL DEFAULT 0,
log_path VARCHAR(255) NOT NULL DEFAULT '/',
last_connect_time TIMESTAMP DEFAULT NULL,
PRIMARY KEY (id)
);
```
## 3. INSERT ACCESS DRONE
```bash
insert into drone(serial, name, recent_log, level, connecting) values('test-serial','drone-test1020','X',1,false);
```
---

# USER DB SETUP
## 1. CREATE MEMBER TABLE
```bash
CREATE TABLE member (
    id BIGINT NOT NULL AUTO_INCREMENT,
    username VARCHAR(255),
    password VARCHAR(255),
    role VARCHAR(255),
    state INT DEFAULT 0,
    PRIMARY KEY (id),
    UNIQUE (username)
);
```
## 2. SEND A CURL REQUEST TO THE SERVER TO REGISTER
```bash
curl -X POST http://<your_url>/dashboard/login \
  -H "Content-Type: application/json" \
  -d '{
        "username": "testid",
        "password": "testpw"
      }'
```

## 3. UPDATE THE STATE
```bash
# 0: REJECT 1: PENDING 2: ACCESS
UPDATE member SET state=2 WHERE username='testid';
```