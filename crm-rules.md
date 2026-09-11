# CRM Rules — iMind AI Marketing

> File này chứa rules quản lý CRM khách hàng.
> Phải đọc trước khi thêm/sửa/xóa thông tin trong `CRM/crm-imind-clients.md`.

---

## 1. FILE TRACKING

| Item | Value |
|------|-------|
| **File** | `CRM/crm-imind-clients.md` |
| **Format** | Markdown table |
| **Ngành hàng** | Y tế, F&B, E-com, Giáo dục, Thời trang, Tư vấn, Khác |
| **Source** | KOC, SEO, Referral, Ads, Khác |

---

## 2. PIPELINE STAGES

```
Lead → Tư vấn → Báo giá → Ký → Cọc đợt 1 → Đang triển khai → Trả đợt 2 → Hoàn thành trả đợt 2 → Tái ký
```

**Lưu ý quan trọng:**
- **Hoàn thành trả đợt 2** ≠ Hoàn thành toàn bộ dự án
- Một dự án có thể đã hoàn thành trả đợt 2 (thanh toán đủ) nhưng bộ phận Operation chưa triển khai xong
- **Tái ký**: Khách quay lại, ký hợp đồng mới (dịch vụ bổ sung, gia hạn, ...)

---

## 3. ID FORMAT

- `XXX-NNN` (3 chữ cái + 3 số)
- Ví dụ: THL-001 (Thành Lợi)
- Viết tắt từ tên khách hàng (3 chữ cái đầu)

---

## 4. COLUMNS ĐẦY ĐỦ

| # | Cột | Required | Mô tả |
|---|-----|----------|-------|
| 1 | **ID** | ✅ Auto | XXX-NNN |
| 2 | **Tên khách hàng** | ✅ | Tên doanh nghiệp |
| 3 | **Ngành** | ✅ | Chọn từ list |
| 4 | **Người liên hệ** | ✅ | Tên người đại diện |
| 5 | **Ngày tiếp nhận** | ✅ | Ngày tiếp nhận (dd/mm/yyyy) |
| 6 | **Nỗi đau** | ✅ | Vấn đề chính |
| 7 | **Giải pháp** | ✅ | Gói dịch vụ đề xuất |
| 8 | **Source** | ❌ | Nguồn lead (ưu tiên thấp) |
| 9 | **Giá trị hợp đồng** | ❌ | VNĐ |
| 10 | **Cọc đợt 1** | ❌ | VNĐ đã thanh toán |
| 11 | **Trả đợt 2** | ❌ | VNĐ còn lại |
| 12 | **Giai đoạn** | ✅ | Chọn từ pipeline |
| 13 | **Next action** | ✅ | Việc tiếp theo |
| 14 | **Deadline** | ❌ | Hạn chót |
| 15 | **Nội dung triển khai** | ❌ | Demo, chi tiết công việc |
| 16 | **Ghi chú** | ❌ | Notes |

---

## 5. RULES XỬ LÝ

### 5.1. Khách hàng mới — Thiếu thông tin

**Quy tắc:**
1. Khi nhận thông tin khách hàng mới, kiểm tra **6 cột bắt buộc**: ID, Tên khách hàng, Ngành, Người liên hệ, Ngày tiếp nhận, Nỗi đau
2. **Nếu thiếu** → Hỏi lại người cung cấp để bổ sung đầy đủ
3. **Nếu người cung cấp phản hồi "chưa có thông tin"** → Điền vào CRM là `"cần update"`
4. Được phép skip các cột không bắt buộc (8, 9, 10, 11, 14, 15, 16)

**Ví dụ:**
> Người dùng: "Khách mới: ABC Clinic, y tế"
> Mai: "Cần thêm thông tin: Người liên hệ, Ngày tiếp nhận (hôm nay?), Nỗi đau chính. Cung cấp giúp Mai nhé!"

---

### 5.2. Thanh toán đủ — Hoàn thành trả đợt 2

**Quy tắc:**
1. Nếu **Cọc đợt 1 ≥ Giá trị hợp đồng** → Trả đợt 2 = 0, ghi vào CRM
2. Giai đoạn tự động chuyển thành **"Hoàn thành trả đợt 2"**
3. **KHÔNG** đổi thành "Hoàn thành" (toàn bộ dự án) vì Operation có thể chưa triển khai xong
4. Ghi chú trong cột Ghi chú: "Đã thanh toán đủ. Operation chưa hoàn tất."

**Ví dụ:**
> Khách A: Giá trị HĐ = 50.000.000 VNĐ, Cọc đợt 1 = 50.000.000 VNĐ
> → Trả đợt 2 = 0, Giai đoạn = "Hoàn thành trả đợt 2"

---

### 5.3. Demo yêu cầu từ đội Sale

**Quy tắc:**
1. Nếu khách hàng đang ở giai đoạn **"Tư vấn"** và đội Sale báo cần đội Operation làm **demo**
2. Thêm nội dung demo vào cột **"Nội dung triển khai"**
3. Cập nhật **Next action**: "Operation chuẩn bị demo cho khách"
4. Ghi rõ deadline demo nếu có

**Ví dụ:**
> Đội Sale: "Cần Operation làm demo cho Thành Lợi Dental về tính năng tra cứu bảo hành"
> Mai: Cập nhật CRM cột "Nội dung triển khai": "Demo tính năng tra cứu bảo hành sản phẩm"

---

### 5.6. Cùng khách hàng — Nhiều dịch vụ

**Quy tắc:**
1. Nếu 1 khách hàng sử dụng **nhiều dịch vụ** khác nhau của iMind → **Mỗi dịch vụ = 1 hàng riêng**
2. Các hàng cùng khách hàng **giữ nguyên ID** (không tạo ID mới)
3. Cột **Giai đoạn** có thể khác nhau giữa các dịch vụ (ví dụ: Website đang triển khai, AI Agent mới Lead)
4. Cột **Giá trị hợp đồng**, **Cọc đợt 1**, **Trả đợt 2** ghi riêng cho từng dịch vụ
5. Cột **Ngày tiếp nhận** giữ nguyên (cùng ngày tiếp nhận khách hàng)

**Ví dụ:**
> Khách THL-001 dùng 2 dịch vụ:
> - Hàng 1: THL-001 | Website + SEO + tra cứu BH | Giai đoạn: Đang triển khai
> - Hàng 2: THL-001 | AI Agent hành chính | Giai đoạn: Lead

---

### 5.4. Thêm khách hàng mới

1. Đọc CRM file hiện tại
2. Check trùng lặp (tên khách hàng + ngành)
3. Nếu trùng → Thông báo, không thêm
4. Nếu mới → Generate ID, thêm row, confirm

---

### 5.5. Cập nhật thông tin khách hàng

1. Tìm khách hàng theo ID hoặc tên
2. Chỉ cập nhật các cột được yêu cầu
3. Không thay đổi ID
4. Confirm sau khi cập nhật

---

## 6. VÍ DỤ DỮ LIỆU

```
| THL-001 | Thành Lợi DentalLab | Y tế | — | Referral | Website sập | Website + SEO + tra cứu BH | — | — | — | Lead | Thu thập thông tin | — | — | FB page |
```

---

*File được tạo: 2026-09-10 | Cập nhật: 2026-09-10*
