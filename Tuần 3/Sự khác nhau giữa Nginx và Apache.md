## **Sự khác nhau giữa Nginx và Apache**

Nginx là một web server được phát hành vào năm 2004 bởi Igor Sysoev, nó thường được sử dụng để xử lý các file tĩnh. Ngoài ra, nginx còn được sử dụng để làm reverse proxy, load balancer, mail proxy và HTTP caching.
Sự khác biệt lớn nhất giữa Nginx và Apache là ở cách thiết kế cấu trúc của chúng. 

=> Apache sử dụng process-driven, tạo ra một thread mới cho mỗi request <-> Nginx thì khác, nginx dùng cấu trúc event-driven để xử lý nhiều request với chỉ một thread*.

Apache

- Tiếp cận theo hướng tiến trình (Process Driven Approach )
- Tạo mới một thread cho mỗi request.
Apache tiếp cận theo hướng multi-threaded. Nó cung cấp một bộ các modules xử lý rất đa dạng. Các modules này về cơ bản bao gồm 3 loại thuật toán xử lý request. Mỗi cái sẽ đáp ứng nhu cầu của các máy chủ (server) khác nhau.
Nginx

- Sử dụng phương pháp hướng sự kiện (Event-Driven approach).
- Xử lý nhiều request với chỉ một thread.
Nginx sử dụng cấu trúc hướng sự kiện và xử lý các request không đồng bộ. Nó được thiết kế sử dụng thuật toán xử lý kết nối theo hướng sự kiện không chặn (non-blocking). Do đó, tiến trình của nó có thể xử lý hàng ngàn kết nối (request) trong 1 luồng xử lý (thread). Với cách xử lý kết nối như vậy cho phép Nginx làm việc rất nhanh và mạnh với lượng tài nguyên hạn chế.


_Khi vừa tham khảo thêm cộng với tìm hiểu về sự khác nhau giữa Number of Process và Entry Process của cPanel (https://github.com/TrungTran0/TrainingVietnix/blob/main/Tu%E1%BA%A7n%201/cPanel_Training.md), khi em dùng load test tạo request thì chỉ có Number of Process tăng vì Open Litespeed sử dụng lsphp, mà lsphp thì nó hoạt động giống như PHP-FPM của nginx nên em có suy ra sự khác biệt chính về cách sử lý process của nginx và apache._

Nguồn tham khảo: https://www.marketenterprise.vn/blog/gioi-thieu-ve-nginx-va-apache.html#:~:text=S%E1%BB%B1%20kh%C3%A1c%20bi%E1%BB%87t%20l%E1%BB%9Bn%20nh%E1%BA%A5t,request%20v%E1%BB%9Bi%20ch%E1%BB%89%20m%E1%BB%99t%20thread*.
