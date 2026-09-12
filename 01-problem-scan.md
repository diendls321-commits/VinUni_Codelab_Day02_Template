# 01 — Problem Scan (Vin Smart Future)

## 🏛️ Bối cảnh

Tôi đóng vai **AI Product Engineer tại Vin Smart Future**, phối hợp với Khối Vận Hành của **Xanh SM (GSM)** để tìm cơ hội tối ưu hóa bằng AI. Sau khi quét qua các mảng kinh doanh của Vingroup, tôi chọn tập trung vào bài toán xử lý sự cố sạc pin thực địa của tài xế Xanh SM — nơi có áp lực thời gian thực rõ ràng và rủi ro vận hành cụ thể.

---

## 🔍 Phase 1 — SCAN: Danh sách bài toán

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Tốn thời gian | Điều phối viên xử lý thủ công các phản hồi khẩn cấp từ tài xế về sự cố hết pin/sạc pin giữa đường (mất 15-20 phút/lượt: tra cứu GPS, tìm trạm sạc trống, soạn tin hướng dẫn). |
| 2 | **Xanh SM** | Lặp lại | So khớp và phân bổ lại cuốc xe khi khách hàng yêu cầu đổi điểm đến giữa chừng. |
| 3 | **VinFast** | Lặp lại | So khớp hóa đơn sạc điện và đối chiếu số liệu trạm sạc đối tác hằng tuần. |
| 4 | **Vinhomes** | AI-upgrade | Phân loại và điều hướng tự động các phản ánh của cư dân trên App Vinhomes Resident (hiện phản hồi rập khuôn, mất 12 tiếng/lượt). |
| 5 | **Vinmec** | Pain từ người khác | Bác sĩ mất 20-30 phút/bệnh nhân để soạn tóm tắt hồ sơ xuất viện, gây quá tải. |
| 6 | **Xanh SM** | Tốn thời gian | Tóm tắt lý do khách hàng hủy chuyến từ ghi âm cuộc gọi và ghi chú tài xế để tìm pattern lỗi hệ thống. |

---

## 🃏 Phase 2 — Quick Problem Cards (Top 3)

### Quick Problem Card #1 — Xanh SM: Xử lý sự cố sạc pin thực địa ⭐ (Bài toán được chọn)

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                        │
│                                                               │
│ Bài toán: Tài xế Xanh SM báo hết pin/sắp hết pin giữa đường  │
│ cần điều phối cứu hộ hoặc chỉ dẫn trạm sạc gần nhất.         │
│ Công ty thành viên: [x] Xanh SM (GSM)                        │
│                                                               │
│ Ai đang đau (Actor)? Tài xế (chờ đợi, lo lắng), Điều phối    │
│ viên (quá tải giờ cao điểm)                                  │
│                                                               │
│ Workflow thủ công hiện tại (5 bước):                          │
│  1. Tài xế gọi báo sự cố ──> 2. Tra GPS xe ──>                │
│  3. Tra trạm sạc trống ──> 4. Soạn & gửi tin chỉ dẫn ──>      │
│  5. Gọi cứu hộ nếu cần                                        │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ 10 phút/lượt)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4 (tự động lấy  │
│ vị trí, tra trạm trống, soạn draft tin nhắn)                  │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                         │
│ Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút          │
│                                                               │
│ Quick Architecture: [x] LLM Feature                           │
└─────────────────────────────────────────────────────────────┘
```

### Quick Problem Card #2 — Vinhomes: Phân loại phản ánh cư dân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                        │
│                                                               │
│ Bài toán: Phản ánh của cư dân (mất nước, hỏng đèn, ồn ào)     │
│ gửi qua App Resident cần được phân loại và chuyển đúng ban    │
│ quản lý tòa nhà.                                              │
│ Công ty thành viên: [x] Vinhomes                              │
│                                                               │
│ Ai đang đau? Cư dân (chờ phản hồi lâu), Ban quản lý (xử lý    │
│ thủ công, dễ chuyển nhầm bộ phận)                             │
│                                                               │
│ Workflow thủ công hiện tại: 1. Cư dân gửi phản ánh ──>         │
│ 2. Nhân viên CSKH đọc và phân loại thủ công ──>                │
│ 3. Chuyển đến đúng ban quản lý ──> 4. Ban quản lý xử lý        │
│                                                               │
│ Bước nào tốn nhất? Bước 2 (⏱ ~12 tiếng phản hồi trung bình)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (phân loại tự     │
│ động + gợi ý mức độ ưu tiên)                                  │
│                                                               │
│ Đo thành công bằng gì? Giảm thời gian phân loại từ 12 tiếng   │
│ ──> dưới 30 phút, độ chính xác phân loại ≥ 90%                 │
│                                                               │
│ Quick Architecture: [x] Rule + LLM Feature                    │
└─────────────────────────────────────────────────────────────┘
```

### Quick Problem Card #3 — Xanh SM: Phân tích lý do hủy chuyến

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                        │
│                                                               │
│ Bài toán: Cần tổng hợp lý do khách/tài xế hủy chuyến từ ghi   │
│ âm cuộc gọi và ghi chú để tìm pattern lỗi hệ thống.            │
│ Công ty thành viên: [x] Xanh SM (GSM)                         │
│                                                               │
│ Ai đang đau? Team Vận hành / Product (thiếu dữ liệu để ra     │
│ quyết định cải tiến)                                          │
│                                                               │
│ Workflow thủ công hiện tại: 1. Thu thập ghi âm & ghi chú ──>   │
│ 2. Nhân viên nghe và tóm tắt thủ công ──>                      │
│ 3. Tổng hợp báo cáo tuần                                      │
│                                                               │
│ Bước nào tốn nhất? Bước 2 (⏱ vài giờ/tuần, dữ liệu lớn)        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (speech-to-text  │
│ + tóm tắt + phân loại lý do)                                  │
│                                                               │
│ Đo thành công bằng gì? Giảm thời gian tổng hợp báo cáo 80%,   │
│ phát hiện đúng ≥ 85% các lý do hủy chuyến phổ biến             │
│                                                               │
│ Quick Architecture: [x] LLM Feature                           │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Quyết định chọn đề tài

Nhóm/cá nhân chọn **Card #1 — Xanh SM: Xử lý sự cố sạc pin thực địa** để Deep-Dive, vì:
- Đây là bài toán **real-time**, ảnh hưởng trực tiếp đến an toàn tài xế và hiệu suất vận hành.
- Có ranh giới an toàn (Operational Boundary) rõ ràng, dễ kiểm chứng bằng adversarial testing (pin < 5%, yêu cầu [DRAFT_ONLY]).
- So với Card #2 (rủi ro pháp lý liên quan phí quản lý/tranh chấp) và Card #3 (tác vụ back-office, không real-time), Card #1 vừa cụ thể vừa có tác động vận hành tức thời hơn.
