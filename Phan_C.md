### Cấu hình docker compose:
- Tạo thư mục: ~/myapp
- Chuyển vào trong thư mục ~/myapp
- Tạo thư mục: ./myweb
- Tạo file ./myweb/index.html

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 101939" src="https://github.com/user-attachments/assets/f258536f-4bbb-4dbb-93c8-39bb8df15232" />

- Tạo file docker-compose.yml để nó sẽ có các dịch vụ sau:
- Khai báo sử dụng nodered/node-red, cổng 1880, dữ liệu nằm tại thư mục ./nodered
- Khai báo sử dụng nginx, cổng 80, cấu hình trong file ./nginx/nginx.conf

  <img width="1909" height="1079" alt="Ảnh chụp màn hình 2026-04-12 102721" src="https://github.com/user-attachments/assets/c9c6d26b-b571-42ab-a6e5-2866bd4a6ec0" />

- Mount thư mục ./myweb thành thư mục /myweb trong nginx
- Mount file ./nginx/nginx.conf vào file /etc/nginx/nginx.conf trong nginx
- Edit file ./nginx/nginx.conf để:
- Cấu hình web server cổng 80
- server_name là sub-domain (sub-domain tuỳ ý của em)
- location / trỏ tới root là thư mục /myweb
- location /api dùng proxy_pass trỏ tới 1 (hoặc nhiều) node http_in của nodered

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 103002" src="https://github.com/user-attachments/assets/91ae901e-dd37-441f-95e6-c3c83c9c8246" />

- Edit file ./nodered/settings.js để nodered bắt buộc đăng nhập
- Chạy docker-compose lần đầu để Node-RED tự sinh file cấu hình trong thư mục ./nodered, sau đó mới tiến hành sửa settings.js và restart lại container

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 104457" src="https://github.com/user-attachments/assets/cf48750d-5f5c-4a19-9228-523313c34eb4" />

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 105132" src="https://github.com/user-attachments/assets/438c0cde-f1e5-41e3-ac26-912b0d42dd96" />

  <img width="1893" height="1052" alt="Ảnh chụp màn hình 2026-04-12 110638" src="https://github.com/user-attachments/assets/9c2ba8fb-5b0d-4c1a-b7bc-24663713bce5" />

  <img width="1893" height="1052" alt="Ảnh chụp màn hình 2026-04-12 110638" src="https://github.com/user-attachments/assets/e7384919-3d34-4543-8b25-f2211abbe76d" />

