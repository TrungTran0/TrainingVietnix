**1. aaPanel**

Một số lưu ý cần biết về aaPanel control panel
- Có 2 bản: miễn phí hoặc có phí
- Chỉ có 1 user, khi cài đặt aaPanel thì user mặc định được cài là www. Cho nên mọi file cấu hình sẽ nằm trong thư mục /www.

![image](https://github.com/user-attachments/assets/2e728aa4-d839-4bec-bba6-674606f4dac5)

Trong đó gồm các thư mục:
- wwwroot: chứa thư mục domain hoặc subdomain (như trong hình là trungtranabc.site và subdomain.trungtranabc.site) và source code web sẽ nằm trong thư mục đó.
- wwwlogs: chứa log của trang web như: access log, error log của trang web. Ví dụ mình xóa file index.html hay index.php thì errror sẽ được lưu vào error log với thông tin là các file hiện tại không match với các file tiêu chuẩn của nó.

**2. CyberPanel**

- Tạo một website hoặc SSL dễ dàng, SSL sử dụng Let’s Encrypt, chỉ cần đúng thư mục thì SSL sẽ được tạo nhanh chóng không lỗi.
- Hỗ trợ cài đặt wordpress nhưng bắt tính phí với bản thường
![image](https://github.com/user-attachments/assets/8ad780fd-6871-4657-bb86-9700aa97d04f)

- Có thể tạo nhiều user hoặc nhiều domain, nếu không tạo user thì file cấu hình sẽ được lưu tại thư mục /home/domain_name
- Log access và log error được lưu trong thư mực log

**3. Direct Admin**

- Cần license để cài đặt 
- Khi mua license thì tùy vào gói mà có thể thêm số lượng user, nên cấu trúc file của nó giống với CyberPanel /home/domain_name
- Có hỗ trợ Reseller
- Giao diện trực quan nhưng không bằng cPanel
- Wesbserver mặc định là Apache

**4. VestaCP**

- Control panel này đã cũ và cũng chỉ hỗ trợ trên những hệ điều hành cũ
- Webserver là Apache/Nginx
- Hỗ trợ nhiều user nhưng không hỗ trợ cho reseller
- Cấu trúc file của nó cũng sẽ theo cấu trúc /home/domain_name
