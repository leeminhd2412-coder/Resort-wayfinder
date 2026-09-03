# Resort Wayfinder – Demo

Web app chỉ đường ngắn nhất trong khu nghỉ dưỡng, có định vị GPS thời gian thực, chuyển đổi tiếng Việt/Anh, và công cụ tự vẽ sơ đồ đường đi kiểu đồ thị (điểm + liên kết).
Không cần cài đặt, không cần đăng nhập — mở bằng trình duyệt (kể cả qua quét QR).

## Tính năng cho khách

- Chọn điểm xuất phát / điểm đến → tính đường ngắn nhất bằng Dijkstra, luôn bám đúng sơ đồ đã vẽ.
- **GPS thật, theo thời gian thực**: "Bắt đầu định vị trực tiếp" — vị trí và tuyến đường tự cập nhật liên tục khi khách di chuyển.
- **GPS ảo**: chạm vào bản đồ để tự đặt vị trí xuất phát (dùng khi không ở resort, hoặc GPS thật lỗi/ngoài vùng bản đồ).
- **Zoom**: nút +/–/⤢ góc phải bản đồ, tự "vừa khung" khi mở trang.
- **Song ngữ VI/EN**: đổi cả giao diện lẫn ảnh bản đồ nền (`map.jpg` / `map_en.jpg`).
- Khách **không hề thấy** công cụ vẽ đường đi — giao diện của khách chỉ có tìm đường + GPS, gọn gàng, không có nút/menu quản trị nào.
- Responsive: điện thoại cuộn dọc, máy tính/tablet 2 cột.
- Phong cách "luxury quiet + natural": xanh rừng đậm, be cát, nhấn vàng đồng, font Cormorant Garamond + Work Sans.

## Câu hỏi thường gặp: GPS thời gian thực có chính xác không?

**Có cập nhật thời gian thực** — dùng `watchPosition`, vị trí trên bản đồ tự động chạy theo khi khách di chuyển ngoài đời, không cần bấm lại. Nhưng **độ chính xác phụ thuộc 2 yếu tố**:

1. **Đã hiệu chỉnh `GPS_CALIBRATION` bằng tọa độ thật hay chưa** (xem mục bên dưới) — hiện tại vẫn là số giả, nên vị trí sẽ hiện sai chỗ cho đến khi bạn đo và thay số thật vào.
2. **Độ chính xác GPS của thiết bị** — ngoài trời trống trải thường lệch khoảng 3-15m, nhưng gần tòa nhà cao, cây rậm, hoặc trong nhà có thể lệch xa hơn (hiệu ứng che khuất tín hiệu vệ tinh) — đây là giới hạn phần cứng của điện thoại/máy tính, không phần mềm nào khắc phục hoàn toàn được.

Bản thân công thức quy đổi tọa độ đã được nâng cấp lên phép biến đổi "tỉ lệ + xoay" (similarity transform) suy ra từ 2 điểm mốc — chính xác hơn nhiều so với chỉ nội suy trục X/Y độc lập, đặc biệt khi bản đồ không thẳng theo hướng Bắc-Nam thật (gần như chắc chắn là vậy với bản đồ resort dạng phối cảnh). Muốn chính xác hơn nữa, có thể nâng cấp lên hiệu chỉnh bằng 3+ điểm mốc — báo lại nếu bạn muốn làm bước đó.

## Công cụ quản trị: vẽ sơ đồ đường đi

### Cách mở (khách không thấy được)

Công cụ quản trị **ẩn hoàn toàn** với người dùng thường. Chỉ hiện khi mở đúng đường link có thêm `?admin=1`, ví dụ:

```
https://<tên-bạn>.github.io/Resort-wayfinder/?admin=1
```

Mở link này sẽ thấy thêm nút "⚙ Công cụ quản trị: sơ đồ đường đi" ở cuối thanh bên — khách dùng link thường (không có `?admin=1`) sẽ không bao giờ thấy nút này hay bất kỳ dấu hiệu nào của việc vẽ đường.

⚠️ Đây **không phải bảo mật thật sự** — chỉ là ẩn UI. Ai biết thêm `?admin=1` vào link đều mở được. Không dán link có `?admin=1` ở nơi công khai; nếu cần chặt chẽ hơn (mật khẩu, đăng nhập), cần bổ sung thêm ở tầng máy chủ.

### Cách vẽ — kiểu đồ thị: điểm + liên kết

Công cụ có 4 chế độ (chọn ở hàng nút trong bảng quản trị):

**🖊 Vẽ chuỗi** — chế độ mặc định. Chạm vào bản đồ để đặt điểm đầu tiên. Mỗi lần chạm tiếp theo:
- Nếu chạm vào chỗ trống → tự tạo 1 **điểm nối mới** (chấm viền nét đứt) và **tự nối** vào điểm chạm trước đó.
- Nếu chạm trúng 1 điểm có sẵn (kể cả điểm có tên như "Quầy lễ tân") → nối thẳng vào đúng điểm đó.

Cứ thế đi dọc theo hình dạng lối đi thật ngoài đời. Muốn vẽ 1 đoạn tách biệt ở chỗ khác (không nối với đoạn vừa vẽ) → bấm **"✂ Bắt đầu đoạn mới"** trước khi chạm điểm tiếp theo.

**✥ Di chuyển điểm** — chạm giữ vào 1 điểm rồi kéo, thả ra để lưu vị trí mới. Áp dụng được cho **mọi loại điểm**, kể cả các chấm có STT (1-13) sẵn trên bản đồ (Quầy lễ tân, Lighthouse Bar...) — không chỉ điểm nối tự do. Các liên kết nối vào điểm đó tự động vẽ lại theo vị trí mới.

**🔗 Nối 2 điểm** — chạm điểm thứ nhất (sáng lên màu nâu đất), rồi chạm điểm thứ hai → tạo 1 liên kết thẳng nối 2 điểm đó. Dùng khi cần thêm đường tắt, nối 2 nhánh đang tách rời, hoặc tạo vòng lặp.

**🗑 Xóa** — chạm 1 điểm nối tự do (viền nét đứt) để xóa điểm đó + toàn bộ liên kết dính vào nó. Chạm gần 1 đoạn đường để xóa riêng đoạn đó (điểm có tên/điểm nối vẫn còn, chỉ mất 1 liên kết).

Mọi thao tác đều có thể **"↩ Hoàn tác"** (undo nhiều bước liên tiếp).

### Kiểu hiển thị điểm (áp dụng chung cho tất cả)

Trong panel quản trị có mục **"Kiểu hiển thị điểm"**:

- **Kích thước**: kéo thanh trượt để tăng/giảm cỡ TẤT CẢ chấm tròn cùng lúc (14–40px).
- **Màu viền**: chọn màu áp dụng cho viền của mọi chấm ở trạng thái bình thường (trạng thái đang chọn/đang trên tuyến đường vẫn giữ màu vàng đồng/xanh rừng riêng để dễ phân biệt, không đổi theo màu này).
- **Đứng yên / Nhấp nháy**: bật hiệu ứng nhấp nháy nhẹ (phóng to thu nhỏ) cho tất cả chấm, hoặc để đứng yên như mặc định.
- **Đường thẳng / Đường cong mượt**: bật "Đường cong mượt" để tuyến đường tự làm mượt qua khúc cua (dùng nội suy Catmull-Rom) thay vì nối thẳng cứng qua từng điểm — càng nhiều điểm nối tự do dọc khúc cua, đường cong càng mượt. Không cần công cụ vẽ cong tay riêng: chỉ cần vẽ chuỗi bình thường với vài điểm ở đoạn cong rồi bật tùy chọn này.

Mọi lựa chọn ở mục này cũng nằm trong đoạn mã "Xuất mã" (biến `MARKER_STYLE`) — dán vào file để áp dụng cho mọi khách.

### Độ chính xác khi vẽ

Mỗi lần chạm quy đổi đúng theo % vị trí thật trên ảnh bản đồ tại đúng thời điểm đó — không phụ thuộc mức zoom hay đã cuộn tới đâu. Nên phóng to (nút +) trước khi vẽ khu vực nhỏ, chi tiết. Khi chạm gần 1 điểm có sẵn (bán kính ~2.8% bản đồ), hệ thống tự "dính" đúng vào điểm đó thay vì tạo điểm mới sát bên.

### Lưu & phát hành cho khách

Mọi thay đổi tự lưu tạm vào trình duyệt (localStorage) ngay khi thao tác — an toàn khi lỡ tắt tab. Nhưng **chỉ trình duyệt của bạn thấy được**. Để mọi khách đều thấy:

1. Vẽ xong toàn bộ sơ đồ và chỉnh kiểu hiển thị ưng ý, bấm **"⇩ Xuất mã"** — 1 đoạn mã hiện ra trong ô văn bản (gồm 4 phần: `JUNCTIONS`, `NODE_OVERRIDES`, `GRAPH_EDGES`, `MARKER_STYLE`).
2. Sao chép toàn bộ đoạn mã đó.
3. Mở `index.html`, tìm 4 dòng:
   ```js
   const JUNCTIONS = {};
   const NODE_OVERRIDES = {};
   const GRAPH_EDGES = [];
   const MARKER_STYLE = { size: 24, borderColor: "#1f3b2f", blink: false, curved: true };
   ```
   **Thay thế toàn bộ** bằng đoạn mã vừa sao chép.
4. Lưu file, Commit + Push lên GitHub.

Nếu thấy sơ đồ hiện đường lạ không nhớ đã vẽ, nhiều khả năng là dữ liệu thử nghiệm cũ còn sót trong trình duyệt — bấm **"🗑 Xóa TOÀN BỘ sơ đồ"** để làm sạch rồi vẽ lại.

### Hiệu chỉnh tỉ lệ mét thật

Quãng đường hiển thị (mét) tự tính từ chiều dài các liên kết đã vẽ, nhân với 1 hệ số duy nhất:

```js
const METERS_PER_UNIT = 0.9;
```

Đo khoảng cách thật giữa 2 điểm bất kỳ đã nối, so với số mét app đang hiển thị cho đúng 2 điểm đó, rồi điều chỉnh hệ số này theo tỉ lệ (đo 1 lần, áp dụng cho toàn bộ bản đồ).

## ⚠️ Hiệu chỉnh GPS (bắt buộc trước khi dùng thật)

Trong `index.html`, tìm khối:

```js
const GPS_CALIBRATION = {
  anchor1: { lat: 15.97000, lng: 108.25000, nodeId: "entrance" },
  anchor2: { lat: 15.96850, lng: 108.24870, nodeId: "beach" },
};
```

Đây là **số giả**, cần thay bằng tọa độ thật: đứng đúng tại điểm "Lối vào" ngoài đời, xem tọa độ GPS hiện tại trên Google Maps, ghi lại `lat, lng`; làm tương tự cho "Beach House" (hoặc điểm khác càng xa điểm đầu càng tốt, để phép quy đổi chính xác hơn). Thay 2 cặp số vào `anchor1`/`anchor2`.

## Thêm điểm có tên mới

Thêm 1 dòng vào `NODES` trong `index.html` (id, tên VI, tên EN, tọa độ x/y theo %). Sau đó dùng chế độ "🔗 Nối 2 điểm" hoặc "🖊 Vẽ chuỗi" trong công cụ quản trị để nối điểm mới vào sơ đồ.

## Chạy thử ở máy

```bash
python3 -m http.server 8000
# mở http://localhost:8000/?admin=1 để vào chế độ quản trị
```

## Đưa lên GitHub Pages

Đưa 4 file (`index.html`, `map.jpg`, `map_en.jpg`, `README.md`) vào repo → Commit → Publish → Settings → Pages → chọn branch main, `/ (root)` → Save.

## Việc cần làm tiếp

- Vẽ toàn bộ sơ đồ đường đi thật bằng công cụ quản trị (mở link kèm `?admin=1`).
- Hiệu chỉnh `METERS_PER_UNIT` và `GPS_CALIBRATION` theo số đo thật.
- Không chia sẻ link có `?admin=1` cho khách; cân nhắc thêm xác thực nếu nhiều người cùng chỉnh sửa lâu dài.
- Thêm villa/cabana cụ thể nếu cần chỉ đường tới từng phòng.
- Cân nhắc bản PWA để dùng offline khi mạng yếu.
