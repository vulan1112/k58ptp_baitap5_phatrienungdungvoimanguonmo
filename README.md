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

cấu hình nginx và ganafana

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c68893a3-ab99-4a81-b631-3ef1623ddbc0" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/465c99dd-c214-41ed-85f3-23d990d79c22" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/07e61867-e076-46d4-b8a3-8c6bb2d59edf" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b280d6d0-073f-4a9f-af03-0a5b65f3ae62" />

<img width="1873" height="998" alt="image" src="https://github.com/user-attachments/assets/07bda17c-2308-4c2d-95a7-f2c5ca5e6fce" />

<img width="1920" height="1075" alt="image" src="https://github.com/user-attachments/assets/beec3bd6-8e6d-40f6-9dd3-203faa0cff55" />

<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/10ce3fd5-6f00-42b3-aad4-7db3d161e2c5" />

cấu hình node

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5cb22a6f-e8fc-4610-a1b7-c3cf625f47d1" />



  + nodered lưu trữ dữ liệu vào 2 database: mariadb để lưu giá trị tức thời
       lưu lịch sử vào influxdb
     + sử dụng grafana để trực quan hoá dữ liệu: vẽ biểu đồ

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7c50d94b-c976-4752-a197-b79c9904f72b" />

   + sử dụng nginx để làm webserver
       chạy 1 trang web html+js+css làm front-end
       js: lấy dữ liệu tức thời trong mariadb qua (ajax | socket) 
           gọi api (api tự build bằng Flask giống bt1)
           api trả về giá trị tức thời trong mariadb
           hiển thị lên web, auto hiển thị số mới khi thay đổi
       sử dụng iframe để gọi grafana
       hiển thị biểu đồ dữ liệu lịch sử của thông số đã lưu

<img width="1920" height="967" alt="image" src="https://github.com/user-attachments/assets/46423133-e3b3-4fe7-b724-d4f407642aa4" />

<img width="1905" height="565" alt="image" src="https://github.com/user-attachments/assets/5a3a3287-8cc4-4c6b-8baf-0afc594f0b1f" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/de6ef719-0547-4580-b391-6c5c35af6061" />

+ QUAN SÁT DỮ LIỆU LỊCH SỬ => GIÁ TRỊ BẤT THƯỜNG
       (VD MIỀN A..B: OK, DƯỚI A: ALERT LOW, TRÊN B: ALERT HIGH)
+ nodered: kết hợp bot Telegram

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9b1cb9d3-8796-4fb7-8971-965106ab0f8e" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/93fff40f-b9fd-49cc-8fd8-96b68700ea30" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b48ece6f-7661-417d-9b4f-b576ced7f737" />

khi dữ liệu not OK, thì gửi tin nhắn từ bot => group trên telegram

<img width="1184" height="2560" alt="image" src="https://github.com/user-attachments/assets/8e465978-def1-4bf4-9b76-08ade6a52cf8" />

<img width="1184" height="2560" alt="image" src="https://github.com/user-attachments/assets/004135d3-ef90-4dad-86ba-84f15cc7f429" />

group đã add bot vào: (nhóm đã có 2 người), add thêm 1875746636 thành 3 người
mỗi khi bot gửi dữ liệu vào nhóm: mọi member of group đều nhận đc

<img width="1920" height="1033" alt="image" src="https://github.com/user-attachments/assets/a2afeae8-0007-4125-8e46-80ecb95f9185" />

<img width="1184" height="2560" alt="image" src="https://github.com/user-attachments/assets/8a0caeda-bde4-45a3-b09f-5c7ab4fc335f" />

nội dung alert: tường minh, có value gây alert

<img width="1184" height="2560" alt="image" src="https://github.com/user-attachments/assets/71beaec6-d2f0-441c-9f57-ea840fea0348" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/35c520fa-c532-4102-800f-db95bcf16236" />

xuất tất cả các container ra file nén.
+ Xuất toàn bộ image Docker ra file .tar

<img width="1422" height="285" alt="image" src="https://github.com/user-attachments/assets/df0acfde-09b0-496d-8fd5-619af77e6b36" />

+ Nén toàn bộ project

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/367e5fc4-3302-4b2e-a7a7-21c445882161" />

+ Xóa toàn bộ containel (lệnh:docker compose down)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3a5765b2-f618-4485-a30f-8af91bf990bc" />

<img width="1852" height="752" alt="image" src="https://github.com/user-attachments/assets/1b6a587c-5cdb-450f-a430-5b5cb1e6edb7" />

+ Xóa image Docker

<img width="1918" height="1080" alt="image" src="https://github.com/user-attachments/assets/c8dd29c5-3cb8-4fcc-8f1f-887cac791834" />

<img width="1920" height="802" alt="image" src="https://github.com/user-attachments/assets/02b62161-3b01-4c9f-b57c-547ea8f0a6c0" />

<img width="1920" height="551" alt="image" src="https://github.com/user-attachments/assets/a8b0424c-302b-4903-af7c-b76c121477a4" />

Khôi phục hệ thống: (load lại các container  từ file nén để khôi phục các container đã xoá)

<img width="1920" height="931" alt="image" src="https://github.com/user-attachments/assets/e1cfee19-20c1-46dc-be54-ab6db1f4f410" />

Chạy lại websever

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/469c7edf-54d1-4da6-b596-a791a138e8da" />

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
