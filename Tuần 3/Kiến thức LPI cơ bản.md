## Kiến thức LPI cơ bản

**1. Ping và hping3**
```
trung-vietnix@trungvietnix:~$ ping vietnix.vn
PING vietnix.vn (103.90.224.90) 56(84) bytes of data.
64 bytes from 103.90.224.90: icmp_seq=1 ttl=53 time=6.40 ms
64 bytes from 103.90.224.90: icmp_seq=2 ttl=53 time=16.9 ms
64 bytes from 103.90.224.90: icmp_seq=3 ttl=53 time=11.6 ms
64 bytes from 103.90.224.90: icmp_seq=4 ttl=53 time=8.49 ms
```
TTL (Time-to-live): là thời gian tồn tại của một gói tin trước khi bị xóa, mỗi khi gói tin đi qua một hop (ví dụ router) thì ttl sẽ giảm đi 1. TTL giúp lưu ngăn các gói tin đi sai đường nếu có lỗi định tuyến.

Time: là thời gian mà gói tin đi từ nguồn đến đích và quay trở lại, time giúp ta biết thời gian phản hồi, được tính bằng mili giây.

Thực hiện hping3 đến vietnix.vn
```
trung-vietnix@trungvietnix:~$ sudo hping3 vietnix.vn
[sudo] password for trung-vietnix:          
HPING vietnix.vn (wlp1s0 103.90.224.90): NO FLAGS are set, 40 headers + 0 data bytes
^C
--- vietnix.vn hping statistic ---
96 packets transmitted, 0 packets received, 100% packet loss
round-trip min/avg/max = 0.0/0.0/0.0 ms
```

- Kết quả của hping3 đến vietnix.vn là không có phản hồi, 100% packet đã bị drop vì một số lý do như firewall đã chặn packet đến từ công cụ hping3 hoặc chưa hoàn thành bắt tay 3 bước hoặc một số thông số trong gói chưa hợp lệ.
- hping3 là công cụ quét mạng và máy phân tích cho giao thức TCP/IP, thay vì ping chỉ sử dụng gói ICMP thì hping3 có thể sử dụng giao thức TCP
- hping3 có thể custom tùy TCP flag (SYN, ACK, FIN, xmas,...) cho gói tin nó muốn gửi
- hping3 còn được sử dụng như một công cụ DoS SYN flood
- Chức năng của hping3 nhiều hơn hoàn toàn so với ping

**2. SSH command**
```
Dùng password: ssh user@host -> nhập password
Dùng key: ssh -i key.pem user@host (sử dụng private key)
Dùng port custom: ssh user@root -p number_port (nếu server của mình cần cấu hình trước)
```
**3. scp command**
```
scp 1 file: scp /path/file user@host:/path/folder (ví dụ: scp /home/trung/text.txt trung123@192.168.1.1:~/)
scp 1 folder: scp -r /path/folder user@host:/path/folder
```
**4. rsync command**
```
rsync file: rsync -avz tmep.txt /home/trung/backup/ (thực hiện sao chep file temp.txt tới địa chỉ chỉ định đồng thời nén và giữ nguyên quyền của tệp)
rsync folder: rsync -av /home/trung/temp /home/trung/backup
rsync increamental: rsync -auv --delete /home/trung/temp /home/trung/backup (sao chép hay update những tệp được thay đổi từ thư mục nguồn và xóa những tệp không có trong thư mục nguồn)
```
**5. cat command**
```
cat nội dung 1 file: cat /path/file (cat /home/trung/temp.txt)
cat dòng thứ <n> trong file: cat -n /path/file | awk 'NR==2'
cat nhiều dòng vào 1 file bằng EOF: cat /path/file > /path/newfile.txt
```
**6. echo command**

- Dùng echo để chèn thêm 1 dòng vào cuối file: echo "new line" >> /path/file.txt
- Dùng echo để overwirte nội dung của file: echo "overwrite" > /path/file.txt (nếu file file.txt chưa tồn tại thì sẽ tự tạo file.txt)

**7. tail/head command**
```
tail /path/file -> hiển thị phần đuôi của file
tail -n 5 /path/file -> hiển thị 5 dòng cuối của file
```
![image](https://github.com/user-attachments/assets/6aa114ad-1091-4e11-b53f-c965d1f83e26)

```
head /path/file -> hiển thị phần đầu của file
head -n 5 /path/file -> hiển thị 5 dòng cầu của file
```
![image](https://github.com/user-attachments/assets/afa27363-c2bd-4f7f-9620-d0ec1782480b)

**8. tail và tailf**
```
tail /path/file -> giúp xem phần đuôi của file
tail -f /path/file -> giúp phần đuôi của file trực tiếp (thường để dùng để xem log, các log mới xuất hiện sẽ hiện lên liên tục)
```
**9. sed command**

Dùng sed để find and replace một string trong file

Ví dụ muốn replace chữ "hello" thành "hi" đầu tiên trong từng dòng trong file file temp.txt: 
```
 sed -i 's/\bhello\b/hi/' /home/trung/temp.txt
```

Tham số -i dùng để chỉnh sửa file trực tiếp và sử dụng \b để bao bọc từ cần replace để đảm bảo replace chính xác

![image](https://github.com/user-attachments/assets/a29c5738-426b-4f30-b539-d1403cd34ddd)

Ví dụ muốn replace chữ "hello" thành "sunny" trong cả file temp.txt ta chỉ cần thêm tham số g ở cuối như ví dụ dưới: 
```
 sed -i 's/\bhello\b/sunny/g' /home/trung/temp.txt
```
![image](https://github.com/user-attachments/assets/ddfb2912-e221-4a5f-9aa1-20a1603ebc0c)

**10. traceroute/tracert command**
```
trung-vietnix@trungvietnix:~$ traceroute vietnix.vn
traceroute to vietnix.vn (103.90.224.90), 30 hops max, 60 byte packets
 1  192.168.0.1 (192.168.0.1)  4.177 ms  4.589 ms  4.555 ms
 2  localhost (27.71.251.149)  4.539 ms  4.556 ms  4.540 ms
 3  10.255.39.247 (10.255.39.247)  8.175 ms 10.255.39.243 (10.255.39.243)  8.162 ms  8.146 ms
 4  * * *
 5  localhost (27.68.236.34)  8.075 ms localhost (27.68.236.46)  8.061 ms localhost (27.68.236.242)  8.048 ms
 6  * 203.113.187.98 (203.113.187.98)  51.615 ms  3.662 ms
 7  static.vnpt.vn (113.171.45.66)  3.649 ms *  3.633 ms
 8  static.vnpt.vn (113.171.46.226)  7.344 ms static.vnpt.vn (113.171.48.190)  7.338 ms static.vnpt.vn (113.171.46.226)  7.333 ms
 9  172.16.34.178 (172.16.34.178)  7.328 ms static.vnpt.vn (113.171.49.186)  7.323 ms static.vnpt.vn (113.171.49.94)  7.319 ms
10  172.16.34.178 (172.16.34.178)  7.313 ms 172.18.19.21 (172.18.19.21)  7.301 ms  3.953 ms
11  103.90.224.90 (103.90.224.90)  7.135 ms  7.567 ms  7.109 ms
Giải thích:
- traceroute đến vietnix.vn tối đa 30 hop được theo dõi
- Các số thứ tự bên trái là các hop hay các router mà gói tin đi qua
- Hop 1: Hop đầu tiên, là thường là IP của router để đi ra mạng, 4.177 ms, 4.589 ms, 4.555 ms là thời gian đi đến router và quay lại (3 lần) 
- Hop 2: Thời gian đi đến một router khác trong mạng mất khoảng 4.5ms
- Hop 3: Thời gian gói tin đi từ 27.71.251.149 đến 10.255.39.247 hoặc 10.255.39.243 mất khoảng 8.1ms
- Hop 4: Có thể là do router này không phản hồi traceroute
- Hop 5: Thời gian gói tin đi từ hop 4 đến 27.68.236.34 hoặc 27.68.236.46 mất khoảng 8.0ms
- Hop 6: Có dấu *, 1 router không phản hồi, và thời gian từ hop 5 đến router 203.113.187.98 có thời gian khác nhau 51.615 ms  3.662 ms
- Hop 7: Gói tin đi đến nhà mạng vpnt, thời gian phản hồi nhanh khoảng 3ms
- Hop 8: Đi đến các ISP của nhà mạng, thời gian phản hồi khoảng 7.3ms
- Hop 9: Thời gian phản hồi khoảng 7.3ms như hop 9
- Hop 10: Đi đến router 172.16.34.178 và 172.18.19.21 thời gian phản hồi khoảng 7ms
- Hop cuối cùng đi đến vietnix và thời gian phản hồi khoảng 7ms
```
**11. netstat command**
```
Hiển thị các socket đang listen: netstat -l
Không show hostanme: netstat --numeric-hosts
Không show portname: netstat --numeric-ports
Không show process name/PID: netstat -a -p
Chỉ show tcp socket: netstat -t
Chỉ show ucp socket: netstat -u
```
**12. sort command**
```
sort theo thứ tự tăng dần: sort /path/file
sort theo thứ tự giảm dần: sort /path/file
sort theo column: sort -k3 /path/file -> sort theo cột thứ 3
```
**13. uniq command**

- lọc ra các dòng lặp lại trong một file: uniq /path/file
- lọc ra các dòng lặp lại trong file và đếm số lượng các dòng lặp lại: uniq -c /path/file

**14. wc command**

- Đếm số dòng trong file: wc -l /path/file
- Đếm số kí tự trong file: wc /path/file -> cột thứ 3 là số character có trong file

**15. chmod, chown, chattr command**

chattr command:
```
chattr +i /path/file -> file này không thể rename, không thể tạo symlink, không thể thực thi, không thể write
chattr +a /path/file -> file này không thể rename, không thể tạo symlink, không thể thực thi, chỉ có thể nối thêm nội dung vào file
chattr +d /path/file -> file này không được backup khi tiến trình dump chạy
```
Phân quyền bằng số, phân quyền bằng chữ: 

- Cho phép owner đọc ghi thực thi, group đọc thực thi, user đọc: chmod 754 /path/file hoặc đệ quy chmod -R 754 /path/directory
   + Với quyền 7 => rwx (4+2+1 <=> 2^2+2^1+2^0)
   + Với quyền 6 => rw- (4+2+0 <=> 2^2+2^1)
   + Với quyền 5 => r-x (4+0+1 <=> 2^2+2^0)
   + Với quyền 4 => r-- (4+0+0 <=> 2^2)
   + Với quyền 3 => -wx (0+2+1 <=> 2^1+2^0)
   + Với quyền 2 => -w- (0+2+0 <=> 2^1)
   + Với quyền 1 => --x (0+0+1 <=> 2^0)
- Thêm quyền thực thi cho file: chmod +x /path/file (lệnh này sẽ cấp quyền thực thi cho cả owner, group và other)
Đổi owner user/group
```
- Đổi đồng thời owner user và group: chown root /path/file
- Đổi mỗi owner user: chown root: /path/file
- Đổi mỗi owner group: chown :root /path/file
- Đổi owner đệ quy: chown -R root /path/directory
```
**16. find command**
```
find các file có đuôi .log: find / -type f -name "*.log"
find các folder có tên abc: find / -type d -name "abc"
find các file có tên abc: find / -type f -name "abc"
find các file có tên abc và thực hiện phần quyền read only cho file: find / -type f -name "abc" | xarg chmod +r
```
**17. cp command**
```
cp file: cp /path/file /path/directory
copy file /home/trung/txt.rar tới /home/trung/download: cp /home/trung/txt.rar /home/trung/download

cp folder: cp -R /path/folder /path/folder
copy folder /home/trung/aaa tới /home/trung/download: cp -R /home/trung/aaa /home/trung/download
```
**18. mv command**
```
mv file: mv /path/file /path/directory
mv file /home/trung/txt.rar tới /home/trung/download: mv /home/trung/txt.rar /home/trung/download
mv folder: mv /path/directory /path/directory
mv folder /home/trung/aaa tới /home/trung/download: mv /home/trung/aaa /home/trung/download
```
**19. cut command**

- cut kí tự thứ <n> trong string: cut -c4 <<< "abcdefghijk" -> lấy ký tự thứ 4 (d) trong chuỗi, nếu cut file mà có dòng thì nó sẽ lấy ký tự thứ 4 của mỗi dòng
- cut từ kí tự thứ <n> trở về sau: cut -c4- <<< "abcdefghijk" -> lấy ký tự thứ 4 trở về sau (defghijk) trong chuỗi, nếu cut file mà có dòng thì nó sẽ lấy ký tự thứ 4 của mỗi dòng đến hết của dòng đó
- cut từ kí tự thứ <n> trở về trước: cut -c1-4 <<< "abcdefghijk" -> lấy ký tự thứ 4 trở về trước (abcd) trong chuỗi,  nếu cut file mà có dòng thì nó sẽ lấy ký tự thứ 4 của mỗi dòng đến ký tự đầu của dòng đó

**20. dig command**
```
Dùng Dig command để kiểm tra resolv record A: dig A vietnix.vn
Dùng Dig command để kiểm tra resolv record MX: dig MX vietnix.vn
Dùng Dig command để kiểm tra resolv record NS: dig NS vietnix.vn

Dùng Dig command để kiểm tra resolv record A với custom DNS: dig A @8.8.8.8 vietnix.vn
Dùng Dig command để kiểm tra resolv record MX với custom DNS: dig MX @8.8.4.4 vietnix.vn
Dùng Dig command để kiểm tra resolv record NS với custom DNS: dig NS @8.8.8.8 vietnix.vn
```
**21. tar/zip/unzip command**
```
- Nén/Giải nén file tar.gz: sudo tar -zxvf tar.gz | zip -r tar.gz.zip tar.gz
- Nén/Giải nén file .zip: unzip temp.zip | zip -r temp.zip temp.txt
```
**22. mount/umount command**

Ví dụ Add thêm một ổ cứng sdb ~ 5gb
- Kiểm tra được có bao nhiêu ổ cứng trên máy chủ: lsblk -> cho ta biết có bao nhiêu ổ cứng, size, và địa chỉ mount của chúng)
- Mount ổ cứng vào /mnt/test (ví dụ đã tạo partition ổ cứng là sdb1): 
```
sudo mount /dev/sdb1 /mnt/test
df -h -> để kiểm tra
```
- Umount /mnt/test:
```
sudo umount /mnt/test
```
**23. Symbolic Links, Hard Links command**

- Khi dùng symbol link sẽ tạo ra một file, file này được trỏ đến file gốc nhưng không có nội dung, nhưng nếu thực hiện chỉnh sửa (vim) file này thì nó sẽ chuyển đến chỉnh sửa ở file gốc.
- Khi dùng hard link sẽ tạo ra một file tương tự với file gốc, file này có nội dung như file gốc và không trỏ đến file gốc như sym link mà trỏ đến vị trí trong memory như file gốc.

Ví dụ về Sym Link và Hard Link:
```
- Tạo Symbol link: ln -s /home/trung/sym.conf /home/trung2/symnew.conf
- Tạo Hard link: ln /home/trung/sym.conf /home/trung2/symnew.conf
```

**24. ls command**

Liệt kê danh sách file/thư mục:
```
- Liệt kê file/thư mục ở thư mục hiện tại: ls
- Liệt kê file/thư mực ở thư mực khác: ls /path/directory
```
Liệt kê danh sách file/thư mục và thuộc tính: 
```
- Liệt kê file/thư mục và thuộc tính ở thư mục hiện tại: ls -l
- Liệt kê file/thư mục và thuộc tính ở thư mực khác: ls -l /path/directory
```
Show file ẩn:
```
- Liệt kê file/thư mục ẩn ở thư mục hiện tại: ls -a
- Liệt kê file/thư mục ẩn ở thư mực khác: ls -a /path/directory
```

**25. ps command**

- show tiến trình: ps aux
- kill tiến trình: kill <process> ||||| kill 897 -> kill process có PID 987

**26. top command**

Dùng lệnh top để kiểm tra tài nguyên cpu đang sử dụng của một vài process đang chạy.

Load average: chỉ số này hiển thị thời gian load hệ thống trong 1 phút, 5 phút và 15 phút cuối. Load average mỗi hệ thống sẽ khác nhau, với single-core thì load average nên dưới 1, còn multi-core thì nên dưới số core.

Zombie process: Số lượng tiến trình không tồn tại hoặc bị hỏng

Sleeping process: Là tiến trình đang sleep để chờ tiến trình nào đó hoặc chờ tài nguyên

us:	Phần trăm CPU dành cho tiến trình người dùng.

sy:	Phần trăm CPU dành cho tiến trình hệ thống.

ni:	Phần trăm CPU dành cho các tiến trình "nice".

id:	Phần trăm CPU rảnh rỗi.

wa:	Phần trăm CPU đang chờ I/O.

hi:	Phần trăm CPU xử lý interrupt phần cứng.

si	:Phần trăm CPU xử lý interrupt phần mềm.

st:	Phần trăm CPU bị steal trong môi trường ảo hóa.

**27. free command**

trung-vietnix@trungvietnix:~$ free
```
               total        used        free      shared  buff/cache   available
               
Mem:        20273992     6305720    11956124     1394352     3737488    13968272

Swap:        5861372           0     5861372
```
- ram used: là dung lượng ram đang được sử dụng hiện tại
- free: là dung lượng còn trống hay available
- buff/cache: dung bộ nhớ dành cho cache và buffer
- shared: dung lương bộ nhớ chia sẻ giưa các process


**28. df command**

- Xem dung lượng disk
```
trung-vietnix@trungvietnix:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           2,0G  2,0M  2,0G   1% /run
efivarfs        184K  157K   23K  88% /sys/firmware/efi/efivars
/dev/nvme0n1p5  118G   15G   98G  13% /
tmpfs           9,7G  392M  9,3G   4% /dev/shm
tmpfs           5,0M  8,0K  5,0M   1% /run/lock
/dev/nvme0n1p1  256M   67M  190M  26% /boot/efi
tmpfs           2,0G  2,6M  2,0G   1% /run/user/1000
```

- Phân vùng mount đến / là phân vùng root chứa tất cả dữ liệu hệ thống của máy, phân vùng này rất quan trọng và nó thường được cấp dung lượng nhiều nhất.
