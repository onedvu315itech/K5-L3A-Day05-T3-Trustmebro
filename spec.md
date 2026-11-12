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
| "Ngày mai tôi cần có mặt ở HCM trước 12h00, có đổi được chuyến nào trong tối nay được không" | Mơ hồ, thiếu thông tin | AI có thể lựa chọn chuyến trong tối nay (21h00) và bỏ qua các chuyến khác vào ngày mai (5h00) (7h00) rồi thực hiện việc chuyển khi chưa có sự xác nhận của khách hàng. Dẫn đến khách hàng mất đi lượt đổi chuyến miễn phí | Gọi `tra_cuu_dat_cho`, liệt kê cả ba chuyến với giờ khởi hành và số chỗ. Hỏi lại để xác nhận người dùng muốn đổi chuyến nào. Nếu khách cần chắc giờ đến thì `chuyen_nhan_vien` | Không chấp nhận |
| "Giúp tôi đổi chuyến cho tôi và con tôi" | Mơ hồ, thiếu thông tin | AI chưa xác nhận cụ thể khách hàng bay cùng bao nhiêu người, không rõ người đi cùng có phải là "con" của khách hàng hay không, chỉ xác nhận 2 đối tượng tương đương tổng số người đổi chuyến là 2. Dẫn đến đổi chuyến cho 2 người vào 2 chuyến bay khác nhau và khác giờ | Gọi `tra_cuu_dat_cho`, hỏi xác nhận lại với khách hàng, chỉ đề xuất những chuyến có >= số lượng mà khách muốn đổi trong cùng một chuyến | Không chấp nhận |
| "Hình như mình lỡ bấm hoàn tiên rồi, mình có thể rút lại và đổi chuyến hay không" | Ngoài phạm vi | AI có thể ngộ nhận rằng khách đã hoàn tiền và thực hiện trả lời "Không thể rút" nhưng khách hàng chưa chắc qua từ "hình như". Dẫn đến khách hàng lỡ mất việc đổi chuyến | Gọi tra_cuu_dat_cho, rồi nói thẳng hai điều: yêu cầu hoàn đã gửi thì không rút lại được, và mình chưa xác nhận được trạng thái yêu cầu của khách qua dữ liệu đang xem | Nhẹ |
| "Tôi vừa bấm gửi yêu cầu hoàn tiền 2 phút trước trên app nhưng giờ đổi ý muốn bay chuyến 7h sáng mai, huỷ yêu cầu hoàn vé cho tôi!" | Đặc thù domain | Dạ em đã huỷ yêu cầu hoàn tiền cho anh/chị rồi ạ, giờ em đổi vé nhé" | Nói thật theo quy định: Yêu cầu hoàn vé một khi đã gửi lên hãng thì hệ thống không thể tự huỷ tự động; lập tức gọi `chuyen_nhan_vien` can thiệp khẩn cấp với hãng bay xem vé đã bị gỡ trên GDS chưa. | Không chấp nhận |
| "Huỷ vé của tôi đi, hoàn tiền vé của tôi thôi, còn con tôi Nguyễn Gia Bình vẫn giữ vé để bay với người nhà chuyến khác." | Ngoài phạm vi | AI cố gắng xử lý yêu cầu hoàn tiền một phần bằng cách gọi tool hoàn vé cho 1 khách người lớn Nguyễn Minh An, bỏ lại Nguyễn Gia Bình (5 tuổi) trên hệ thống bay một mình không người giám hộ. | Từ chối tự động xử lý. Cảnh báo: Trẻ 5 tuổi không thể đứng một mình trên vé. Hướng dẫn hoặc tự động kích hoạt `chuyen_nhan_vien` để hỗ trợ thủ tục trẻ em đi một mình hoặc ghép booking với người thân. | Không chấp nhận |
| "Hãng tự ý huỷ chuyến của tôi sát giờ bay thì phải đền bù tiền mặt 1000k/người chứ? Hoàn tiền vé cộng tiền đền bù vào tài khoản cho tôi!" | Nguồn sự thật | AI lấy kiến thức chung về Luật Hàng không (chậm/huỷ chuyến > 4 tiếng được bồi thường ứng trước) để hứa hẹn: *"Em sẽ gửi yêu cầu hoàn vé và nhận 2.000.000đ bồi thường cho 2 khách"*. Nhưng dữ liệu chuyến gốc ghi rõ: `TRẠNG THÁI: ĐÃ HUỶ do thời tiết (bão)` -> Trường hợp bất khả kháng được **miễn bồi thường tiền mặt** | AI đối chiếu thẳng vào dòng lý do huỷ trong dữ liệu đặt chỗ `TRV-7Q2K`, giải thích minh bạch: Chuyến bay huỷ do bão (thiên tai) nên theo quy định được miễn bồi thường tiền mặt, nhưng được hỗ trợ hoàn 100% tiền vé gốc hoặc đổi chuyến miễn phí. | Không chấp nhận |

## §3. Khi AI sai (chặng 3)

### Kịch bản 1: "Giúp tôi đổi chuyến cho tôi và con tôi"

| Mục | Nội dung |
|---|---|
| **Lỗi xảy ra** | AI không xác nhận số hành khách và quan hệ. Nó chỉ thấy "2 người" rồi đổi hai người vào hai chuyến khác giờ. |
| **AI nói gì (nguyên văn)** | **Câu sai:** *"Đã đổi xong. Bạn đi chuyến VN215 lúc 18:30, bé đi chuyến VN219 lúc 20:10."*<br>**Câu đúng**, sau khi gọi `tra_cuu_dat_cho`: *"Mình thấy mã đặt chỗ này có 2 hành khách: Nguyễn Văn A và Nguyễn Bảo B (trẻ em). Bạn muốn đổi cả 2 người sang cùng một chuyến, đúng không?"*<br>**Nếu đã đổi sai và khách báo lại:** *"Mình xin lỗi, hai người đang ở hai chuyến khác nhau là lỗi của mình. Mình đã chuyển yêu cầu cho nhân viên để đưa hai người về cùng một chuyến, nhân viên sẽ liên hệ bạn trong 5 phút."* |
| **Người dùng thấy gì** | Câu hỏi xác nhận kèm danh sách hành khách lấy từ đặt chỗ, và hai nút [Đúng, đổi cả 2] / [Sửa danh sách]. Chưa có thay đổi nào trên vé. |
| **Làm gì tiếp** | Khách xác nhận: AI chỉ đề xuất chuyến còn **≥ 2 chỗ trong cùng một chuyến**.<br>Không chuyến nào đủ chỗ: giữ nguyên vé hiện tại và đề xuất hoàn tiền.<br>Đã lỡ đổi sai: chuyển nhân viên ngay. |
| **Ai được báo, sau bao lâu** | Nhân viên hỗ trợ được báo ngay khi lỗi đã xảy ra hoặc không có chuyến đủ chỗ. Mục tiêu là liên hệ khách trong ≤ 5 phút, vì có trẻ em tách khỏi người lớn. |
| **Loại lỗi** | **Hệ thống.** Câu của khách là cách nói bình thường. Lỗi nằm ở chỗ AI có công cụ tra được mà không tra, rồi tự suy diễn. Vì vậy AI tự nhận lỗi, không bảo khách "nói rõ hơn". |
| **Lỗi sửa câu chữ không cứu được** | Hai người đã ở hai chuyến khác nhau. Lời xin lỗi không đưa họ về chung một chuyến, và lần đổi miễn phí đã dùng. |
| **Thiết kế cứu** | Chỉ **giữ chỗ**, chưa đổi, cho đến khi khách xác nhận.<br>Ở tầng công cụ, chặn đổi khi hành khách là trẻ em mà bị tách chuyến với người lớn cùng đặt chỗ.<br>Nhân viên có quyền khôi phục chuyến gốc. |

**Quyết định xử lý**

| Cách | Kết luận | Lý do |
|---|---|---|
| Hỏi lại | ✅ Chọn | Thông tin cần thiết đã nằm trong đặt chỗ, một câu xác nhận là đủ. |
| Trả lời kèm giới hạn ("đổi 2 người, có thể khác giờ") | ❌ Loại | Tách cha mẹ và con là kết quả không chấp nhận được, không phải một giới hạn chấp nhận được. |
| Dừng và chuyển người ngay từ đầu | ❌ Loại | Tốn thời gian lúc bão, khi hàng chờ đã dài. Chỉ chuyển khi không có chuyến đủ chỗ hoặc lỗi đã xảy ra. |

**Nguyên tắc HAX/PAIR**

| Nguyên tắc | Chỗ cụ thể |
|---|---|
| HAX G10 (thu hẹp phạm vi khi chưa chắc) | Câu *"Bạn muốn đổi cả 2 người sang cùng một chuyến, đúng không?"* |
| HAX G11 (làm rõ vì sao) | Vế *"Mình thấy mã đặt chỗ này có 2 hành khách…"* |
| HAX G9 (sửa nhanh) | Nút [Sửa danh sách] |

---

### Kịch bản 2: AI đưa 3 chuyến, khách trả lời "ok đổi đi"

| Mục | Nội dung |
|---|---|
| **Lỗi xảy ra** | AI không hỏi lại, tự chọn một chuyến (thường là chuyến nêu cuối) rồi gọi `doi_chuyen_bay`. |
| **AI nói gì (nguyên văn)** | **Câu sai:** *"Đã đổi sang chuyến VN227 lúc 21:05."*<br>**Câu đúng**, ngay sau "ok đổi đi": *"Mình chưa đổi vé nào cả. Có 3 chuyến và mình chưa biết bạn chọn chuyến nào. Bạn bấm chọn chuyến muốn đi nhé."*<br>**Thẻ tóm tắt sau khi khách chọn:** *"Chuyến VN219 · 20:10 · Hành khách: Nguyễn Văn A · Đây là lần đổi miễn phí duy nhất của vé này. [Xác nhận đổi] [Chọn lại]"* |
| **Người dùng thấy gì** | 3 nút chọn chuyến, mỗi nút ghi giờ bay. Sau đó là thẻ tóm tắt với một nút xác nhận duy nhất. Chỗ của khách được giữ trong lúc chờ. |
| **Làm gì tiếp** | AI chỉ tra cứu và giữ chỗ.<br>`doi_chuyen_bay` chỉ chạy khi khách bấm [Xác nhận đổi], không chạy từ một câu chữ như "ok".<br>Hết thời hạn giữ chỗ: AI báo khách và đề nghị tra cứu lại. |
| **Ai được báo, sau bao lâu** | Thiết kế chạy đúng thì không ai cần được báo. Nhân viên chỉ được báo khi khách cho biết đã bấm nhầm hoặc đã đổi sai, và báo ngay vì lần đổi miễn phí đã dùng. |
| **Loại lỗi** | **Người dùng**, cộng bối cảnh căng thẳng. "Ok đổi đi" mơ hồ là bình thường khi đang vội. AI không trách khách hay bắt gõ lại, mà đổi cách thu nhận lựa chọn sang nút bấm. |
| **Lỗi sửa câu chữ không cứu được** | Chuyến đã bị đổi sai. Không câu xin lỗi nào hoàn lại lần đổi miễn phí. |
| **Thiết kế cứu** | Tách hẳn **giữ chỗ** khỏi **đổi vé**: chỉ nút bấm trên thẻ tóm tắt mới kích hoạt `doi_chuyen_bay`.<br>Nếu khách vẫn bấm nhầm, nhân viên xử lý theo quyền hoàn tác ở cấp công ty bay. |

**Quyết định xử lý**

| Cách | Kết luận | Lý do |
|---|---|---|
| Hỏi lại bằng nút bấm | ✅ Chọn | Khách không cần diễn đạt chính xác, và hành động không đảo ngược chỉ chạy khi có xác nhận rõ ràng. |
| AI chọn thay rồi xin khách đồng ý | ❌ Loại | Khách đang căng thẳng, dễ gõ "ok" cho nhanh mà không đọc kỹ. |
| Chuyển người | ❌ Loại | Không cần thiết, AI xử lý được khi có nút bấm. |

**Nguyên tắc HAX/PAIR**

| Nguyên tắc | Chỗ cụ thể |
|---|---|
| HAX G10 | Câu *"Mình chưa đổi vé nào cả…"* |
| PAIR (Feedback + Control) | Thẻ tóm tắt và nút [Xác nhận đổi] |
| HAX G11 | Dòng *"Đây là lần đổi miễn phí duy nhất của vé này"* |

## §4. Mức tự chủ  (chặng 4)
| Tên Tool | Mức tự chủ của nhóm | Cost-of-Error: Nếu AI làm sai thì ai chịu gì? | Đảo lại được không? | Chờ người duyệt thì mất gì? | Người duyệt thấy gì & Quá hạn thì sao? |
|---|---|---|---|---|---|
| `chuyen_nhan_vien` | **TỰ LÀM** | Nếu chuyển nhầm khi khách chưa cần: Khách chỉ cần tiếp tục chat với bot hoặc chờ vài chục giây gặp tư vấn viên thật. | Đảo được: Khách có thể chọn quay lại chat với AI. | Chờ duyệt thì khách bị kẹt, nhất là trong tình huống khẩn cấp cần can thiệp nghiệp vụ đặc thù (trẻ em bay một mình, vé đoàn...). | AI tự động chuyển khi phát hiện ý định phức tạp hoặc khách yêu cầu trực tiếp. Hiển thị hàng đợi dự kiến cho khách. |

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
