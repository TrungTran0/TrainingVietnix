**1. Firewall**

Inbound Rules (nếu Server là server cung cấp dịch vụ, cho phép client truy cập vào port của dịch vụ và không mở truy cập vào những port khác) : Chọn tiếp New Rule → Rule type (chọn Port) → Protocol and Ports (chọn UDP hay TCP để apply, chọn port chỉ định hoặc tất cả port)  → Action (chọn cho phép hoặc chặn) → Profile (chọn khi nào rule được apply) → Cuối cùng là đặt tên cho rule để dễ nhận biết rule này làm gì.

_Ví dụ rule để cho phép truy cập port 80._

![image](https://github.com/user-attachments/assets/5b2a9898-b791-492c-aa0f-a73a3f5c2730)

Để thực hiện allow/block IP trên Windows Server ta vào Windows Firewall with Advanced Security → Chọn :
Inbound Rules →  New Rule → Rule Type (chọn custom) → Program (chọn all) → Protocol and Port (ví dụ rule này chặn ping, ta chọn Protocol type là ICMPv4) → Scope (có thể chọn IP chỉ định hoặc range IP hoặc IP trong một đường mạng) → Action (chọn block connection) → Profile chọn tất cả → Name (đặt tên cho rule)

![image](https://github.com/user-attachments/assets/6bd04e0c-31ae-4f7a-b045-5eb6aef3c991)

_Thực hiện chặn IP của znews.vn_

![image](https://github.com/user-attachments/assets/a2297832-7912-41f5-83dd-6baeac170f75)
![image](https://github.com/user-attachments/assets/0264f93d-818e-4ed0-9f04-1b886cde05e9)

_Chặn IP chỉ định ping đến_

![image](https://github.com/user-attachments/assets/bd2d3581-8de9-4c1b-9177-0bdb069206c0)

_Giới hạn IP chỉ cho mỗi IP của admin truy cập từ xa_

![image](https://github.com/user-attachments/assets/01afb257-4fc0-42e7-b482-e94603066863)

**2. Cài đặt Webserver IIS**

_**Cài đặt Wordpress**_

Thực hiện cài đặt các roles cần thiết: IIS (Web Server, Management tool, CGI)
![image](https://github.com/user-attachments/assets/1e006da3-359e-4794-a5f1-d3f9ee43b7da)
Tiếp theo cần tải PHP Manager để quản lý các phiên bản của PHP
![image](https://github.com/user-attachments/assets/a2ec51c3-1e43-49da-8e65-e023a351dd07)
Tiếp theo cần tải MySQL để wordpress ghi dữ liệu vào database
![image](https://github.com/user-attachments/assets/d12218c7-00cb-4242-94fd-9430eef03d5c)
Tạo một site trungtranabc.site
![image](https://github.com/user-attachments/assets/f3c77025-e9c1-4aca-9d0b-52544773f4a6)
Tạo thư mục và tải wordpress vào trong đó (lưu ý về quyền của các user để tránh lỗi truy cập trang web)
![image](https://github.com/user-attachments/assets/aa41459f-5d60-410f-a0fd-69904bacd285)
Truy cập trang web và cấu hình database
![image](https://github.com/user-attachments/assets/1cc37d83-e780-4dcc-b60f-57fd099a0945)

_**Cài đặt SSL**_

Tạo thư mục .well-known trong thư mục trang web và file check để ZeroSSL vefify
![image](https://github.com/user-attachments/assets/8a6972a8-e7e9-48f0-ac73-ad5e61879c35)
Sau khi ZeroSSL đã verify ta tải 3 cert về và thực hiện chuyển đổi sang định dạng của windows
Tải openssl trên windows và thực hiện chuyển cert sang dạng pkcs12
```
openssl pkcs12 -export -out certificate.p12 -inkey private.key -in full_chain.crt
```
Trước đó ta cần gộp 2 cert lại thành file full_chain.crt
```
copy -b certificate.crt + ca_bundle.crt full_chain.crt
```
![image](https://github.com/user-attachments/assets/0b4dd39f-1017-4e9c-b9ec-3065bafc6eb1)
Sau khi convert cert sang định sang pkcs12, ta thực hiện đổi đuôi file sang pfx không cần dùng convert tool, vì 2 định dạng pkcs12 và pfx cấu trúc format binary giống nhau.

Cuối cùng thưc hiện import ceertificate vào là hoàn tất
![image](https://github.com/user-attachments/assets/db4110e2-ca72-4427-b961-ecfbadd51870)
![image](https://github.com/user-attachments/assets/7f390271-d43b-4653-8282-7575f9ddfea6)

_**Cài đặt thêm một vài plugin phổ biến**_
![image](https://github.com/user-attachments/assets/5861ebec-72cb-4197-ad92-a5f12d9df25d)


**3. Cài đặt SQL Server**

_**Cài SQL Server 2019**_
![image](https://github.com/user-attachments/assets/ab2c46da-ec49-469d-9bf8-5eecbb17ca68)
![image](https://github.com/user-attachments/assets/23e45420-2300-4378-917f-57aa0d60dabc)
_**Cài SQL Server 2008**_
![image](https://github.com/user-attachments/assets/14161ccd-65cf-4f61-b5b9-9e4b34614b41)
_**Cài SQL Server 2012**_
![image](https://github.com/user-attachments/assets/ee360ff1-3eba-4429-9cb8-ddb3a5aed121)
_**Cài SQL Server 2016**_
![image](https://github.com/user-attachments/assets/d1c2e39f-cc14-41e6-936b-1376f6980d5a)
