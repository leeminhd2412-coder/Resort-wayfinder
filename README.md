# Resort Wayfinder – Demo

Web app chỉ đường ngắn nhất trong khu nghỉ dưỡng, dựa trên bản đồ có sẵn (`map.jpg`).
Không cần cài đặt, không cần đăng nhập — mở bằng trình duyệt (kể cả qua quét QR).

## Chạy thử ở máy

```bash
python3 -m http.server 8000
# mở http://localhost:8000
```

## Cách hoạt động

- `NODES` trong `index.html`: danh sách điểm trên bản đồ, tọa độ tính theo % kích thước ảnh gốc (1697×1200px).
- `EDGES`: các đoạn đường nối giữa 2 điểm + trọng số (mét ước lượng).
- Thuật toán Dijkstra tìm đường ngắn nhất giữa 2 điểm được chọn, vẽ đè lên bản đồ bằng SVG.

**Lưu ý:** tọa độ và trọng số hiện là ước lượng bằng mắt — cần đo lại chính xác theo bản đồ thật (đo khoảng cách pixel so với 1 đoạn đã biết chiều dài thật) trước khi dùng chính thức cho khách.

## Đưa lên GitHub Pages (miễn phí, có QR truy cập)

```bash
git init
git add .
git commit -m "Resort wayfinder demo"
git branch -M main
git remote add origin https://github.com/<tên-bạn>/<tên-repo>.git
git push -u origin main
```

Sau đó vào **Settings → Pages** của repo, chọn branch `main`, thư mục `/ (root)` → Save.
Trang sẽ chạy tại `https://<tên-bạn>.github.io/<tên-repo>/`. Dùng link đó tạo mã QR (ví dụ tại qr-code-generator.com) để khách quét.

## Việc cần làm tiếp

- Hiệu chỉnh lại tọa độ node + đường đi cho khớp bản đồ thật.
- Thêm các phòng villa/cabana cụ thể nếu cần chỉ đường tới từng phòng.
- Cân nhắc thêm định vị GPS ngoài trời và bản PWA để dùng offline.
