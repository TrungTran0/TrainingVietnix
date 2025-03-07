- Email Accounts: giúp tạo, quản lý, thêm xóa sửa account email.
- Email forwarders: giúp forward một email gửi tới mail A (trong hosting) cũng sẽ được gửi tới mail B nào đó bất kì, hoặc có thể forward tới một user nào đó trong cùng domain.
![image](https://github.com/user-attachments/assets/e4c6598f-70f7-4fcc-887c-6049eb88d474)
![image](https://github.com/user-attachments/assets/05ed0958-8c15-48a0-b3c9-f9f424553904)
-  Email routing: giúp định tuyến tuyến đường các mail được gửi tới server sẽ đi đâu (nên chọn mặc định là Local Mail Exchanger nếu không sẽ không gửi nhận mail được)
-  Autoresponders: giúp trả lời mail tự động khi người khác gửi mail đến, hữu ích khi mình đang trong kỳ nghỉ hay bận gì đó, hoặc như trong thực tế thì một số công ty sử dụng chức năng này để thông báo rằng họ đã nhận được mail của mình. Default Address: catch bất kỳ email nào gửi tới người dùng không hợp lệ trong domain của mình (hoặc mail đó chưa có DKIM, SPF, DMARC), mình có thể chọn options là:
    +   Discard: gửi một message báo lỗi tới người gửi mail.
    +   Forward: forward email đó tới một địa chỉ email khác ví dụ như có một mail nào để chỉ để nhận những mail invalid.
![image](https://github.com/user-attachments/assets/b5019f86-a6f4-4cc5-a18c-b2fe994de673)
![image](https://github.com/user-attachments/assets/f82e915a-76dc-44f4-8489-d70e9015635d)
- Delivery Report: hiển thị tình trạng của các mail được gửi đi và được gửi đến, ví dụ mail blackpig@trungtranabc.site gửi tới mail trunghihi@gmail.com nhưng check không thấy nhận, thì mình có thể xem nguyên nhân lỗi ở đây, giống như debug vậy.
- Global Email Filters: Tạo filter áp lên toàn bộ email.
- Email Filters: chọn một account để áp filter lên riêng account đó.
  + Ví dụ tạo một filter forward email đến với subject tồn tại (Advertisement, Coupon, Discount) đến email blackpink@trugntranabc.site thì sẽ forward đến email blackpig@trungtranabc.site)
![image](https://github.com/user-attachments/assets/fc372546-b5c9-4776-a1b9-92c073cc5ae9)
![image](https://github.com/user-attachments/assets/717b8b9d-58f4-4ddd-82c9-2d4327903a3a)
- Email Deliverability: dùng cái này để quan sát, kiểm tra xem các record DKIM, SPF, DMRC PRT hợp lệ chưa, Nếu chưa thì khi user trong domain đó sẽ không gửi mail được hoặc mail sẽ bị đánh dấu là spam và bị chuyển vào Junk. Công ty cung cấp DKIM và SPF nên chỉ cần áp 2 thằng này đúng là valid.
  + ![image](https://github.com/user-attachments/assets/008b502e-0212-4ad0-9fc3-677b38486181)
  + ![image](https://github.com/user-attachments/assets/05e4b3c8-e896-428d-aefd-f48f44d1fa77)
- Address Importer: giúp thêm một loạt account email bằng file CSV hoặc XLS, nếu công ty 1000 nhân sự thì không thể add chay bằng Email Accounts được.
- Spam Filter: lọc các email được biết là spam nó sẽ chuyển tới Spam Box, Spam box sẽ chuyển tất cả message được tính là spam vào một thư mục riêng để mình xem xét dựa trên spam score đã được đánh dấu, khi message đã đạt ngưỡng là spam thì thằng Spam Threshold Score sẽ gắn các tag "SPAM" vào subject của cái mail đó. Hoặc thay vì chuyển tới Spam Box thì mình có thể kích hoạt Auto-Delete để tự động xóa nó luôn.
- Encryption: giúp generate public key và private key bởi GnuPG dựa trên thông tin mình nhập vào.
- Email Disk Usage: chọn tài khoản email để xem dung lượng sử dụng trên từng thư mục của account đó. Nhưng nên xem bằng Email Account hơn.
