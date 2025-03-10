## Lợi ích khi dùng Nginx làm reverse proxy

Có 2 lợi ích chính khi dùng nginx làm reverse proxy:

- Giúp cải thiện hiệu xuất: Bằng cách lưu trữ cache các resource, nginx reverse proxy có thể giảm tải cho máy chủ web và cải thiện thời gian phản hồi cho máy khách.
- Tăng bảo mật: Reverse proxy một phần giúp giấu đi backend proxy để thêm secure, giống như mô hình internet-public-private, khi muốn ssh vào mạng private thì phải ssh vào được mạng public rồi sau đó mới ssh vào mạng private.

Ngoài ra cũng có một số lợi ích khác:
- SSL termination: nginx có thể làm điểm cuối kết nối SSL, giúp  chuyển tải quá trình xử lý SSL khỏi máy chủ web và cải thiện hiệu suất.
- Load balance: nginx cung cấp nhiều thuật toán cân bằng tải khác nhau và có thể phân phối các yêu cầu của khách hàng trên nhiều máy chủ web, cải thiện tính khả dụng và khả năng mở rộng của ứng dụng web.

Nguồn tham khảo: https://medium.com/@mak0024/nginx-as-a-reverse-proxy-benefits-and-best-practices-928863bfd317
