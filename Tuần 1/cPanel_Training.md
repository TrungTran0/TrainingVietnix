  **cPanel là gì ?**

Ở trên chúng ta đã nói đại khái về dùng hosting để build trang web, vậy để vận hành được trang web chúng ta phải quản lý được nó. cPanel giúp người dùng hoặc danh nghiệp quản lý hosting của họ dễ dàng hơn so với việc quản lý bằng các lệnh trên linux (nếu server họ được build trên linux).

cPanel là một bảng điều khiển web hosting (Control Panel) giúp quản lý website một cách dễ dàng thông qua giao diện đồ họa (GUI). Nó hỗ trợ nhiều tính năng như quản lý file, database, email, SSL, DNS, subdomain, tài nguyên server, v.v. Nhờ có cPanel, người dùng chỉ cần vài click chuột để thực hiện các tác vụ mà trước đây cần dùng dòng lệnh.

Ví dụ về việc cài ssl cho trang web: với xử lý trên command line trên linux ta phải cung cấp thông tin, tự generate ra các file như private.key, file .crt, và rồi phải up lên thư mục; nhưng với cPanel chỉ cần thao tác trên giao diện, nó sẽ giúp thực hiện một cách tự động dễ dàng.
Các chức năng của cPanel:

Tổng quan giao diện:
![image](https://github.com/user-attachments/assets/3b8b1b09-c000-4931-8c33-89968f00d42d)
1. Các công cụ của cPanel
2. Thanh search để tìm kiếm công cụ nhanh hơn
3. Thông tin chung về hosting như tên user, IP,…
4. Bảng hiển thị thông số như RAM, số add-on domain, processes, số tài khoản mail, dung lượng disk,…

Ở phần thông số có vài chỗ lưu ý ở phần Disk Usage vs Database Disk Usage và Entry Processes vs Number of Processes.

_** Với Disk Usage & Database Disk Usage, dưới đây là 2 hình ảnh trước và sau khi em tạo một trang web wordpress:_
![image](https://github.com/user-attachments/assets/ca9cc663-0e2a-4ee2-8572-20d2d5f29fb3)

![image](https://github.com/user-attachments/assets/f0e7beb4-7b6d-4cfc-ae2f-75723d04a8af)

-> Xem xét sự khác biệt thì em thấy source wordpress được lưu vào disk usage, còn dung lượng database được lưu vào database disk usage, nó chia ra như vậy em tìm hiểu là do database và file của mysql được sở hữu của user mysql chứ không phải cpanel, nên nếu user cpanel thao tác tác vụ vượt qua quota của Disk Usage thì họ không thể thao tác thêm, còn nếu client thao tác trên hosting mà khiến Database disk usage tăng lên thì vẫn có thể thao tác vì không liên quan đến disk usage.

** Với Number of Processes và Entry Processes.

**Files:**
- File Manager: giúp quản lý file hay thư mục dễ dàng, trực quan hơn với giao diện, thông thường khi sử dụng phần mềm FTP client để tải file thì ta sẽ sử dụng thông qua giao thức FPT port 21, còn File Manager trên cpanel sử dụng giao thức HTTPs để up download file.
- Images: chỉnh sửa hình ảnh, frame hình hay convert đuôi hình
- Directory Privacy: Cài đặt mật khẩu cho directory cho trang web, có thể tránh được việc tấn công Directory traversal attack hoặc truy cập trái phép.
- Disk Usage: xem dung lượng của từng thư mục, trong đó có hiển thị dung lượng file ẩn nhưng đó là tổng dụng lượng tất cả file ẩn. Để xem chi tiết thì có thể dùng lệnh sau:
  + Xem chi tiết size từng file ẩn thư mục hiện tại không recurisve: du -sch .[^.]*
  + Xem chi tiết size tổng file ẩn thư mục hiện tại không recurisve: du -sch .[^.]* | tail -n 1
- FTP Accounts: tạo tài khoản FTP để có thể truy cập vào thư mục thông qua các phần mềm FTP client như FileZilla, Cyberduck,...
- Backup (của cPanel): backup toàn bộ, có một nhược điểm là với những khách hàng dùng wordpress themes có dạng parent child thì khi restore thì có thể bị lỗi themes
- JetBackup 5 (của CloudLinux): có backup toàn bộ (kiểu snapshoot), có giao diện trực quan hơn về từng thư mục, xịn xò hơn backup mặc định của cPanel.
- File and Directory Restoration: chức năng để restore file backup
- Git Version Control: clone repo trên github về thông qua giao diện

**Databases (databse sử dụng gồm mySQL và MariaDB):**
- phpMyAdmin: quản lý database dễ dàng thông qua phpMyAdmin
- Manage My Databases: xem dung lượng database, thêm database, add user vào database,…
- Database Wizard: cũng tạo database nhưng theo từng bước
- Remote Database Access: thêm host nào để cho phép kết nối database từ xa.

**Domains:**
- WordPress Management: tạo, xóa, sửa website wordpress, quản lý themes, tình trạng SSL,…
- Domains: quản lý, tạo, xóa domain
+ Để tạo một subdomain trang web, ta nhập subdomain của primary domain rồi chọn chỗ lưu file.
+ Để tạo một add-on domain trang web, ta nhập tên domain khác và không chọn Share document root.
+ Redirects: cho phép một trang web trong hosting chuyển hướng sang một trang web khác
+ Zone Editor: quản lý zone của domain, thêm xóa sửa các record, nếu có sai sót ta có thể reset lại bằng cách chọn Actions → Reset DNS Zone.

**Metrics:**
- Visitors: xem log access vào trang web mình, có thể dùng để xác định một cuộc đấn công DdoS
- Errors: Xem log error của trang web
- Bandwidth: xem tổng lưu lượng truy cập web
- Resource usage (không có trong cpanel thường): có thể dùng để xem lưu lượng processes, kiểm tra nguyên nhân hosting khách chậm, process nào thời điểm nào ngốn tài nguyên.

**Security:**
- SSH Access: tạo key để có thể truy cập hosting bằng key ssh
- IP Blocker: chặn IP truy cập vào trang web, có thể chặn single IP, chặn theo range,…
- SSL/TLS, dùng để cài đặt SSL cho trang web, có thể cài tự động bằng cPanel sử dụng Let’s Encrypt, hoặc có thể cài thủ công bằng Zero SSL (nó generate cho mình mấy file crt,…) rồi mình tự add chay vào trang web trong phần manage SSL Hosts.
- SSL/TLS Status: quản lý các trang đã cài SSL chưa, hoăc cũng có thể dùng để cài tự động SSL luôn.
- Iminify360: cũng là một tính năng không có sẵn trong bản cpanel free dùng để quét mã độc trong source trang web, nếu có thì nó sẽ tự động xóa (không thể biết được là mã độc chỗ nào) và báo khách kiểm tra lại source code của mình.
