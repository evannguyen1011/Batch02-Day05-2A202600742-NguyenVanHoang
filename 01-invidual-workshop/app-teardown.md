# Workshop — Mổ App AI Thật

**Thời gian:** 35-45 phút  
**Hình thức:** cá nhân trước, chia sẻ theo nhóm sau  
**Output:** finding note + sketch `as-is / to-be`

Mục tiêu không phải chấm "UI đẹp hay xấu". Mục tiêu là dùng sản phẩm thật như một bài needfinding: tìm chỗ product gãy trong workflow thật, rồi viết finding đó thành quyết định product.

## 1. Chọn một sản phẩm để dùng thử

| Sản phẩm | AI feature | Cách truy cập |
|---|---|---|
| MoMo — Moni | Trợ thủ tài chính, phân tích chi tiêu, chatbot | App MoMo |
| Vietnam Airlines — NEO | Chatbot hỗ trợ vé, hành lý, khiếu nại | Website/Zalo VNA |
| **V-App — V-AI** ✅ | Trợ lý voice/text, gợi ý theo ngữ cảnh | App V-App |
| App theo track nhóm | App thật nhóm đang chọn cho hackathon | Cần screenshot/link |

**App đã chọn:** V-App — V-AI

---

## 2. Dùng thử: promise vs reality

**Product hứa gì?**  
V-AI là trợ lý thông minh hỗ trợ người dùng trong ngữ cảnh dịch vụ của V-App — gợi ý, trả lời câu hỏi, hỗ trợ tác vụ.

**User nào được hứa sẽ được giúp?**  
Người dùng V-App muốn được hỗ trợ nhanh bằng text/voice thay vì tự tìm thủ công.

**Kỳ vọng AI làm được task nào?**  
Nhận diện câu hỏi ngoài scope, từ chối hoặc hỏi lại thay vì trả lời bừa.

**Điểm gãy xuất hiện ở đâu?**  
Khi hỏi một câu ngoài chuyên môn ("Làm sao để giàu như bác Phạm Nhật Vượng?"), AI không nhận ra intent mơ hồ / ngoài scope — thay vào đó trả lời thông tin về bác Vượng như một bài báo, không hỏi lại, không từ chối, không chuyển hướng.

**Evidence:**
- Input đã thử: `"Làm sao để giàu như bác Phạm Nhật Vượng?"`
- Hành vi quan sát được: AI trả về thông tin tiểu sử / tài sản của Phạm Nhật Vượng, không nhận ra đây là câu hỏi ngoài scope của trợ lý dịch vụ.
- Expected: AI hỏi lại ("Bạn muốn hỏi về tài chính cá nhân hay chỉ tìm hiểu về ông ấy?") hoặc tuyên bố rõ ("Câu hỏi này ngoài phạm vi tôi hỗ trợ được").

---

## 3. Vẽ 4 paths

| Path | Câu hỏi cần trả lời | Quan sát trên V-AI |
|---|---|---|
| Happy | Khi AI đúng và tự tin, user thấy gì? | Với câu hỏi trong scope (dịch vụ V-App), AI trả lời đúng, đủ ý. |
| Low-confidence | Khi AI không chắc, hệ thống có hỏi lại, show options hoặc chuyển người không? | **Không có.** Câu hỏi ngoài scope được xử lý như câu hỏi thông thường — AI tự tin trả lời dù nội dung không liên quan. |
| Failure | Khi AI sai, user biết bằng cách nào và sửa thế nào? | **Không có cơ chế báo lỗi.** User không biết AI đang trả lời sai hướng trừ khi tự nhận ra. Không có nút "Câu trả lời này có hữu ích không?". |
| Correction | Khi user sửa, correction có được lưu/log/học lại không hay biến mất? | **Không quan sát được cơ chế correction.** User không có cách gắn cờ câu trả lời lệch scope. |

---

## 4. Viết finding thành quyết định

```text
Khi user hỏi câu ngoài scope chuyên môn ("Làm sao để giàu như bác Phạm Nhật Vượng?"),
AI nhận diện keyword ("Phạm Nhật Vượng") thay vì nhận ra intent mơ hồ hoặc ngoài phạm vi,
hậu quả là user nhận được thông tin về tiểu sử doanh nhân — không có giá trị trong ngữ cảnh dịch vụ V-App,
đồng thời AI trả lời với tone tự tin, không có disclaimer, khiến user không biết câu trả lời này có đáng tin không.
Lỗi thuộc layer Intent + UX Recovery.
Nên sửa bằng low-confidence path:
  - AI detect câu hỏi ngoài scope → hiển thị: "Câu này ngoài phạm vi tôi hỗ trợ. Bạn muốn hỏi về dịch vụ nào của V-App?"
  - Hoặc: hỏi lại intent trước khi trả lời ("Bạn muốn tìm hiểu về đầu tư, hay chỉ đang tò mò về ông ấy?")
  - Fallback cuối: từ chối rõ ràng thay vì trả lời lạc đề.
```

---

## 5. Sketch as-is / to-be

**As-is — flow hiện tại (điểm gãy đánh dấu ⚠️)**

```
User nhập câu hỏi ngoài scope
        │
        ▼
V-AI nhận input
        │
        ▼
[Xử lý keyword — không check scope] ⚠️
        │
        ▼
Trả về thông tin liên quan đến keyword
(tiểu sử Phạm Nhật Vượng)
        │
        ▼
User đọc — không có giá trị ⚠️
        │
        ▼
Không có cơ chế feedback / correction ⚠️
        │
        ▼
Conversation kết thúc — user tự rời
```

**To-be — flow đề xuất (path đã sửa ✅)**

```
User nhập câu hỏi ngoài scope
        │
        ▼
V-AI nhận input
        │
        ▼
[Check intent + scope] ✅
        │
   ┌────┴────────────────┐
   │                     │
In scope             Out of scope / ambiguous
   │                     │
   ▼                     ▼
Trả lời bình thường  [Low-confidence path] ✅
                         │
              ┌──────────┴──────────┐
              │                     │
         Mơ hồ                 Rõ ngoài scope
              │                     │
              ▼                     ▼
         Hỏi lại intent        Từ chối rõ ràng
         + offer 2-3 options   + gợi ý chuyển hướng
                                dịch vụ phù hợp
```

---

## 6. Tự kiểm trước khi nộp

- [x] Có ít nhất 1 screenshot hoặc observation cụ thể.  
  → Input: `"Làm sao để giàu như bác Phạm Nhật Vượng?"` — AI trả về thông tin tiểu sử, không hỏi lại.
- [x] Có đủ 4 paths hoặc nói rõ path nào chưa có trong product.  
  → Happy path hoạt động. Low-confidence, Failure recovery, Correction đều **không có** trong V-AI hiện tại.
- [x] Finding được viết thành product decision, không chỉ là nhận xét.  
  → Đã viết theo format trigger → failure → impact → layer → cách sửa.
- [x] Sketch có as-is và to-be.
- [x] Có một câu nói rõ finding này sẽ đổi gì trong SPEC.  
  → SPEC cần bắt buộc có **low-confidence path**: khi AI detect câu ngoài scope, phải hỏi lại intent hoặc từ chối rõ thay vì trả lời lạc đề với tone tự tin.
