## Lợi ích khi dùng Nginx làm reverse proxy trên 1 server

Lý do dùng Nginx làm reverse proxo cho Apache:
Mục đích chính đó là lợi dụng việc tốc độ xử lý nhanh nội dung tĩnh của nginx:
 - Thứ nhất là đỡ cho việc một mình Apache xử lý cả static content và dynamic content
 - Thứ hai là tốc độ xử lý nội dung tĩnh của Nginx nhanh hơn nhiều so với Apache

Dưới đây là hình so sánh tốc độ xử lý file tĩnh giữa 3 web service:
 ![image](https://github.com/user-attachments/assets/d7fdadde-a147-4d5e-833c-31f374456333)

** _Có thể thấy tốc độ xử lý của Nginx nhanh hơn gấp đôi so với Apache_


Vậy để thực hiện được điều này, khi cấu hình nginx làm reverse proxy trong site /etc/nginx/site-available/domain ta cần thêm 2 field này vào:
![image](https://github.com/user-attachments/assets/4c1d3ad9-b5e9-4abb-8b43-b67ca8bfad07)

Trường này dành cho nội dung tĩnh và cache để tăng hiệu suất
```
location ~* \.(?:ico|css|js|gif|jpe?g|png|woff2?|eot|ttf|otf|svg|mp4|webp)$ {
        expires 7d;
        log_not_found off;
        access_log off;
    }
```

Trường này là trường phụ dành cho các resource như font chữ, các tệp đuôi mở rộng nói chung
```
location ~* \.(?:svgz?|ttf|ttc|otf|eot|woff2?)$ {
        add_header Access-Control-Allow-Origin "*";
        expires 7d;
        access_log off;
    }
```
