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

Tìm hiểu sơ qua tại sao nginx có thể xử lý nhanh hơn apache về file tĩnh:

Lý do cơ bản đó là do kiến trúc xử lý kết nối của nginx khác với apache và thời gian ra mắt của 2 sản phẩm: apache release (4/1995), nginx release (10/2004).
- NGINX ra mắt sau Apache, với sự hiểu biết tốt hơn về các vấn đề hiện tại mà các trang web có quy mô lớn sẽ phải đối mặt. NGINX được xây dựng từ đầu bằng cách sử dụng thuật toán xử lý kết nối theo hướng event-driven, non-blocking, asynchronous.
- NGINX tạo ra các worker process (những process làm việc với connections, nó đọc và ghi nội dung vào disk, và giao tiếp với app server, handle request từ clients, mỗi quy trình có khả năng xử lý hàng chục nghìn kết nối). Các worker process đạt được điều này bằng cách phát triển một cơ chế loop nhanh kiểm tra và xử lý các sự kiện theo định kỳ. Bằng cách tách công việc thực tế khỏi các kết nối, mỗi worker process chỉ có thể tập trung vào một kết nối khi có sự kiện mới xảy ra.
- Mỗi connection của worker process được đặt trong vòng lặp sự kiện, nơi chúng cùng tồn tại với các kết nối khác. Các sự kiện được xử lý không đồng bộ trong vòng lặp, cho phép công việc được thực hiện theo cách non-blocking. Liên kết bị xóa khỏi vòng lặp sau khi nó đóng.
- NGINX có thể scale cực kỳ tốt với tài nguyên tối thiểu nhờ phương pháp handle connection của nó. Vì máy chủ là single-thread và không có quy trình bổ sung nào được tạo ra để xử lý mỗi kết nối mới, nên việc sử dụng bộ nhớ và CPU vẫn tương đối ổn định, ngay cả khi máy chủ chịu load lớn.

Nguồn tham khảo:
```
https://gist.github.com/lukecav/ed29bd779ccb2f8a46fc8b30ed48c141
https://cyberpanel.net/blog/apache-vs-nginx
https://blog.litespeedtech.com/2018/03/05/compare-openlitespeed-to-nginx-and-apache/
```
