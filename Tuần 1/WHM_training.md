**MỘT SỐ CASE HAY SỬ DỤNG TRONG WHM**

_Để tìm kiếm một subdomain hay  add-on domain của một user:_

- Cách 1: Dùng List Subdomains:
![image](https://github.com/user-attachments/assets/4497d4b1-1012-4b1e-a998-ecdebd465038)

- Cách 2: Sử dụng terminal: Có thể dùng lệnh trong terminal để tìm xem sub/add-on domain thuộc user nào: cat /etc/userdatadomains | grep [domain]
![image](https://github.com/user-attachments/assets/022d60b6-3262-48e7-8449-d2dc8a320be4)


_Để thay đổi default domain:_

- Cách 1: Ta có thể vào Home -> Account Function -> Modify an Account để thay đổi.
![image](https://github.com/user-attachments/assets/515d073c-869e-4105-b4e4-5c3e81fd62f1)

![image](https://github.com/user-attachments/assets/4d965f26-e462-4b1e-93c7-9b0efd94f561)

- Cách 2: Thay đổi ngay trên portal: Nên sử dụng cách này vì đơn giản và tránh conflict, sai sót.

![image](https://github.com/user-attachments/assets/2cbd0d4d-5087-431b-872b-54e7c1773a22)


_Để transfer account_

Trường hợp hay xảy ra khi khách hàng muốn nâng cấp gói hosting
- Đầu tiên kiểm tra có thể truy cập ssh vào server cần trans, sau đó truy cập vào server hosting cần trans tới. Vào Transfer → Transfer tool.
Field Remote Server Address là nhập địa chỉ server cần chuyển, và nhập thông tin cần thiết, rồi nhấn Scan Remote Server.
![image](https://github.com/user-attachments/assets/63dcdea0-e7f8-419f-9044-dd6ad7118c3b)
- Giao diện ở dưới hiện ra và chọn account hosting cần chuyển đổi và nhấn Copy thể tiến hành quá trình.
![image](https://github.com/user-attachments/assets/c41088f5-ff6b-4b42-a0ed-8b6362f5838c)
- Sau khi chuyển xong thì account bên server cũ sẽ bị suspend, sau đó ta cần trỏ domain về hosting mới.
![image](https://github.com/user-attachments/assets/20e7ea80-b662-4eb1-b61e-0bed030d436c)


_Để Reload user hosting_

Trường hợp này hay xảy ra khi hosting của họ yếu và chạy nhiều tiến trình, hoặc đang có quá nhiều truy cập. Để reload hosting ta có 2 cách:
- Cách 1: Sử dụng lệnh:
cagefs -m <user> (lệnh này có chỉ chạy được trên cpanel có plugin cagefs, còn trên bản thường thì không có)

- Cách 2: Sử dụng trực tiếp trên portal:

![image](https://github.com/user-attachments/assets/c42151b4-a60c-4782-9033-30a89771939a)


_Để kiểm tra process đang chạy của một user_

Dùng lệnh ps aux | grep <user>
![image](https://github.com/user-attachments/assets/f2ec2248-d990-4aa1-bbb1-11cd29e515fd)


_Để tìm kiếm subdomain hoặc tìm kiếm add-on domain của một user_

- Cách 1: Sử dụng giao diện: Xem phần List Subdomain trong Account Information
![image](https://github.com/user-attachments/assets/2e544d0f-2984-4dbc-a4ab-7fc64e9c9d55)

- Cách 2: Dùng lệnh cat /etc/userdatadomains | grep [domain]
![image](https://github.com/user-attachments/assets/791eaa2c-0d34-465d-b21f-080a238c57af)

