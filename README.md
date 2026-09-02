# Resort Wayfinder – Demo

Web app chỉ đường ngắn nhất trong khu nghỉ dưỡng, có định vị GPS thời gian thực và chuyển đổi tiếng Việt/Anh.
Không cần cài đặt, không cần đăng nhập — mở bằng trình duyệt (kể cả qua quét QR).

## Tính năng

- **Bản đồ khởi đầu trống, không có sẵn đường nối nào** — bạn tự vẽ toàn bộ lối đi thật bằng công cụ quản trị (xem bên dưới). Trước khi vẽ, chọn 2 điểm bất kỳ sẽ báo "Không tìm được đường đi" — đó là bình thường, vì chưa có đường nào được vẽ.
- Khi đã vẽ, đường ngắn nhất được tính bằng Dijkstra và **luôn bám đúng theo hình dạng đã vẽ tay**, không cắt ngang qua nhà/hồ.
- **GPS thật, theo thời gian thực**: bấm "Bắt đầu định vị trực tiếp" — vị trí khách tự động cập nhật liên tục (`watchPosition`), điểm xuất phát và tuyến đường tự vẽ lại mỗi khi khách di chuyển.
- **GPS ảo**: nếu khách không ở resort (demo, thử nghiệm, hoặc GPS thật báo lỗi/nằm ngoài bản đồ), chuyển sang chế độ "GPS ảo" rồi chạm/click vào bất kỳ đâu trên bản đồ để tự đặt vị trí xuất phát.
- **Zoom bản đồ**: nút +/–/⤢ ở góc phải bản đồ. Bản đồ tự "vừa khung" theo kích thước màn hình khi mở trang hoặc đổi kích thước cửa sổ.
- **Song ngữ VI/EN kèm đổi cả ảnh bản đồ**: nút VI/EN góc trên phải đổi toàn bộ giao diện *và* đổi ảnh nền sang bản đồ tiếng Anh (`map_en.jpg`) / tiếng Việt (`map.jpg`).
- Giao diện responsive: điện thoại cuộn dọc (bảng điều khiển trên, bản đồ dưới); máy tính/tablet tự chuyển 2 cột (bảng điều khiển cố định bên trái, bản đồ chiếm phần còn lại, vừa chiều cao màn hình).
- Phong cách "luxury quiet + natural": xanh rừng đậm, be cát, nhấn vàng đồng, font Cormorant Garamond (tiêu đề) + Work Sans (nội dung).

## Vẽ đường đi thật — bắt buộc phải làm trước khi dùng

Bản đồ không có đường mặc định nào cả — mọi kết nối giữa 2 điểm chỉ tồn tại sau khi bạn tự vẽ. Công cụ vẽ theo kiểu **1 tuyến dài đi qua nhiều điểm, tự tách thành từng đoạn** — giống như bạn đang lần theo 1 con đường thật ngoài đời, không cần chọn "Từ/Đến" trước.

1. Kéo xuống cuối thanh bên, bấm **"⚙ Công cụ quản trị: vẽ đường đi thật"** để mở bảng công cụ.
2. Bấm **"✎ Bắt đầu vẽ tuyến"**.
3. **Chạm đúng vào 1 điểm (chấm tròn)** trên bản đồ để bắt đầu — đây là điểm neo đầu tiên.
4. Tiếp tục chạm dọc theo đúng hình dạng lối đi thật (mỗi chạm là 1 điểm gấp khúc, hiện chấm nhỏ màu nâu đất). Khi lối đi đi ngang qua 1 điểm khác, **chạm trúng đúng chấm tròn đó** — hệ thống tự nhận ra (hiện chấm lớn màu vàng đồng) và **tự cắt thành 1 đoạn** nối điểm neo trước đó với điểm này, rồi tiếp tục tuyến từ điểm này.
5. Cứ thế đi hết toàn bộ tuyến đường, chạm qua bao nhiêu điểm neo cũng được — mỗi lần "Hoàn tất" có thể tạo ra nhiều đoạn cùng lúc.
6. Lỡ chạm nhầm thì bấm **"↩ Hoàn tác điểm"** (xóa điểm chạm gần nhất, kể cả điểm neo). Bấm **"✕ Hủy vẽ"** để bỏ toàn bộ tuyến đang vẽ dở.
7. Bấm **"✓ Hoàn tất tuyến"** khi xong — các đoạn được lưu tạm vào trình duyệt (localStorage) và hiện ngay trên bản đồ.
8. Lặp lại bước 2-7 cho từng tuyến đường khác cho đến khi phủ hết khu nghỉ dưỡng.
9. Khi xong toàn bộ, bấm **"⇩ Xuất mã"** — một đoạn mã JavaScript hiện ra. **Sao chép**, mở `index.html`, tìm dòng:
   ```js
   const CUSTOM_PATHS = {};
   ```
   và **thay thế toàn bộ** bằng đoạn mã vừa sao chép, lưu file, Commit + Push lên GitHub.

**Độ chính xác:** mỗi lần chạm quy đổi đúng theo % vị trí trên ảnh bản đồ tại đúng thời điểm đó, không phụ thuộc mức zoom hay đã cuộn màn hình tới đâu — nên phóng to (nút +) trước khi vẽ khu vực nhỏ, chi tiết để chạm chuẩn hơn. Khi chạm gần 1 điểm có sẵn (trong bán kính ~3% bản đồ), hệ thống tự "dính" đúng vào tọa độ điểm đó thay vì lấy tọa độ chạm thô, nên không lo bị lệch.

**Xóa dữ liệu:**
- Xóa 1 đoạn cụ thể: chọn đúng cặp điểm ở mục "Xóa đường đã vẽ" → bấm "🗑 Xóa 1 đoạn (theo cặp điểm)".
- Xóa sạch toàn bộ (kể cả dữ liệu thử nghiệm cũ còn sót trong trình duyệt): bấm **"🗑 Xóa TOÀN BỘ đường đã vẽ"**.

⚠️ **Đường vẽ chỉ lưu trong trình duyệt của bạn** (localStorage) — khách khác sẽ không thấy cho đến khi bạn làm xong bước 9 (dán mã vào file và đưa lên GitHub). Vì dữ liệu này nằm trong trình duyệt chứ không phải trong code, **nếu thấy bản đồ hiện đường lạ mà bạn không nhớ đã vẽ, nhiều khả năng đó là dữ liệu thử nghiệm cũ còn sót lại** — bấm "Xóa TOÀN BỘ" để làm sạch rồi vẽ lại. Công cụ này hiện không có mật khẩu bảo vệ — nên ẩn/gỡ nút "Công cụ quản trị" trước khi đưa link chính thức cho khách, hoặc thêm xác thực nếu cần dùng lâu dài.

### Hiệu chỉnh tỉ lệ mét thật

Quãng đường hiển thị (mét) được tự tính từ chiều dài đường bạn vẽ, nhân với 1 hệ số duy nhất:

```js
const METERS_PER_UNIT = 0.9;
```

Nếu số mét hiển thị chưa khớp thực tế: đo khoảng cách thật (bước chân hoặc thước) giữa 2 điểm bất kỳ đã vẽ, so với số mét app đang hiển thị cho đúng 2 điểm đó, rồi điều chỉnh `METERS_PER_UNIT` theo tỉ lệ (ví dụ thực tế dài gấp đôi số hiển thị → nhân đôi hệ số này). Chỉ cần chỉnh 1 lần, áp dụng cho toàn bộ bản đồ.

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

## Thêm điểm mới

Thêm 1 dòng vào `NODES` trong `index.html`, với id, tên tiếng Việt (`vi`), tên tiếng Anh (`en`), tọa độ `x`,`y` (% theo ảnh bản đồ). Sau đó vào công cụ quản trị vẽ đường nối điểm mới này với các điểm lân cận như hướng dẫn ở trên.

## Chạy thử ở máy

```bash
python3 -m http.server 8000
# mở http://localhost:8000
```

## Đưa lên GitHub Pages

Đưa 4 file (`index.html`, `map.jpg`, `map_en.jpg`, `README.md`) vào repo → Commit → Publish → Settings → Pages → chọn branch main, `/ (root)` → Save.

## Việc cần làm tiếp

- Vẽ toàn bộ lối đi thật bằng công cụ quản trị (bản đồ hiện đang trống).
- Hiệu chỉnh `METERS_PER_UNIT` theo khoảng cách thật đo ngoài đời.
- Hiệu chỉnh `GPS_CALIBRATION` bằng tọa độ GPS đo thật tại 2 điểm trên bản đồ.
- Ẩn/khóa công cụ quản trị trước khi phát hành chính thức cho khách.
- Thêm villa/cabana cụ thể nếu cần chỉ đường tới từng phòng.
- Cân nhắc bản PWA để dùng offline khi mạng yếu.
