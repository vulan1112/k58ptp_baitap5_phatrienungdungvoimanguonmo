# k58ptp_baitap5_phatrienungdungvoimanguonmo
VU LAN_K225480106036_Phát triển ứng dụng với mã nguồn mở_bài tập 05

BÀI TẬP LỚN

môn: phát triển ứng dụng với mã nguồn mở - tee0421

bt1: Ubuntu + Docker: DÙNG DOCKER ĐỂ BUILD myapi

bt2: django (python): WEB QUẢN LÝ TIỆM CẦM ĐỒ

bt3: wordpress (php) + mariadb + phpmyadmin (chỉ để xem các tables)

bt4: wordpress + n8n + bot telegram+gemini: auto đăng bài bằng cách chát

bt5:
# I.lý thuyết:
## 1.Docker là gì?

+ Docker là một nền tảng mã nguồn mở cho phép đóng gói ứng dụng và tất cả các thành phần phụ thuộc (thư viện, file cấu hình, môi trường hệ thống…) vào trong một đơn vị gọi là container. Nhờ container, ứng dụng có thể chạy giống hệt nhau trên mọi máy tính, từ laptop cá nhân đến server thật, mà không lo gặp lỗi “chạy trên máy em thì được, trên máy thầy thì không hay chạy máy các bạn cũng chạy được như máy em nếu có môi trường giống máy em”.

## 2.Các keyword được sử dụng trong docker-compose.yml

để mô tả 1 service, network, volume,...

liệt kê + ý nghĩa của từ khoá đó + ví dụ minh hoạ

Em xin liệt kê một số keyword quan trọng đã sử dụng trong bài:

+ version: Khai báo phiên bản Docker Compose đang dùng (em dùng '3.9').
+ services: Phần chính để định nghĩa các container trong dự án.
+ image: Chỉ định image Docker sẽ kéo về (ví dụ: grafana/grafana:latest).
+ container_name: Đặt tên dễ nhớ cho container.
+ restart: Chính sách khởi động lại. Em hay dùng unless-stopped hoặc always.
+ ports: Mở cổng để truy cập từ ngoài vào (ví dụ: "8080:80").
+ volumes: Lưu dữ liệu ra ngoài container để không mất khi xóa container.
+ environment: Thiết lập các biến môi trường như password, username.
+ depends_on: Khai báo service này phải khởi động sau service kia.
+ build: Dùng khi tự build image từ Dockerfile (em dùng cho Flask API).
  
## 3.Ưu điểm khi triển app sử dụng docker là gì?
+ Ứng dụng chạy nhất quán trên mọi máy.
+ Dễ dàng chia sẻ dự án cho người khác chỉ qua một file docker-compose.yml.
+ Tiết kiệm tài nguyên hơn máy ảo.
+ Dễ backup, khôi phục và di chuyển giữa các server "Hỗ trợ phát triển nhanh, làm việc nhóm và CI/CD tốt".
+ Hỗ trợ phát triển và triển khai nhanh chóng "Dễ scale khi cần mở rộng".
## 4.dùng docker: tạo app, test app OK trên laptop cá nhân
giờ muốn triển khai app này trên máy chủ thật ko có internet
thì các bước cần làm là?

1.Test và chạy ổn định trên laptop cá nhân.

2.Xuất toàn bộ image thành file .tar bằng lệnh docker save.

3.Copy toàn bộ thư mục dự án + file .tar lên server (qua USB).

4.Trên server: Load image bằng lệnh docker load -i *.tar.

5.Chạy docker compose up -d để khởi động hệ thống.
 
# II.Thực hành áp dụng: APP MONITOR + ALERT DATA REALTIME
   Sử dụng docker compose có nhiều serivce và các thành phần cần thiết để tạo thành ứng dụng:
## nodered liên tục lấy dữ liệu từ nguồn nào đó (chứng khoán, thời tiết, giá vàng,...)nguồn thực tế, số liệu luôn động sau thời gian ngắn

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ed575981-7217-42ef-ad96-97285e31a5ee" />

<img width="1920" height="1074" alt="image" src="https://github.com/user-attachments/assets/f8f7245e-d843-4780-9358-220be89bd857" />

<img width="1920" height="827" alt="image" src="https://github.com/user-attachments/assets/be13efaf-4987-4978-827c-55a0098ac1e8" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/70fede0e-9166-49c0-8341-3b9c500fe4bf" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7bf27201-e656-4a8e-ba76-09ede6e006f6" />

  + nodered lưu trữ dữ liệu vào 2 database: mariadb để lưu giá trị tức thời
       lưu lịch sử vào influxdb
     + sử dụng grafana để trực quan hoá dữ liệu: vẽ biểu đồ
     + sử dụng nginx để làm webserver
       chạy 1 trang web html+js+css làm front-end
       js: lấy dữ liệu tức thời trong mariadb qua (ajax | socket) 
           gọi api (api tự build bằng Flask giống bt1)
           api trả về giá trị tức thời trong mariadb
           hiển thị lên web, auto hiển thị số mới khi thay đổi
       sử dụng iframe để gọi grafana
       hiển thị biểu đồ dữ liệu lịch sử của thông số đã lưu
     + QUAN SÁT DỮ LIỆU LỊCH SỬ => GIÁ TRỊ BẤT THƯỜNG
       (VD MIỀN A..B: OK, DƯỚI A: ALERT LOW, TRÊN B: ALERT HIGH)
     + nodered: kết hợp bot Telegram
       khi dữ liệu not OK, thì gửi tin nhắn từ bot => group trên telegram
       group đã add bot vào: (nhóm đã có 2 người), add thêm 1875746636 thành 3 người
       mỗi khi bot gửi dữ liệu vào nhóm: mọi member of group đều nhận đc
       nội dung alert: tường minh, có value gây alert

     xuất tất cả các container ra file nén.
     xoá mọi container đang chạy
     load lại các container  từ file nén để khôi phục các container đã xoá
=========
quá trình làm: chụp ảnh lại, mô tả cho ảnh

  lưu vào trong github => paste link access public của repo: vào file excel online

=====================

cả 5 bài tập:

biên tập lại xíu để phù hợp với bản print

đóng quyển, ko cần bìa xanh, ko cần giấy bóng kính

header+footer của các trang giấy: có tên + masv, bài tập lớp Môn, số trang

trang 1 có đầy đủ thông tin cá nhân.

lưu ở bm để chấm điểm.
