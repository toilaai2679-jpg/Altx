# V507 — AI Vision backend

## 1) Yêu cầu
- Node.js 20 trở lên.
- Một máy chủ có HTTPS (Render, Railway, VPS, hoặc hosting Node.js).
- Anthropic API key hợp lệ. Chỉ lưu key trong biến môi trường máy chủ, không dán vào HTML.

## 2) Triển khai
1. Tải và giải nén gói này.
2. Đưa `server.js` lên dịch vụ Node.js.
3. Đặt biến môi trường:
   - `ANTHROPIC_API_KEY` = API key của mày (secret trên máy chủ).
   - `ALLOWED_ORIGIN` = origin chính xác đang host HTML, ví dụ `https://tool.example.com`. Có thể nhiều origin, phân cách bằng dấu phẩy. Nếu HTML chỉ mở bằng `file://`, hãy host HTML qua HTTPS để CORS hoạt động ổn định.
   - `PORT` = cổng do host cấp (thường host tự đặt).
   - `ANTHROPIC_MODEL` = model Vision khả dụng với tài khoản, mặc định `claude-sonnet-4-20250514`.
4. Lệnh chạy: `node server.js`.
5. Kiểm tra `https://TEN-MIEN-CUA-MAY/api/ai` phải trả JSON `{"ok":true,"service":"v507-ai-vision"}`.

## 3) Cấu hình HTML
Mở `vanhiepV507-1.html`, tìm dòng:
`window.AI_BACKEND_URL = "https://YOUR-DOMAIN.example/api/ai";`
Thay URL mẫu bằng URL HTTPS thật của backend, ví dụ `https://ai-api.example.com/api/ai`. Giữ nguyên phần còn lại, sau đó host lại HTML.

Frontend hiện dùng endpoint này cho GET health-check và POST ảnh + prompt; backend trả nguyên định dạng phản hồi Anthropic mà hàm đọc phiên đang chờ.

## 4) Bảo vệ endpoint
- Không bao giờ đưa `ANTHROPIC_API_KEY` vào HTML hoặc localStorage.
- Đặt `ALLOWED_ORIGIN` chính xác, bật HTTPS, giới hạn quota/chi phí ở nhà cung cấp AI.
- Server có giới hạn kích thước body và rate limit cơ bản theo IP; với dịch vụ công khai nên bật thêm rate limiting ở reverse proxy/hosting và theo dõi log/quota.
- CORS không phải cơ chế xác thực. Nếu endpoint cần dùng riêng tư, thêm xác thực người dùng phía server trước khi công khai.

## 5) Kiểm thử
- Mở URL health-check trong trình duyệt.
- Mở V507, bật chức năng chia sẻ màn hình/AI đọc phiên, chọn cửa sổ/tab game có bảng thống kê phiên.
- Kiểm tra trạng thái AI; khi backend nhận diện được, mã phiên sẽ được chuyển cho hàm xử lý hiện có.

Gói này chỉ thay URL cấu hình AI và thêm backend mẫu; không nhúng API key và không chủ động thay đổi các phần giao diện/chức năng khác.
