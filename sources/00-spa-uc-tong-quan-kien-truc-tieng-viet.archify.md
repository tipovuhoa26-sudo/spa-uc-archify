# Nên xây mới hay mua phần mềm cho chuỗi 2 chi nhánh The Heaven Lounge (Spa Úc)? {#spa-uc-master-tieng-viet}

Thực hiện **dùng thử 14 ngày** (`Quyết định Q4`) trên Timely hoặc Kitomba theo 3 kịch bản `K3, K4, K9` trước khi ký hợp đồng xây mới (`Phương án C`).[](source:wp22)

- Phần mềm sẵn có bỏ trống **4 khoảng trống cốt lõi**: `G1` tàu điện, `G2` hoa hồng lương Úc, `G4` sổ nợ GST, `G5` nghỉ đột xuất.[](source:wp22)
- Chi phí vận hành hằng tháng khi tự xây (`440-550 AUD`) xấp xỉ phương án mua (`496-516 AUD`). Do đó, phương án xây chỉ hòa vốn khi tiết kiệm `trên 32 giờ/tháng` hoặc chia sẻ cho `13-20 spa`.[](source:wp22)
- Việc diễn giải `Award MA000005`, tính `Super 12%` (`Payday Super 01/07/2026`), khai `STP Phase 2` và sổ cái `BAS/GST` bắt buộc đặt ở phần mềm chuẩn của Úc (`Employment Hero`, `Deputy`, `Xero`).[](source:fw2025)
- Kiến trúc MVP gồm **17 màn hình** chia theo **4 cổng vai trò**, thực thi **42 quy tắc nghiệp vụ** (`NT1-NT19`) và che bệnh án khỏi lễ tân (`SEC-008`).[](source:prd10)

@answer scope="2 chi nhánh Melbourne · 7 nhân sự · 17 màn hình MVP · 19 nguyên tắc NT1-NT19"
@source wp22 title="Whitepaper Kiến trúc Mini-ERP Spa Úc v2.2 (3.862 dòng)" url=https://theheavenlounge.com/
@source ba10 title="Tài liệu Phân tích Nghiệp vụ & Product Blueprint v1.0 (2.148 dòng)" url=https://theheavenlounge.com/
@source prd10 title="Tài liệu Yêu cầu Sản phẩm MVP PRD v1.0 (733 dòng)" url=https://theheavenlounge.com/
@source asbuilt title="Báo cáo Kiểm toán 3 File HTML Prototype As-Built (894 dòng)" url=https://theheavenlounge.com/
@source fw2025 title="Fair Work Commission - Hair and Beauty Industry Award 2020 [MA000005]" url=https://www.fwc.gov.au/

## Hiện trạng vận hành của The Heaven Lounge tại Melbourne đang vướng ở đâu? {#hien-trang-spa-uc}

Chuỗi vận hành 2 cơ sở tại Melbourne dưới 1 mã số thuế `ABN` (`ORG-01`) với đội ngũ 6 đến 7 kỹ thuật viên (`Việt, Anh, Thái`). Nhu cầu khách rất tốt (`4.9/5 sao` trên Google), nhưng web hiện tại (`WordPress 7.1.3` + `Contact Form 7 v6.1.4`) chỉ gửi biểu mẫu về email mà không khóa lịch thời gian thực.[](source:wp22)

@facts
| Chi nhánh 1 (Brunswick) | `550 Sydney Rd, Brunswick VIC 3056` (Hội đồng `Merri-bek SAL20361`, dân số `29.861`, tuổi trung vị `33`) |
| Chi nhánh 2 (North Melbourne) | `342 Victoria St, North Melbourne VIC 3051` (cách `~8 km`, di chuyển `30-45 phút` bằng tàu điện tram) |
| Bảng giá thực tế (AUD, gồm GST) | Giá ưu đãi tiền mặt vs Giá thẻ chuẩn: `Head Spa Combo` (`$60/$67`, `$100/$111`, `$150/$166`), `Hot Stone` (`$105/$116`, `$145/$160`) |
| Quy mô giao dịch giả định | `45 lịch/ngày` (`1.170 lịch/tháng`), đơn trung bình `120 AUD`, `90%` thanh toán thẻ (`phí thẻ ~1.769 AUD/tháng`) |
| Lệnh cấm phụ phí thẻ RBA | Từ `01/10/2026`, cấm thu phụ phí thẻ Visa/Mastercard; bắt buộc hiển thị một giá chuẩn rõ ràng và áp giảm giá tiền mặt (`PAY-001`) |
| Hiện trạng 3 file HTML Prototype | `8.904 dòng mã` tách rời trên 3 khóa `localStorage` riêng biệt, chưa có máy chủ chung và chưa đồng bộ dữ liệu chéo |

## Làm sao giải quyết bài toán đặt lịch liên chi nhánh và KTV nghỉ đột xuất? {#luong-dat-lich-va-nghi-dot-xuat}

Hệ thống hợp nhất lịch ca, chứng chỉ hành nghề, thời gian đi lại giữa 2 chi nhánh và lịch hẹn vào một hàm tính duy nhất (`NT1`). Khi kỹ thuật viên báo nghỉ đột xuất, lịch hẹn tuyệt đối không bị xóa mà chuyển vào hàng đợi `Need Attention` (`BKG-008`).[](source:ba10)

@workflow id=luong-dieu-phoi-vn title="Luồng Kiểm tra Điều kiện 5 Bước & Xử lý KTV Nghỉ Đột xuất"
| b2 | Đủ 5 điều kiện? | backend |
| b3 | Chèn giờ tàu | backend |
| b5 | Khách chọn KTV? | security |
| b7 | Khóa & Consent | database |
| b2 > b3 | có | emphasis |
| b2 > b5 | nghỉ |
| b3 > b7 | khóa | emphasis |
| b5 > b7 | đồng ý | security |

@steps id=steps-luong-dieu-phoi-vn
| st-b2 | Đủ 5 điều kiện? | Chỉ cho phép nhận lịch khi KTV thỏa mãn cả 5 điều kiện: `Kỹ năng hoạt động + Đã hoàn thành đào tạo + Chứng chỉ còn hạn (HLTINF005/Laser) + Có ca làm việc + Không trùng lịch` (`SKL-003`). | luong-dieu-phoi-vn | b2 |
| st-b3 | Chèn giờ tàu | Nếu KTV đổi chi nhánh giữa `Brunswick` và `North Melbourne` trong cùng ngày, hệ thống tự chèn khối di chuyển `30-45 phút` và xuất sang bảng lương là giờ công đi lại (`MA000005 khoản 20.9`). | luong-dieu-phoi-vn | b3 |
| st-b5 | Khách chọn KTV? | Nếu khách đã chọn đích danh KTV (`Selected Therapist` `BKG-007`), lễ tân không được tự ý đổi ngầm mà phải chuyển sang trạng thái chờ khách xác nhận (`Waiting for Customer Decision`). | luong-dieu-phoi-vn | b5 |
| st-b7 | Khóa & Consent | Với dịch vụ xâm lấn, Laser/IPL hoặc phun xăm (`đủ 18 tuổi`), hệ thống chặn chuyển sang `In Service` nếu khách chưa ký biểu mẫu đồng ý điện tử (`TRT-002`). | luong-dieu-phoi-vn | b7 |

## Kiến trúc Tài chính, GST và Bảng lương Đầu vào hoạt động như thế nào? {#kien-truc-tai-chinh-luong}

Nguyên tắc `NT6` đến `NT12` tách bạch 5 khái niệm: **Tiền thu, Doanh thu dịch vụ, Nợ trả trước, Thuế GST và Tiền tip giữ hộ**. Mini-ERP không tự tính lương Award mà khóa `Payroll Input Snapshot` mỗi 2 tuần để xuất sang phần mềm lương chuẩn của Úc.[](source:wp22)

@sequence id=seq-thu-ngan-vn title="Giao dịch Nguyên tử 6 Bước khi Hoàn tất Dịch vụ & Chốt Kỳ Lương"
| actor letan | Lễ tân POS (B5) | frontend |
| actor core | Lõi Mini-ERP | backend |
| actor db | Sổ Giao dịch & Nợ | database |
| actor xero | Xero / MYOB | external |
| actor payroll | Employment Hero | external |
| phase | 1. Thu ngân Tách dòng & Giao dịch Nguyên tử 6 Bước |
| letan | core | Hoàn tất dịch vụ (Giá Tiền mặt/Thẻ + Gói + Tip) |
| core | db | Ghi Doanh thu/Trừ gói + GST theo loại + Hoa hồng 100% | emphasis |
| db | letan | Hóa đơn & Cập nhật số dư sự kiện (NT7) | return |
| phase | 2. Chốt ca Hằng ngày & Khóa Kỳ lương 2 Tuần |
| core | xero | Đẩy Bút toán Tổng hợp Ngày theo Chi nhánh (NT12) |
| core | payroll | Xuất Payroll Input Snapshot Đã khóa (Giờ + Đi lại + Hoa hồng + Tip) | security |

@steps id=steps-thu-ngan-vn
| st-pos | Lễ tân POS (B5) | Hỗ trợ thanh toán gộp (`Tiền mặt + Thẻ + Voucher`), áp giá ưu đãi tiền mặt hoặc giá thẻ chuẩn (`PAY-001`), và ghi nhận riêng khoản tip khách gửi qua thẻ (`PAY-004`). | seq-thu-ngan-vn | letan |
| st-db | Sổ Giao dịch & Nợ | Ghi 6 sự kiện trong 1 giao dịch CSDL. Phân biệt rõ `Voucher mệnh giá tiền` (tính GST khi dùng) và `Gói dịch vụ cụ thể` (tính GST khi bán) theo quy định ATO (`NT6`). | seq-thu-ngan-vn | db |
| st-xero | Xero / MYOB | Nhận bút toán tổng hợp hằng ngày sau khi chốt ca theo từng chi nhánh (`Xero Tracking Categories`) và bút toán hoàn tiền riêng biệt (`NT12`). | seq-thu-ngan-vn | xero |
| st-pay | Employment Hero | Nhận tệp dữ liệu đầu vào đã khóa (`COM-006`) để tính phụ cấp cuối tuần/ngày lễ theo `Award MA000005`, `Super 12%` (`Payday Super 01/07/2026`) và nộp `STP Phase 2`. | seq-thu-ngan-vn | payroll |

## Danh mục 14 Hồ sơ Kiến trúc Archify trong Bộ Bàn giao gồm những gì? {#danh-muc-14-ho-so}

Bộ tài liệu tại `spa-uc-archify/` bao phủ từ chiến lược đầu tư đến 11 sơ đồ kỹ thuật chuẩn `showcase`.[](source:prd10)

| Mã Hồ sơ | Loại Artifact | Tên File Bàn giao (Bấm để mở) | Nội dung Cốt lõi | Trạng thái Kiểm định |
| --- | --- | --- | --- | --- |
| `01` {#art-01} | `explainer` | [01-spa-uc-executive-whitepaper-explainer.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/01-spa-uc-executive-whitepaper-explainer.html) | Chiến lược Build vs Buy (`Q0-Q4`), so sánh 10 hãng SaaS, TCO 3 năm, pháp lý Úc & `NT1-NT19` | `Showcase Pass` {good:0 Warnings} |
| `02` {#art-02} | `explainer` | [02-spa-uc-mvp-prd-blueprint-explainer.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/02-spa-uc-mvp-prd-blueprint-explainer.html) | Ma trận phân quyền RBAC (`SEC-008`), 17 màn hình MVP (`A1-D6`), lộ trình `R0-R6` & `UNK-01..09` | `Showcase Pass` {good:0 Warnings} |
| `03` {#art-03} | `explainer` | [03-spa-uc-prototype-as-built-handover-explainer.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/03-spa-uc-prototype-as-built-handover-explainer.html) | Kiểm toán 3 file HTML (`6.970 dòng`), nâng cấp `W1-W7` KTV & 12 điểm cần thay thế khi lên Cloud | `Showcase Pass` {good:0 Warnings} |
| `04` {#art-04} | `architecture` | [04-spa-uc-target-system-architecture.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/04-spa-uc-target-system-architecture.html) | Kiến trúc Modular Monolith, cổng RBAC, PostgreSQL Sydney, Outbox & kết nối Xero/Payroll/EFTPOS | `Showcase Pass` {good:Browser Verified} |
| `05` {#art-05} | `compare` | [05-spa-uc-asbuilt-vs-target-compare.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/05-spa-uc-asbuilt-vs-target-compare.html) | So sánh kiến trúc (`Delta Diff`) giữa 3 file HTML `localStorage` và Kiến trúc Cloud MVP đích | `28/28 Checks` {good:Delta Verified} |
| `06` {#art-06} | `workflow` | [06-spa-uc-booking-dispatch-workflow.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/06-spa-uc-booking-dispatch-workflow.html) | Luồng đặt lịch đa kênh, kiểm tra 5 điều kiện, chèn `30-45p` tàu điện & xử lý `Need Attention` | `Showcase Pass` {good:Browser Verified} |
| `07` {#art-07} | `lifecycle` | [07-spa-uc-appointment-lifecycle.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/07-spa-uc-appointment-lifecycle.html) | Máy trạng thái lịch hẹn (`Request -> Confirmed -> Checked-In -> In Service -> Completed` + Ngoại lệ) | `Showcase Pass` {good:Browser Verified} |
| `08` {#art-08} | `sequence` | [08-spa-uc-service-checkout-sequence.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/08-spa-uc-service-checkout-sequence.html) | Trình tự cổng Consent (`TRT-002`), quẹt thẻ lũy đẳng, giao dịch nguyên tử 6 bước & hoàn tiền sau chốt ca | `Showcase Pass` {good:Browser Verified} |
| `09` {#art-09} | `dataflow` | [09-spa-uc-finance-gst-payroll-dataflow.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/09-spa-uc-finance-gst-payroll-dataflow.html) | Luồng dữ liệu 5 chiều: Tiền thu, Doanh thu, Sổ nợ trả trước, GST 2 chế độ & Snapshot lương 2 tuần | `Showcase Pass` {good:0 Crossings} |
| `10` {#art-10} | `erd` | [10-spa-uc-core-domain-erd.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/10-spa-uc-core-domain-erd.html) | Sơ đồ thực thể quan hệ 8 bảng cốt lõi (`branches`, `staff_profiles`, `appointments`, `ledger_entries`...) | `Showcase Pass` {good:Browser Verified} |
| `11` {#art-11} | `class` | [11-spa-uc-eligibility-commission-class.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/11-spa-uc-eligibility-commission-class.html) | Mô hình lớp `SlotValidator` (`NFR-009`), `EligibilityEngine`, `CommissionEngine` & `PayrollSnapshot` | `Showcase Pass` {good:Browser Verified} |
| `12` {#art-12} | `tree` | [12-spa-uc-capability-module-tree.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/12-spa-uc-capability-module-tree.html) | Cây phân rã chức năng 4 cổng vai trò và 17 màn hình MVP gắn với các đợt phát hành `R0-R6` | `Showcase Pass` {good:Browser Verified} |
| `13` {#art-13} | `timeline` | [13-spa-uc-incident-sla-timeline.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/13-spa-uc-incident-sla-timeline.html) | Dòng thời gian xử lý KTV báo ốm buổi sáng, di chuyển tàu điện 45p và SLA 1 giờ cho đánh giá 1-2 sao | `Showcase Pass` {good:Browser Verified} |
| `14` {#art-14} | `waterfall` | [14-spa-uc-booking-availability-waterfall.html](https://htmlpreview.github.io/?https://github.com/tipovuhoa26-sudo/spa-uc-archify/blob/main/14-spa-uc-booking-availability-waterfall.html) | Giản đồ thác nước độ trễ khóa lịch chống trùng `TSTZRANGE` (`285ms < ngân sách 300ms` theo `NFR-001`) | `Showcase Pass` {good:Browser Verified} |

## Cần kiểm tra những điều kiện nào trước khi ký hợp đồng triển khai Giai đoạn 1? {#danh-sach-kiem-tra}

Hoàn tất 5 hạng mục xác minh dưới đây trước khi khóa phạm vi xây dựng `Phương án C` (`R0-R6`).[](source:wp22)

- [ ] **Chốt `Quyết định Q0` (`SX01, CT09`):** Xác nhận hệ thống chỉ phục vụ `The Heaven Lounge` hay mở rộng cho `15-20 spa` (`NFR-010`).
- [ ] **Chạy thử 14 ngày (`Quyết định Q4`):** Đo số giờ thủ công trên `Timely` hoặc `Kitomba` với 3 kịch bản `K3, K4, K9`.
- [ ] **Xác nhận Kế toán & Lương (`UNK-01, UNK-02`):** Chốt phần mềm kế toán (`Xero/MYOB`) và lương (`Employment Hero/KeyPay`) đang dùng tại Melbourne.
- [ ] **Xác nhận máy EFTPOS (`UNK-03`):** Kiểm tra máy quẹt thẻ tại 2 chi nhánh là `Tyro`, `Square`, `Stripe` hay `Zeller`.
- [ ] **Khóa quy tắc hoa hồng (`UNK-04`):** Chốt tính hoa hồng trên **giá gốc** hay **giá sau giảm tiền mặt** (`PAY-001`) trước đợt `R4`.
