# Resort Wayfinder – Demo

Web app chỉ đường ngắn nhất trong khu nghỉ dưỡng, có định vị GPS và chuyển đổi tiếng Việt/Anh.
Không cần cài đặt, không cần đăng nhập — mở bằng trình duyệt (kể cả qua quét QR).

## Tính năng

- Tìm đường ngắn nhất giữa 2 điểm, đường đi **luôn bám theo đúng lối đi** được vẽ trên bản đồ (thuật toán Dijkstra chỉ di chuyển theo các cạnh `EDGES` đã khai báo, không cắt ngang qua nhà/hồ).
- **GPS thật, theo thời gian thực**: bấm "Bắt đầu định vị trực tiếp" — vị trí khách tự động cập nhật liên tục (dùng `watchPosition`), điểm xuất phát và tuyến đường tự vẽ lại mỗi khi khách di chuyển.
- **GPS ảo**: nếu khách không ở resort (demo, thử nghiệm, hoặc GPS thật báo lỗi/nằm ngoài bản đồ), chuyển sang chế độ "GPS ảo" rồi chạm/click vào bất kỳ đâu trên bản đồ để tự đặt vị trí xuất phát.
- Chuyển đổi **VI / EN** ở góc trên phải — dịch toàn bộ giao diện và tên điểm.
- Giao diện responsive: điện thoại hiển thị dạng cuộn dọc (bảng điều khiển trên, bản đồ dưới); máy tính/tablet tự chuyển sang bố cục 2 cột (bảng điều khiển bên trái cố định, bản đồ chiếm phần còn lại, cao vừa màn hình).

## ⚠️ Việc bắt buộc phải làm trước khi dùng thật: hiệu chỉnh GPS

GPS chỉ trả về kinh độ/vĩ độ thật (lat/lng), còn bản đồ là ảnh phẳng nên cần 1 phép quy đổi. Trong `index.html`, tìm khối:

```js
const GPS_CALIBRATION = {
  anchor1: { lat: 15.97000, lng: 108.25000, nodeId: "entrance" },
  anchor2: { lat: 15.96850, lng: 108.24870, nodeId: "beach" },
};
```

Đây hiện là **số giả**, cần thay bằng tọa độ thật:

1. Đứng đúng tại vị trí "Lối vào" ngoài đời, mở Google Maps → xem tọa độ GPS hiện tại → ghi lại `lat, lng`.
2. Làm tương tự tại vị trí "Beach House" (hoặc 1 điểm khác càng xa điểm 1 càng tốt, để phép quy đổi chính xác hơn).
3. Thay 2 cặp số vào `anchor1` và `anchor2`.

Cách quy đổi hiện dùng phép nội suy tuyến tính đơn giản (không xoay bản đồ) — đủ dùng cho bản đồ gần như thẳng hướng Bắc-Nam/Đông-Tây. Nếu bản đồ bị xoay lệch nhiều so với hướng thật, cần nâng cấp lên phép biến đổi có xoay (similarity transform) để chính xác hơn.

## Thêm điểm / đường đi mới

- Thêm 1 điểm: thêm dòng vào `NODES` với id, tên tiếng Việt (`vi`), tên tiếng Anh (`en`), tọa độ `x`,`y` (% theo ảnh `map.jpg`, ảnh gốc 1697×1200px).
- Nối đường đi: thêm dòng vào `EDGES`: `["id_a","id_b", khoảng_cách_mét]`.

## Chạy thử ở máy

```bash
python3 -m http.server 8000
# mở http://localhost:8000
```

## Đưa lên GitHub Pages

Xem hướng dẫn chi tiết đã trao đổi trong lần triển khai trước — tóm tắt: đưa 3 file (index.html, map.jpg, README.md) vào repo → Commit → Publish → Settings → Pages → chọn branch main, `/ (root)` → Save.

## Việc cần làm tiếp

- Đo lại tọa độ node + trọng số đường đi cho khớp bản đồ thật (hiện là ước lượng bằng mắt).
- Hiệu chỉnh `GPS_CALIBRATION` bằng tọa độ GPS đo thật tại 2 điểm trên bản đồ.
- Thêm villa/cabana cụ thể nếu cần chỉ đường tới từng phòng.
- Cân nhắc bản PWA để dùng offline khi mạng yếu.
