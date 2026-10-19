# Toàn Bộ 7 Luồng Vận Hành Thực Tế Của The Heaven Lounge (Spa Úc) Hoạt Động Như Thế Nào? {#tong-quan-7-luong-spa-uc}

Hai chi nhánh **Brunswick (`3056`)** và **North Melbourne (`3051`)** vận hành qua **7 luồng nghiệp vụ khép kín** từ lúc mở ca sáng đến kỳ chốt lương 2 tuần.[](source:ba10)

- **Luồng 1 & 2 (Xếp ca & Đặt lịch):** Dùng 1 hồ sơ KTV duy nhất, kiểm tra **5 điều kiện** (`SKL-003`), chèn **30–45p đi tram có lương** (`NT2`) và khóa `TSTZRANGE`.[](source:ba10)
- **Luồng 3 (KTV nghỉ đột xuất):** Không xóa lịch khách; đẩy vào hàng đợi `Need Attention` (`Màn B2`) và rẽ nhánh theo `Selected Therapist` (`BKG-007`) hoặc `Auto-Assigned` (`BKG-008`).[](source:prd10)
- **Luồng 4 & 5 (Đón khách & Tại giường):** Lễ tân chỉ thấy cờ an toàn (`SEC-008`), chặn vào phòng nếu thiếu chữ ký `Consent` (`TRT-002`), KTV bấm 3 chạm ghi `SOAP` và trừ `BOM`.[](source:prd10)
- **Luồng 6 & 7 (Thu ngân, SLA & Lương):** Tách 5 dòng tiền, GST kép (`NT6–NT8`), xử lý 1–2 sao trong **1 giờ** và khóa `Payroll Snapshot`.[](source:wp22)

@answer scope="7 luồng vận hành khép kín · 4 vai trò (Khách, Lễ tân, KTV, Chủ Spa) · 17 màn hình MVP"
@source wp22 title="Báo cáo Chiến lược & Kiến trúc Mini-ERP Spa Úc v4.0" url=https://theheavenlounge.com/
@source ba10 title="Phân tích Nghiệp vụ & Bản vẽ Sản phẩm Spa Úc" url=https://theheavenlounge.com/
@source prd10 title="Đặc tả Yêu cầu Sản phẩm MVP PRD (17 Màn hình)" url=https://theheavenlounge.com/

## Bức tranh toàn cảnh: 4 Vai trò phối hợp qua 7 Luồng trong ngày như thế nào? {#buc-tranh-4-vai-tro}

Hệ thống chia 17 màn hình vào 4 cổng vai trò để mỗi người chỉ thao tác đúng việc tại quầy hoặc tại giường.[](source:prd10)

| Thứ tự Luồng | Tên Luồng Vận Hành | Thời điểm diễn ra | Vai trò & Màn hình tham gia | Chốt chặn / Quy tắc bảo vệ cốt lõi |
| --- | --- | --- | --- | --- |
| **Luồng 1** {#wf-01} | **Xếp ca & Điều động liên chi nhánh** | Trước giờ mở cửa | Chủ Spa (`D1`), Lễ tân (`B1`), KTV (`C1`) | 1 hồ sơ KTV duy nhất (`STF-001`) + Tự chèn `30–45p` đi tram có lương (`NT2`) |
| **Luồng 2** {#wf-02} | **Đặt lịch Online / Walk-in & Cổng 5 điều kiện** | Khi khách đặt chỗ | Khách (`A1–A2`), Lễ tân (`B1`) | Giá kép Cash/Card (`PAY-001`) + Kiểm tra 5 điều kiện (`SKL-003`) + Khóa `TSTZRANGE` |
| **Luồng 3** {#wf-03} | **Cấp cứu lịch hẹn khi KTV nghỉ đột xuất** | Khi KTV báo ốm/nghỉ | KTV (`C4`), Lễ tân & Quản lý (`B2`) | Đẩy vào `Need Attention`; cấm đổi ngầm nếu khách chọn `Selected Therapist` (`BKG-007`) |
| **Luồng 4** {#wf-04} | **Check-in, Cờ an toàn & Chốt chặn Consent** | Khi khách tới quầy | Lễ tân (`B3, B4`), Khách ký iPad | Ẩn bệnh án khỏi Lễ tân (`SEC-008`) + Chặn vào ca nếu chưa ký `Consent` (`TRT-002`) |
| **Luồng 5** {#wf-05} | **Phục vụ tại giường, Ghi SOAP & Trừ kho BOM** | Trong phòng điều trị | KTV trên PWA VI/EN/TH (`C1, C2`) | Ẩn SĐT khách + Đồng hồ 3 chạm + Thẻ chọn nhanh `SOAP` + Trừ định mức vật tư `BOM` |
| **Luồng 6** {#wf-06} | **Thu ngân tách bill & Giao dịch nguyên tử 6 bước** | Khi khách thanh toán | Lễ tân POS (`B5`), Ví KTV (`C3`) | Tách Tiền thu / Doanh thu / Nợ gói / GST kép (`NT6`) / Hoa hồng `100%` / Tip giữ hộ |
| **Luồng 7** {#wf-07} | **SLA sự cố 1–2 sao, Kết ca & Chốt lương 2 tuần** | Sau ca & Mỗi 2 tuần | Quản lý (`D6`), Chủ Spa (`D3, D4`) | Điều tra 1–2 sao trong `1 giờ` (`REV-003`) + Bút toán Xero ngày + Khóa `Payroll Snapshot` |

## Luồng 1, 2 & 3: Đặt lịch liên chi nhánh và xử lý KTV nghỉ đột xuất ra sao? {#luong-1-2-3-dat-lich-nghi-dot-xuat}

Khi khách đặt lịch tại Brunswick hoặc North Melbourne, hệ thống kiểm tra 5 điều kiện hành nghề và tự chèn đệm đi tàu điện.[](source:ba10)

@workflow id=wf-dat-lich-cap-cuu title="Luồng 1–3: Kiểm Tra 5 Điều Kiện, Chèn Giờ Tram & Cấp Cứu Lịch Khi KTV Nghỉ"
| n1 | Đủ 5 điều kiện? | backend |
| n2 | Chèn 45p Tram | backend |
| n3 | Khóa TSTZRANGE | database |
| n4 | Hỏi Ý Kiến Khách | security |
| n1 > n2 | hợp lệ | emphasis |
| n2 > n3 | giữ chỗ | emphasis |
| n1 > n4 | nghỉ ốm | security |

@steps id=steps-luong-1-2-3
| s1 | Đủ 5 điều kiện? | Kiểm tra 5 điều kiện (`SKL-003`): Kỹ năng mở, Đã đào tạo, Chứng chỉ `HLTINF005/Laser` còn hạn, Có ca trực và đủ 18 tuổi nếu phun xăm. | wf-dat-lich-cap-cuu | n1 |
| s2 | Chèn 45p Tram | Nếu KTV vừa làm ca trước ở chi nhánh kia, hệ thống tự khóa `30–45 phút` đi tram. Khoảng thời gian này được tính là giờ di chuyển có lương (`MA000005 khoản 20.9`). | wf-dat-lich-cap-cuu | n2 |
| s3 | Khóa TSTZRANGE | Hệ thống khóa cứng khung giờ trong PostgreSQL (`0%` trùng lịch) và gửi SMS kèm link ký trước phiếu cam kết y khoa (`Màn A2`). | wf-dat-lich-cap-cuu | n3 |
| s4 | Hỏi Ý Kiến Khách | Khi KTV nghỉ đột xuất, lịch chuyển vào `Need Attention` (`Màn B2`). Nếu khách chọn `Selected Therapist`, hệ thống bắt buộc gửi SMS xin ý kiến khách (`BKG-007`). | wf-dat-lich-cap-cuu | n4 |

## Luồng 4 & 5: Đón khách tại quầy, ký cam kết y khoa và phục vụ tại giường thế nào? {#luong-4-5-check-in-tai-giuong}

Luồng đón khách và điều trị được thiết kế để bảo vệ an toàn y khoa cho khách và bảo mật dữ liệu cho chủ spa.[](source:prd10)

@workflow id=wf-checkin-bedside title="Luồng 4–5: Check-In, Chốt Chặn Consent (TRT-002) & 3 Chạm Tại Giường"
| c1 | Check-In & Cờ | frontend |
| c2 | Cổng Ký Consent | security |
| c3 | 3 Chạm & SOAP | backend |
| c4 | Trừ Kho BOM | database |
| c1 > c2 | kiểm ký | security |
| c2 > c3 | vào phòng | emphasis |
| c2 > c4 | trừ vật tư | emphasis |

@steps id=steps-luong-4-5
| sc1 | Check-In & Cờ | Lễ tân mở `Màn B3` và chỉ thấy **cờ cảnh báo vận hành** (dị ứng, thai kỳ) để đón khách. Toàn bộ ghi chú bệnh án `SOAP` được ẩn khỏi Lễ tân (`SEC-008`). | wf-checkin-bedside | c1 |
| sc2 | Cổng Ký Consent | Với dịch vụ Laser/IPL, xâm lấn hoặc phun xăm, hệ thống chặn cứng nút `In Service` nếu thiếu chữ ký điện tử (`TRT-002`). Khách chưa ký sẽ ký ngay trên iPad tại quầy (`Màn B4`). | wf-checkin-bedside | c2 |
| sc4 | 3 Chạm & SOAP | KTV mở `Màn C2` (giao diện tối, đa ngữ Việt/Anh/Thái, ẩn SĐT khách). KTV bấm đếm giờ và chạm thẻ nhanh để lưu bệnh án mã hóa `SOAP`. | wf-checkin-bedside | c3 |
| sc5 | Trừ Kho BOM | Khi KTV bấm hoàn tất tại giường, hệ thống tự động trừ định mức vật tư tiêu hao `BOM` khỏi kho chi nhánh (`Màn D5`). Ca hẹn đồng thời chuyển sang chờ thu ngân. | wf-checkin-bedside | c4 |

## Luồng 6 & 7: Thu ngân tách bill 6 bước, xử lý đánh giá 1–2 sao và khóa lương 2 tuần ra sao? {#luong-6-7-thu-ngan-sla-luong}

Khi khách thanh toán tại `Màn B5`, hệ thống thực thi giao dịch nguyên tử 6 bước, gửi khảo sát và chuẩn bị dữ liệu kế toán — bảng lương.[](source:wp22)

@sequence id=seq-luong-6-7 title="Luồng 6–7: Tất Toán 6 Bước, SLA Sự Cố 1 Giờ & Khóa Sổ Lương 2 Tuần"
| actor pos | Lễ Tân & Quản Lý | frontend |
| actor erp | Lõi Mini-ERP | backend |
| actor ledger | Sổ Cái & Ví KTV | database |
| actor guest | Khách Hàng (SMS) | external |
| actor au | Xero & Payroll Úc | external |
| phase | 1. Luồng 6 — Thu Ngân Tách Bill & Giao Dịch Nguyên Tử 6 Bước |
| pos | erp | Thanh toán gộp (Cash / Card + Gói + Tip) |
| erp | ledger | Ghi 6 bước: Tiền, Doanh thu, Nợ gói, GST kép, Hoa hồng | emphasis |
| phase | 2. Luồng 7A — Khảo Sát 1–5 Sao & SLA 1 Giờ Khi Có Phản Hồi 1–2 Sao |
| erp | guest | Gửi SMS khảo sát 1–5 sao (REV-001) |
| guest | erp | Chấm 1–2 sao -> Mở phiếu sự cố SLA 1 giờ (Màn D6) | return |
| phase | 3. Luồng 7B — Kết Ca Cuối Ngày & Khóa Payroll Snapshot 2 Tuần |
| pos | erp | Chốt két cuối ca & Khóa kỳ lương 2 tuần |
| erp | au | Đẩy bút toán Xero ngày & Xuất Payroll Snapshot | security |

@steps id=steps-luong-6-7
| s6a | Sổ Cái & Ví KTV | Ghi 6 nghiệp vụ trong 1 giao dịch CSDL: tách `Monetary Voucher` (tính GST khi dùng) khỏi `Service Package` (tính GST khi bán). Hệ thống đối soát liên chi nhánh và cộng hoa hồng `100%` vào ví KTV (`Màn C3`). | seq-luong-6-7 | ledger |
| s6b | Khách Hàng (SMS) | Mọi đánh giá 1–2 sao đều tự động mở phiếu sự cố tại `Màn D6`. Quản lý bắt buộc điều tra trong **1 giờ**, liên hệ khách **trong ngày** và ghi phương án đền bù trước khi đóng phiếu (`REV-002..004`). | seq-luong-6-7 | guest |
| s6c | Xero & Payroll Úc | Hằng ngày đẩy bút toán tổng hợp theo chi nhánh sang `Xero/MYOB`. Mỗi 2 tuần khóa `Payroll Input Snapshot` xuất sang `Employment Hero` để tính phụ cấp `MA000005`, `12% Payday Super` và nộp `STP Phase 2`. | seq-luong-6-7 | au |
