**1. Email Server**
Sử dụng Postfix làm MTA 
Sử dụng Dovecot làm MDA

Cài đặt Dovecot và Postfix: apt install dovecot-core dovecot-imapd dovecot-pop3d
Cài đặt Postfix: apt install postfix

Cấu hình Postfix trong đường dẫn: /etc/postfix/main.cf

```
  myhostname = mail.trungtranabc.site
  mydomain = trungtranabc.site
  myorigin = $mydomain
  inet_interfaces = all
  inet_protocols = all
  mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
  mynetworks = 127.0.0.0/8
  home_mailbox = Maildir/
  smtpd_banner = $myhostname ESMTP
```

Sau đó restart lại Postfix

Cấu hình file /etc/dovecot/conf.d/10-mail.conf để sử dụng Maildir:
```
mail_location = maildir:~/Maildir
```
Cấu hình file /etc/dovecot/conf.d/10-auth.conf để bật xác thực:
```
disable_plaintext_auth = no
auth_mechanisms = plain login
```
Sau đó restart lại Dovecot

Tạo record MX trên domain: MX trungtranabc.site 10 mail.trungtranabc.site

Khai báo SPF trên domain: "v=spf1 +mx +a +ip4:14.225.220.235 ~all"

Cài đặt DKIM bằng openDKIM: sudo opendkim-genkey -s mail -d trungtranabc.site (tạo 2 key: mail.private và mail.txt)

![image](https://github.com/user-attachments/assets/50128c10-433c-430b-9502-c64af0ed70bc)

Sau đó tạo một thư mục để tạo mã và cấp quyền cho nó:
```
sudo mkdir -p /etc/opendkim/keys/trungtranabc.site
sudo opendkim-genkey -s mail -d trungtranabc.site
sudo chown -R opendkim:opendkim /etc/opendkim/keys
```

Tiếp theo cấu hình lại file /etc/opendkim.conf để lưu các đường dẫn key:
```
Mode    sv
Canonicalization    relaxed/simple
KeyTable    /etc/opendkim/key.table
SigningTable    /etc/opendkim/signing.table
TrustedHosts    /etc/opendkim/trusted.hosts
```
Tạo key:
Tạo file /etc/opendkim/key.table:
```
mail._domainkey.trungtranabc.site trungtranabc.site:mail:/etc/opendkim/keys/trungtranabc.site/mail.private
```
Tạo file /etc/opendkim/signing.table:
```
*@trungtranabc.site mail._domainkey.trungtranabc.site
```
Tạo file /etc/opendkim/trusted.hosts:
```
127.0.0.1
localhost
trungtranabc.site
mail.trungtranabc.site
```
_Test gửi một mail đến gmail _
![image](https://github.com/user-attachments/assets/9fa6d371-b630-4c6f-8174-b9748e17937c)
![image](https://github.com/user-attachments/assets/6ebb124d-407d-42dd-8832-1314520a3d8c)
![image](https://github.com/user-attachments/assets/df9c1a3f-3973-4f9c-9f98-256dda536776)
==> Mail được gửi đến gmail nhưng bị chuyển vào thư rác do IP VN nằm trong blacklist
Nếu muốn gửi mail bằng một tài khoản khác, chỉ cần tạo một user mới và gửi mail bằng user đó.

**2. FTP Server (vsftpd)**
- Nếu sử dụng firewall ta cần mở các port cần thiết:
  ```
  ufw allow 21/tcp
  ufw allow 20/tcp
  ```
- Thực hiện cài đặt vsftpd:
  ```
  apt install vsftpd
  ```
- Chỉnh sửa/thêm một số dòng trong file cấu hình /etc/vsftpd.conf.
- Trên control panel thay vì bấm FTP Account để tạo account và link account đó đến một thư mục nào đó, thì ta sẽ dùng lệnh để thực hiện như sau:
    + Tạo account: sudo adduser ftptrung
    + Gắn accound này vào thư mục chỉ định: sudo usermod -d /var/www/html ftptrung
- Sử dụng FTP client (FileZilla) để kết nối:

![image](https://github.com/user-attachments/assets/dfac4f8c-1b2d-4a01-a8a0-051e44b355ae)
![image](https://github.com/user-attachments/assets/be782ab9-0719-4e6a-9fcc-6b300d95e9dc)

- FTP ở trên chưa sử dụng mã hóa, khi file truyền đi có nguy cơ bị sniff cho nên ta phải cấu hình mã hóa SSL/TLS.
- Để thực hiện dùng openssl để tạo key.
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem
```
- Sau đó vào file cấu hình /etc/vsftpd.conf và add 2 key này vào
```
rsa_cert_file=/etc/ssl/private/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.pem
```
