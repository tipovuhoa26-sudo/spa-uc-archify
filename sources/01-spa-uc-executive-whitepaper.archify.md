# Nên tự xây dựng hay mua phần mềm SaaS cho chuỗi 2 chi nhánh The Heaven Lounge tại Melbourne? {#spa-uc-whitepaper}

Thực hiện **dùng thử 14 ngày** (`Quyết định Q4`) trên `Timely` hoặc `Kitomba` theo 3 kịch bản `K3, K4, K9` trước khi ký hợp đồng xây mới `Phương án C`.[](source:wp22)

- Cả 10 phần mềm tại Úc đều hụt **4 khoảng trống cốt lõi**: `G1` tàu điện, `G2` hoa hồng lương Úc, `G4` sổ nợ GST, `G5` nghỉ đột xuất.[](source:wp22)
- Chi phí vận hành hằng tháng khi tự xây (`440-550 AUD`) tương đương phương án mua (`496-516 AUD`). Trong khi đó, phí quẹt thẻ `1,4%` chiếm tới `1.769 AUD/tháng`.[](source:wp22)
- Lõi tự xây (`Phương án C`) chỉ hòa vốn khi tiết kiệm trên `32 giờ thủ công/tháng` hoặc chia sẻ nền tảng cho `13-20 spa` tại Úc (`SX01`).[](source:wp22)
- Việc tính lương `Award MA000005`, `Super 12%` (`Payday Super 01/07/2026`), `STP Phase 2` và sổ cái `BAS/GST` bắt buộc đặt ở `Employment Hero`, `Deputy` và `Xero`.[](source:fw2025)

@answer scope="2 chi nhánh Melbourne · 7 nhân sự · 45 lịch/ngày · đơn trung bình 120 AUD · kiểm chứng 26/09/2026"
@source wp22 title="Whitepaper Kiến trúc Mini-ERP Spa Úc v2.2 (3.862 dòng)" url=https://theheavenlounge.com/
@source fw2025 title="Fair Work Commission - Hair and Beauty Industry Award 2020 [MA000005]" url=https://www.fwc.gov.au/
@source ato2026 title="ATO Payday Super (01/07/2026), STP Phase 2 & GSTR 2003/5 Voucher Rules" url=https://www.ato.gov.au/
@source rba2025 title="RBA Review of Merchant Card Payment Costs and Surcharging (Cấm phụ phí thẻ 01/10/2026)" url=https://www.rba.gov.au/
@source acl2019 title="Australian Consumer Law s.99B - Thời hạn thẻ quà tặng tối thiểu 36 tháng" url=https://consumer.gov.au/

## Hiện trạng vận hành của The Heaven Lounge và 6 rào cản pháp lý tại bang Victoria là gì? {#boi-canh-phap-ly}

Chuỗi vận hành 2 chi nhánh tại Melbourne dưới 1 mã số thuế `ABN` (`ORG-01`) với 6 đến 7 kỹ thuật viên (`Việt, Anh, Thái`). Website hiện tại (`WordPress 7.1.3` + `Contact Form 7 v6.1.4`) chỉ gửi biểu mẫu về email mà không khóa lịch thời gian thực.[](source:wp22)

@facts
| Chi nhánh 1 (Brunswick) | `550 Sydney Rd, Brunswick VIC 3056` (`Merri-bek SAL20361`, dân số `29.861`, tuổi trung vị `33`, thu nhập hộ `1.976 AUD/tuần`) |
| Chi nhánh 2 (North Melbourne) | `342 Victoria St, North Melbourne VIC 3051` (cách `~8 km`, di chuyển `30-45 phút` bằng tàu điện tram qua `Elizabeth St`) |
| Bảng giá thực tế (AUD, gồm GST) | Giá tiền mặt vs Giá thẻ chuẩn: `Head Spa Combo` (`$60/$67`, `$100/$111`, `$150/$166`), `Hot Stone` (`$105/$116`, `$145/$160`) |
| Phụ cấp lương `MA000005` (01/07/2025) | Bậc 3 cơ bản `27,18 AUD/giờ` (`Casual 33,98 AUD`), Thứ Bảy `36,15 AUD`, Chủ Nhật `54,36 AUD`, Ngày lễ `67,95 AUD`, đi lại liên chi nhánh `0,99 AUD/km` |
| Hạn mức Super & STP Phase 2 | `Superannuation Guarantee 12%` từ `01/07/2025`. Từ `01/07/2026` (`Payday Super`), tiền hưu trí phải vào quỹ trong `7 ngày làm việc` kể từ ngày trả lương |
| Thuế GST & Bảo vệ người tiêu dùng | `GST 10%` (`GSTR 2003/5`): Voucher tiền mặt tính GST khi dùng; gói dịch vụ tính GST khi bán. Thẻ quà tặng có hạn tối thiểu `36 tháng` (`ACL s.99B`) |
| Quyền riêng tư & An toàn bức xạ | `Privacy Act 1988` yêu cầu che bệnh án khỏi lễ tân (`SEC-008`). Bang Victoria bắt buộc chứng chỉ `Radiation Act 2005`, `HLTINF005` và đủ `18 tuổi` khi phun xăm |

@callout level=warning title="Lệnh cấm phụ phí thẻ RBA (Hiệu lực 01/10/2026) và Ưu đãi tiền mặt (PAY-001)"
| Ngân hàng Dự trữ Úc (`RBA`) cấm thu phụ phí đối với thẻ `Visa / Mastercard` từ ngày `01/10/2026`. |
| Spa bắt buộc niêm yết **một giá thẻ chuẩn duy nhất** (gồm GST) và áp **giảm giá tiền mặt** khi thu ngân (`PAY-001`). |

## Ban lãnh đạo cần đi qua 5 cổng quyết định (Q0 đến Q4) theo trình tự nào? {#trinh-tu-quyet-dinh}

Tài liệu Whitepaper bác bỏ việc ký ngay hợp đồng xây dựng `278 man-days` (`QT-2026-SPA-01`). Ban lãnh đạo cần đi qua 5 cổng tuần tự (`Q0 -> Q4 -> Q1 -> Q2 -> Q3`) dựa trên số liệu dùng thử thực tế.[](source:wp22)

@workflow id=decision-gate-flow title="Trình tự 5 Cổng Quyết định Đầu tư (Q0 đến Q3)"
| q0 | Q0: Quy mô? | frontend |
| q4 | Q4: Thử 14 ngày | backend |
| q1 | Q1: Chọn A/C? | security |
| q3 | Q2-Q3: Khóa MVP | database |
| q0 > q4 | đo | emphasis |
| q0 > q1 | giá |
| q4 > q3 | chốt | emphasis |
| q1 > q3 | khóa | emphasis |

@steps id=decision-gate-steps
| step-q0 | Q0: Quy mô? | Chốt mục tiêu xây riêng cho `The Heaven Lounge` hay thương mại hóa cho `15-20 spa` tại Úc (`SX01`). Nếu làm sản phẩm thương mại, bắt buộc gắn `organisation_id` (`NFR-010`) ngay từ đầu. | decision-gate-flow | q0 |
| step-q4 | Q4: Thử 14 ngày | Chạy thử `Timely` hoặc `Kitomba` trong 2 tuần với 3 kịch bản `K3, K4, K9`. Bước này đo chính xác số giờ thủ công còn lại mỗi kỳ lương (`CT15`). | decision-gate-flow | q4 |
| step-q1 | Q1: Chọn A/C? | Chọn `Phương án A` (Mua SaaS), `Phương án A+` (Mua SaaS + Xây cầu nối Lương/Ca) hoặc `Phương án C` (Xây Lõi Mini-ERP + Tích hợp Xero & Lương). Bác bỏ `Phương án B` (tự xây phần mềm lương/thuế). | decision-gate-flow | q1 |
| step-q3 | Q2-Q3: Khóa MVP | Giữ đặt lịch, giờ tàu điện, sổ nợ trả trước và chia hoa hồng trong Lõi Mini-ERP. Ủy thác tính lương Award, `Super 12%`, `STP Phase 2` và sổ cái `BAS` cho phần mềm chuẩn Úc trên 17 màn hình (`A1-D6`). | decision-gate-flow | q3 |

## Vì sao cả 10 phần mềm SaaS tại Úc đều hụt các luồng K3, K4, K6/K7 và K9? {#so-sanh-10-hang-saas}

Không phần mềm đơn lẻ nào gom đủ lịch đa chi nhánh, giờ tàu điện và chia hoa hồng combo. Chúng cũng thiếu sổ nợ GST 2 chế độ và động cơ tính lương `Award MA000005`.[](source:wp22)

| Nền tảng & Xuất xứ | Tàu điện Liên chi nhánh (`K4 / G1`) | Xử lý Nghỉ đột xuất (`K3 / G5`) | Hoa hồng & Cầu nối Lương (`K9 / G2`) | Sổ nợ Trả trước & GST (`K6-K7 / G4`) | Đồng bộ Kế toán (`K8`) | Chi phí Tháng (5 KTV) |
| --- | --- | --- | --- | --- | --- | --- |
| Timely (NZ) {#v-timely} | Khối bận chặn đặt trùng chi nhánh; không tự chèn giờ đi lại {caution:Một phần} | Kéo thả từng lịch thủ công; không gợi ý người thay {bad:Thủ công} | Ghi nhận cho thu ngân thay vì KTV làm (`NC18`); chỉ xuất CSV {bad:Hụt G2} | Có sổ nợ, nhưng **hoàn tiền không tự đồng bộ sang Xero** (`NC21`) {caution:Lỗi Hoàn tiền} | Đồng bộ hóa đơn Xero; ngừng MYOB từ `01/03/2026` | `325 AUD` (`Elevate` 5x`$65`) |
| Kitomba (NZ) {#v-kitomba} | Đa chi nhánh ở gói Enterprise; không tự chèn giờ đi lại | Nhắn SMS từng khách khi KTV ốm (`NC85`); đổi lịch tay {caution:SMS Lẻ} | Hỗ trợ 7 bậc hoa hồng (`NC32`); xuất CSV thủ công sang lương | Theo dõi số dư gói và thẻ quà tặng liên chi nhánh (`NC30`) {good:Sổ nợ Tốt} | Chỉ đẩy tổng kết ca (`NC28`) | Báo giá Enterprise riêng |
| Shortcuts (AU) {#v-shortcuts} | Nhân sự đa điểm (`NC119`); phân ca cơ bản | Báo cáo lịch bị ảnh hưởng & gửi SMS hàng loạt (`NC118`) {good:Báo cáo Tốt} | Báo cáo hoa hồng mở rộng (`NC121`); không nối API lương Úc | Báo cáo bù trừ thẻ quà tặng giữa các chi nhánh (`NC123`) {good:Liên chi nhánh} | Tải tổng kết ngày thủ công; lệch làm tròn GST dòng | Báo giá riêng + `49 AUD` Xero |
| Phorest (IE/AU) {#v-phorest} | Hồ nhân sự chung (`Works In` `NC44`); không chèn giờ tàu | Kéo thả kèm cảnh báo khách chọn đích danh (`NC105`) | Tính lại kỳ cũ theo tỷ lệ mới (`NC112`); **hỏng lương Úc** {bad:Lỗi Hồi tố} | **Ghi nhận doanh thu gói ngay khi bán** (`NC45`); sửa số dư tay {bad:Sai AASB 15} | Bút toán 1h sáng; thiếu phân loại chi nhánh Xero | Báo giá riêng (`SMS 8,5-12c`) |
| Fresha (UK) {#v-fresha} | Các điểm độc lập; cấu hình tay (`NC03`) | Tự ý đổi ngầm lịch không chọn đích danh (`NC02`) | Khóa kỳ lương đã chốt (`NC05`); không có động cơ Award Úc | Thẻ quà tặng khóa cứng trong 1 pháp nhân (`NC12`) | Đồng bộ Xero (`07/2026`) | `150 AUD` + `20%` phí khách mới {bad:Loại bỏ DA1} |
| Deputy (AU) {#v-deputy} | Ca gãy đa chi nhánh (`TH18`), **mù thông tin lịch hẹn khách** {caution:Mù Lịch hẹn} | Đổi ca nhân sự; không biết lịch hẹn của khách | Tích hợp sẵn động cơ `MA000005` (`TH15`) {good:Động cơ Award} | Không áp dụng (chỉ chấm công & phân ca) | Xuất sang Xero Payroll & Employment Hero | `61,25 AUD` (7x`$8,75` Core) |
| Employment Hero (AU) {#v-eh} | Duyệt nghỉ & phân ca; mù lịch hẹn spa | Chỉ duyệt đơn xin nghỉ phép | Chuẩn `MA000005` (`TH21`), `Super 12%`, Payday Super, `STP Phase 2` {good:Chuẩn Lương Úc} | Không áp dụng (hệ thống HR & Lương) | Đồng bộ bút toán Xero & MYOB | `100,00 AUD` (`$10/người`, tối thiểu `$100`) |
| Phương án C (Sunext) {#v-optionc} | Tự chèn `30-45p` tàu điện + xuất giờ công đi lại (`NT1, NT2`) {good:Giải quyết G1} | Hàng đợi `Need Attention` + khóa `Selected Therapist` (`BKG-007`) {good:Giải quyết G5} | Chia hoa hồng combo `= 100%` + khóa `Payroll Snapshot` (`NT9, NT10`) {good:Giải quyết G2} | Sổ cái sự kiện + GST theo loại voucher (`NT5, NT6, NT7`) {good:Giải quyết G4} | Bút toán ngày theo chi nhánh + hoàn tiền riêng (`NT12`) | `440-550 AUD` tổng vận hành |

## Tổng chi phí sở hữu 3 năm (TCO) giữa Phương án A, A+ và C khác nhau thế nào? {#mo-hinh-tco-3-nam}

Việc thay thế `Timely` (`325 AUD/tháng`) và `Deputy` (`61,25 AUD/tháng`) **không** giúp bỏ `Xero` (`70,91-130 AUD/tháng`) hay phần mềm lương Úc (`61,25-100 AUD/tháng`). Khi cộng AWS Sydney (`135-225 AUD/tháng`) và SMS Twilio (`135 AUD/tháng`), tổng chi phí mọi phương án đều quanh mức `440-550 AUD/tháng`. Trong khi đó, phí quẹt thẻ (`1.769 AUD/tháng`) gấp `3,5 lần` chi phí phần mềm.[](source:wp22)

```mermaid title="Chi phí Vận hành Hằng tháng bằng AUD chưa gồm GST (Kiểm chứng 26/09/2026)"
xychart-beta
  x-axis ["SaaS Spa", "Phần mềm Lương", "Kế toán Xero", "Cloud & SMS", "Tổng Vận hành", "Phí Thẻ (1.4%)"]
  y-axis "AUD / Tháng" 0 --> 1900
  bar "Phương án A1 (Timely+Deputy+Xero)" [325, 61, 130, 0, 516, 1769]
  bar "Phương án A2 (Timely+EH+Xero)" [325, 100, 71, 0, 496, 1769]
  bar "Phương án C (Xây Lõi+EH+Xero)" [0, 100, 71, 315, 486, 1769]
```

| Kịch bản Hòa vốn | Ngân sách Xây dựng (AUD) | Giờ Thủ công Cần tiết kiệm (`35 AUD/giờ`) | Giờ Thủ công Cần tiết kiệm (`50 AUD/giờ`) | Khấu hao Nền tảng Đa Spa (`36 tháng`) | Kết luận Đầu tư |
| --- | --- | --- | --- | --- | --- |
| Cầu nối Gọn nhẹ (`Phương án A+`) {#be-aplus} | `30.000 AUD` | `23,8 giờ/tháng` | `16,7 giờ/tháng` | `1 chuỗi` = `833 AUD/tháng` | Khả thi cho 1 chuỗi nếu tổng hợp lương `G2` mất `trên 2 ngày/kỳ` {good:Rủi ro Thấp} |
| Lõi Giai đoạn 1 (`Phương án C1`) {#be-c1} | `60.000 AUD` | `47,6 giờ/tháng` | `33,3 giờ/tháng` | `5 spa` = `333 AUD/tháng` mỗi spa | Hòa vốn khi tiết kiệm `~32 giờ/tháng` (`40k-58k AUD` công sức) {caution:Ngưỡng Hòa vốn} |
| Phạm vi Đầy đủ (`Phương án C2`) {#be-c2} | `100.000 AUD` | `79,4 giờ/tháng` | `55,6 giờ/tháng` | `10 spa` = `278 AUD/tháng` mỗi spa | Đòi hỏi thương mại hóa cho nhiều spa (`SX01`) |
| Nền tảng Thương mại (`Phương án C3`) {#be-c3} | `150.000 AUD` | `119,0 giờ/tháng` | `83,3 giờ/tháng` | `13 spa` = `320 AUD/tháng` (`20 spa` = `208 AUD/tháng`) | Rẻ hơn Timely (`325 AUD/tháng`) khi đạt từ `13 spa` trở lên {bad:Cần Quy mô} |

## Ranh giới System of Record bảo vệ việc tuân thủ pháp luật Úc như thế nào? {#ranh-gioi-system-of-record}

Theo 19 nguyên tắc (`NT1` đến `NT19`), Lõi Mini-ERP là `System of Record` duy nhất cho **sự kiện vận hành**. Hệ thống ủy thác việc tính thuế và phụ cấp lương cho các phần mềm được chứng nhận tại Úc.[](source:wp22)

@sequence id=sor-boundary-arch title="Ranh giới System of Record & Tuân thủ Pháp lý Úc (NT1 đến NT19)"
| actor web | 4 Cổng Vai trò (RBAC) | frontend |
| actor core | Lõi Mini-ERP Spa Úc | backend |
| actor db | Sổ Cái & Hồ sơ Consent | database |
| actor payroll | Employment Hero | external |
| actor xero | Xero / MYOB GL | external |
| phase | 1. System of Record Vận hành (Lõi Mini-ERP) |
| web | core | Kiểm tra RBAC & Phạm vi Chi nhánh (NT14 / SEC-008) |
| core | db | Giao dịch Nguyên tử 6 Bước & Dòng GST (NT6-NT8) | emphasis |
| db | web | Hóa đơn & Số dư Sự kiện Trả trước (NT7) | return |
| phase | 2. Động cơ Pháp lý Chuẩn Úc (Đồng bộ Outbox) |
| core | xero | Bút toán Tổng hợp Ngày sau Chốt ca theo Chi nhánh (NT12) |
| core | payroll | Tệp Payroll Input Snapshot Đã khóa 2 Tuần (NT10) | security |

@steps id=sor-boundary-steps
| step-rbac | 4 Cổng Vai trò (RBAC) | Thực thi phân quyền máy chủ (`NT14`) cho `Chủ Spa`, `Quản lý`, `Lễ tân`, `Kỹ thuật viên` và `Khách hàng`. Lễ tân chỉ thấy cảnh báo vận hành; bệnh án chi tiết bị che hoàn toàn (`SEC-008`). | sor-boundary-arch | web |
| step-core | Lõi Mini-ERP Spa Úc | Chạy động cơ kiểm tra lịch hợp nhất (`NT1`) và tự chèn `30-45p` tàu điện (`NT2`). Đồng thời kiểm tra chứng chỉ hành nghề, độ tuổi `>= 18` (`NT19`) và chia hoa hồng (`NT9`). | sor-boundary-arch | core |
| step-db | Sổ Cái & Hồ sơ Consent | Lưu số dư trả trước theo sự kiện (`NT7`), bút toán bất biến (`NT8`) và hồ sơ đồng ý `7 năm` (`Privacy Act 1988`). Tách riêng 5 dòng tiền thu, doanh thu, nợ, GST và tip (`NT6`). | sor-boundary-arch | db |
| step-payroll | Employment Hero | Nhận tệp `Payroll Input Snapshot` đã khóa mỗi 2 tuần (`Giờ công`, `Giờ đi lại`, `Hoa hồng`, `Tip giữ hộ`). Phần mềm tự tính phụ cấp `MA000005`, `Super 12%` (`Payday Super`) và nộp `STP Phase 2` (`NT10`). | sor-boundary-arch | payroll |
| step-xero | Xero / MYOB GL | Nhận bút toán tổng hợp ngày sau khi chốt ca theo từng chi nhánh (`Xero Tracking Categories`). Hệ thống cộng tổng GST từng dòng và đẩy riêng bút toán hoàn tiền (`NT12`). | sor-boundary-arch | xero |

## Cần xác minh những gì trước khi ký hợp đồng triển khai Giai đoạn 1? {#danh-sach-kiem-tra-wp}

Whitepaper chỉ ra 46 câu hỏi mở (`CT01-18, KT01-09, TL01-07, SX01-12`) và 4 điểm lệch trong báo giá `QT-2026-SPA-01` (hết hạn `16/10/2026`). Hoàn tất danh sách kiểm tra dưới đây trước khi khóa hợp đồng.[](source:wp22)

- [ ] **Giải quyết `Quyết định Q0` (`SX01 & CT09`):** Xác nhận xây riêng cho `The Heaven Lounge` hay mở rộng `15-20 spa` (`NFR-010`). Chốt trần ngân sách tháng của Chị Thủy.
- [ ] **Thực hiện dùng thử 14 ngày (`Q4 / CT15`):** Chạy thử `Timely` hoặc `Kitomba` trên các luồng `K3, K4, K9` để đo số giờ thủ công mỗi kỳ lương.
- [ ] **Xác nhận Kế toán & Lương (`UNK-01, UNK-02`):** Kiểm tra kế toán đang dùng `Xero` hay `MYOB`, và lương đang chạy trên `Employment Hero` hay `KeyPay`.
- [ ] **Xác nhận máy EFTPOS (`UNK-03`):** Kiểm tra máy quẹt thẻ tại 2 chi nhánh là `Tyro`, `Square`, `Stripe` hay `Zeller` để chọn cách tích hợp.
- [ ] **Khóa quy tắc hoa hồng và tip (`UNK-04 & KT05`):** Chốt hoa hồng tính trên **giá gốc** hay **giá sau giảm tiền mặt**, và cách khấu trừ `PAYG` cho tip thẻ.
- [ ] **Hiệu chỉnh báo giá `QT-2026-SPA-01` (`Q3`):** Đưa cổng đặt lịch (`A1-A3`) và `SMS/Email` vào Giai đoạn 1. Loại bỏ việc tự lập trình bộ máy tính lương Award.

