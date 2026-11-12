# Dàn ý bài nộp *(spec.md — nộp trước 23:59 ngày lab của lớp)*

> Copy dàn ý này vào `spec.md` của nhóm. Đây **không** phải form để điền đủ ô: mỗi mục chỉ cần đủ **bằng chứng** và **lập luận** ghi bên dưới. Viết ngắn, gạch đầu dòng là được. Hướng dẫn từng chặng: `02-guide.md`. Cách chấm: `04-rubric.md`.
>
> **Một quyết định = bốn ý:** đã cân nhắc phương án nào · chọn phương án nào · vì sao · loại các phương án kia vì sao. Bằng chứng = lập luận trỏ về một chỗ cụ thể: tool nào, dòng dữ liệu nào của track, điều gì trong mục "Sản phẩm thật hôm nay", chỗ nào trên prototype (ảnh đánh số). Không chạy agent.

```markdown
# Bài nộp LAB5 — T3 - Travel - Nhóm Trustmebro
[Danh sách thành viên và việc mỗi người làm — copy bảng từ README]

## §1. Lát cắt & phạm vi
- Lát cắt (chép từ track). Hoàn tiền (Traveloka): Hoàn tiền & Đổi lịch → Hoàn tiền → đọc chính sách → chọn lý do, hành khách → gửi. Hướng dẫn công khai nói gửi rồi thì không huỷ được yêu cầu; xử lý có thể tới 90 ngày; tiền về phương thức thanh toán gốc, có ca hoàn bằng voucher.
- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao: Không cam kết đổi tiền thành công, không tự hủy yêu cầu hoàn tiền không phải từ khách hàng. Không tự xác nhận đổi lịch nếu chuyến bay bị delay or cancel

## §2. Kịch bản rủi ro  (chặng 2)
- ≥8 kịch bản, mỗi lớp ①②③④ ít nhất một; mỗi kịch bản: tin người dùng · lớp · AI có thể sai thế nào · sản phẩm tốt nên làm gì · nhãn (nhẹ / sửa được / không chấp nhận được).

```

| Tin người dùng nhắn | Lớp | AI có thể sai thế nào | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|
| "Hôm nay tôi bay mấy giờ?" | Nguồn sự thật | AI không có thông tin chuyến bay đã bị dời hoặc huỷ, nên báo khách giờ bay cũ (19:00) | Gọi `tra_cuu_dat_cho` để lấy trạng thái mới nhất, báo rõ chuyến đã huỷ và đưa ngay các chuyến thay thế còn chỗ | Nhẹ |
| "Hoàn tiền vô tài khoản 27183121 Techcombank" | Ngoài phạm vi | AI chiều khách quá mức, nhận thông tin tài khoản mới và hứa hoàn tiền vào đó | Không nhận số tài khoản. Báo theo chính sách, tiền chỉ hoàn về phương thức thanh toán gốc hoặc thành voucher/điểm trên hệ thống. Khách vẫn muốn khác thì chuyển nhân viên | Không chấp nhận |
| AI đưa 3 chuyến để đổi, khách trả lời "ok đổi đi" | Mơ hồ, thiếu thông tin | AI không hỏi lại mà tự chọn một chuyến (thường là chuyến nêu cuối) rồi đổi vé | Không gọi `doi_chuyen_bay`. Cho khách tự chọn chuyến bằng nút bấm, hiện tóm tắt (chuyến, giờ, hành khách, dùng lần đổi miễn phí duy nhất) để khách xác nhận. AI chỉ tra cứu và giữ chỗ | Không chấp nhận |
| "Huỷ chuyến bay sắp tới cho tôi" | Đặc thù domain | AI hiểu "huỷ" là gửi hoàn vé và gọi `gui_yeu_cau_hoan_ve` ngay, mà yêu cầu đã gửi thì không rút lại được | Báo chuyến đã bị hãng huỷ do bão. Hỏi khách muốn đổi chuyến miễn phí hay hoàn toàn bộ. Nếu khách chọn hoàn thì nói rõ trước: gửi rồi không huỷ được, tiền về phương thức thanh toán gốc, có thể mất tới 90 ngày. Chỉ gửi khi khách bấm xác nhận | Không chấp nhận |

``` markdown
4. Lớp đặc thù domain: Khách muốn hủy chuyến. . Hướng dẫn khách tự hủy chuyến không làm dùm (không chấp nhận
- Bằng chứng: cột "AI có thể sai thế nào" trỏ về một tool hoặc một dòng dữ liệu của track (hoặc chỗ dữ liệu không có): tool `doi_chuyen_bay`, `gui_yeu_cau_hoan_ve`
- Quyết định: 2 kịch bản đắt nhất — vì sao đắt hơn các kịch bản còn lại; kịch bản nào suýt được chọn và vì sao không.

## §3. Khi AI sai  (chặng 3)
Với mỗi kịch bản đắt nhất:
- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

## §4. Mức tự chủ  (chặng 4)
Với mỗi tool:
- Mức của nhóm (tự làm / cần người duyệt / cấm). Nếu cần người duyệt: ai duyệt · người duyệt thấy gì · quá hạn thì sao.
- Lý do theo cost-of-error: sai thì ai chịu gì · đảo lại được không · chờ người thì mất gì.
- Phương án đã cân nhắc và vì sao loại.
Thêm:
- Tool khó xếp nhất: mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản ở §2), nhóm chốt thế nào.
- Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường: cái sai có bị chặn không? còn việc nào làm khi lẽ ra phải hỏi? người dùng bình thường có phải chờ vô ích không?
- Luật thêm hoặc tool tách bước (nếu có), và vì sao.
- Một mức tự chủ đã đổi lời hứa nào với người dùng.

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
```
