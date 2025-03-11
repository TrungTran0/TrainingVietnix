1. Ping đến vietnix.vn
trung-vietnix@trungvietnix:~$ ping vietnix.vn
PING vietnix.vn (103.90.224.90) 56(84) bytes of data.
64 bytes from 103.90.224.90: icmp_seq=1 ttl=53 time=6.40 ms
64 bytes from 103.90.224.90: icmp_seq=2 ttl=53 time=16.9 ms
64 bytes from 103.90.224.90: icmp_seq=3 ttl=53 time=11.6 ms
64 bytes from 103.90.224.90: icmp_seq=4 ttl=53 time=8.49 ms
TTL (Time-to-live): là thời gian tồn tại của một gói tin trước khi bị xóa, mỗi khi gói tin đi qua một hop thì ttl sẽ giảm đi 1. TTL giúp lưu 
Time: là thời gian mà gói tin đi từ nguồn đến đích và quay trở lại, time giúp ta biết thời gian phản hồi.

2. SSH command

Dùng password: ssh user@host -> nhập password
Dùng key: ssh -i key.pem user@host (sử dụng private key)
Dùng port custom: ssh user@root -p number_port (nếu server của mình cần cấu hình trước)

3. scp command

scp 1 file: scp /path/file user@host:/path/file

scp 1 folder: scp -r /path/folder user@host:/path/folder

**4. rsync command**

rsync file

rsync folder

rsync increamental

5. cat command

cat nội dung 1 file: cat /path/file

cat dòng thứ <n> trong file: cat -n /path/file | awk 'NR==2'

cat nhiều dòng vào 1 file bằng EOF: cat /path/file > /path/newfile.txt

6. echo command

Dùng echo để chèn thêm 1 dòng vào cuối file: echo "new line" >> /path/file.txt

Dùng echo để overwirte nội dung của file: echo "overwrite" > /path/file.txt

7. tail/head command
cat /path/file | tail
cat /path/file | tail -n 5

cat /path/file | head
cat /path/file | head -n 5

8. tail và tailf
tail /path/file -> giúp xem phần đuôi của file
tail -f /path/file -> giúp phần đuôi của file trực tiếp (thường để dùng để xem log, các log mới xuất hiện sẽ hiện lên liên tục)

**9. sed command**

Dùng sed để find and replace một string trong file

10. traceroute/tracert command

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

Các số thứ tự bên trái là các hop hay các router mà gói tin đi qua, 

11. netstat command
Hiển thị các socket đang listen: netstat -l
Không show hostanme: netstat --numeric-hosts
Không show portname: netstat --numeric-ports
Không show process name/PID: netstat -a -p
Chỉ show tcp socket: netstat -t
Chỉ show ucp socket: netstat -u

12. sort command
sort theo thứ tự tăng dần: sort /path/file
sort theo thứ tự giảm dần: sort /path/file
sort theo column: sort -k3 /path/file -> sort theo cột thứ 3

13. uniq command
lọc ra các dòng lặp lại trong một file: uniq /path/file
lọc ra các dòng lặp lại trong file và đếm số lượng các dòng lặp lại: uniq -c /path/file

14. wc command
Đếm số dòng trong file: wc -l /path/file
Đếm số kí tự trong file: wc /path/file -> cột thứ 3 là số character có trong file

15. chmod, chown, chattr command
chattr command:
chattr +i /path/file -> file này không thể rename, không thể tạo symlink, không thể thực thi, không thể write
chattr +a /path/file -> file này không thể rename, không thể tạo symlink, không thể thực thi, chỉ có thể nối thêm nội dung vào file
chattr +d /path/file -> file này không được backup khi tiến trình dump chạy

Phân quyền bằng số, phân quyền bằng chữ: 
- Cho phép owner đọc ghi thực thi, group đọc thực thi, user đọc: chmod 754 /path/file hoặc đệ quy chmod -R 754 /path/directory
- Thêm quyền thực thi cho file: chmod +x /path/file
Đổi owner user/group
- Đổi đồng thời owner user và group: chown root /path/file
- Đổi mỗi owner user: chown root: /path/file
- Đổi mỗi owner group: chown :root /path/file
- Đổi owner đệ quy: chown -R root /path/directory

16. find command

find các file có đuôi .log: find / -type f -name "*.log"
find các folder có tên abc: find / -type d -name "abc"
find các file có tên abc: find / -type f -name "abc"
find các file có tên abc và thực hiện phần quyền read only cho file: find / -type f -name "abc" | xarg chmod +r

17. cp command
cp file: cp /path/file /path/directory
copy file /home/trung/txt.rar tới /home/trung/download: cp /home/trung/txt.rar /home/trung/download

cp folder: cp -R /path/folder /path/folder
copy folder /home/trung/aaa tới /home/trung/download: cp -R /home/trung/aaa /home/trung/download

18. mv command
mv file: mv /path/file /path/directory
mv file /home/trung/txt.rar tới /home/trung/download: mv /home/trung/txt.rar /home/trung/download
mv folder: mv /path/directory /path/directory
mv folder /home/trung/aaa tới /home/trung/download: mv /home/trung/aaa /home/trung/download

19. cut command
cut kí tự thứ <n> trong string: cut -c4 <<< "abcdefghijk" -> lấy ký tự thứ 4 (d) trong chuỗi, nếu cut file mà có dòng thì nó sẽ lấy ký tự thứ 4 của mỗi dòng
cut từ kí tự thứ <n> trở về sau: cut -c4- <<< "abcdefghijk" -> lấy ký tự thứ 4 trở về sau (defghijk) trong chuỗi, nếu cut file mà có dòng thì nó sẽ lấy ký tự thứ 4 của mỗi dòng đến hết của dòng đó
cut từ kí tự thứ <n> trở về trước: cut -c1-4 <<< "abcdefghijk" -> lấy ký tự thứ 4 trở về trước (abcd) trong chuỗi,  nếu cut file mà có dòng thì nó sẽ lấy ký tự thứ 4 của mỗi dòng đến ký tự đầu của dòng đó

20. dig command
Dùng Dig command để kiểm tra resolv record A: dig A vietnix.vn
Dùng Dig command để kiểm tra resolv record MX: dig MX vietnix.vn
Dùng Dig command để kiểm tra resolv record NS: dig NS vietnix.vn

Dùng Dig command để kiểm tra resolv record A với custom DNS: dig A @8.8.8.8 vietnix.vn
Dùng Dig command để kiểm tra resolv record MX với custom DNS: dig MX @8.8.4.4 vietnix.vn
Dùng Dig command để kiểm tra resolv record NS với custom DNS: dig NS @8.8.8.8 vietnix.vn

21. tar/zip/unzip command
- Nén/Giải nén file tar.gz: sudo tar -zxvf tar.gz | zip -r tar.gz.zip tar.gz
- Nén/Giải nén file .zip: unzip temp.zip | zip -r temp.zip temp.txt

22. mount/umount command

- Add thêm một ổ cứng sdb ~ 5gb

- Kiểm tra được có bao nhiêu ổ cứng trên máy chủ

- Mount ổ cứng vào /mnt/test

- Umount /mnt/test

23. Symbolic Links, Hard Links command

- Khi dùng symbol link sẽ tạo ra một file, file này được trỏ đến file gốc nhưng không có nội dung, nhưng nếu thực hiện chỉnh sửa (vim) file này thì nó sẽ chuyển đến chỉnh sửa ở file gốc.
- Khi dùng hard link sẽ tạo ra một file tương tự với file gốc, file này có nội dung như file gốc và không trỏ đến file gốc như sym link mà trỏ đến vị trí trong memory như file gốc.

Ví dụ về Sym Link và Hard Link:
- Tạo Symbol link: ln -s /home/trung/sym.conf /home/trung2/symnew.conf
- Tạo Hard link: ln /home/trung/sym.conf /home/trung2/symnew.conf

24. ls command
Liệt kê danh sách file/thư mục
- Liệt kê file/thư mục ở thư mục hiện tại: ls
- Liệt kê file/thư mực ở thư mực khác: ls /path/directory
Liệt kê danh sách file/thư mục và thuộc tính: 
- Liệt kê file/thư mục và thuộc tính ở thư mục hiện tại: ls -l
- Liệt kê file/thư mục và thuộc tính ở thư mực khác: ls -l /path/directory
Show file ẩn:
- Liệt kê file/thư mục ẩn ở thư mục hiện tại: ls -a
- Liệt kê file/thư mục ẩn ở thư mực khác: ls -a /path/directory

25. ps command
show tiến trình: ps aux
kill tiến trình: kill <process> ||||| kill 897 -> kill process có PID 987

26. top command

Dùng lệnh top để kiểm tra tài nguyên cpu đang sử dụng của một vài process đang chạy
Load average: chỉ số này hiển thị thời gian load hệ thống trong 1 phút, 5 phút và 15 phút cuối. Load average mỗi hệ thống sẽ khác nhau, với single-core thì load average nên dưới 1, còn multi-core thì nên dưới số core. Với average là 0.75 nghĩa là 75%.
Zombie process: Số lượng tiến trình không tồn tại hoặc bị hỏng
Sleeping process: 
Giải thích về Load average, us, sy, ni, id, wa, hi, si, st, zombie process, sleeping process
