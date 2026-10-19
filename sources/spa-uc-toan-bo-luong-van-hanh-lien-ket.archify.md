# The Heaven Lounge (Spa Úc) — Toàn Bộ 7 Luồng Vận Hành & Kiến Trúc Liên Kết Trên 1 Trang Duy Nhất {#spa-uc-mot-trang-lien-ket}

@meta quality_profile=showcase style=editorial

Hai chi nhánh **Brunswick (`3056`)** và **North Melbourne (`3051`)** vận hành như một **dây chuyền 7 luồng nghiệp vụ khép kín**.[](source:wp22) Đầu ra của mỗi luồng là **chốt chặn mở khóa bắt buộc (`Hard Gate`)** cho luồng kế tiếp.[](source:wp22)

- **Mạch Điều phối ([Luồng 1 -> 2 -> 3](#luong-1-2-3-xep-ca-dat-lich-cap-cuu)):** Ca trực KTV và **khóa 45p đi tram (`NT2`)** ở Luồng 1 nạp thẳng vào **Cổng 5 điều kiện (`SKL-003`)** ở Luồng 2. Cờ `Selected / Auto-Assigned` từ Luồng 2 quyết định nhánh xử lý khi KTV nghỉ đột xuất ở Luồng 3.[](source:ba10)
- **Mạch Lâm sàng ([Luồng 4 -> 5](#luong-4-5-checkin-consent-tai-giuong)):** Lịch `Confirmed` khi Check-in tại Luồng 4 sẽ ẩn bệnh án khỏi Lễ tân (`SEC-008`) và chặn cửa phòng bằng **chữ ký `Consent` (`TRT-002`)**. Khi mở khóa, màn hình **3 chạm tại giường** ở Luồng 5 ghi `SOAP`, trừ kho `BOM` và chuyển thẳng ra quầy POS.[](source:prd10)
- **Mạch Tài chính ([Luồng 6 -> 7 -> 1](#luong-6-thu-ngan-6-buoc-gst-hoa-hong)):** Thu ngân Luồng 6 tách **Tiền thu, Doanh thu, Nợ gói, GST kép, Hoa hồng `100%` và Tip**. Trạng thái `Completed` kích hoạt **SLA `1h`**, sổ **Xero** và lương **`MA000005`** ở Luồng 7.[](source:wp22)

@answer scope="1 Trang Duy Nhất · 7 Luồng Liên Kết Khép Kín · 11 Sơ Đồ Tương Tác · 17 Màn Hình MVP · 8 Bảng CSDL"
@source wp22 title="Whitepaper Kiến trúc Mini-ERP Spa Úc v4.0 (Brunswick & North Melbourne)" url=https://github.com/tipovuhoa26-sudo/spa-uc-archify
@source ba10 title="Phân tích Nghiệp vụ & Bản vẽ Sản phẩm Spa Úc (Product Blueprint)" url=https://github.com/tipovuhoa26-sudo/spa-uc-archify
@source prd10 title="Đặc tả Yêu cầu Sản phẩm MVP PRD (17 Màn hình & 42 Quy tắc)" url=https://github.com/tipovuhoa26-sudo/spa-uc-archify
@source fw2025 title="Fair Work Commission — Hair and Beauty Industry Award [MA000005] & ATO GST" url=https://www.fwc.gov.au/

## 1. Bản Đồ Liên Kết Tổng Thể: 7 Luồng Vận Hành Nối Tiếp Nhau Như Thế Nào? {#ban-do-lien-ket-7-luong span=12}

Sơ đồ dưới đây trình bày **mạch liên kết nhân quả từ đầu đến cuối** giữa 7 luồng nghiệp vụ tại **The Heaven Lounge**.[](source:wp22) Hãy bấm nút **▶ Duyệt từng bước** hoặc bấm vào từng nút để xem dữ liệu bàn giao giữa các luồng.[](source:ba10)

@workflow id=wf-master-chain source=16-spa-uc-master-end-to-end-workflow.json

@steps id=steps-master-chain
| m1 | [Mắt xích 1 -> 2: Xếp ca liên chi nhánh & Khóa 45p đi tàu điện nạp vào Cổng đặt lịch](node:wf-master-chain.w1-roster) | Từ `Màn D1`, lịch ca trực và khóa tàu điện `30–45p có lương` nạp thẳng vào bộ tính giờ trống của Luồng 2. [Xem chi tiết Luồng 1–3](#luong-1-2-3-xep-ca-dat-lich-cap-cuu). |
| m2 | [Mắt xích 2 -> Cổng 5 Điều kiện: Kiểm tra chứng chỉ, kỹ năng và khóa giữ chỗ TSTZRANGE](node:wf-master-chain.w2-gate) | Khách đặt lịch (`Màn A1–A2`) hoặc Lễ tân (`Màn B1`) đều đi qua cổng 5 điều kiện (`SKL-003`). Hệ thống khóa cứng khung giờ bằng `TSTZRANGE` trong PostgreSQL. |
| m3 | [Mắt xích 2 -> 3 -> 4: Nhánh cấp cứu lịch hẹn khi KTV báo nghỉ đột xuất](node:wf-master-chain.w3-leave) | Khi KTV báo ốm, các lịch `Confirmed` tự động nhảy vào hàng đợi `Need Attention` (`Màn B2`). Hệ thống kiểm tra cờ `Selected Therapist` để [hỏi ý kiến khách qua SMS](node:wf-master-chain.w3-customer) trước khi trả lịch về quầy. |
| m4 | [Mắt xích 2/3 -> 4 -> 5: Đón khách Check-In, Chốt chặn Consent (TRT-002) & Phục vụ tại giường](node:wf-master-chain.w4-5-care) | Tại quầy (`Màn B3–B4`), hệ thống ẩn bệnh án khỏi Lễ tân (`SEC-008`) và chặn vào phòng nếu thiếu chữ ký `Consent` (`TRT-002`). Khi đã ký, KTV mở màn hình 3 chạm (`Màn C2`) để ghi `SOAP` và trừ kho `BOM`. [Xem Luồng 4–5](#luong-4-5-checkin-consent-tai-giuong). |
| m5 | [Mắt xích 5 -> 6: Chốt kết quả ca làm & Thực thi giao dịch thu ngân nguyên tử 6 bước](node:wf-master-chain.w6-checkout) | Danh sách KTV thực tế từ phòng điều trị truyền thẳng ra quầy POS (`Màn B5`). Thu ngân tách bill `Split-Tender`, áp giá Cash/Card, tách `GST kép`, đối soát liên chi nhánh và chia hoa hồng `100%`. [Xem Luồng 6](#luong-6-thu-ngan-6-buoc-gst-hoa-hong). |
| m6 | [Mắt xích 6 -> 7 -> 1: Kích hoạt SLA sự cố 1 giờ, Kết sổ Xero & Khóa bảng lương Úc 2 tuần](node:wf-master-chain.w7-payroll) | Sự kiện `Completed` gửi SMS khảo sát `1–5 sao` và bật [cảnh báo SLA 1 giờ](node:wf-master-chain.w7-sla) nếu khách chấm 1–2 sao. Cuối kỳ, hệ thống đẩy bút toán `Xero` và khóa `Payroll Snapshot`. [Xem Luồng 7](#luong-7-hau-mai-sla-xero-payroll). |

| Móc Xích Chuyển Giao | Sự Kiện Kích Hoạt (Trigger) | Dữ Liệu Bàn Giao Sang Luồng Kế Tiếp (Payload) | Chốt Chặn Kiểm Soát Bắt Buộc (Hard Gate) | Liên Kết Nhảy Nhanh |
| --- | --- | --- | --- | --- |
| **Luồng 1 -> Luồng 2** {#h-1-2} | Xuất bản ca trực tuần (`Màn D1`) {good:Roster Active} | `staff_id`, `branch_id`, khung giờ trực, khối khóa `Paid Travel Block 30–45m` | Chặn tuyệt đối mọi khung giờ đặt lịch rơi vào thời gian KTV đang di chuyển tàu điện | [Mở Sơ Đồ Luồng 1–3](node:wf-booking-dispatch.travel-block) · [Tới Mục 2](#luong-1-2-3-xep-ca-dat-lich-cap-cuu) |
| **Luồng 2 -> Luồng 3** {#h-2-3} | KTV báo nghỉ ốm/đột xuất (`Màn C4`) {caution:Need Attention} | Danh sách lịch `Confirmed` bị ảnh hưởng + Cờ `Selected Therapist` hoặc `Auto-Assigned` | Nếu cờ là `Selected Therapist` (`BKG-007`), CẤM đổi ngầm mà bắt buộc gửi SMS xin phép khách | [Mở Cổng Đích Danh](node:wf-booking-dispatch.check-selected) · [Tới Mục 2](#luong-1-2-3-xep-ca-dat-lich-cap-cuu) |
| **Luồng 2/3 -> Luồng 4** {#h-3-4} | Khách có mặt tại quầy Lễ tân (`Màn B3`) {good:Checked-In} | `appointment_id (Confirmed)`, cờ an toàn y khoa, cờ dịch vụ `requires_consent` | Kích hoạt `SEC-008` che bệnh án `SOAP` khỏi Lễ tân và bật cổng kiểm tra chữ ký `TRT-002` | [Mở Trạng Thái Check-In](node:lc-appointment.checked-in) · [Tới Mục 3](#luong-4-5-checkin-consent-tai-giuong) |
| **Luồng 4 -> Luồng 5** {#h-4-5} | Khách ký xong `Digital Consent` (`Màn B4`) {good:In Service} | `consent_hash`, cảnh báo dị ứng/thai kỳ, thời lượng đếm ngược từng chặng | Chặn cứng nút `Start Service` nếu dịch vụ yêu cầu cam kết mà chưa có chữ ký hợp lệ | [Mở Sơ Đồ Tại Giường](node:wf-checkin-bedside.c3) · [Tới Mục 3](#luong-4-5-checkin-consent-tai-giuong) |
| **Luồng 5 -> Luồng 6** {#h-5-6} | KTV bấm `Complete` & Lưu `SOAP` (`Màn C2`) {good:Ready for POS} | Danh sách KTV thực hiện thực tế, tỷ lệ thời lượng Combo, phiếu trừ kho `BOM` | Bắt buộc lưu thẻ `SOAP` mã hóa và chốt KTV thực tế trước khi mở thanh toán POS | [Mở Trình Tự Thu Ngân](node:seq-checkout.pos) · [Tới Mục 4](#luong-6-thu-ngan-6-buoc-gst-hoa-hong) |
| **Luồng 6 -> Luồng 7** {#h-6-7} | Hoàn tất thu ngân 6 bước (`Màn B5`) {good:Completed} | Bút toán Doanh thu, Nợ gói, GST kép, Bù trừ liên chi nhánh, Hoa hồng `100%`, Tip giữ hộ | Giao dịch nguyên tử CSDL: Tổng hoa hồng phải bằng `100.00%` và Tip tách khỏi doanh thu | [Mở Luồng Sổ Cái & Lương](node:df-finance-gst.tx-ledger) · [Tới Mục 5](#luong-7-hau-mai-sla-xero-payroll) |
| **Luồng 7 -> Luồng 1** {#h-7-1} | Kết ca ngày & Khóa kỳ lương 14 ngày (`Màn D3`) {good:Loop Closed} | Điểm chất lượng `1–5 sao`, giờ công thực tế, phụ cấp đi tram `45m`, chi phí lương/doanh thu | Đóng băng `Payroll Input Snapshot` bất biến để nộp `STP Phase 2` và tối ưu ca trực tuần mới | [Mở ERD 8 Bảng CSDL](node:erd-core-domain.payroll_snapshots) · [Tới Mục 6](#kien-truc-du-lieu-va-17-man-hinh-lien-ket) |

## 2. Luồng 1, 2 & 3 Liên Kết: Xếp Ca Tàu Điện (Tram 45p) -> Đặt Lịch 5 Điều Kiện (`TSTZRANGE`) -> Cấp Cứu Nghỉ Đột Xuất {#luong-1-2-3-xep-ca-dat-lich-cap-cuu span=12}

Ba luồng đầu tiên gắn kết trực tiếp trên một trục điều phối thời gian thực.[](source:ba10) **Luồng 1** khóa giờ đi tàu điện giữa hai cơ sở, còn **Luồng 2** lọc 5 điều kiện và khóa chỗ trong `285ms`.[](source:ba10) Khi KTV báo ốm, **Luồng 3** tự động hứng các lịch bị ảnh hưởng để xử lý ngay.[](source:ba10)

@workflow id=wf-booking-dispatch source=06-spa-uc-booking-dispatch-workflow.json

@waterfall id=wf-latency source=14-spa-uc-booking-availability-waterfall.json

@steps id=steps-luong-1-2-3
| s1 | [Luồng 1 (Đầu vào): Xếp ca 1 hồ sơ KTV duy nhất & Chèn 30–45p đi tàu điện có lương](node:wf-booking-dispatch.travel-block) | Khi KTV làm ca sáng ở `Brunswick` và ca chiều ở `North Melbourne`, hệ thống tự chèn `30–45p` đi tram (`NT2`). Giờ đi lại này được tính lương theo `MA000005 khoản 20.9` ([xem span 35ms](node:wf-latency.span-travel)). |
| s2 | [Móc xích Luồng 1 -> Luồng 2: Khách chọn chi nhánh, Giá kép Cash/Card & Chế độ chọn KTV](node:wf-booking-dispatch.pick-mode) | Khách đặt lịch (`Màn A1`) xem bảng giá kép Tiền mặt / Thẻ chuẩn (`PAY-001`). Khách chọn `Selected Therapist` (đích danh KTV) hoặc `Auto-Assigned` (tự phân bổ). |
| s3 | [Luồng 2: Cổng kiểm tra đồng thời 5 điều kiện hành nghề (SKL-003)](node:wf-booking-dispatch.check-elig) | Hệ thống kiểm tra 5 điều kiện từ Luồng 1: Kỹ năng mở, Đã đào tạo, Chứng chỉ `HLTINF005/Laser` còn hạn và Đúng ca trực. Khách cũng phải đủ `18 tuổi` nếu làm dịch vụ xâm lấn ([xem span 70ms](node:wf-latency.span-elig)). |
| s4 | [Luồng 2: Khóa giữ chỗ nguyên tử bằng PostgreSQL TSTZRANGE trong 80ms](node:wf-booking-dispatch.lock-slot) | CSDL khóa trực tiếp khoảng giờ `[start_time, end_time + buffer)` bằng chỉ mục `GiST` ([xem span 80ms](node:wf-latency.span-dblock)). Sau đó API trả về xác nhận và link ký cam kết ([xem span phản hồi](node:wf-latency.span-resp)). |
| s5 | [Móc xích Luồng 2 -> Luồng 3: KTV báo nghỉ đột xuất -> Đẩy lịch vào hàng đợi Need Attention](node:wf-booking-dispatch.leave-alert) | Khi KTV báo ốm trên `Màn C4`, hệ thống quét toàn bộ lịch `Confirmed` bị ảnh hưởng. Các lịch này chuyển ngay vào hàng đợi `Need Attention` (`Màn B2`) mà không bị xóa. |
| s6 | [Luồng 3: Rẽ nhánh điều phối theo cờ Selected Therapist vs Auto-Assigned](node:wf-booking-dispatch.check-selected) | Nếu cờ là `Auto-Assigned`, lễ tân đổi KTV tương đương bằng 1 chạm ([gán KTV thay thế](node:wf-booking-dispatch.reassign-ktv)). Nếu là `Selected Therapist` (`BKG-007`), hệ thống cấm đổi ngầm và gửi SMS xin phép khách. |
| s7 | [Móc xích Luồng 3 -> Luồng 4: Bàn giao lịch hẹn đã xác nhận sang Quầy Lễ tân Check-In](node:wf-booking-dispatch.consent-checkin) | Sau khi đặt lịch (Luồng 2) hoặc đổi KTV hợp lệ (Luồng 3), lịch giữ trạng thái `Confirmed`. Dữ liệu bàn giao thẳng tới quầy đón khách tại [Mục 3: Luồng 4 & 5](#luong-4-5-checkin-consent-tai-giuong). |

## 3. Luồng 4 & 5 Liên Kết: Check-In Lễ Tân (`SEC-008`) & Chốt Chặn Consent (`TRT-002`) -> Trị Liệu 3 Chạm Tại Giường, Ghi `SOAP` & Trừ Kho `BOM` {#luong-4-5-checkin-consent-tai-giuong span=12}

Nhận lịch `Confirmed` từ **[Luồng 2 và Luồng 3](#luong-1-2-3-xep-ca-dat-lich-cap-cuu)**, hai luồng tiếp theo kiểm soát vòng đời lịch hẹn từ quầy lễ tân vào tới giường điều trị.[](source:prd10) **Luồng 4** ẩn bệnh án (`SEC-008`) và chặn cửa phòng bằng chữ ký `Consent` (`TRT-002`). Khi cổng này mở, **Luồng 5** ghi nhận kết quả thực tế tại giường để chuyển sang thu ngân.[](source:prd10)

@lifecycle id=lc-appointment source=07-spa-uc-appointment-lifecycle.json

@workflow id=wf-checkin-bedside title="Luồng 4 & 5 Chi Tiết: Từ Cổng Check-In & Consent (TRT-002) Sang Tablet 3 Chạm Tại Giường"
| c1 | Check-In & Cờ | frontend |
| c2 | Cổng Ký Consent | security |
| c3 | 3 Chạm & SOAP | backend |
| c4 | Trừ Kho BOM | database |
| c1 > c2 | kiểm ký | security |
| c2 > c3 | vào phòng | emphasis |
| c3 > c4 | trừ vật tư | emphasis |

@steps id=steps-luong-4-5
| c-step1 | [Móc xích nhận từ Luồng 2 & 3: Chuyển trạng thái Confirmed sang Checked-In tại quầy](node:lc-appointment.checked-in) | Khách tới quầy (`Màn B3`) từ trạng thái [Đã xác nhận](node:lc-appointment.confirmed) (hoặc sau khi xử lý [Need Attention](node:lc-appointment.need-attention) và [Chờ khách chọn](node:lc-appointment.waiting-guest)). Lễ tân bấm chuyển lịch sang `Checked-In`. |
| c-step2 | [Luồng 4 (Bảo mật SEC-008): Ẩn bệnh án chi tiết khỏi Lễ tân, chỉ hiển thị cờ an toàn](node:wf-checkin-bedside.c1) | Theo luật quyền riêng tư Úc (`APPs`), màn hình Lễ tân ẩn toàn bộ bệnh án `SOAP` cũ. Lễ tân chỉ thấy cờ cảnh báo vận hành như dị ứng, thai kỳ hoặc huyết áp. |
| c-step3 | [Luồng 4 (Cổng chặn cứng TRT-002): Bắt buộc ký Digital Consent trước khi vào phòng](node:wf-checkin-bedside.c2) | Với dịch vụ Laser/IPL hoặc xâm lấn, hệ thống **khóa nút `In Service`** nếu thiếu chữ ký điện tử. Khách chưa ký online sẽ ký ngay trên iPad tại quầy (`Màn B4`). |
| c-step4 | [Móc xích Luồng 4 -> Luồng 5: Mở khóa trạng thái Đang phục vụ (In Service) tại giường](node:lc-appointment.in-service) | Khi chữ ký `Consent` đã lưu, lịch chuyển sang `In Service`. Hệ thống đẩy thông báo thời gian thực xuống máy tính bảng của KTV trong phòng (`Màn C2`). |
| c-step5 | [Luồng 5: Vận hành đồng hồ 3 chạm nền tối & Ghi nhận KTV thực tế cho gói Combo](node:wf-checkin-bedside.c3) | Trên màn hình tối đa ngữ (`Việt/Anh/Thái`) đã ẩn SĐT khách, KTV bấm `Bắt đầu -> Tạm dừng -> Hoàn tất`. Nếu làm Combo, hệ thống ghi nhận từng KTV thực tế. |
| c-step6 | [Móc xích Luồng 5 -> Luồng 6: Lưu thẻ nhanh SOAP, trừ kho BOM & Đẩy hóa đơn ra quầy POS](node:wf-checkin-bedside.c4) | KTV chạm thẻ nhanh `SOAP` mã hóa lưu 7 năm và hệ thống tự trừ kho vật tư `BOM`. Ca hẹn chuyển thẳng ra quầy POS tại [Mục 4: Luồng 6 Thu Ngân](#luong-6-thu-ngan-6-buoc-gst-hoa-hong). |

## 4. Luồng 6 Liên Kết: Giao Dịch Thu Ngân Nguyên Tử 6 Bước — Split-Tender, GST Kép (`ATO`), Bù Trừ Liên Chi Nhánh & Hoa Hồng `100%` {#luong-6-thu-ngan-6-buoc-gst-hoa-hong span=12}

Nhận **danh sách KTV thực tế từ [Luồng 5](#luong-4-5-checkin-consent-tai-giuong)** và **bảng Giá Kép từ [Luồng 2](#luong-1-2-3-xep-ca-dat-lich-cap-cuu)**, Luồng 6 tại `Màn B5` thực thi giao dịch tài chính cốt lõi.[](source:wp22) Cả **6 nghiệp vụ kế toán** được khóa trong **1 giao dịch CSDL nguyên tử (`Atomic Transaction`)** và chảy vào 5 luồng sổ cái bên dưới.[](source:wp22)

@sequence id=seq-checkout source=08-spa-uc-service-checkout-sequence.json

@dataflow id=df-finance-gst source=09-spa-uc-finance-gst-payroll-dataflow.json

@steps id=steps-luong-6
| p-step1 | [Bước 1 (Nhận từ Luồng 2 & Luồng 5): Thanh toán gộp Split-Tender & Áp giá Tiền mặt/Thẻ](node:seq-checkout.pos) | Lễ tân nhận ca làm từ Luồng 5 trên `Màn B5` ([Thu ngân POS](node:df-finance-gst.pos-checkout)). Khách có thể trả gộp `Tiền mặt + Thẻ EFTPOS + Gói trả trước` theo đúng giá ưu đãi tiền mặt hoặc giá thẻ chuẩn (`PAY-001`). |
| p-step2 | [Bước 2: Quẹt thẻ EFTPOS lũy đẳng (Idempotent) & Trừ công nợ gói trả trước theo sự kiện](node:seq-checkout.eftpos) | Hệ thống gọi [máy thẻ EFTPOS](node:seq-checkout.eftpos) kèm `idempotency_key` chống trừ tiền 2 lần. Đồng thời [Bộ xử lý trả trước](node:df-finance-gst.prepaid-engine) trừ số dư trên [CSDL Công nợ](node:df-finance-gst.liability-store) (`NT7`). |
| p-step3 | [Bước 3: Tự động bù trừ công nợ liên chi nhánh (Brunswick <-> North Melbourne)](node:df-finance-gst.branch-settlement) | Nếu gói mua ở `Brunswick` ([Bán gói/thẻ](node:df-finance-gst.prepaid-sale)) nhưng dùng ở `North Melbourne`, hệ thống tự sinh bút toán bù trừ nội bộ giữa 2 cơ sở (`NT8`). |
| p-step4 | [Bước 4: Phân loại và tách thuế GST kép chuẩn thông tư ATO (NT6)](node:df-finance-gst.gst-classifier) | [Bộ phân loại GST kép](node:df-finance-gst.gst-classifier) tách 2 chế độ thuế Úc. `Monetary Voucher` trích `1/11 GST` lúc dùng, còn `Service Package` đã thu GST lúc bán nên ghi `$0 GST` lúc dùng. |
| p-step5 | [Bước 5: Chia hoa hồng đa KTV bắt buộc tổng = 100.00% & Tách riêng tiền Tip giữ hộ](node:df-finance-gst.comm-privacy-gate) | Dựa trên KTV thực tế từ Luồng 5, [Cổng hoa hồng](node:df-finance-gst.comm-privacy-gate) chia tỷ lệ đảm bảo tổng đúng `100.00%` (`COM-002`). Khoản `Tip giữ hộ` được tách riêng khỏi doanh thu và GST. |
| p-step6 | [Bước 6 (Móc xích Luồng 6 -> Luồng 7): Ghi Sổ cái bất biến & Kích hoạt Hậu mãi, Xero và Bảng lương](node:df-finance-gst.tx-ledger) | Khi ghi xong vào [Sổ cái & Lương DB](node:seq-checkout.ledger), lịch khóa trạng thái [Completed](node:lc-appointment.completed). Sự kiện này kích hoạt ngay **3 quy trình hậu mãi và bảng lương** tại [Mục 5: Luồng 7](#luong-7-hau-mai-sla-xero-payroll). |

## 5. Luồng 7 Liên Kết & Khép Vòng Lặp: Khảo Sát `1–5 Sao` (SLA `1 Giờ`), Kết Sổ `Xero` & Khóa Bảng Lương Úc `STP Phase 2` {#luong-7-hau-mai-sla-xero-payroll span=12}

Luồng 7 nhận **sự kiện `Completed` từ [Luồng 6](#luong-6-thu-ngan-6-buoc-gst-hoa-hong)** để khép kín chu trình hậu mãi và lương Úc.[](source:wp22) Quy trình này cũng gom **giờ đi tram `45p` từ [Luồng 1](#luong-1-2-3-xep-ca-dat-lich-cap-cuu)** cùng **giờ làm từ [Luồng 5](#luong-4-5-checkin-consent-tai-giuong)**.[](source:prd10)

@timeline id=tl-incident-sla source=13-spa-uc-incident-sla-timeline.json

@class id=cls-engines source=11-spa-uc-eligibility-commission-class.json

@steps id=steps-luong-7
| l7-1 | [Liên kết chuỗi sự kiện trong ngày: Từ ca nghỉ ốm buổi sáng (07:15) đến di chuyển tàu điện (12:15)](node:tl-incident-sla.evt-tram-transit) | Sơ đồ nối liền lúc [KTV báo nghỉ ốm 07:15](node:tl-incident-sla.evt-sick-leave), [đẩy vào hàng đợi B2 07:16](node:tl-incident-sla.evt-need-attention) và [khách đồng ý đổi KTV 07:42](node:tl-incident-sla.evt-guest-consent) (Luồng 3). Tiếp đó là mốc [khóa 45p đi tram có lương 12:15](node:tl-incident-sla.evt-tram-transit) (Luồng 1). |
| l7-2 | [Nhánh 7A (14:05 — Ngay sau Luồng 6): Tất toán 6 bước & Gửi SMS khảo sát 1–5 sao công bằng](node:tl-incident-sla.evt-checkout-survey) | Ngay khi tất toán ở Luồng 6 lúc `14:05`, hệ thống gửi SMS khảo sát `1–5 sao` (`REV-001`). Quy trình tuân thủ luật `ACL`, tuyệt đối không chặn đánh giá thấp. |
| l7-3 | [Nhánh 7A (14:20 -> 15:05): Cảnh báo đánh giá 1–2 sao & Điều tra nội bộ trong SLA 1 giờ](node:tl-incident-sla.evt-1h-investigation) | Khi khách chấm 2 sao lúc `14:20` ([cảnh báo 2 sao](node:tl-incident-sla.evt-low-rating-alert)), hệ thống mở phiếu sự cố trên `Màn D6`. Quản lý phải đối chiếu `SOAP` (Luồng 5) và lịch sử đổi KTV (Luồng 3) trong **SLA 1 giờ** (`15:05`). |
| l7-4 | [Nhánh 7A (16:30 — Khôi phục trong ngày): Liên hệ khách, tặng voucher đền bù & Đóng phiếu sự cố](node:tl-incident-sla.evt-guest-resolution) | Quản lý gọi điện xin lỗi khách ngay trong ngày (`16:30`) và tặng voucher đền bù có ghi vết sổ cái. Phiếu chỉ được đóng khi đã nhập biên bản `resolution_note` (`REV-004`). |
| l7-5 | [Nhánh 7B & 7C: Đẩy bút toán Xero cuối ngày & Khóa Payroll Snapshot 2 tuần/lần](node:df-finance-gst.payroll-snapshot) | Cuối ngày, hệ thống đẩy bút toán theo chi nhánh sang [Xero / MYOB](node:df-finance-gst.xero-bas). Mỗi 2 tuần, hệ thống khóa [Payroll Input Snapshot](node:df-finance-gst.payroll-snapshot) xuất sang [Employment Hero](node:df-finance-gst.au-payroll) tính lương `MA000005` và quay về [Luồng 1](#luong-1-2-3-xep-ca-dat-lich-cap-cuu). |

## 6. Xương Sống Kiến Trúc & Dữ Liệu Liên Kết Cả 7 Luồng: 8 Bảng CSDL (`ERD`), 17 Màn Hình (`Tree`) & Kiến Trúc Đích (`Architecture`) {#kien-truc-du-lieu-va-17-man-hinh-lien-ket span=12}

Vì sao cả 7 luồng trên truyền trạng thái tức thời giữa **Brunswick** và **North Melbourne** mà không lệch số liệu như 3 file HTML `localStorage` cũ?[](source:wp22) Đó là nhờ **8 bảng CSDL cốt lõi (`ERD`)**, **cây 17 màn hình theo 4 cổng vai trò (`Tree`)** và **kiến trúc `Modular Monolith` trên PostgreSQL Sydney (`Architecture`)** dưới đây.[](source:prd10)

@erd id=erd-core-domain source=10-spa-uc-core-domain-erd.json

@tree id=tr-17-screens source=12-spa-uc-capability-module-tree.json

@architecture id=arch-system source=04-spa-uc-target-system-architecture.json

@steps id=steps-kien-truc-du-lieu
| k1 | [Liên kết qua Bảng staff_profiles & branches (Luồng 1 -> Luồng 2 -> Luồng 7)](node:erd-core-domain.staff_profiles) | Bảng [branches](node:erd-core-domain.branches) và [staff_profiles](node:erd-core-domain.staff_profiles) nối [Cổng D: Chủ Spa (D1-D2)](node:tr-17-screens.m-d1-d2) với [Động cơ Đặt lịch](node:arch-system.booking-engine) để đồng bộ ca trực và khóa đi tram. |
| k2 | [Liên kết qua Bảng appointments & customers (Luồng 2 -> Luồng 3 -> Luồng 4)](node:erd-core-domain.appointments) | Bảng [appointments](node:erd-core-domain.appointments) trên [PostgreSQL DB](node:arch-system.postgres-core) nối liền [Màn A1-A2 Đặt lịch](node:tr-17-screens.m-a1-a2) với [Màn B1-B2 Điều phối & Need Attention](node:tr-17-screens.m-b1-b2). |
| k3 | [Liên kết qua Bảng clinical_records & Cổng RBAC (Luồng 4 -> Luồng 5)](node:erd-core-domain.clinical_records) | [Cổng RBAC](node:arch-system.rbac-gateway) và [Cổng Consent & SOAP](node:arch-system.clinical-consent) bảo vệ bảng [clinical_records](node:erd-core-domain.clinical_records) tại [Kho Bệnh án 7 Năm](node:arch-system.consent-vault). Cấu trúc này nối bước ký cam kết ở [Màn B3-B4](node:tr-17-screens.m-b3-b4) với [Màn C1-C2 Tại giường](node:tr-17-screens.m-c1-c2). |
| k4 | [Liên kết qua Bảng ledger_entries & payroll_snapshots (Luồng 6 -> Luồng 7 -> Xero/Payroll)](node:erd-core-domain.ledger_entries) | Khi [Màn B5 Thu ngân POS](node:tr-17-screens.m-b5) hoàn tất 6 bước qua [Thu ngân & Lương](node:arch-system.pos-payroll-engine), dữ liệu ghi vào [ledger_entries](node:erd-core-domain.ledger_entries) và [payroll_snapshots](node:erd-core-domain.payroll_snapshots). Từ đó [Hàng đợi Outbox](node:arch-system.sync-outbox) đồng bộ sang [EH & Kế toán Xero](node:arch-system.au-payroll-xero). |

| Bảng CSDL Cốt Lõi (ERD) | Luồng Ghi Dữ Liệu (Upstream) | Luồng Đọc & Kế Thừa (Downstream) | Các Màn Hình MVP Tương Tác Trực Tiếp | Quy Tắc Bảo Vệ Toàn Vẹn Liên Luồng |
| --- | --- | --- | --- | --- |
| `branches` & `services` {#tb-1} | **Luồng 1** (Cấu hình cơ sở, phòng, BOM & giá kép) | **Luồng 2, 4, 5, 6** (Đặt lịch, kiểm Consent, trừ kho, tính giá) | `D1, D2` (Danh mục), `A1` (Chọn dịch vụ), `B1` (Lịch tổng) | Gắn cứng cờ `requires_consent`, định mức vật tư `BOM` và bảng giá Tiền mặt/Thẻ (`PAY-001`) |
| `staff_profiles` {#tb-2} | **Luồng 1** (Xếp ca đa chi nhánh & khóa Tram `45p`) | **Luồng 2, 3, 5, 7** (Lọc 5 điều kiện, đổi KTV, tính lương) | `D1` (Xếp ca), `B2` (Need Attention), `C1, C4` (Lịch & Nghỉ phép) | 1 hồ sơ KTV duy nhất xuyên 2 chi nhánh (`STF-001`) + Tự động khóa `30–45p` đi tàu điện (`NT2`) |
| `customers` {#tb-3} | **Luồng 2 & 4** (Tạo hồ sơ khách, cập nhật cờ an toàn) | **Luồng 4, 5, 6, 7** (Đón khách, điều trị, trừ ví, gửi SMS) | `A2` (Xác nhận), `B3` (Hồ sơ khách), `C2` (Tại giường — ẩn SĐT) | Ẩn số điện thoại khách khỏi KTV và ẩn bệnh án chi tiết khỏi Lễ tân (`SEC-008`) |
| `appointments` {#tb-4} | **Luồng 2 & 3** (Tạo lịch `Confirmed`, điều phối `Need Attention`) | **Luồng 4, 5, 6, 7** (Check-in, `In Service`, `Completed`, SLA) | `A1–A2`, `B1–B2`, `C1–C2` | Khóa chống trùng `TSTZRANGE EXCLUDE USING gist` (`0%` double-booking) & giữ cờ `Selected Therapist` |
| `clinical_records` {#tb-5} | **Luồng 4** (Ký `Digital Consent`) & **Luồng 5** (Ghi `SOAP`) | **Luồng 5** (Tra cứu lịch sử điều trị) & **Luồng 7** (Điều tra SLA 1h) | `B4` (Ký Consent iPad/QR), `C2` (Thẻ nhanh SOAP), `D6` (Sự cố) | Chặn vào phòng nếu thiếu chữ ký `TRT-002` + Mã hóa bệnh án lưu trữ tối thiểu `7 năm` (`NT16`) |
| `ledger_entries` {#tb-6} | **Luồng 6** (Giao dịch thu ngân nguyên tử 6 bước) | **Luồng 7** (Kết ca ngày, bù trừ liên chi nhánh, đẩy `Xero`) | `B5` (POS Thu ngân & Kết ca), `C3` (Ví KTV), `D4` (Đối soát GST) | Sổ cái bất biến chỉ ghi thêm (`Append-Only`), tách bạch GST kép (`NT6`) & chia hoa hồng đúng `100%` |
| `payroll_snapshots` {#tb-7} | **Luồng 7** (Gom dữ liệu từ **Luồng 1 + Luồng 5 + Luồng 6**) | **Employment Hero / ATO STP2** & Quay vòng về **Luồng 1** | `D3` (Duyệt & Khóa Payroll Snapshot 2 tuần), `D6` (SLA 1–2 sao) | Khóa cứng bản chụp đầu vào lương bất biến (`COM-006`) phục vụ `Award MA000005` & `12% Payday Super` |
