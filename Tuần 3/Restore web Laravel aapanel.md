Cài đặt aapanel

Add site, upload source code, chỉnh docroot, chỉnh sửa thông tin database

Chạy lệnh này bị lỗi vì thiếu file nên ta cần dùng composer lại
```
php artisan migrate
```



Chạy composer install -> bị putenv() -> vào file php.ini xóa putenv trong disable_functions
![image](https://github.com/user-attachments/assets/b09f5833-349f-4efa-93ea-632f36d93d62)

![image](https://github.com/user-attachments/assets/a33c33a5-9cea-4bfa-9281-d0f2a13f8a01)

Chạy lại composer install -> bị thiếu php extension fileinfo -> cài thêm ext fileinfo
![image](https://github.com/user-attachments/assets/ca2c487a-ff3e-45e8-abda-7a169ae8962d)

Chạy lại composer install -> bị thiếu proc_open -> vào php.ini xóa proc_open trong disable_functions
![image](https://github.com/user-attachments/assets/6fb540e9-4685-4f37-867d-94c081d4efa0)

Hết lỗi -> chạy composer install -> composer update

Lúc này trang web vẫn còn lỗi vì open_basedir đang giới hạn quyền truy cập của php vào /www/wwwroot/laravel.trungtranabc.site/public/
![image](https://github.com/user-attachments/assets/1aeab344-ed8b-4116-b9e9-4b6aeae8caae)

Vào file php.ini thêm như hình dưới và tắt Anti-XSS attack và nhớ kiểm tra lại quyền của thư mục -> reload lại PHP

![image](https://github.com/user-attachments/assets/bae7e6b0-a597-4981-be36-6188194489cf)

![image](https://github.com/user-attachments/assets/6895623c-ac65-4dbd-992f-449602cc687a)

Đã truy cập vào được trang web

![image](https://github.com/user-attachments/assets/6090231c-78b5-4b35-9b5f-3506f8fc4acb)

Chạy lại lệnh dưới để đồng bộ database và tạo dữ liệu mẫu:
```
php artisan migrate
php artisan db:seed
```

Bị thiếu key
![image](https://github.com/user-attachments/assets/024d4fda-b751-4cd4-b83d-6479b213da40)

Chạy lệnh dưới để tạo key:
```
php artisan key:generate
```
**=>>> Restore web thành công**
