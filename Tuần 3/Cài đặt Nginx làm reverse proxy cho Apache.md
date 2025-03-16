# Các bước cài Nginx làm reverse proxy cho apache trên Ubuntu 20

**Cài đặt apache2:**
```
sudo apt update
sudo apt install apache2
```
**Cài đặt PHP7.4**
```
sudo apt -y install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt -y install php7.4
sudo apt-get install -y php7.4-cli php7.4-json php7.4-common php7.4-mysql php7.4-zip php7.4-gd php7.4-mbstring php7.4-curl php7.4-xml php7.4-bcmath
php -v
```
**Chỉnh sửa port cho apache để nó listen ở port 8080 và 8081 trong file /etc/apache2/ports.conf:**

![image](https://github.com/user-attachments/assets/f8947eaf-9497-4e49-86d7-0a92887b80d0)

**Tạo 2 site cho 2 trang web wordpress và laravel:**

![image](https://github.com/user-attachments/assets/91bfd89d-d3ce-403d-a984-c2528af5bd5e)

![image](https://github.com/user-attachments/assets/5e1adf7e-19c2-4e83-a2ef-1fe899ea34fd)


**Run lại apache cho nó đổi port listen:**
```
systemctl restart apache2
```
**Tạo 2 directory /var/www/html/wordpress và /var/www/html/laravel và tải 2 framework wordpress và laravel về**

**Tải mysql:**
```
sudo apt update
sudo apt install mysql-server
sudo systemctl start mysql.service
```

**Tạo database và user cho wordpress:**
```
mysql -u root -p
create database WORDPRESS;
create user 'TRUNGWORDPRESS'@'localhost' IDENTIFIED WITH mysql_native_password BY 'WORDPRESS123@';
GRANT ALL ON `WORDPRESS`.* TO 'TRUNGWORDPRESS'@'localhost';
flush privileges;
```

**Tạo database và user cho laravel:**
```
mysql -u root -p
create database laravel;
create user 'laravel'@'localhost' IDENTIFIED WITH mysql_native_password BY 'laravel123@';
GRANT ALL ON `laravel`.* TO 'laravel'@'localhost';
flush privileges;
```

**Cài đặt Nginx:**
```
sudo apt update
sudo apt install nginx
```

**Xóa file default trong /etc/nginx/site-available và symlink của nó bên enabled**

**Tạo 2 file /etc/nginx/sites-available/wordpress và /etc/nginx/sites-available/nginx:**
```
vim /etc/nginx/sites-available/wordpress:
server {
    server_name wordpress.trung.vietnix.tech;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

     }
     location ~* \.(?:ico|css|js|gif|jpe?g|png|woff2?|eot|ttf|otf|svg|mp4|webp)$ {
        expires 7d;
        log_not_found off;
        access_log off;
      }
    location ~* \.(?:svgz?|ttf|ttc|otf|eot|woff2?)$ {
        add_header Access-Control-Allow-Origin "*";
        expires 7d;
        access_log off;
    }
}
```

```
vim /etc/nginx/sites-available/laravel:
server {
    server_name laravel.trung.vietnix.tech;

    location / {
        proxy_pass http://localhost:8081;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
     location ~* \.(?:ico|css|js|gif|jpe?g|png|woff2?|eot|ttf|otf|svg|mp4|webp)$ {
        expires 7d;
        log_not_found off;
        access_log off;
      }
    location ~* \.(?:svgz?|ttf|ttc|otf|eot|woff2?)$ {
        add_header Access-Control-Allow-Origin "*";
        expires 7d;
        access_log off;
    }
}
```
**Sau khi tạo 2 site bên nginx ta tạo symlink đến /etc/nginx/site-enabled/**
```
ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled/
ln -s /etc/nginx/sites-available/laravel /etc/nginx/sites-enabled/
```

**Tiếp theo cấu hình database phù hợp vào file wp-config.php của wordpress và file .env của laravel (php artisan migrate - để đồng bộ database cho laravel**

**Cuối cùng thực hiện cài SSL cho 2 trang web**
```
certbot --nginx -d wordpress.trung.vietnix.tech
certbot --nginx -d laravel.trung.vietnix.tech
```
Khi cài xong thì 2 file /etc/nginx/sites-available/laravel /etc/nginx/sites-available/wordpress sẽ được certbot generate ra một số trường như dưới đây:
![image](https://github.com/user-attachments/assets/6cda3e03-49e7-43b8-9697-e71ce891de6f)

![image](https://github.com/user-attachments/assets/cf883574-7135-4329-9c46-5ad64281db56)

# Cấu hình Apache chỉ listen trên một port

**Cấu hình site phía Nginx**

Sử dụng một file cấu hình site bên nginx thay vì trước đó là wordpress và nginx trong địa chỉ /etc/nginx/site-available/
Phía bên nginx chỉ cần gộp 2 site thành 1 site, và phần proxy_pass cùng trỏ về port 8080

![image](https://github.com/user-attachments/assets/3a081221-329c-47e0-8c5a-3a6e0bd7c9a5)

**Cấu hình site phía Apache**

Bên Apache ta cũng gộp lại thành 1 file conf, và cho virtualhost cùng lắng nghe trên port 8080 
![image](https://github.com/user-attachments/assets/f01af6ee-56b8-4b66-bb2a-70b41ad228c2)

-> Dường như là chỉ cần chuyển port lại về cùng 1 port, khi request đến nó sẽ tự phân biệt request bằng ServerName và trỏ đến docroot của site đó.

**Cấu hình SSL**

certbot --nginx -d wordpress.trung.vietnix.tech -d laravel.trung.vietnix.tech
Sau khi xong, certbot sẽ tự generate thêm cấu hình bên nginx

![image](https://github.com/user-attachments/assets/be05dd77-000d-428a-8dc9-a44479566920)

