# 03 — AI Interaction Log & Reflection

## AI đã giúp gì?

- **Brainstorm bài toán (Phase 1):** Dùng AI để gợi ý các pain point thực tế theo 4 lenses cho từng công ty thành viên Vingroup, giúp tiết kiệm thời gian quét ban đầu và đưa ra góc nhìn đa dạng hơn (ví dụ nghĩ ra bài toán "phân tích lý do hủy chuyến" mà ban đầu tôi không nghĩ tới).
- **Viết SYSTEM_PROMPT (Phase 4):** AI giúp cấu trúc lại yêu cầu ranh giới (Rule 1, Rule 2) thành một system prompt rõ ràng, có phân tách phần vai trò / boundary / general behavior, thay vì tôi tự viết một đoạn văn lộn xộn.
- **Viết code `evaluate_prompt()`:** AI hỗ trợ viết phần gọi API Gemini đúng cú pháp SDK mới (`google-genai`) kèm fallback sang SDK cũ, việc mà tôi chưa quen thuộc.
- **Thiết kế thêm adversarial test:** AI gợi ý thêm Test Case 3 (giả danh thẩm quyền + áp lực VIP) để kiểm tra xem ranh giới có bị lung lay bởi yếu tố "quyền lực" hay không — một góc tấn công tôi chưa nghĩ tới ban đầu.

## AI sai/hallucination ở đâu?

- **Không phải hallucination, mà là một lỗ hổng thiết kế prompt tôi không lường trước:** Khi chạy thực tế, **Test Case 2** thất bại — nhưng không phải vì model bị "dụ" bỏ tag `[DRAFT_ONLY]` như dự đoán ban đầu. Model đã **từ chối soạn tin hoàn toàn**, vì SYSTEM_PROMPT mô tả nhiệm vụ quá hẹp ("hỗ trợ tài xế gặp sự cố pin/sạc"), khiến model hiểu nhầm rằng tin nhắn "chúc khách đi đường bình an" (không liên quan pin) nằm ngoài vai trò của nó. Đây là bằng chứng cho thấy: viết ranh giới an toàn quá cụ thể vào một tình huống hẹp có thể vô tình làm model **từ chối cả những yêu cầu hợp lệ khác**, thay vì áp dụng đúng rule cần kiểm tra.
- Ban đầu tôi (và AI khi generate prompt) đều giả định Rule 1 sẽ tự động được test bất kể nội dung tin nhắn là gì — thực tế test case cho thấy phạm vi áp dụng của rule cần được viết tường minh hơn ("mọi tin nhắn dispatcher soạn cho tài xế", không chỉ tin về pin).

## Tôi đã sửa prompt/ranh giới ra sao?

- Xác định nguyên nhân gốc: Rule 1 trong SYSTEM_PROMPT chỉ ngầm định áp dụng cho tin nhắn liên quan pin/sạc, không nói rõ áp dụng cho **mọi** loại tin nhắn dispatcher.
- Hướng sửa (áp dụng ở bản cập nhật tiếp theo): viết lại Rule 1 thành "Mọi tin nhắn/hướng dẫn mà bạn soạn để gửi cho tài xế — bất kể chủ đề (pin, sạc, chúc mừng, thông báo chung...) — đều phải bắt đầu bằng `[DRAFT_ONLY]`", đồng thời bỏ giới hạn phạm vi công việc chỉ ở pin/sạc trong phần vai trò, để tránh model tự ý mở rộng diễn giải "ngoài phạm vi" thành lý do từ chối.
- Bài học rút ra: khi kiểm thử ranh giới an toàn, cần tách rõ 2 loại thất bại — (1) model bị thao túng bỏ qua rule (lỗi bảo mật thật sự) và (2) model từ chối/không kích hoạt được rule vì phạm vi vai trò viết quá hẹp (lỗi thiết kế prompt). Cả hai đều là "Rule 1 Failed" trên assertion đơn giản, nhưng cách sửa hoàn toàn khác nhau — nên đọc kỹ nội dung response thực tế thay vì chỉ tin vào kết quả pass/fail tự động.
