### D. (Bonus - không bắt buộc)
 + tạo thư mục ./myapi
 + tạo file ./myapi/app.py sử dụng Python + Flask để làm gì đó funny

 <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 215722" src="https://github.com/user-attachments/assets/c4545110-76f0-4cd3-b4bf-d098a0239e50" />

 + tạo file ./myapi/requirements.txt chứa các thư viện mà app.py sử dụng (theo như app.py ví dụ thì requirements.txt chỉ cần có nội dung: flask)
 + tạo file ./myapi/Dockerfile để khai báo sử dụng Python 3.9 slim

   <img width="1919" height="1077" alt="Ảnh chụp màn hình 2026-04-12 215855" src="https://github.com/user-attachments/assets/74bb0a41-d2ff-4021-8313-1a4167290374" />

 # Sử dụng phiên bản Python nhẹ (alpine) để giảm dung lượng image
 FROM python:3.9-slim

 # Thiết lập thư mục làm việc bên trong container
 WORKDIR /app

 # Sao chép file requirements vào và cài đặt thư viện
 COPY requirements.txt .
 RUN pip install --no-cache-dir -r requirements.txt

 # Sao chép toàn bộ mã nguồn vào container
 COPY . .

 # Thông báo container sẽ chạy ở cổng 9630
 EXPOSE 9630

 # Lệnh khởi chạy ứng dụng
 CMD ["python", "app.py"]
 + Sửa đổi docker-compose để sử dụng myapp (xem phần tham khảo ở dưới)

  <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 220300" src="https://github.com/user-attachments/assets/8bc3af91-836c-46e6-9f84-b4e081a40698" />

 + Sửa đổi nginx/nginx.conf để /api trỏ tới service myapp cổng 9630

 <img width="1919" height="1079" alt="Ảnh chụp màn hình 2026-04-12 224331" src="https://github.com/user-attachments/assets/38a8f322-49f3-483a-8380-269b9e1bdf96" />

 <img width="1919" height="1049" alt="Ảnh chụp màn hình 2026-04-12 224339" src="https://github.com/user-attachments/assets/bf75216c-e480-4b71-a883-bae794a264e7" />
