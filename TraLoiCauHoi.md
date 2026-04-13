1. Tại sao phải dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED?
   - Quản lý tập trung (Single Entry Point): Nếu chỉ có một mình Node-RED thì việc trỏ thẳng Tunnel vào đó là hoàn toàn được. Tuy nhiên, dự án có nhiều thành phần (Frontend Web, Backend Python API, Node-RED). Nginx đóng vai trò là "lễ tân", giúp phân luồng giao thông: ai vào thư mục gốc / thì trả về web tĩnh, ai gọi /api thì đẩy sang Python, ai gọi /nodered thì đẩy sang Node-RED.
   - Bảo mật & Ẩn cấu trúc: Nginx giúp che giấu các cổng thực tế (3333, 1880) và kiến trúc bên trong. Khách truy cập bên ngoài chỉ tương tác qua cổng 80 của Nginx.
   - Xử lý file tĩnh: Nginx cực kỳ tối ưu và nhanh gọn trong việc phục vụ các file HTML, CSS, JS tĩnh (như file index.html), thay vì bắt các nền tảng nặng nề hơn tự xử lý.
2. Sự khác biệt giữa việc Mount file và Mount thư mục trong Docker là gì?
   - Mount thư mục (Directory Bind Mount): Gắn toàn bộ một thư mục từ máy thật (Host) vào máy ảo (Container). Mọi thay đổi (thêm, sửa, xóa file) ở cả 2 bên đều được đồng bộ ngay lập tức. Thường dùng cho mã nguồn web, thư mục lưu log.
   - Mount file (File Bind Mount): Chỉ gắn duy nhất một file cụ thể (ví dụ: nginx.conf). File đó trong container sẽ bị ghi đè bởi file ngoài máy thật. Thường dùng để truyền file cấu hình tĩnh.
   - Lưu ý kỹ thuật: Mount file đôi khi gặp lỗi "đứt gãy" liên kết nếu bạn dùng một số trình soạn thảo (như Vim) để sửa file trên máy thật, vì các trình soạn thảo này thường tạo ra file tạm rồi ghi đè file gốc làm đổi Inode của file. Mount thư mục không bị lỗi này.
3. Nếu thay đổi file index.html ở máy Ubuntu, nội dung trên web có thay đổi ngay không? Tại sao?
   - Có, nội dung sẽ thay đổi ngay lập tức (chỉ cần F5/tải lại trang web).
   - Vì: Nhờ cơ chế Bind Mount trong Docker. Thay vì "copy" file vào trong container, Docker tạo ra một "cánh cửa thần kỳ" (symbolic link cấp thấp) nhìn thẳng từ container ra ổ cứng của máy Ubuntu. Khi Nginx đọc file index.html, nó thực chất đang đọc trực tiếp file nằm trên máy thật theo thời gian thực.
4. docker-compose.yml khai báo các services có phần restart: always hoặc restart: unless-stopped : chúng để làm gì?
   - Đây là các Restart Policies (Chính sách khởi động lại), giúp hệ thống tự động phục hồi mà không cần can thiệp bằng tay:
     + always: Docker sẽ luôn luôn khởi động lại container này bất kể lý do gì (container bị lỗi crash, hoặc máy chủ Ubuntu vừa bị cúp điện khởi động lại).
     + unless-stopped: Tương tự always, nhưng thông minh hơn một chút. Nếu chủ động gõ lệnh dừng nó (docker stop), thì khi máy chủ khởi động lại, Docker sẽ "tôn trọng" quyết định đó và để nó tiếp tục ngủ.
5. Cách khai báo để tất cả các services đều dùng chung 1 network? lợi ích của việc khai báo này là gì? Sửa đổi file docker-compose để tất cả các service đều dùng chung 1 network.
    - Lợi ích:
      + Phân giải DNS tự động: Các container có thể gọi thẳng tên nhau (ví dụ từ Nginx gọi sang API bằng URL http://api-python:3333) thay vì phải dùng địa chỉ IP tĩnh.
      + Bảo mật: Tạo ra một mạng LAN ảo khép kín. Những container không nằm trong network này sẽ không thể nhòm ngó hay giao tiếp được.
```
  version: '3.8'
services:
  nginx:
    image: nginx:latest
    networks:
      - my_app_network
  
  api-python:
    image: python-app
    networks:
      - my_app_network

  cloudflared:
    image: cloudflare/cloudflared:latest
    networks:
      - my_app_network
# Khai báo mạng chung ở cuối file
networks:
  my_app_network:
    driver: bridge
```
6. Tìm cách đưa Cloudflare Token vào trong file .env rồi sau đó thêm .env vào file .gitignore trước khi push code lên github. Tại sao nói đây là điều quan trọng về bảo mật mã nguồn?
  - Cách làm:
    + Tạo file .env cùng thư mục chứa file docker-compose.yml. Ghi vào đó: TUNNEL_TOKEN=eyJh...
    + Sửa file docker-compose.yml: Gọi biến đó ra bằng ${TUNNEL_TOKEN}.
    + Mở file .gitignore, thêm dòng chứa chữ .env vào và lưu lại.
  - Tại sao cực kỳ quan trọng? Nếu đưa Token thẳng vào docker-compose.yml rồi đẩy lên GitHub (đặc biệt là public repo), bất kỳ ai cũng có thể đọc được Token này. Họ có thể dùng Token đó để cướp quyền đường hầm, chạy Tunnel từ máy của họ, hoặc thậm chí tấn công ngược vào hạ tầng mạng cục bộ thông qua Cloudflare Zero Trust. .gitignore ngăn chặn Git theo dõi file .env, giữ bí mật này chỉ nằm duy nhất trên máy chủ Ubuntu.
7. Tại sao chúng ta nên thêm hậu tố :ro khi mount file cấu hình Nginx?
  - :ro viết tắt của Read-Only (Chỉ đọc).
  - Mục đích: Bảo vệ hệ thống. Khi mount file nginx.conf với :ro, container Nginx chỉ có thể đọc nội dung file để chạy, hoàn toàn không có quyền sửa hay xóa file đó trên máy thật. Nếu chẳng may Nginx bị hacker khai thác lỗ hổng và thâm nhập, chúng cũng không thể thay đổi cấu hình mạng  để chuyển hướng truy cập độc hại được.
8. Khi dùng Cloudflare Tunnel: có cần thiết phải mở cổng cho các service nữa không?
  - Không, hoàn toàn không cần thiết, và nên đóng chúng lại.
  - Lý do: Tunnel (container cloudflared) đang nằm cùng một network nội bộ (như đã nói ở câu 5) với Nginx, Python, Node-RED. Tunnel có thể đi cửa sau, nói chuyện trực tiếp với các cổng nội bộ (80, 3333, 1880) của các container này. Việc ánh xạ cổng ra ngoài máy thật bằng ports: - "3000:80" chỉ để tự test ở localhost. Khi đã có Tunnel, nên xóa hẳn phần ports đi để hệ thống an toàn tuyệt đối, đóng sập mọi cánh cửa bên ngoài và chỉ tiếp khách đi từ đường hầm Cloudflare vào mà thôi.
