# CRM Rules — iMind AI Marketing

> File này chứa rules quản lý CRM khách hàng.
> Phải đọc trước khi thêm/sửa/xóa thông tin trong `CRM/crm-imind-clients.md` và `CRM/crm-sale-schedule.md`.

---

## 1. CẤU TRÚC CRM

### 1.1. File chính: `CRM/crm-imind-clients.md`
4 section chính:
| # | Section | Nội dung |
|---|---------|----------|
| 1 | **Hợp đồng** | ID, Tên KH, Ngày tiếp nhận, Ngày BĐ/KT HĐ, Chiết khấu, Giá trước/sau CK, Giá VAT |
| 2 | **Sale** | Giai đoạn Sale, Số tiền đợt tiếp theo, Thời hạn đợt tiếp theo, Ghi chú |
| 3 | **Order** | Số lượng + Đơn vị (tối đa 4 slot) |
| 4 | **Operation** | Ngày bắt đầu/kết thúc, Tiến độ (tối đa 4 slot), Trung %, Tài % |

### 1.2. Sub-file: `CRM/crm-sale-schedule.md`
- Chứa bảng 12 cột thanh toán (đợt 1 → đợt 12).
- CRM chính chỉ hiển thị **đợt tiếp theo chưa thanh toán**, không hiển thị toàn bộ 12 cột.
- Khi 1 đợt đã thanh toán → CRM chính auto-advance sang đợt tiếp theo.

### 1.3. Layout
- **Main row**: Dòng tổng cho 1 khách hàng (tên KH, tổng giá trị).
- **Subrow (↳SVC1, ↳SVC2...)**: Chi tiết từng dịch vụ, cùng ID với main.
- Mỗi dịch vụ = 1 subrow riêng trong tất cả 4 section.

---

## 2. PIPELINE STAGES

```
Lead → Tư vấn/Demo → Báo giá → Thanh toán đợt 1 → Thanh toán đợt 2 → Thanh toán đợt 3... → Hoàn thành
```

**Lưu ý:**
- **Hoàn thành** = Thanh toán đủ + Operation triển khai xong
- **Hoàn thành trả đợt 2** ≠ Hoàn thành toàn bộ dự án (Operation có thể chưa xong)
- **Tái ký**: Khách quay lại, ký HĐ mới (dịch vụ bổ sung/gia hạn)
- CRM tự động advance giai đoạn Sale khi thanh toán đợt tiếp theo

---

## 3. ID FORMAT

### 3.1. Main ID
- `XXX-NNN`: 3 chữ cái viết tắt tên KH + 3 số (THL-001, KKS-001...)

### 3.2. Sub-service ID

**Round 1 (Lead):** `XXX-NNN-LEAD1`, `XXX-NNN-LEAD2`, ...
- Khi Sale tiếp nhận khách mới, chưa biết rõ dịch vụ → dùng LEAD1, LEAD2 để đánh số thứ tự.
- Mỗi dịch vụ khác nhau = 1 LEAD riêng.

**Round 2 (Sau chốt giá):** `XXX-NNN-SC` (Main ID + Service Code)
- Khi đã chốt giá và lên HĐ → chuyển từ LEAD sang Service Code chính thức.
- Service code lấy từ danh sách dịch vụ chuẩn của iMind:

| Service Code | Dịch vụ |
|---|---|
| `AAA` | AI Agent |
| `15SEO` | SEO 15 bài |
| `Guideline` | Content Guideline |
| `15SOCIAL` | 15 bài Social/Tháng |
| `MKT-Basic` | Marketing Basic |
| `MKT-Adv` | Marketing Advanced |
| `MKT-Pro` | Marketing Pro |
| `VIDAI30-Basic` | Video AI 30 bản — Basic |
| `VIDAI30-Pro` | Video AI 30 bản — Pro |

**Custom / Tailored services:**
- Với dịch vụ không có trong danh sách chuẩn → User cung cấp custom code trong Round 2.
- Format: `XXX-NNN-CUST` (tạm thời nếu chờ user define).
- Ví dụ: `THL-001-WEBSITE`, `AWT-001-VIDEO-CUSTOM`

### 3.3. Ví dụ
```
THL-001             → Main: Thành Lợi DentalLab
THL-001-LEAD1       → Round 1: Dịch vụ 1 (chưa rõ tên)
THL-001-LEAD2       → Round 1: Dịch vụ 2 (chưa rõ tên)
THL-001-AAA         → Round 2: AI Agent (đã chốt)
TYH-001-15SOCIAL    → Round 2: 15 bài Social (đã chốt)
KKS-001-VIDAI30-Basic → Round 2: Video AI 30 bản Basic (đã chốt)
```

---

## 4. CỘT BẮT BUỘC THEO ROUND

| Round | Trigger | Cột cần điền |
|-------|---------|--------------|
| **Round 1** | "tôi cần nhập thông tin đợt 1 cho khách mới" | Tên KH*, Ngành*, Ngày tiếp nhận*, Giai đoạn = Lead |
| **Round 2** | "Trích xuất thông tin hiện tại của khách [ID], tôi muốn update" | Ngày BĐ HĐ**, Ngày KT HĐ**, Chiết khấu**, Giá trước CK**, Order details |

**Lưu ý Round 1:** KHÔNG hỏi "Khách quan tâm dịch vụ gì?" → Chỉ cần 3 cột bắt buộc, phần dịch vụ để LEAD1/LEAD2.

**Lưu ý Round 2:** Sale cung cấp **giá gốc (trước chiết khấu)** + **% chiết khấu** → AI tự tính:
- Giá sau chiết khấu = Giá gốc × (1 - %CK)
- Giá VAT 8% = Giá sau CK × 1.08

---

## 5. AI AUTO-CALC LOGIC

| Cột | Công thức |
|-----|-----------|
| Giá sau chiết khấu | `Giá trước CK × (1 - Chiết khấu%)` |
| Giá VAT 8% | `Giá sau CK × 1.08` |
| Tiến độ N% | `(Op số lượng N / Order số lượng N) × 100` |
| Trung/Tài % | `(Công sức cá nhân / Tổng Op quantity) × 100` |

---

## 6. TRIGGER COMMANDS

### 6.1. Đợt 1 — Khách hàng mới
```
Trigger: "tôi cần nhập thông tin đợt 1 cho khách mới"
Yêu cầu: Tên KH, Ngành, Ngày tiếp nhận
Hành động:
  1. AI gen Main ID (XXX-NNN)
  2. Tạo Sub-service ID = LEAD1 (thêm LEAD2, LEAD3... nếu KH có nhiều dịch vụ)
  3. Tạo main row + subrow LEAD1 trong 4 section
  4. Default: Giai đoạn Sale = Lead, Tiến độ = 0%
  5. KHÔNG hỏi "Khách quan tâm dịch vụ gì?" — để Round 2 quyết định
Confirm: "Đã thêm khách hàng XXX-NNN vào CRM với ID dịch vụ LEAD1"
```

### 6.2. Đợt 2 — Update sau khi chốt giá
```
Trigger: "Trích xuất thông tin hiện tại của khách [ID], tôi muốn update thông tin"
Hành động:
  1. Pull thông tin hiện tại (show user)
  2. Nhận thông tin mới từ Sale:
     - Ngày BĐ/KT HĐ
     - Chiết khấu (%)
     - Giá gốc (trước chiết khấu)
     - Số lượng Order
     - Service code (nếu đã chốn dịch vụ chuẩn)
  3. AI tự tính:
     - Giá sau CK = Giá gốc × (1 - %CK)
     - Giá VAT 8% = Giá sau CK × 1.08
  4. Chuyển Sub-service ID: LEAD1 → Service Code (AAA, 15SOCIAL, VIDAI30-Basic...)
  5. Cập nhật cả 4 section
  6. Cập nhật crm-sale-schedule.md (auto-split payment theo số tháng/nghỉ hạn)
  7. Giai đoạn Sale tự advance: Báo giá
Confirm: "Đã cập nhật XXX-NNN-SC: HĐ từ dd/mm/yyyy đến dd/mm/yyyy, CK x%, Giá sau CK = xx,đ, VAT = xx,đ"
```

### 6.3. Thanh toán đợt tiếp theo
```
Trigger: "Thanh toán đợt [N] cho khách [ID]"
Hành động:
  1. Update crm-sale-schedule.md: Đánh dấu đợt N đã thanh toán
  2. CRM chính: Số tiền đợt tiếp theo → đợt N+1, thời hạn → hạn đợt N+1
  3. Giai đoạn Sale: tự advance (N+1)
Confirm: "Đã cập nhật thanh toán đợt N cho XXX-NNN-SC. Đợt tiếp: [N+1] — Số tiền: xx,đ, Hạn: dd/mm/yyyy"
```

### 6.4. Sale cung cấp thông tin thanh toán
```
Trigger: "Cập nhật thanh toán cho khách [ID]" (hoặc trong lúc Round 2)
Yêu cầu: Số tiền cần thanh toán, Thời hạn thanh toán
Hành động:
  1. Điền vào crm-sale-schedule.md tại đợt tương ứng
  2. Đồng bộ ngược vào CRM chính: Số tiền đợt tiếp theo + Thời hạn
Confirm: "Đã cập nhật thanh toán cho XXX-NNN-SC"
```

---

## 7. RULES XỬ LÝ

### 7.1. Khách mới — Thiếu thông tin
1. Khi nhận thông tin khách mới, kiểm tra **3 cột bắt buộc Round 1**: Tên KH, Ngành, Ngày tiếp nhận
2. Nếu thiếu → Hỏi lại người cung cấp
3. Nếu "chưa có thông tin" → Điền `"cần update"`
4. Tất cả cột khác đều có thể để trống trong Round 1

### 7.2. Dịch vụ thêm cho khách cũ
1. Nếu khách thêm dịch vụ → Thêm subrow mới (LEAD3, LEAD4... hoặc SC mới)
2. Các subrow giữ nguyên ID khách, khác số LEAD/SC
3. Giai đoạn Sale của dịch vụ mới = Lead
4. Giai đoạn Sale của dịch vụ cũ không đổi
5. Main row Hợp đồng: cập nhật tổng giá trị

### 7.3. Demo yêu cầu từ Sale
1. Ghi nội dung demo vào **Sale section → Ghi chú thêm**
2. Nếu demo ảnh hưởng đến giai đoạn Sale → Update: Tư vấn/Demo
3. Operation không cần action cho demo (trừ khi demo = deployment)

### 7.4. Thanh toán đủ — Hoàn thành trả đợt cuối
1. Nếu đã thanh toán đủ **giá trị HĐ** → Ghi chú "Đã thanh toán đủ. Operation chưa hoàn tất."
2. **KHÔNG** đổi giai đoạn Sale thành "Hoàn thành" cho đến khi Operation xong
3. Giai đoạn Sale giữ nguyên "Thanh toán đợt N" cho đến khi Op hoàn tất

### 7.5. Operation tiến độ
1. Khi Op update tiến độ → AI tính % tự động
2. Tiến độ % = (Op quantity / Order quantity) × 100
3. Nếu Op chưa bắt đầu → Tiến độ = 0%
4. Nếu Op hoàn tất → Tiến độ = 100% → Kiểm tra giai đoạn Sale để quyết định "Hoàn thành" chưa

---

## 8. VÍ DỤ DỮ LIỆU

### Round 1 — Khách mới
```
User: "tôi cần nhập thông tin đợt 1 cho khách mới"
Mai: "OK! Cần thông tin: Tên doanh nghiệp, Ngành, Ngày tiếp nhận (hôm nay?)"
User: "ABC Clinic, Y tế, hôm nay"
Mai: "Đã thêm ABC-001 | ABC Clinic | Y tế | dd/mm/yyyy | Giai đoạn: Lead"
```

### Round 2 — Update sau chốt giá
```
User: "Trích xuất thông tin hiện tại của khách ABC-001, tôi muốn update thông tin"
Mai: "
Hợp đồng: ABC-001 | ABC Clinic | Y tế | dd/mm/yyyy | — | — | — | — | — | —
Sale: ABC-001 | Lead | — | — | —
Order: ABC-001 | — | — | — | — | — | — | — | —
Op: ABC-001 | — | — | 0 | — | — | — | — | — | — | 0% | 0%
⬆️ Nhập thông tin mới:"
User: "Ngày BĐ 15/09, Ngày KT 15/12, CK 5%, Giá 50tr, Order 30 content"
Mai: "
✅ Đã cập nhật ABC-001:
HĐ: 15/09/2026 → 15/12/2026 | CK 5%
Giá trước CK: 50.000.000đ
Giá sau CK: 47.500.000đ
VAT 8%: 51.300.000đ
Sale: Báo giá | Đợt tiếp theo: 23.750.000đ | Hạn: 15/10/2026
Order: 30 | content | — | — | — | — | — | —
Op: Tiến độ 0% | 0% | 0%
"
```

### Thanh toán
```
User: "Thanh toán đợt 1 cho ABC-001"
Mai: "Đã cập nhật thanh toán đợt 1 cho ABC-001. Đợt tiếp: 2 — 23.750.000đ — Hạn: 15/12/2026"
```

---

*File được tạo: 2026-09-12 | Cập nhật: 2026-09-12*
