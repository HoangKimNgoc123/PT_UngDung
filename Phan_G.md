### Triển khai ứng dụng đến End-user
+ Trong Cloudflare: Tạo tunnel (đường hầm), chọn loại triển khai cho docker

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 225412" src="https://github.com/user-attachments/assets/74ed75f7-95fc-4e46-8a80-aed2a987f25e" />

+ Convert lệnh docker run ... sang dạng docker compose
+ Khai báo kết quả convert vào trong file docker-compose.yml
+ Chạy lại docker compose
+ Public ứng dụng bằng cách thêm 1 router trỏ tới container đang chạy trong docker, dữ liệu sẽ đi qua tunnel, url dạng sub-domain
+ Kiểm tra url sub-domain đã hoạt động public cho mọi end-user

  <img width="1908" height="1072" alt="Ảnh chụp màn hình 2026-04-12 225600" src="https://github.com/user-attachments/assets/1d7c26a6-f28c-4c06-a067-b3f22a80e266" />

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 230830" src="https://github.com/user-attachments/assets/3685e3af-b337-4347-acf2-9af2af0e897f" />

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 230748" src="https://github.com/user-attachments/assets/8c172f90-ec89-4452-bc81-8998d3f09c9e" />

+ Link url đã hoạt động public cho mọi end-user: https://ngoc.hoangkimngoc-mobile.id.vn/
