Cài đặt apache2:

sudo apt update
sudo apt install apache2

Cài đặt PHP7.4

sudo apt -y install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt -y install php7.4
sudo apt-get install -y php7.4-cli php7.4-json php7.4-common php7.4-mysql php7.4-zip php7.4-gd php7.4-mbstring php7.4-curl php7.4-xml php7.4-bcmath
php -v

Chỉnh sửa port cho apache listen ở port 8080 trong file /etc/apache2/ports.conf:
![image](https://github.com/user-attachments/assets/8f5c2172-d465-4d48-8440-9d570bf8184d)

Cài đặt nginx:

sudo apt update
sudo apt install nginx

Run lại apache cho nó đổi port listen:

systemctl restart apache2

Lúc này nginx listen port 80, apache listen port 8080
![image](https://github.com/user-attachments/assets/38dd23f6-0f15-45ca-8c07-044165a113ee)

Tạo site wordpress trong /etc/apache2/site-available
![image](https://github.com/user-attachments/assets/46897c8d-3c6a-4a45-b887-6317113008a2)

