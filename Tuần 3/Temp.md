Cài đặt apache2:
```
sudo apt update
sudo apt install apache2
```
Cài đặt PHP7.4
```
sudo apt -y install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt -y install php7.4
sudo apt-get install -y php7.4-cli php7.4-json php7.4-common php7.4-mysql php7.4-zip php7.4-gd php7.4-mbstring php7.4-curl php7.4-xml php7.4-bcmath
php -v
```
Chỉnh sửa port cho apache listen ở port 8080 trong file /etc/apache2/ports.conf:
![image](https://github.com/user-attachments/assets/8f5c2172-d465-4d48-8440-9d570bf8184d)

Cài đặt nginx:
```
sudo apt update
sudo apt install nginx
```
Run lại apache cho nó đổi port listen:
```
systemctl restart apache2
```
Lúc này nginx listen port 80, apache listen port 8080
![image](https://github.com/user-attachments/assets/38dd23f6-0f15-45ca-8c07-044165a113ee)

Tạo site wordpress.conf trong /etc/apache2/site-available
![image](https://github.com/user-attachments/assets/46897c8d-3c6a-4a45-b887-6317113008a2)

Tạo site wordpress trong /etc/nginx/site-available (xong rồi tạo symlink: ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled/)
![image](https://github.com/user-attachments/assets/16633106-aa4f-4eb1-9f01-bfff5b3b0ec8)

Tải mysql:
```
sudo apt update
sudo apt install mysql-server
sudo systemctl start mysql.service
```
Tạo database và user cho wordpress:
```
mysql -u root -p
create database WORDPRESS;
create user 'TRUNGWORDPRESS'@'localhost' IDENTIFIED WITH mysql_native_password BY 'WORDPRESS123@';
GRANT ALL ON `WORDPRESS`.* TO 'TRUNGWORDPRESS'@'localhost';
flush privileges;
```
upload source code wordpress lên docroot
![image](https://github.com/user-attachments/assets/62837788-905a-40d5-9e6b-0391d8e7f595)

Chỉnh lại quyền cho thư mục wordpress: chmod -R 755 /var/www/html/wordpress 
![image](https://github.com/user-attachments/assets/f2f51bd4-9817-4242-a8f2-05611c6e7172)

Với chown -R www-data:www-data /var/www/html/wordpress 

Chỉnh sửa lại thông tin database trong file wp-config
![image](https://github.com/user-attachments/assets/2d3a8f73-95eb-4630-ba00-00728e6ed3cb)

Import file database vào database
![image](https://github.com/user-attachments/assets/2c50ec9e-3998-4619-aaaa-7b333930d264)

Cần chỉnh sửa nội dung phần "option" trong database

Vào database:
```
mysql -u root -p
use WORDPRESS
select * from Sa3QIZ_options where option_id=1 or  option_id=2;
```
![image](https://github.com/user-attachments/assets/0f826c4b-4203-4a5b-a7d5-d21239c67d19)

Sửa lại siteurl với home
```
UPDATE Sa3QIZ_options
SET option_value = 'http://wordpress.trungtranabc.site'
WHERE option_id = 1 OR option_id = 2;
```
![image](https://github.com/user-attachments/assets/18cd155d-4c91-4f57-9947-0ac71aed7420)

Sau đó cài certbot
```
apt install certbot
sudo apt-get install python3-certbot-nginx
certbot --nginx -d wordpress.trungtranabc.site
```
![image](https://github.com/user-attachments/assets/6d778871-b289-4ff1-b7a0-1c9bd0a5157e)

===============================================================================================================

Tạo site laravel.conf trong /etc/apache2/site-available
![image](https://github.com/user-attachments/assets/39b7807d-28ce-42ab-8566-56399636a2ff)

Tạo site laravel trong /etc/nginx/site-available (xong rồi tạo symlink: ln -s /etc/nginx/sites-available/laravel /etc/nginx/sites-enabled/)
![image](https://github.com/user-attachments/assets/ae1faa71-dae8-4326-8760-f55df1fd421d)

Upload source code và move hết ra thư mục /var/www/html/laravel
![image](https://github.com/user-attachments/assets/2d67eaf7-60d4-47c8-85df-e18f65a5c732)

Tạo database và user cho laravel:
```
mysql -u root -p
create database laravel;
create user 'laravel'@'localhost' IDENTIFIED WITH mysql_native_password BY 'laravel123@';
GRANT ALL ON `laravel`.* TO 'laravel'@'localhost';
flush privileges;
```

Cài đặt composer:
```
sudo apt update
sudo apt install php-cli unzip
cd ~
curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
HASH=`curl -sS https://composer.github.io/installer.sig`
php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

Chỉnh sửa lại thông tin trong file .env trong /var/www/html/laravel (đổi tên lại thành .env bỏ example đi)
![image](https://github.com/user-attachments/assets/1afcead8-e616-4c94-bfa1-7ad591f47ccb)

Kiểm tra lại xem php hiện tại là bao nhiêu (php -v) nếu không phải bản 7.4 như lúc đầu cài thì dùng lệnh này để đổi lại
```
sudo update-alternatives --set php /usr/bin/php7.4
```

Vào thư mục /var/www/html chạy lệnh
```
composer install
composer update
php artisan migrate
php artisan db:seed
```

Chỉnh lại quyền truy cập của laravel
```
chmod -R 755 /var/www/html/laravel
chown -R data-www:data-www /var/www/html/laravel
```

Truy cập web thử, vào thư mục laravel chạy lệnh: php artisan key:generate

![image](https://github.com/user-attachments/assets/5caf1fe1-6c95-48ea-928d-b4cb9eb0065f)
