**1. cPanel**

Email
- Email Accounts: giúp tạo, quản lý, thêm xóa sửa account email.
- Email forwarders: giúp forward một email gửi tới mail A (trong hosting) cũng sẽ được gửi tới mail B nào đó bất kì, hoặc có thể forward tới một user nào đó trong cùng domain.
- Email routing: giúp định tuyến tuyến đường các mail được gửi tới server sẽ đi đâu
- Autoresponders: giúp trả lời mail tự động khi người khác gửi mail đến, hữu ích khi mình đang trong kỳ nghỉ hay bận gì đó, hoặc như trong thực tế thì một số công ty sử dụng chức năng này để thông báo rằng họ đã nhận được mail của mình.
Default Address: catch bất kỳ email nào gửi tới người dùng không hợp lệ trong domain của mình (hoặc mail đó chưa có DKIM, SPF, DMARC), mình có thể chọn options là:
    + Discard: gửi một message báo lỗi tới người gửi mail
    + Forward: forward email đó tới một địa chỉ email khác ví dụ như có một mail nào để chỉ để nhận những mail invalid
- Delivery Report: hiển thị tình trạng của các mail được gửi đi và được gửi đến, ví dụ mail blackpig@trungtranabc.site gửi tới mail trunghihi@gmail.com nhưng check không thấy nhận, thì mình có thể xem nguyên nhân lỗi ở đây, giống như debug vậy.
- Global Email Filters: Tạo filter áp lên toàn bộ email.
- Email Filters: chọn một account để áp filter lên riêng account đó.
- Email Deliverability: dùng cái này để quan sát, kiểm tra xem các record DKIM, SPF, DMRC PRT hợp lệ chưa, Nếu chưa thì khi user trong domain đó sẽ không gửi mail được hoặc mail sẽ bị đánh dấu là spam và bị chuyển vào Junk.
  + Với TXT record chưa được khai báo hợp lệ
    ![image](https://github.com/user-attachments/assets/95e5f4cb-cb24-4784-90d3-47b91fdedf97)
  + Với TXT record được khai báo hợp lệ
    ![image](https://github.com/user-attachments/assets/f38c62a5-22d5-484f-b58a-d2c63ee1c8b2)
  => Khi TXT record hợp lệ, match với google thì email mới có thể đến được hòm thư đến của gmail
  ![image](https://github.com/user-attachments/assets/7ad3e649-b930-46d7-bbae-564dec654366)

- Address Importer: giúp thêm một loạt account email bằng file CSV hoặc XLS, nếu công ty 1000 nhân sự thì không thể add chay bằng Email Accounts được
- Spam Filter: lọc các email được biết là spam nó sẽ chuyển tới Spam Box, Spam box sẽ chuyển tất cả message được tính là spam vào một thư mục riêng để mình xem xét dựa trên spam score đã được đánh dấu, khi message đã đạt ngưỡng là spam thì thằng Spam Threshold Score sẽ gắn các tag "***SPAM***" vào subject của cái mail đó. Hoặc thay vì chuyển tới Spam Box thì mình có thể kích hoạt Auto-Delete để tự động xóa nó luôn.
- Encryption: giúp generate public key và private key bởi GnuPG dựa trên thông tin mình nhập vào
- BoxTrapper: giúp filter chi tiết tránh các email spam, dựa trên whiltelist blacklist ignore list, filter nội dung trong message như sender header,...
- Email Disk Usage: chọn tài khoản email để xem dung lượng sử dụng trên từng thư mục của account đó


**2. WHM**

_Server Configuration:_
- Basic WebHost Manager: 
  + Ở phần trên hiển thị mục Contact Information, Basic Config (hiển thị cấu hình như ipv4 của server, tên card mạng, thư mục mặc định mà các user tạo và file cấu hình sẽ nằm ở đó,...)
  + Ở phần dưới hiển thị nameserver, mình có thể sửa hoặc add thêm nameserver vào.
- Server Time: chọn timezone để sync với time server.
- Tweak settings: giúp điều chỉnh các thông số mặc định cho các user.


_Service Configuration:_
- Exim Configuration Manager: Exim là một MTA - Mail Transfer Agent giúp việc gửi nhận mail cho mail server, file cấu hình của nó nằm trong /etc/exim.conf, giao diện trên WHM giúp quản lý dễ hơn với Bacsic Editor và Advanced Editor (modify parameter với value cần kiến thức vững).
- Mailserver configuration: server mail được sử dụng trong WHM là Dovecot, giống POP3 hay IMAP nó cũng cung cấp MUAs - Mail User Agent truy cập vào mail họ, ví dụ khi user dùng outlook để truy cập mail thì thằng outlook chính là MUAs. Thì trong Mailserver Configuration này nó sẽ cho nhiều lựa chọn để tùy chỉnh chức năng cảu Dovecot như protocol cho phép hay ipv6 hay thời gian xóa thư trong thùng rác.

