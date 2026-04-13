### Cài đặt Ubuntu + Docker
1. Cài đặt hệ điều hành Ubuntu 24.04.4 LTS
- Sử dụng một trong các công cụ để giả lập: HyperV (có sẵn của windows), VirutualBox (Miễn phí), VM_Ware (bản quyền)
- Download file iso để cài đặt.
- Cấu hình mạng trong Ubuntu (và công cụ giả lập) để cho phép truy cập SSH vào Ubuntu từ cmd của windows

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-11 220101" src="https://github.com/user-attachments/assets/50441a8d-39a9-4704-99f9-9d14e93e34c7" />

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-11 220203" src="https://github.com/user-attachments/assets/1b227a21-6452-4e9f-9b5e-b3bb0338d7cb" />

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-11 220305" src="https://github.com/user-attachments/assets/5db7b821-38d8-419a-9fbb-08d33617625f" />

- Ví dụ:
  + để ssh tới ubuntu ở ip 192.168.100.123, user là admin thì mở CMD trên windows,
  + gõ lệnh: ssh admin@192.168.100.123
  + hệ thống sẽ yêu cầu nhập password (chú ý : password sẽ không hiện ra)
  + sau khi login thành công sẽ thấy màn hình chào hỏi của ubuntu
  
  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-13 170126" src="https://github.com/user-attachments/assets/8fcc5506-3875-4ce8-b751-c12ea9f9fe2d" />

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 095143" src="https://github.com/user-attachments/assets/09c95264-c3b2-4812-b689-75e476cbfd71" />

- Tìm hiểu các lệnh cơ bản của ubuntu, các lệnh cần tìm hiểu:
  + Liệt kê các file trong thư mục: ls
  + Tạo thư mục: mkdir nameFolder
  + Chuyển thư mục làm việc: cd path
  + Copy file: cp file_nguồn path/file_đích
  + Thay đổi quyền thao tác file: sudo chmod xxx filename
  + Edit file: sudo nano tenfile
  + CTRL+o : lưu nội dung sau khi edit
  + CTRL+x : thoát edit file
  + Xem ip của máy ubuntu: ip -4 addr

    <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 100406" src="https://github.com/user-attachments/assets/6d584fd1-1064-4836-85fb-737569cb0ab0" />

- Cài đặt docker cho Ubuntu
  + Kiểm tra phiên bản docker vừa cài đặt, kiểm tra phiên bản của docker compose
  + Cấu hình để docker chạy mà không cần tiền tố sudo
  + Tìm hiểu tập lệnh của docker và docker compose
  +Đảm bảo tường lửa trên Ubuntu đã cho phép các cổng 80, 1880, 9630 (Lệnh: sudo ufw allow ...)

<img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 095611" src="https://github.com/user-attachments/assets/2a092512-fb44-48d6-a115-cebf7ddd03c3" />
