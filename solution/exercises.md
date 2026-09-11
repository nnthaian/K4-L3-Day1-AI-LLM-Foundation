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
> Ở temperature = 0.0, câu trả lời thường ổn định và có tính dự đoán cao. Khi tăng dần, cách diễn đạt và lựa chọn sự thật có xu hướng đa dạng và sáng tạo hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Mình sẽ đặt temperature khoảng 0.2 đến 0.3 cho chatbot hỗ trợ khách hàng. Mức này giúp câu trả lời tương đối nhất quán, chính xác và ít lan man, đồng thời vẫn đủ linh hoạt để diễn đạt tự nhiên thay vì lặp lại máy móc.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Workload là 10,000 × 3 × 350 = 10.5 triệu token đầu ra/ngày. Với bảng giá đã cho trong file template.py, GPT-4o tốn khoảng 105/ngày, trong khi GPT-4o-mini tốn khoảng 6.30/ngày. Điều đó đồng nghĩa với việc GPT-4o đắt khoảng 16.7 lần. GPT-4o xứng đáng với chi phí cao hơn khi xử lý các yêu cầu phức tạp, cần chất lượng và khả năng suy luận tốt. Còn GPT-4o-mini phù hợp với các tác vụ đơn giản và số lượng lớn như FAQ, phân loại yêu cầu hoặc trả lời thông tin cơ bản.


---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> System prompt định hướng rõ hành vi và phong cách của model dù câu hỏi giống hệt nhau. Với vai trò giáo viên tiểu học, phản hồi thường ngắn hơn, từ vựng đơn giản, dùng ví dụ gần gũi như “quyển sổ chung” hoặc “chuỗi hộp”. Trong khi đó, vai trò chuyên gia tài chính sẽ dài và chuyên sâu hơn, dùng các thuật ngữ chuyên ngành. Vì vậy, system prompt không nhất thiết thay đổi kiến thức nền của model, nhưng thay đổi cách model lựa chọn nội dung, mức độ chi tiết, từ vựng, ví dụ và giọng điệu để phù hợp với vai trò được chỉ định.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Chênh nhau khoảng 20%. Tiếng Việt thường tốn nhiều token hơn tiếng Anh cùng độ dài vì cách tokenizer phân tách tiếng Việt, đặc biệt với từ có dấu và các chuỗi ký tự, có thể tạo ra nhiều token hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi model tạo nội dung dài hoặc mất nhiều thời gian xử lý, vì người dùng có thể nhìn thấy kết quả xuất hiện từng phần thay vì phải chờ toàn bộ phản hồi hoàn tất. Non-streaming phù hợp với các phản hồi ngắn, dữ liệu có cấu trúc hoặc khi ứng dụng cần nhận toàn bộ kết quả như một khối duy nhất trước khi xử lý tiếp.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp API giảm quá tải bằng cách tăng dần thời gian chờ trước khi thử lại. Nếu hàng nghìn client đều chờ đúng 1 giây rồi retry, chúng sẽ gửi request cùng lúc và làm server càng quá tải. Exponential backoff giúp các request được giãn ra, để server có thời gian phục hồi.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona: Trợ lý học tập lập trình cho sinh viên
> System prompt: Bạn là một trợ lý học tập chuyên hỗ trợ sinh viên học lập trình và AI. Hãy luôn trả lời bằng tiếng Việt, trừ khi người dùng yêu cầu ngôn ngữ khác. Giải thích kiến thức rõ ràng, dễ hiểu và phù hợp với trình độ của người học. Với những vấn đề phức tạp, hãy chia thành từng bước nhỏ và đưa ví dụ thực tế hoặc code ngắn khi cần. Trả lời ngắn gọn, tập trung vào ý chính và không lan man. Nếu không chắc chắn về thông tin, hãy nói rõ thay vì tự đưa ra thông tin sai. Khi người dùng hỏi bài tập, hãy ưu tiên hướng dẫn cách làm và giải thích lý do thay vì chỉ đưa đáp án.
> Giải thích: “Trả lời bằng tiếng Việt” giúp thống nhất ngôn ngữ, “ngắn gọn, dễ hiểu” giúp người học dễ tiếp thu và “chia thành từng bước” giúp giải thích các vấn đề phức tạp rõ ràng hơn.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là chưa có bộ nhớ dài hạn nên không thể nhớ thông tin của người dùng giữa các cuộc trò chuyện.
> Đề xuất cải thiện: Xây dựng long-term memory để lưu những thông tin được người dùng đánh dấu là quan trọng, chẳng hạn như profile và mục tiêu học tập. Sau mỗi cuộc trò chuyện, hệ thống có thể trích xuất thông tin quan trọng, lưu vào database và đưa thông tin phù hợp vào context của các cuộc trò chuyện sau. Đồng thời, cần giới hạn dữ liệu được lưu và cho phép người dùng xem hoặc xóa thông tin đã lưu.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
