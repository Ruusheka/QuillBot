# Deployment Guide (Ubuntu)

## 1. Install Dependencies
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3-pip python3-venv python3-dev default-libmysqlclient-dev build-essential mysql-server nginx -y
```

## 2. Configure MySQL
```sql
CREATE DATABASE electrical_db;
CREATE USER 'django_user'@'localhost' IDENTIFIED BY 'secure_password';
GRANT ALL PRIVILEGES ON electrical_db.* TO 'django_user'@'localhost';
FLUSH PRIVILEGES;
```

## 3. Setup Project
```bash
mkdir -p /var/www/electrical_machines_qna
cd /var/www/electrical_machines_qna
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## 4. Run Migrations & Static
```bash
python manage.py makemigrations qna
python manage.py migrate
python manage.py loaddata fixtures.json
python manage.py collectstatic
```

## 5. Gunicorn & Nginx Setup
* Create a systemd service for Gunicorn
* Configure Nginx to proxy to Gunicorn socket
