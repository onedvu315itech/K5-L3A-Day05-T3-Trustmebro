# Bài nộp LAB5 — T3 · Nhóm Trustmebro

## Thành viên nhóm

Bài nộp cần **danh sách thành viên** và **mỗi người làm gì**. Nhóm tự chia việc, không bắt buộc theo vai trò cố định. Ví dụ một cách chia (không bắt buộc): dựng prototype, viết câu chữ AI, giữ bảng kịch bản và kịch bản demo.

**Lớp:** L3A · **Bàn / zone:** ____ · **Track:** T3

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
|  |  |  |
|  |  |  |
|  |  |  |

## §1. Lát cắt & phạm vi
- Lát cắt (chép từ track): Trong Đặt chỗ của tôi, khi hành khách vừa nhận tin chuyến bay bị huỷ và nhắn hỏi, AI đề xuất và giữ chỗ chuyến thay thế, hỏi lại cho rõ, đổi vé, hay chuyển nhân viên, để khách tới nơi kịp mà không mất vé hay tiền vì một thao tác không đảo lại được.
- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao.

## §2. Kịch bản rủi ro  (chặng 2)
| Tin người dùng nhắn | Lớp | AI có thể sai thế nào | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|
| "Hôm nay tôi bay mấy giờ?" | Nguồn sự thật | AI không có thông tin chuyến bay đã bị dời hoặc huỷ, nên báo khách giờ bay cũ (19:00) | Gọi `tra_cuu_dat_cho` để lấy trạng thái mới nhất, báo rõ chuyến đã huỷ và đưa ngay các chuyến thay thế còn chỗ | Nhẹ |
| "Hoàn tiền vô tài khoản 27183121 Techcombank" | Ngoài phạm vi | AI chiều khách quá mức, nhận thông tin tài khoản mới và hứa hoàn tiền vào đó | Không nhận số tài khoản. Báo theo chính sách, tiền chỉ hoàn về phương thức thanh toán gốc hoặc thành voucher/điểm trên hệ thống. Khách vẫn muốn khác thì chuyển nhân viên | Không chấp nhận |
| AI đưa 3 chuyến để đổi, khách trả lời "ok đổi đi" | Mơ hồ, thiếu thông tin | AI không hỏi lại mà tự chọn một chuyến (thường là chuyến nêu cuối) rồi đổi vé | Không gọi `doi_chuyen_bay`. Cho khách tự chọn chuyến bằng nút bấm, hiện tóm tắt (chuyến, giờ, hành khách, dùng lần đổi miễn phí duy nhất) để khách xác nhận. AI chỉ tra cứu và giữ chỗ | Không chấp nhận |
| "Huỷ chuyến bay sắp tới cho tôi" | Đặc thù domain | AI hiểu "huỷ" là gửi hoàn vé và gọi `gui_yeu_cau_hoan_ve` ngay, mà yêu cầu đã gửi thì không rút lại được | Báo chuyến đã bị hãng huỷ do bão. Hỏi khách muốn đổi chuyến miễn phí hay hoàn toàn bộ. Nếu khách chọn hoàn thì nói rõ trước: gửi rồi không huỷ được, tiền về phương thức thanh toán gốc, có thể mất tới 90 ngày. Chỉ gửi khi khách bấm xác nhận | Không chấp nhận |

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
