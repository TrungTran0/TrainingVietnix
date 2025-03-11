## Lý thuyết cơ bản

1. SSL

- SSL (Secure Socket Layer) là một giao thức bảo mật giúp mã hóa dữ liệu trước khi truyền tải từ client lên server để đảm bảo tính toàn vẹn và tính bí mật
- Có 3 cách chứng thực SSL:
  + DV SSL (Domain Validated): Chứng chỉ được xác thực với cấp độ tên miền
  + OV SSL (Organization Validated): Chứng chỉ được xác thực với cấp độ doanh nghiệp/tổ chức
  + EV SSL (Extended Validated): Chứng chỉ được xác thực với cấp độ doanh nghiệp/tổ chức mở rộng, nghiêm ngặt hơn thường các doanh nghiệp lớn đa quốc gia mới sử dụng
- CSR (Certificate Signing Request) là một tệp yêu cầu ký chứng chỉ được tạo ra khi yêu cầu chứng chỉ SSL từ một CA (Certificate Authority). Tệp này chứa thông tin như khóa công khai, tên miền, thông tin tổ chức và các chi tiết khác để CA có thể xác thực yêu cầu và cấp chứng chỉ SSL.
- Generate by openssl:
  trung-vietnix@trungvietnix:~$ openssl genpkey -algorithm RSA -out private.key -aes256 -> nhập pass
  trung-vietnix@trungvietnix:~$ openssl req -new -key private.key -out tech.training.vietnix.tech.csr -> nhập pass và điền thông tin
- PEM (Privacy Enhanced Mail) là một định dạng mã hóa phổ biến được sử dụng cho chứng chỉ SSL và các khóa bảo mật. Tệp PEM có thể chứa chứng chỉ công khai, private key, hoặc chứng chỉ CA, và thường có phần mở rộng .pem, .crt, .cer, hoặc .key.
- Private Key là một phần quan trọng trong quá trình mã hóa và giải mã của SSL. Khi ta tạo một chứng chỉ SSL, private key sẽ được lưu trữ trên máy chủ và dùng để giải mã các dữ liệu được mã hóa bằng public key.
- PFX (Personal Information Exchange) là một định dạng tệp chứa chứng chỉ SSL, private key, và các chứng chỉ CA trung gian (nếu có). Định dạng này được sử dụng để xuất khẩu chứng chỉ và khóa từ máy chủ và có phần mở rộng .pfx hoặc .p12

2. Domain
- Domain là tên dùng để nhận diện trang web giúp truy cập trang web dễ dàng hơn là địa chỉ IP.
- Các trạng thái phổ biến của domain
```
Active: Domain đang hoạt động và có thể truy cập.
Expired: Domain hết hạn và chưa được gia hạn.
Suspended: Domain bị treo do không tuân thủ các chính sách của nhà cung cấp dịch vụ.
Pending: Domain đang trong quá trình chuyển nhượng hoặc gia hạn.
```
- Subdomain là một phần của domain chính, giúp phân chia các khu vực hoặc dịch vụ khác nhau trên cùng một domain. Ví dụ: blog.example.com là một subdomain của example.com
- Virtual HostsL cho phép nhiều trang web hoặc dịch vụ có thể hoạt động trên cùng một địa chỉ IP và máy chủ, virtual host có kiểu như: name-based, ip-base hoặc port-base

3. Mail Server
- MX (Mail Exchange) Record là bản ghi trong DNS xác định máy chủ nào chịu trách nhiệm nhận email cho một domain. MX record chỉ định tên máy chủ và mức độ ưu tiên của nó khi gửi email.
- DKIM (DomainKeys Identified Mail): Một cơ chế để xác thực tính toàn vẹn của email và bảo vệ không bị giả mạo.
- SPF (Sender Policy Framework): Giúp xác định các máy chủ email được phép gửi email từ domain cụ thể, nhằm ngăn chặn việc giả mạo địa chỉ gửi email.
- PTR (Pointer Record): Là bản ghi DNS ngược, giúp xác định tên miền của một địa chỉ IP, thường được sử dụng trong quá trình kiểm tra email để xác nhận tính hợp lệ của địa chỉ IP.

4. DNS
- DNS (Domain Name System) là hệ thống phân giải tên miền giúp phân giải tên miền thành địa chỉ IP
- Các loại record DNS:

    A Record: Chuyển đổi tên miền thành địa chỉ IP.
    CNAME Record: Chuyển hướng tên miền này sang tên miền khác.
    MX Record: Xác định máy chủ nhận email.
    TXT Record: Dùng để lưu trữ thông tin về domain, thường dùng cho các mục đích như SPF, DKIM.
    NS Record: Xác định máy chủ DNS của domain.
