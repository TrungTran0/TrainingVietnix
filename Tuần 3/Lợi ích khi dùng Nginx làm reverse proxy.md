## Lợi ích khi dùng Nginx làm reverse proxy

Có 2 lợi ích chính khi dùng nginx làm reverse proxy:

- Giúp cải thiện hiệu xuất: Bằng cách lưu trữ cache các resource, nginx reverse proxy có thể giảm tải cho máy chủ web và cải thiện thời gian phản hồi cho máy khách.
- Tăng bảo mật: Reverse proxy một phần giúp giấu đi backend proxy để thêm secure, giống như mô hình internet-public-private, khi muốn ssh vào mạng private thì phải ssh vào được mạng public rồi sau đó mới ssh vào mạng private.

Ngoài ra cũng có một số lợi ích khác:
- SSL termination: nginx có thể làm điểm cuối kết nối SSL, giúp  chuyển tải quá trình xử lý SSL khỏi máy chủ web và cải thiện hiệu suất.
- Load balance: nginx cung cấp nhiều thuật toán cân bằng tải khác nhau và có thể phân phối các yêu cầu của khách hàng trên nhiều máy chủ web, cải thiện tính khả dụng và khả năng mở rộng của ứng dụng web.


Tìm hiểu sâu hơn về cách mà nginx có thể làm được như vậy:
- Với tính bảo mật: vì nginx đứng trước server làm reverse proxy nó sẽ đứng chắn trước các request đến và che giấu toàn bộ backend phía sau.
  + Trong bài lab đơn giản em chỉ thực hiện đẩy các request đến từ cổng 80 listen bởi nginx sang cổng 8080 listen bởi apache. Nhưng thực tế mình tùy loại request mình sẽ chuyển đến từng web server tương ứng, sau đó nginx lấy phản hồi và gửi lại cho client.
![image](https://github.com/user-attachments/assets/b6f04350-4a23-401f-9adc-8212ce1d777b)
  + Để chuyển hướng, nginx có tính năng proxy_pass http://host_chi_dinh;
  + Ngoài việc chuyển hướng gói http ra, nginx còn hỗ trợ chuyển hướng các giao thức khác:
    + fastcgi_pass - chuyển hướng đến một FastCGI server
    + uwsgi_pass chuyển hướng đến một uwsgi server
    + scgi_pass chuyển hướng đến một SCGI server
    + memcached_pass chuyển hướng đến memcached server
  + Ví dụ cho chuyển hướng đến FastCGI Server:
    + Syntax là: fastcgi_pass address;
    + Khi muốn chuyển hướng FastCGI đến một server khác: fastcgi_pass host:9000;
    + Hoặc có thể chuyển hướng đến unix socket: fastcgi_pass unix:/tmp/fastcgi.socket; Với chuyển hướng tới socket sẽ không phù hợp với mô hình này vì nó chỉ sử dụng trên 1 host
  + Vì khi đứng trước backend server nên mặc định nó sẽ relay IP của nó đến backend server, điều này sẽ khiến việc ghi log của backed server không chính xác nên ta cần phải cấu hình lại giá trị của proxy_set_header:
    ```
     proxy_set_header X-Real-IP $remote_addr;
     proxy_set_header X-Forwarded-Host $host;
     proxy_set_header X-Forwarded-Port $server_port;
    ```
- Với tăng hiệu xuất: 
  + Nginx hỗ trợ giá trị gọi là buffer, nó giúp cho backend có thể phản hồi nhanh không cần phải chờ client nhận dữ liệu hay hạn chế trải nghiệm truy cập chậm từ client, có thể nói nó giống như cache nhưng không giống hoàn toàn.
  + Buffer size thường bằng với memmory của page hoặc có thể nhỏ hơn
    ```
    Syntax: 	proxy_buffer_size size;
    Default: 	proxy_buffer_size 4k|8k;
    Context: 	http, server, location
    ```
  + Ví dụ:
    ```
    proxy_buffers 16 4k;
    proxy_buffer_size 2k;
    ```
    16: là số lượng buffer
    4k: là số lượng mỗi buffer (4KB)
   + **Tuy nhiên tùy trường hợp mà ta có nên sử dụng buffer hay không, nếu như trang web sử dụng TCP thì nên dùng buffer để giảm khối lượng load, đối với các trang web xem video sử dụng UDP thì nên tắt buffer để giảm bớt độ trễ.**
      ```
      proxy_buffering off;
      ```
  - Với Load balancing:
    + Nginx sử dụng một số thuật toán load balancing sau:
      + round-robin — các yêu cầu đến máy chủ ứng dụng được phân phối theo kiểu round-robin, nó sẽ chia từng loại request cho từng server
      + least-connected — gửi request đến server có ít request nhất hay còn dư nhiều resource nhất
      + ip-hash - sử dụng một hash function để xác định máy chủ nào được chọn cho request kế tiếp
    + Ví dụ về cấu hình value trong nginx để thực hiện load balancing:
      ```
      http {
        upstream myapp1 {
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
        }

      server {
        listen 80;

        location / {
            proxy_pass http://myapp1;
          }
        }
      }
      ```
```
Nguồn tham khảo: https://medium.com/@mak0024/nginx-as-a-reverse-proxy-benefits-and-best-practices-928863bfd317
Nguồn tham khảo: https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/
Nguồn tham khảo: https://nginx.org/en/docs/
Nguồn tham khảo: https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-setup-Nginx-reverse-proxy-servers-by-example
Nguồn tham khảo: https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
Nguồn tham khảo: http://nginx.org/en/docs/http/load_balancing.html
```
