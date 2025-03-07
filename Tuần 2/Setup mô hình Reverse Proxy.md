Mô hình: 

![image](https://github.com/user-attachments/assets/c91be62b-be8f-4045-a06c-d60b8bb7ede6)

Cơ bản lý thuyết thì mình sẽ cho thằng Apache2 chỉ listen nginx trên một cổng riêng 8080, thằng Nginx listen trên cổng 80, 443 và chuyển tiếp (proxy) đến thằng Apache2.

- Cấu hình Apache2 listen port 8080:
![image](https://github.com/user-attachments/assets/3c9a312a-a9e2-43f1-8342-b27af1e304d8)
![image](https://github.com/user-attachments/assets/33a6eb0d-c020-4b92-be4e-a7667367e0a9)


- Cấu hình Nginx chuyển tiếp sang port 8080 Apache2:
![image](https://github.com/user-attachments/assets/c809ba41-1887-47d8-a264-6a126e7510de)

- Kiểm tra port:
![image](https://github.com/user-attachments/assets/d34cad91-e2b2-47ce-aeb2-9c3cd9506d77)

- Nếu như mà muốn thêm 1 subdomain thì chỉ cần thêm Virtual Host listen port 8080, và thêm bên Nginx một server listen 80 và proxy_pass qua 8080.
![image](https://github.com/user-attachments/assets/f36f3596-a541-4e62-b849-8d240e56470c)
![image](https://github.com/user-attachments/assets/f3ce8c9b-2d0f-473e-8268-1ec37031587c)

- Có thể test xem nginx đã làm proxy chưa bằng cách:
  + Xem PHP info (Server API là Apache2)
![image](https://github.com/user-attachments/assets/ebbc4afc-1b92-406a-914b-ca03a9e0afe3)
  + Tắt nginx -> truy cập domain:80 bị lỗi -> truy cập domain:8080 vẫn bình thường
![image](https://github.com/user-attachments/assets/6078f173-1b1a-4194-b6de-274ac3b403d2)
![image](https://github.com/user-attachments/assets/22bbebdc-394a-42c4-99f6-d29c396eebe5)
