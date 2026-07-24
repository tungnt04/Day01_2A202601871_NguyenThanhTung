# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi tăng temperature từ 0.0 lên 1.5, câu trả lời trở nên đa dạng và sáng tạo hơn. Ở mức 0.0, phản hồi ổn định, ngắn gọn và gần như giống nhau giữa các lần chạy; còn ở mức 1.5, mô hình sử dụng từ ngữ phong phú hơn, có thể bổ sung nhiều chi tiết hoặc cách diễn đạt khác nhau. Temperature càng cao thì tính ngẫu nhiên của câu trả lời càng lớn.


### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature khoảng 0.2–0.3 cho chatbot hỗ trợ khách hàng. Mức này giúp câu trả lời ổn định, nhất quán và hạn chế việc mô hình tự suy diễn hoặc sáng tạo quá mức.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Với khoảng 10.000 người dùng, mỗi người gọi API 3 lần mỗi ngày, GPT-4o sẽ có chi phí cao hơn GPT-4o-mini khoảng 5–10 lần (tùy theo bảng giá tại thời điểm sử dụng). GPT-4o phù hợp cho các tác vụ cần chất lượng cao như phân tích tài liệu, lập trình hoặc suy luận phức tạp. GPT-4o-mini phù hợp cho chatbot hỏi đáp, chăm sóc khách hàng hoặc các tác vụ đơn giản để giảm chi phí.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Tôi sẽ chọn temperature khoảng 0.2–0.3 cho chatbot hỗ trợ khách hàng. Mức này giúp mô hình trả lời ổn định, nhất quán và hạn chế việc tự suy diễn thông tin. Đối với chatbot hỗ trợ, độ chính xác quan trọng hơn tính sáng tạo.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với khoảng 30.000 lượt gọi API mỗi ngày (10.000 người dùng × 3 lần), GPT-4o sẽ có chi phí cao hơn GPT-4o-mini nhiều lần, thường khoảng 5–10 lần tùy bảng giá. GPT-4o phù hợp với các bài toán cần suy luận phức tạp, phân tích tài liệu hoặc lập trình. GPT-4o-mini phù hợp với chatbot chăm sóc khách hàng, hỏi đáp thông thường hoặc các tác vụ có lưu lượng lớn để tối ưu chi phí.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming đặc biệt quan trọng đối với chatbot hoặc trợ lý AI vì người dùng nhìn thấy câu trả lời xuất hiện ngay lập tức, giúp giảm cảm giác phải chờ đợi và cải thiện trải nghiệm sử dụng. Ngược lại, non-streaming phù hợp khi phản hồi ngắn hoặc khi hệ thống cần xử lý toàn bộ kết quả trước khi hiển thị, chẳng hạn như tạo báo cáo hoặc xuất dữ liệu.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm tải cho máy chủ khi API đang quá tải bằng cách tăng dần thời gian chờ sau mỗi lần thử lại. So với delay cố định, cách này tránh việc nhiều client cùng gửi lại yêu cầu vào cùng một thời điểm. Nếu hàng nghìn client đều retry sau đúng 1 giây, máy chủ sẽ tiếp tục bị quá tải và có thể xảy ra hiện tượng "retry storm", làm hệ thống khó phục hồi.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Tôi yêu cầu "trả lời bằng tiếng Việt" để phù hợp với người học trong lớp. Cụm từ "ngắn gọn, chính xác" giúp mô hình tập trung vào nội dung chính và tránh trả lời lan man. Việc yêu cầu "đưa ví dụ minh họa" giúp người học dễ hiểu và dễ áp dụng hơn.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất của trợ lý hiện tại là chỉ lưu tối đa 3 lượt hội thoại nên dễ quên các thông tin đã trao đổi từ trước. Một cải tiến phù hợp là bổ sung bộ nhớ dài hạn bằng cách lưu lịch sử hội thoại vào cơ sở dữ liệu hoặc sử dụng vector database để tìm lại các nội dung liên quan khi người dùng tiếp tục cuộc trò chuyện.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
