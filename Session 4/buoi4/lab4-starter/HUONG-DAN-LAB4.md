# Lab 4 – Từ Portfolio CV V3 đến V4 responsive

FIT4014 – Buổi 4. Mục tiêu: trang của bạn dùng được ở màn hẹp, trung bình và rộng; bạn giải thích được vì sao chọn từng breakpoint và có minh chứng kiểm thử.

## Trước khi bắt đầu

1. Giải nén, mở cả thư mục bằng VS Code. Mở `index.html` bằng trình duyệt (hoặc Live Server).
2. Sao lưu bản gốc: chép thư mục thành `lab4-v3-goc`, hoặc `git commit -m "V3 gốc"`.
3. Bạn đã có V3 của mình chạy được: làm các bước dưới đây trên mã của bạn, bộ mã này chỉ để đối chiếu. Chưa có V3 chạy được: dùng bộ mã này và thay nội dung minh họa bằng nội dung của bạn (không cần số điện thoại, địa chỉ nhà).

**Ba quy tắc trong suốt buổi**

- Mỗi lần sửa một thay đổi → lưu → tải lại trang → nhìn khác biệt.
- Không dùng `display: none` để giấu nội dung chính cho hết lỗi.
- Bị kẹt quá 3 phút: tìm phần tử đang tràn (lệnh bên dưới) rồi giơ thẻ vàng.

**DevTools cần dùng**

- Mở DevTools: `F12`. Bật chế độ thiết bị: `Ctrl + Shift + M`, chọn *Responsive*, gõ độ rộng vào ô kích thước.
- Chụp ảnh cả trang: menu ⋮ của Device Toolbar → *Capture full size screenshot*.
- Kiểm tra có cuộn ngang không (gõ vào tab Console):

```js
document.documentElement.scrollWidth > innerWidth   // false là đạt
```

- Tìm phần tử đang vượt mép phải màn hình:

```js
[...document.querySelectorAll("body *")].filter(e => e.getBoundingClientRect().right > innerWidth)
```

---

## Tiết 1 – Nhìn lỗi của V3

1. Mở trang ở 1440 px, rồi đổi về 320 px. Trang còn đọc và dùng được không?
2. Chụp ảnh ở 320 px, lưu thành `evidence/before-320.png`.
3. Ghi một lỗi vào `evidence/phieu-loi.md` theo câu: *"Tôi mong… nhưng hiện tại…"*.
4. Tìm, **chưa sửa**:
   - Trong `index.html`: trang đã có thẻ `<meta name="viewport" …>` chưa?
   - Trong `css/style.css`: những dòng nào đặt chiều rộng bằng số px cố định? Ghi lại tên bộ chọn.

## Tiết 2 – Chọn breakpoint theo nội dung

1. Kéo chậm độ rộng từ 320 px lên 1440 px. Quan sát phần **Dự án** và **menu**: ở độ rộng nào hai card đặt cạnh nhau vẫn thoải mái? Ở độ rộng nào ba card vừa?
2. Ghi B1 và B2 dự kiến vào `evidence/ma-tran-kiem-thu.md`, mỗi điểm một câu lý do: *"B1 = … px vì card/menu bắt đầu chật ở…"*.
3. Tự kiểm: nếu card đã chật ở 700 px mà bạn chọn 768 px, khoảng 700–767 px sẽ ra sao?

> Không chọn breakpoint chỉ vì "đó là kích thước tablet". 768 px và 1024 px chỉ là ví dụ.

## Tiết 3 – Làm màn hẹp dùng được (mobile-first)

1. Chụp `evidence/before-1440.png`.
2. Thêm thẻ viewport vào `<head>` của `index.html`.
3. Đặt `box-sizing: border-box` cho mọi phần tử.
4. Đổi `.container` từ chiều rộng cố định sang chiều rộng linh hoạt (`width: 100%` kết hợp `max-width`).
5. **Tách CSS.** Đọc lần lượt từng khối trong `style.css` và hỏi: *quy tắc này chỉ hợp với màn rộng hay dùng được cho mọi màn?*

   | Dấu hiệu quy tắc chỉ hợp với màn rộng | Ví dụ |
   |---|---|
   | Chiều rộng px lớn trên khối hoặc ảnh | `width: 640px`, `width: 300px` |
   | Chia nhiều cột | `grid-template-columns: repeat(3, …)` |
   | Xếp các khối lớn thành hàng ngang | `display: flex` cho phần giới thiệu, liên hệ |
   | Cấm xuống dòng | `white-space: nowrap` |
   | Khoảng cách, cỡ chữ quá lớn cho điện thoại | `gap: 64px`, `font-size: 56px` |

   Phần còn lại là **CSS cơ sở một cột**. Tạm chuyển các quy tắc màn rộng vào cuối tệp, trong một chú thích `/* DÀNH CHO MEDIA QUERY */`, để dùng ở Tiết 4.
6. Kiểm tra ở 320 px: không cuộn ngang, đủ tên, giới thiệu, dự án, liên hệ và menu. Chụp `evidence/after-320.png`.
7. Đổi máy với bạn bên cạnh, ghi một nhận xét cụ thể vào cuối `evidence/phieu-loi.md`.

## Tiết 4 – Mở rộng và kiểm thử điểm vỡ

1. Viết `@media (min-width: …px) { … }` cho B1, đưa các quy tắc hai cột vào. Làm tiếp B2. Media query đặt **sau** CSS cơ sở.
2. **Menu:** bấm `Tab` từ đầu trang. Bạn có thấy liên kết nào đang được chọn không? Nếu không, tìm quy tắc đang xóa viền focus và thay bằng `:focus-visible` có viền rõ.
3. **Ảnh:** ở 320 px còn ảnh nào vượt khung không?
4. **Card dự án 3:** ở 320 px, đường dẫn dài có vượt mép card không? Gợi ý tìm hiểu: `overflow-wrap`. Nếu card trong grid vẫn không co lại, tìm hiểu `min-width: 0` hoặc `minmax(0, 1fr)`.
5. Thử ngay trước, tại và sau mỗi breakpoint (ví dụ B1 − 1, B1, B1 + 1) và một lần xoay ngang. Ghi vào ma trận.
6. Chụp ảnh bản V4 ở 320, 768, 1440 px: `evidence/v4-320.png`, `evidence/v4-768.png`, `evidence/v4-1440.png`.

## Tiết 5 – Chứng minh và nộp A2.1

1. Hoàn thiện ma trận: zoom 200%, xoay ngang, tiêu đề dài.
2. Hoàn thiện ít nhất một lỗi thật theo Expected – Actual – Fix – Retest; ảnh trước và sau chụp **cùng độ rộng**.
3. Tự rà TC1–TC2 (mục cuối tệp này).
4. Đóng gói theo hướng dẫn trên LMS. Giải nén thử gói vừa tạo, mở lại trang và các ảnh minh chứng trước khi nộp.

---

## Checklist tự kiểm trước khi nộp

**Responsive (TC3)**
- [ ] Có thẻ viewport; `.container` không còn chiều rộng px cố định.
- [ ] CSS cơ sở là một cột; các quy tắc nhiều cột nằm trong `@media (min-width: …)`.
- [ ] Có ít nhất hai breakpoint, mỗi cái có câu lý do theo nội dung.
- [ ] Ở 320 px: không cuộn ngang, không mất nội dung chính.
- [ ] Menu dùng được bằng phím Tab và thấy rõ vị trí focus.
- [ ] Ảnh không vượt khung; card chịu được tiêu đề và đường dẫn dài.

**Kiểm thử và minh chứng (TC4)**
- [ ] `evidence/ma-tran-kiem-thu.md` có ít nhất 320/768/1440 px và các điểm sát breakpoint.
- [ ] `evidence/phieu-loi.md` có ít nhất một lỗi đủ Expected – Actual – Fix – Retest.
- [ ] Ảnh trước/sau có tên ghi rõ độ rộng.

**Phần đã làm ở Buổi 2–3 (TC1–TC2)**
- [ ] Wireframe/yêu cầu có trong gói nộp.
- [ ] HTML dùng đúng thẻ ngữ nghĩa: `header`, `nav`, `main`, `section`, `footer`, `article`.
- [ ] Mọi ảnh có `alt` mô tả nội dung.

**Quy cách (TC5)**
- [ ] Tên tệp, cấu trúc thư mục, hạn nộp đúng hướng dẫn LMS.

## Không thuộc Lab 4

Menu hamburger, hiệu ứng chuyển động, form mới, container queries. Những phần này học ở Buổi 5 hoặc là nội dung mở rộng.
