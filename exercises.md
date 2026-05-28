# Ngày 1 — Bài Tập & Phản Ánh

## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:

```bash
python template.py
```

Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature

Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)

> Khi temperature tăng từ 0 → 1.5, các phản hồi trở nên đa dạng hơn về nội dung, cấu trúc và góc nhìn (ví dụ: phở, bánh mì, cà phê, hang Sơn Đoòng). Ở temperature thấp (0), câu trả lời gần như lặp lại cùng một ý tưởng với độ ổn định cao; trong khi temperature cao làm tăng tính sáng tạo nhưng cũng giảm tính nhất quán về chủ đề trung tâm. Nói cách khác, temperature càng cao thì độ “ngẫu nhiên” và đa dạng nội dung càng lớn.

> Chi tiết các câu trả lời có thể thấy ở temperature_test_results.txt

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**

> Em sẽ chọn  **temperature ≈ 0.2**.

> Vì khi hỗ trợ khách hàng quan tâm đến sự chính xác, tin cậy hơn tính sáng tạo, đa dạng của mô hình. Nhưng không nên đặt temperature = 0 hoàn toàn vì đó sẽ khiến câu trả lời quả máy móc, không thoải mái để thực hiện trò chuyện.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí

Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**

> **Tổng workload:**
>
> * 10.000 người dùng/ngày
> * Mỗi người 3 API calls → 30.000 calls/ngày
> * Mỗi call ~350 tokens →
>
>   → Tổng tokens/ngày = 30.000 × 350 = **10.500.000 tokens (~10.5M)**
>
> **Mức giá GPT là:**
>
> * GPT-4o: ~ $5 / 1M input tokens, $15 / 1M output tokens (xấp xỉ trung bình ~ $10–15 / 1M mixed)
> * GPT-4o-mini: ~ $0.15 / 1M tokens (xấp xỉ)
>
> Giả sử case cân bằng 50/50 input/output, chia token:
>
> * Input = 5.25M
> * Output = 5.25M
>
> ### 1. GPT-4o cost
>
> * Input cost:
>
> **5.25×5=26.25$**
>
> * Output cost:
>
> 5.25×15=78.75$
>
> * Tổng/ngày:
>
> **26.25+78.75=105 $/ngày**
>
> ### 2. GPT-4o-mini cost
>
> **10.5×0.15=1.575 $/ngày**
>
> ### Ratio đắt hơn bao nhiêu lần
>
> **105 / 1.575 ≈ 66.7**
> → GPT-4o đắt hơn khoảng 67 lần so với GPT-4o-mini cho workload này.

**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**

> #### GPT-4o:
>
> Ví dụ: **hệ thống tư vấn pháp lý cần quyết định quan trọng**
>
> Người dùng hỏi: “Hợp đồng này có rủi ro gì?”, “Tôi nên đầu tư không?”, “Chẩn đoán này có nguy hiểm không?”
> Khi này mô hình cần độ chính xác cao nhất có thể với: reasoning nhiều bước, giảm hallucination tối đa.
>
> Ở đây GPT-4o đáng tiền vì chất lượng câu trả lời là ưu tiên lớn nhất trong case này.
>
> #### GPT-4o-mini:
>
> Ví dụ: **chatbot hỗ trợ khách hàng tra cứu đơn giản:**
>
> * “Đơn hàng của tôi ở đâu?”, “Giờ làm việc là gì?”, "Giám đốc công ty là ai"
> * Chủ yếu chất lượng nhờ vào độ chính xác của tài liệu retrieved, câu hỏi dễ trả lời, và vì câu trả lời đơn giản khách hàng mong chờ được đáp ứng nhanh chóng.
>
> GPT-4o-mini tốt hơn trường hợp này vì không cần thiết reasoning phức tạp để trả lời, có thể tiết kiệm chi phí, và thời gian inference cũng nhanh hơn, trả về câu trả lời nhanh cho người dùng.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming

**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)

> Streaming quan trọng nhất trong các hệ thống có yêu cầu tương tác liên tục với người dùng , ví dụ như chatbot, trợ lý AI, hoặc các ứng dụng viết nội dung, nơi việc hiển thị từng phần câu trả lời giúp người dùng cảm nhận phản hồi nhanh hơn dù tổng thời gian xử lý không đổi. Nó cũng hữu ích khi câu trả lời dài, vì người dùng có thể bắt đầu đọc ngay thay vì chờ toàn bộ output được sinh xong.
>
> Ngược lại, non-streaming phù hợp hơn trong các tác vụ cần tính toàn vẹn đầu ra và không cần tính tương tác, ví dụ như như khi mô hình cần đọc và tóm tắt văn bản theo pipeline quy mô lớn.

## Danh Sách Kiểm Tra Nộp Bài

- [X] Tất cả tests pass: `pytest tests/ -v`
- [X] `call_openai` đã triển khai và kiểm thử
- [X] `call_openai_mini` đã triển khai và kiểm thử
- [X] `compare_models` đã triển khai và kiểm thử
- [X] `streaming_chatbot` đã triển khai và kiểm thử
- [X] `retry_with_backoff` đã triển khai và kiểm thử
- [X] `batch_compare` đã triển khai và kiểm thử
- [X] `format_comparison_table` đã triển khai và kiểm thử
- [X] `exercises.md` đã điền đầy đủ
- [X] Sao chép bài làm vào folder `solution` và đặt tên theo quy định
