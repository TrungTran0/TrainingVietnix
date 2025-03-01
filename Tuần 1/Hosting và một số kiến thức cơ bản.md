1. Hosting là gì ?
Thông thường khi muốn build một trang web chúng ta phải cần một server để chạy dịch vụ, cần thuê leased line từ nhà mạng để có được IP tĩnh để người dùng truy cập vào IP đó đến server của chúng ta, ngoài ra còn một số điều kiện khác. Để có được những điều trên là rất tốn kém, cho nên Hosting xuất hiện để giúp chúng ta giải quyết điều đó. Hosting là một dịch vụ cho thuê không gian lữu trữ trên server của nhà cung cấp, chúng ta chỉ cần đẩy source code lên trên đó và trỏ tên miền, vậy là chúng ta đã có được một website có thể truy cập ngoài public.

Ngoài ra hosting không chỉ dùng để lưu trữ code website mà còn dùng để lưu trữ email, database, file,...

Phân loại dễ hiểu về hosting:
- Shared Hosting: Nhiều website dùng chung tài nguyên trên một server.
- VPS (Virtual Private Server): Server ảo hóa một phần tài nguyên riêng cho chúng ta thuê.
- Dedicated Server: Thuê một server vật lý riêng không dụng chạm với ai.
- Cloud Hosting: đây là kiểu lưu trữ website trên cloud, nghĩa là thay vì chúng ta lưu trữ trên một server vật lý đơn lẻ, thì cloud hosting sẽ kết nối nhiều server lại với nhau (cluster).


2. Kiến thức để thuê hosting, sản phẩm công ty.
Do có nhiều loại hosting nên sẽ được chia ra thành rất nhiều kiểu, vì vậy chúng ta cần biết về thông số của chúng:
- Đầu tiên là CPU, đây là vấn đề quan trọng cần chọn kĩ tùy theo dịch vụ của bạn cần sử dụng là gì, CPU càng mạnh sẽ xử lý các tiến trình nhanh giúp trang web hoạt động mượt.
 + Core quyết định số lượng tác vụ chạy đồng thời, nếu như trang web với các truy vấn cơ bản thì nên sử dụng CPU nhiều core.
 + Xung nhịp ảnh hưởng đến tốc đọ xử lý tac vụ đơn lẻ, nếu như web chạy dịch vụ như game thì nên chọn xung cao.
- Tiếp theo là RAM, với RAM thì đỡ quan trọng hơn CPU, RAM ảnh hưởng đến xử lý dữ liệu hay số lượng truy cập đồng thời do vậy tùy thuộc vào dịch vụ trang web bạn cung cấp thì sẽ chọn RAM phù hợp, với web blog thì chỉ cần RAM khoảng 4GB trở lên là ổn áp.
- Ổ đĩa hiện giờ đa số là kiểu SSD hoặc NVMe SSD, NVMe sẽ mạnh hơn SSD nên dùng cho website lớn hay lưu nhiều dữ liệu.
- Số lần backup một ngày mà gói đó cung cấp, số lượng tên miền.
- Loại xác thực SSL: đại khái sẽ có các kiểu như:
 + Sử dụng SSL miễn phí (Let's encrypt): thường dùng cho web cá nhân
 + DV SSL: giá rẻ phù hợp cho doanh nghiệp
 + EV SSL: doanh nghiệp cực lớn, giá cao.


3. DNS là gì ?
- DNS viết tắt cho Domain Name System, nó giúp phân giải tên miền của trang web thành địa chỉ IP. Nó giống việc chúng ta chỉ nhớ tên người dùng trong danh bạ chứ không nhớ số điện thoại của họ. Giống việc ta chỉ biết địa chỉ trang web là facebook.com chứ không nhớ địa chỉ IP của nó.

- Vậy thì DNS nó phân giải tên miền thế nào ?
Để biết nó phân giải thế nào thì mình cần biết cấu trúc của tên miền:
![image](https://github.com/user-attachments/assets/0ab2814a-f37f-4068-a0d1-18b90e9c4d16)

- Cấu trúc của một domain:
 + Protocol: là giao thức, một quy tắc chuẩn mà các dữ liệu khi truyền đi phải tuân theo quy tắc của giao thức đó
 + Sub-domain: là phần đứng trước domain chính, có thể có hoặc không.
 + Second Level Domain: là domain chính và là phần quan trọng nhất, nó chứa thông tin của đơn vị đăng ký tên miền trên nhà cung cấp tên miền.
 + Top level domain (TLD): là phần đuôi, sau dấu chấm ‘.’ của second level domain, cho biết tên miền của ta được quản lý bởi một nhà cung cấp nào, như trong nước hoặc ngoài nước, chia ra để quản lí.
 + Page-path: đường dẫn trang, có thể là thư mục hoặc là file để chưa nội dung trang web.

Ví dụ ta truy cập facebook.com
  1. Browser sẽ kiểm tra cache nếu trước đó ta đã truy cập facebook.com thì nó sẽ lấy IP trong cache ra và sử dụng luôn.
  2. Nếu chưa có cache thì nó sẽ gửi request để hỏi DNS Server của nhà mạng - ISP.
  3. Resolver gửi request lên Root DNS Server, nhưng hiện tại server này nó chưa biết IP nhưng nó sẽ lấy TLD .com
  4. Root DNS chỉ đến TLD Server .com, server này sẽ tiếp tục chỉ đến DNS của facebook.com
  5. TLD server gửi request đến authority domain của facebook để lấy record IP chính xác. Và cuối cùng là trả về IP.

- Các loại record DNS, các record này được chứa trong một vùng gọi là DNS Zone:
 + A record: trỏ tên miền đến IPv4. Ví dụ: facebook.com -> 157.200.20.20
 + AAAA record: trỏ tên miền đến IPv6. Ví dụ: 2001:db::ff00:8329
 + CNAME record: tạo alias cho một domain. Ví dụ: www.facebook.com -> example.com
 + MX record: xác định máy chủ email của domain. Ví dụ: facebook.com -> mail.google.com
 + TXT record: lưu thông tin tùy chỉnh thường dùng cho SPF, SKIM, xác mình google. Ví dụ: facebook.com → "v=spf1 include:_spf.google.com ~all"
 + NS record: xác định name server của domain. Ví dụ facebook.com -> ns1.facebook.com
