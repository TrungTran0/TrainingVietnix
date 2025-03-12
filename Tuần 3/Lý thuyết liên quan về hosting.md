## 1. SSL

- SSL (Secure Socket Layer) là một giao thức bảo mật giúp mã hóa dữ liệu trước khi truyền tải từ client lên server để đảm bảo tính toàn vẹn và tính bí mật
- Có 3 cách chứng thực SSL:
  + DV SSL (Domain Validated): Chứng chỉ được xác thực với cấp độ tên miền
  + OV SSL (Organization Validated): Chứng chỉ được xác thực với cấp độ doanh nghiệp/tổ chức
  + EV SSL (Extended Validated): Chứng chỉ được xác thực với cấp độ doanh nghiệp/tổ chức mở rộng, nghiêm ngặt hơn thường các doanh nghiệp lớn đa quốc gia mới sử dụng
- CSR (Certificate Signing Request) là một tệp yêu cầu ký chứng chỉ được tạo ra khi yêu cầu chứng chỉ SSL từ một CA (Certificate Authority). Tệp này chứa thông tin như khóa công khai, tên miền, thông tin tổ chức và các chi tiết khác để CA có thể xác thực yêu cầu và cấp chứng chỉ SSL.
- Generate by openssl:
  ```
  trung-vietnix@trungvietnix:~$ openssl genpkey -algorithm RSA -out private.key -aes256 (1) -> nhập pass
  trung-vietnix@trungvietnix:~$ openssl req -new -key private.key -out tech.training.vietnix.tech.csr (2)-> nhập pass và điền thông tin
  ```
  Lệnh (1) thực hiện generate một file private key sử dụng thuật toán RSA
  
  Lệnh (2) request generate key file csr tech.training.vietnix.tech.csr từ private key ở trên
  
- PEM (Privacy Enhanced Mail) là một định dạng mã hóa phổ biến được sử dụng cho chứng chỉ SSL và các khóa bảo mật. Nó là một kiểu file Public Key Infrastructure (PKI) sử dụng cho key và cho chứng chỉ, và thường có phần mở rộng .pem, .crt, .cer, hoặc .key.
- Private Key là một phần quan trọng trong quá trình mã hóa và giải mã của SSL, khi ta tạo một chứng chỉ SSL private key ký chữ ký vào csr, private key sẽ được lưu trữ trên máy chủ và dùng để giải mã các dữ liệu được mã hóa bằng public key.
- PFX (Personal Information Exchange) là file chứa cả private key và csr, nếu các tệp còn riêng lẻ trên windows ta cần gộp lại thành 1 file pfx, ta thực hiện chuyển đổi sang định dạng pkcs12:
  ```
  openssl pkcs12 -export -out certificate.p12 -inkey private.key -in full_chain.crt
  ```

  Sau khi convert cert sang định sang pkcs12, ta thực hiện đổi đuôi file sang pfx không cần dùng convert tool, vì 2 định dạng pkcs12 và pfx cấu trúc format binary giống nhau.
  
## 2. Domain
- Domain là tên dùng để nhận diện trang web giúp truy cập trang web dễ dàng hơn là địa chỉ IP.
- Các trạng thái phổ biến của domain
```
Ok/Active: Trạng thái thể hiện tên miền hoạt động bình thường sau khi đăng ký.
pendingDelete: Tên miền hết hạn đăng ký và chuẩn bị xóa.
pendingRestore: Tên miền đã hết hạn đang chờ về trạng thái khôi phục (Active). 
addPeriod: Trạng thái sau khi vừa đăng ký tên miền.
inactive: Tên miền đã được đăng ký nhưng Name Server chưa thể liên kết với tên miền.
pendingCreate: Đang chờ Đăng ký
pendingUpdate: tên miền đang chờ update
pendingTransfer: tên miền đang chờ Chuyển đổi nhà đăng ký.
```
- Subdomain là một phần của domain chính, có thể tạo không giới hạn subdomain và có nội dung khác với domain chính. Ví dụ: blog.vietnix.vn, news.vietnix.vn
- Virtual Hosts:
  +  cho phép nhiều trang web hoặc dịch vụ có thể hoạt động trên cùng một địa chỉ IP và máy chủ để giảm chi phí,
  +  Virtual host có kiểu như: name-based, ip-base hoặc port-base
      +  Name-Based Virtual Host: Cho phép nhiều tên miền sử dụng chung một địa chỉ IP.
      +  IP-Based Virtual Host: Gán mỗi tên miền một địa chỉ IP riêng biệt.
      +  Port-Based Virtual Host: Sử dụng số cổng (Port) để phân biệt các website trên cùng một địa chỉ IP.

## 3. Mail Server
- MX Record là viết tắt của Mail Exchanger Record được định nghĩa là một bản ghi trong DNS zone dùng để định vị Mail Server cho một Domain. Một tên miền có thể được gán bởi nhiều bản ghi MX, việc này giúp cho các email của bạn không bị mất đi dữ liệu nếu ngưng hoạt động một thời gian. Ví dụ khi ta gửi email tới trung@vietnix.vn, mail server sẽ xác định domain vietnix.vn có MX record trỏ đến mail server nào ví dụ như mail.vietnix.vn, thì nó sẽ tìm địa chỉ IP của mail server mail.vietnix.vn và gửi đến địa chỉ IP mail đó, và mail server mail.vietnix.vn sẽ tìm user trung để gửi mail đến.
Ví dụ một sample MX record:
```
MX record: vietnix.tech IN MX 5 mail.vietnix.tech
```

- DKIM (DomainKeys Identified Mail): Một cơ chế để xác thực tính toàn vẹn của email và bảo vệ không bị giả mạo, nếu một email không phù hợp thì có thể sẽ bị discard hoặc chuyển vào spam.

Ví dụ khi ta gửi một email đến trung@vietnix.vn:
```
1. DKIM sẽ sử dụng private key để ký vào mail đó.
2. Sau khi mail đi đến mail server của vietnix.vn nó sẽ tìm txt record DKIM chứa bản ghi public key được public.
3. nó sẽ lấy public key này để giải private key trong mail, nếu như phù hợp thì nó có thể xác định mail này được xác định và toàn vẹn hợp lệ.
```
Ví dụ một bản ghi txt DKIM:
```
"v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA1ES45j1RnHP9xmX6xdbJu2RFvCj5V6lbREBlW24ULKl7gw3ZaLbNUVZE2DeQsTAC6cYlyrQenVZC5WY8BkpYYb0Oy3FMvE3HIs1aJ2jhiUbyAFFayL2KtyBfdQm+RS+b1EiMQk0pLpGi8M/xeNpgZKKj+V8+e+YSykUNJv5326LaoM86x20c9FSC1PiDYdG+09oERduifAwlJlHWdD4vBJSOPMdqBX4LpxCFofzOBdRLQTG9Sc9R2zfcl0t0jZU6jgo65PiMMe8a2A4hCN5xTc5oWGoqE5e...;"
```
- SPF (Sender Policy Framework): SPF là một cơ chế xác thực email dựa trên việc xác định các máy chủ được phép gửi email thay mặt cho một miền cụ thể. Chủ sở hữu miền sẽ cung cấp thông tin SPF bằng cách thiết lập các bản ghi DNS cho miền của họ. SPF giúp xác định các máy chủ email được phép gửi email từ domain cụ thể, nhằm ngăn chặn việc giả mạo địa chỉ gửi email. Khi một email được nhận, máy chủ email của người nhận sẽ kiểm tra xem máy chủ gửi có được phép gửi email thay mặt cho miền đó hay không.
Ví dụ cho một txt SPF record:
```
"v=spf1 +mx +a +ip4:103.200.100.110 ~all"
```
- PTR (Pointer Record): Là bản ghi DNS ngược, giúp xác định tên miền của một địa chỉ IP, thường được sử dụng trong quá trình kiểm tra email để xác nhận tính hợp lệ của địa chỉ IP.
Ví dụ về một PTR record:
```
179.23.220.220.in-addr.arpa. PTR host1000.vietnix.vn.
dig 179.23.200.xxx.in-addr.arpa.
23.200.103.in-addr.arpa. 1800	IN	SOA	ns1.vietnix.net. hostmaster.23.200.103.in-addr.arpa.
```

## 4. DNS
- DNS (Domain Name System) là hệ thống phân giải tên miền giúp phân giải tên miền thành địa chỉ IP
- Cấu trúc của một tên miền:

![image](https://github.com/user-attachments/assets/77f30306-9419-4175-b684-43614a73992c)

Ví dụ về cách phân giải tên miền khi truy cập đến vietnix.vn
```
    1. Browser sẽ kiểm tra cache nếu trước đó ta đã truy cập vietnix.vn thì nó sẽ lấy IP trong cache ra và sử dụng luôn.
    2. Nếu chưa có cache thì nó sẽ gửi request để hỏi DNS Server của nhà mạng - ISP.
    3. Resolver gửi request lên Root DNS Server, nhưng hiện tại server này nó chưa biết IP nhưng nó sẽ lấy TLD .vn
    4. Root DNS chỉ đến TLD Server .vn tìm đến cung cấp tên miền quốc gia, server này sẽ tiếp tục chỉ đến DNS của vietnix.vn
    5. TLD server gửi request đến authority domain của vietnix để lấy record IP chính xác. Và cuối cùng là trả về IP.
```
- Các loại record DNS:

    A Record: Dùng để xác định địa IP của tên miền
Ví dụ A record:
```
vietnix.tech IN A 179.23.220.220.
```
    CNAME Record: Dùng để chuyển hướng tên miền này sang tên miền khác.
Ví dụ:
```
www.vietnix.vn IN CNAME vietnix.vn.
mail.vietnix.vn IN CNAME mailserver.vietnix.vn.
```
    MX Record: Dùng để tìm xác định máy chủ email
Ví dụ MX record:
```
MX record: vietnix.tech IN MX 5 mail.vietnix.tech (số 5 ở đây là priority, số càng thấp thì priority càng cao)
```
  - TXT Record: Dùng để lưu trữ thông tin về domain, thường dùng cho các mục đích như SPF, DKIM.
  - NS Record: Dùng để xác định máy chủ DNS của domain.
Ví dụ NS Record:
```
vietnix.vn. NS ns1.vietnix.net.
vietnix.vn. NS ns1.vietnix.net.
```
