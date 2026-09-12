# CRM — iMind AI Marketing

> Cập nhật: 2026-09-12
> Người tạo: Mai (iMind Update)
> Rules: `CRM/crm-rules.md`
> Payment Schedule: `CRM/crm-sale-schedule.md`

---

## 1. HỢP ĐỒNG

> `*` = Required Round 1 (sau khi tiếp nhận khách) | `**` = Required Round 2 (sau khi chốt báo giá & lên HĐ)
> Main row = Tổng khách hàng | Subrow = Chi tiết dịch vụ

| ID | Tên doanh nghiệp / khách hàng | Ngành | Ngày tiếp nhận khách* | Ngày bắt đầu HĐ** | Ngày kết thúc HĐ** | Chiết khấu** | Giá trước CK** | Giá sau CK (AI) | Giá VAT 8% (AI) |
|---|---|---|---|---|---|---|---|---|---|
| **THL-001** | **Thành Lợi DentalLab** | **Y tế** | 10/09/2026 | — | — | — | — | — | — |
| THL-001-LEAD1 | Website + SEO + tra cứu BH | Y tế | 10/09/2026 | — | — | — | — | — | — |
| THL-001-LEAD2 | AI Agent hành chính | Y tế | 10/09/2026 | — | — | — | — | — | — |
| **TYH-001** | **TY Health Supplements** | **Y tế** | 09/09/2026 | — | — | — | 44.000.000 | — | — |
| TYH-001-LEAD1 | Content social (2 gói × 2.5tr) | Y tế | 09/09/2026 | — | — | — | 5.000.000 | — | — |
| TYH-001-LEAD2 | Video AI (300 video × 130k) | Y tế | 09/09/2026 | — | — | — | 39.000.000 | — | — |
| **KKS-001** | **Kim Khuê Beauty & Spa** | **Sắc đẹp / Thẩm mỹ** | 09/09/2026 | — | — | — | 5.000.000 | — | — |
| KKS-001-LEAD1 | 30 video AI/tháng | Sắc đẹp / Thẩm mỹ | 09/09/2026 | — | — | — | 5.000.000 | — | — |
| **NHA-001** | **Công ty TNHH công nghệ Nhà Sạch** | **Thiết bị gia dụng** | 11/09/2026 | — | — | — | — | — | — |
| NHA-001-CUST | cần update | Thiết bị gia dụng | 11/09/2026 | — | — | — | — | — | — |
| **AWT-001** | **Adt Wine & Art** | **Rượu vang** | 11/09/2026 | — | — | — | — | — | — |
| AWT-001-CUST | Video AI hộp quà | Rượu vang | 11/09/2026 | — | — | — | — | — | — |

---

## 2. SALE

> Bảng thanh toán chi tiết 12 đợt: `CRM/crm-sale-schedule.md`
> CRM chính chỉ hiển thị **đợt tiếp theo chưa thanh toán** — tự advance khi đợt hiện tại đã thanh toán.
> Default Giai đoạn = Lead. Tự advance: Thanh toán đợt 1 → "Thanh toán đợt 2", v.v.

| ID | Giai đoạn Sale | Số tiền đợt tiếp theo | Thời hạn đợt tiếp theo | Ghi chú thêm về khách hàng |
|---|---|---|---|---|
| THL-001-LEAD1 | Báo giá | 25.000.000 | 15/10/2026 | Website sập — cần brief Dev. Next: Thu thập thông tin chi tiết |
| THL-001-LEAD2 | Lead | — | — | Chờ triển khai xong Website mới tư vấn AI Agent |
| TYH-001-LEAD1 | Lead | — | — | Kênh nội dung lộn xộn, fanpage bị flag. Next: Thu thập thông tin chi tiết |
| KKS-001-LEAD1 | Lead | — | — | Bà chủ tự dựng, không đủ số lượng video TikTok. Next: Thu thập thông tin chi tiết |
| NHA-001-LEAD1 | Lead | — | — | cần update |
| AWT-001-LEAD1 | Lead | — | — | Cần video AI tạo sinh theo avatar người gửi |

---

## 3. ORDER

| ID | Số lượng 1 | Đơn vị 1 | Số lượng 2 | Đơn vị 2 | Số lượng 3 | Đơn vị 3 | Số lượng 4 | Đơn vị 4 |
|---|---|---|---|---|---|---|---|---|
| THL-001-LEAD1 | 1 | Website+SEO+tra cứu BH | — | — | — | — | — | — |
| THL-001-LEAD2 | 1 | AI Agent hành chính | — | — | — | — | — | — |
| TYH-001-LEAD1 | 2 | gói content/tháng | — | — | — | — | — | — |
| TYH-001-LEAD2 | 300 | video AI (30-50s) | — | — | — | — | — | — |
| KKS-001-LEAD1 | 30 | video AI/tháng | — | — | — | — | — | — |
| NHA-001-CUST | — | cần update | — | — | — | — | — | — |
| AWT-001-CUST | — | cần update | — | — | — | — | — | — |

---

## 4. OPERATION

| ID | Ngày bắt đầu | Ngày kết thúc | Tiến độ 1 | Đơn vị 1 | Tiến độ 2 | Đơn vị 2 | Tiến độ 3 | Đơn vị 3 | Tiến độ 4 | Đơn vị 4 | Trung | Tài |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| THL-001-LEAD1 | — | — | 0 | website | — | — | — | — | — | — | 0% | 0% |
| THL-001-LEAD2 | — | — | 0 | AI Agent | — | — | — | — | — | — | 0% | 0% |
| TYH-001-LEAD1 | — | — | 0 | gói content | — | — | — | — | — | — | 0% | 0% |
| TYH-001-LEAD2 | — | — | 0 | video | — | — | — | — | — | — | 0% | 0% |
| KKS-001-LEAD1 | — | — | 0 | video/tháng | — | — | — | — | — | — | 0% | 0% |
| NHA-001-CUST | — | — | — | cần update | — | — | — | — | — | — | — | — |
| AWT-001-CUST | — | — | — | cần update | — | — | — | — | — | — | — | — |

---

*Cập nhật: 2026-09-12*
