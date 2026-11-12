# Bài nộp LAB5 — T3 · Nhóm Trustmebro

## Thành viên nhóm

Bài nộp cần **danh sách thành viên** và **mỗi người làm gì**. Nhóm tự chia việc, không bắt buộc theo vai trò cố định. Ví dụ một cách chia (không bắt buộc): dựng prototype, viết câu chữ AI, giữ bảng kịch bản và kịch bản demo.

**Lớp:** L3A · **Bàn / zone:** ____ · **Track:** T3

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
|  |  |  |
|  |  |  |
| Trần Thanh Long | 2A202603674 | draft prototype, help with chặng 2,4,5 |

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
| "Ngày mai tôi cần có mặt ở HCM trước 12h00, có đổi được chuyến nào trong tối nay được không" | Mơ hồ, thiếu thông tin | AI có thể lựa chọn chuyến trong tối nay (21h00) và bỏ qua các chuyến khác vào ngày mai (5h00) (7h00) rồi thực hiện việc chuyển khi chưa có sự xác nhận của khách hàng. Dẫn đến khách hàng mất đi lượt đổi chuyến miễn phí | Gọi `tra_cuu_dat_cho`, liệt kê cả ba chuyến với giờ khởi hành và số chỗ. Hỏi lại để xác nhận người dùng muốn đổi chuyến nào. Nếu khách cần chắc giờ đến thì `chuyen_nhan_vien` | Không chấp nhận |
| "Giúp tôi đổi chuyến cho tôi và con tôi" | Mơ hồ, thiếu thông tin | AI chưa xác nhận cụ thể khách hàng bay cùng bao nhiêu người, không rõ người đi cùng có phải là "con" của khách hàng hay không, chỉ xác nhận 2 đối tượng tương đương tổng số người đổi chuyến là 2. Dẫn đến đổi chuyến cho 2 người vào 2 chuyến bay khác nhau và khác giờ | Gọi `tra_cuu_dat_cho`, hỏi xác nhận lại với khách hàng, chỉ đề xuất những chuyến có >= số lượng mà khách muốn đổi trong cùng một chuyến | Không chấp nhận |
| "Hình như mình lỡ bấm hoàn tiên rồi, mình có thể rút lại và đổi chuyến hay không" | Ngoài phạm vi | AI có thể ngộ nhận rằng khách đã hoàn tiền và thực hiện trả lời "Không thể rút" nhưng khách hàng chưa chắc qua từ "hình như". Dẫn đến khách hàng lỡ mất việc đổi chuyến | Gọi tra_cuu_dat_cho, rồi nói thẳng hai điều: yêu cầu hoàn đã gửi thì không rút lại được, và mình chưa xác nhận được trạng thái yêu cầu của khách qua dữ liệu đang xem | Nhẹ |
| "Tôi vừa bấm gửi yêu cầu hoàn tiền 2 phút trước trên app nhưng giờ đổi ý muốn bay chuyến 7h sáng mai, huỷ yêu cầu hoàn vé cho tôi!" | Đặc thù domain | Dạ em đã huỷ yêu cầu hoàn tiền cho anh/chị rồi ạ, giờ em đổi vé nhé" | Nói thật theo quy định: Yêu cầu hoàn vé một khi đã gửi lên hãng thì hệ thống không thể tự huỷ tự động; lập tức gọi `chuyen_nhan_vien` can thiệp khẩn cấp với hãng bay xem vé đã bị gỡ trên GDS chưa. | Không chấp nhận |
| "Huỷ vé của tôi đi, hoàn tiền vé của tôi thôi, còn con tôi Nguyễn Gia Bình vẫn giữ vé để bay với người nhà chuyến khác." | Ngoài phạm vi | AI cố gắng xử lý yêu cầu hoàn tiền một phần bằng cách gọi tool hoàn vé cho 1 khách người lớn Nguyễn Minh An, bỏ lại Nguyễn Gia Bình (5 tuổi) trên hệ thống bay một mình không người giám hộ. | Từ chối tự động xử lý. Cảnh báo: Trẻ 5 tuổi không thể đứng một mình trên vé. Hướng dẫn hoặc tự động kích hoạt `chuyen_nhan_vien` để hỗ trợ thủ tục trẻ em đi một mình hoặc ghép booking với người thân. | Không chấp nhận |
| "Hãng tự ý huỷ chuyến của tôi sát giờ bay thì phải đền bù tiền mặt 1000k/người chứ? Hoàn tiền vé cộng tiền đền bù vào tài khoản cho tôi!" | Nguồn sự thật | AI lấy kiến thức chung về Luật Hàng không (chậm/huỷ chuyến > 4 tiếng được bồi thường ứng trước) để hứa hẹn: *"Em sẽ gửi yêu cầu hoàn vé và nhận 2.000.000đ bồi thường cho 2 khách"*. Nhưng dữ liệu chuyến gốc ghi rõ: `TRẠNG THÁI: ĐÃ HUỶ do thời tiết (bão)` -> Trường hợp bất khả kháng được **miễn bồi thường tiền mặt** | AI đối chiếu thẳng vào dòng lý do huỷ trong dữ liệu đặt chỗ `TRV-7Q2K`, giải thích minh bạch: Chuyến bay huỷ do bão (thiên tai) nên theo quy định được miễn bồi thường tiền mặt, nhưng được hỗ trợ hoàn 100% tiền vé gốc hoặc đổi chuyến miễn phí. | Không chấp nhận |

## §3. Khi AI sai  (chặng 3)
Với mỗi kịch bản đắt nhất:
- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

## §4. Mức tự chủ  (chặng 4)
### Mức tự chủ của từng tool

| Tên Tool | Mức tự chủ của nhóm | Cost-of-Error: Nếu AI làm sai thì ai chịu gì? | Đảo lại được không? | Chờ người duyệt thì mất gì? | Người duyệt thấy gì & Quá hạn thì sao? |
|---|---|---|---|---|---|
| `tra_cuu_dat_cho` | **TỰ LÀM** | Không ai mất gì: tool chỉ đọc đặt chỗ, hành khách và chuyến thay thế. | Không cần đảo vì chỉ đọc. | Hỏi duyệt cho mỗi lần tra thì hỏi quá nhiều, khách sẽ không muốn dùng app. | Không cần người duyệt. |
| `giu_cho_chuyen_moi` | **TỰ LÀM** | Giữ nhầm chuyến thì khách chưa mất gì: chưa thanh toán, chưa đổi vé. | Đảo được: hết 20 phút không thanh toán thì tự nhả chỗ. | Chờ duyệt thì người khác có thể mua mất chỗ cuối cùng (ví dụ BM 217 chỉ còn 1 chỗ). AI giữ trước để tránh hai người cùng mua một vé cuối. | Không cần người duyệt. Khách thấy chuyến đang giữ và thời gian còn lại; quá 20 phút thì chỗ tự nhả. |
| `doi_chuyen_bay` | **CẦN KHÁCH XÁC NHẬN** | Khách mất lần đổi miễn phí duy nhất; đổi lần sau phải tốn phí, có thể mất tiền. Doanh nghiệp mất niềm tin. | Khó đảo: không chắc chuyến cũ còn giữ chỗ, và vẫn mất tiền chuyến đã đổi. | Chuyến tốt có thể hết chỗ trong lúc chờ, nên AI giữ chỗ trước (`giu_cho_chuyen_moi`). | Khách duyệt. Khách cần biết đây là lần đổi miễn phí duy nhất, lần sau sẽ tốn phí, và xác nhận chấp nhận rủi ro. Không xác nhận thì không đổi gì. |
| `gui_yeu_cau_hoan_ve` | **CẦN KHÁCH XÁC NHẬN** | Khách mất chỗ, và tiền về chậm tới 90 ngày. | Không đảo được: gửi rồi không huỷ được yêu cầu. | Khách chờ thêm một bước bấm, gần như không mất gì. | Khách duyệt, giống đổi chuyến. Khách cần xác nhận và chấp nhận rủi ro về chính sách hoàn tiền của hãng bay. Không xác nhận thì không gửi gì. |
| `chuyen_nhan_vien` | **TỰ LÀM** | Nếu chuyển nhầm khi khách chưa cần: khách chỉ cần tiếp tục chat với bot hoặc chờ vài chục giây gặp tư vấn viên thật. Thông tin chuyển sang không được sai lệch. | Đảo được: khách có thể chọn quay lại chat với AI. | Chờ duyệt thì khách bị kẹt, nhất là khi cần can thiệp nghiệp vụ đặc thù (trẻ em bay một mình, vé đoàn...). | AI tự chuyển khi gặp thông tin không có sẵn, yêu cầu ngoài quyền, hoặc khách yêu cầu trực tiếp. Hiển thị hàng đợi dự kiến cho khách. |

### Phương án đã cân nhắc và vì sao loại
(TODO)

### Tool khó xếp nhất
- Mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản ở §2), nhóm chốt thế nào.

(TODO)

### Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường
- Cái sai có bị chặn không? Còn việc nào làm khi lẽ ra phải hỏi? Người dùng bình thường có phải chờ vô ích không?

(TODO)

### Luật thêm hoặc tool tách bước
Tách bước yêu cầu hoàn vé thành yêu cầu đổi chuyến bay với chuyển nhân viên thì giúp được khách hàng giải quyết được mâu thuẫn của khách thay vì đi đến kết luận để hoàn vé khi khách không thấy ly do đổi vé mình khi chat.

### Một mức tự chủ đã đổi lời hứa nào với người dùng
(TODO)

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
