# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> temperature ở các giá trị thấp làm câu trả lời ổn định hơn, còn giá trị cao tạo nhiều biến thể hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature trong khoảng 0,2 đến 0,3 Vì chatbot hỗ trợ khách hàng cần đảm bảo tính nhất quán, chính xác và độ tin cậy.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> GPT-4o có giá $10 / 1M output tokens, GPT-4o mini là $0.60 / 1M output tokens.
- Workload: 10.000 * 3 * 350 = 10.500.000 tokens = 10.5M tokens/ngày
- Chi phí ngày:
GPT-4o	$10/1M	$105
GPT-4o mini	$0.60/1M	$6.30
-> GPT-4o đắt khoảng 16,7 lần GPT-4o mini xét riêng output tokens.
- Trường hợp xứng đáng: xử lý các vấn đề phức tạp
---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với giáo viên tiểu học, model sẽ trả lời ngắn, dùng từ đơn giảngiản. Ví dụ: "Blockchain is like a digital notebook that everyone can see but nobody can tear pages out of. When something happens, like a transaction, it gets written into a "block." Then, this block is connected to the previous block, like links in a chain. Once the information is added, it is very difficult to change or delete. This helps people trust that the information is safe and has not been secretly changed. Think of it like a classroom notebook that everyone has a copy of, so if someone tries to change a page, the others can notice.”. 
Với chuyên gia tài chính, model có xu hướng trả lời dài và chuyên sâu hơn. Ví dụ: "Blockchain is a decentralized distributed ledger that records transactions in a sequence of cryptographically linked blocks. Each block contains transaction data, a timestamp, and typically a cryptographic hash of the previous block, creating an immutable chain of records. Transactions are validated through a consensus mechanism rather than a central authority, with mechanisms such as Proof of Work or Proof of Stake used by different blockchain networks. This architecture provides transparency, tamper resistance, and decentralized trust, making blockchain applicable to cryptocurrencies, smart contracts, and various financial applications."

-> system prompt định hướng persona, giọng điệu, độ sâu, từ vựng và cách trình bày của model, giúp cùng một model thích ứng với các đối tượng và mục đích khác nhau.
### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> - count_tokens: 150
- Ước lượng: 133
- Chênh nhau 12.8%.
- Tiếng Việt thường tốn nhiều token hơn tiếng Anh cùng độ dài vì cách tokenizer chia từ dựa trên các chuỗi ký tự phổ biến; tiếng Việt có nhiều từ được tạo bởi các âm tiết cách nhau bằng dấu cách và chứa dấu/Unicode, nên một “từ” có thể bị tách thành nhiều token.
---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi cần hiển thị phản hồi cho người dùng càng sớm càng tốt, đặc biệt với chatbot, trợ lý AI hoặc các câu trả lời dài, vì người dùng có thể đọc từng phần ngay khi model đang tạo nội dung thay vì phải chờ toàn bộ phản hồi hoàn tất. Ngược lại, non-streaming phù hợp hơn khi cần nhận toàn bộ kết quả rồi mới xử lý tiếp, chẳng hạn như tạo JSON có cấu trúc, kết quả phân tích, dữ liệu dùng cho backend hoặc các tác vụ không yêu cầu phản hồi ngay lập tức.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff có lợi thế là thời gian chờ sẽ tăng dần sau mỗi lần retry, ví dụ 1s → 2s → 4s → 8s, giúp giảm tải cho API và cho hệ thống có thời gian phục hồi. Nếu hàng nghìn client cùng retry với delay cố định 1 giây, chúng có thể gửi request lại gần như cùng lúc, tạo ra một “retry storm” khiến API càng quá tải và tiếp tục trả lỗi.
---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Tôi chọn persona trợ giảng cho khóa học AI, với system prompt: "Bạn là trợ giảng của khóa học AI thực hành về mô hình ngôn ngữ lớn (LLM) và cách gọi API. Mọi thuật ngữ như temperature, token, top_p, prompt đều hiểu theo nghĩa trong lĩnh vực LLM. Luôn trả lời bằng tiếng Việt, ngắn gọn trong 3–5 câu, kèm một ví dụ cụ thể khi cần. Nếu không chắc chắn, hãy nói rõ là bạn không chắc thay vì đoán." Lựa chọn quan trọng nhất là câu "hiểu theo nghĩa trong lĩnh vực LLM": với persona ban đầu chỉ ghi "trợ giảng thân thiện của khóa AI", khi hỏi "Temperature là gì?" model đã giải thích nhiệt độ vật lý (°C, °F), còn sau khi thêm câu này model trả lời đúng về tham số sampling. Yêu cầu "ngắn gọn trong 3–5 câu" giúp giữ câu trả lời dễ đọc trên terminal và kiểm soát chi phí.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là history chỉ giữ 3 lượt gần nhất, nên trợ lý quên thông tin quan trọng từ đầu cuộc hội thoại: khi thử 5 lượt, tôi nói "Tôi thích màu xanh lá" ở lượt 1, đến lượt 5 hỏi lại thì model trả lời "Xin lỗi, tôi không biết bạn thích màu gì". Cải thiện đề xuất là dùng bộ nhớ tóm tắt: khi history vượt 6 message, thay vì cắt bỏ, gọi call_openai_mini với prompt "Tóm tắt các thông tin quan trọng về người dùng trong đoạn hội thoại sau thành 1–2 câu", rồi ghép bản tóm tắt vào sau persona trong system prompt (ví dụ "Thông tin đã biết: người dùng thích màu xanh lá"). Cách này giữ được ý chính mà số token input vẫn bị giới hạn, và dùng mini để tóm tắt nên chi phí thấp. Ngoài ra, thống kê hiện chỉ đếm tin nhắn user và câu trả lời mà bỏ qua persona và history được gửi lại mỗi lượt, nên nên đếm token trên toàn bộ messages gửi đi để báo cáo chi phí đúng hơn.
---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
