# Kiến trúc Sản phẩm & 17 Màn hình MVP (R0–R6) vận hành 42 quy tắc của The Heaven Lounge như thế nào? {#spa-uc-mvp-prd}

Triển khai **17 màn hình MVP** (`A1-D6`) trên 4 cổng vai trò qua 7 đợt phát hành (`R0-R6`) để thực thi 42 quy tắc nghiệp vụ.[](source:prd10)

- Hệ thống phân quyền máy chủ (`SEC-001` đến `SEC-008`) gắn mọi thao tác với `role + branch_scope + staff_id` và che bệnh án khỏi lễ tân.[](source:ba10)
- Một hàm kiểm tra lịch duy nhất (`NFR-009`) kiểm soát cả đặt lịch web, kéo thả lịch lễ tân và gán người thay thế (`NFR-001`).[](source:ba10)
- Giao dịch hoàn tất dịch vụ thực thi **6 bước nguyên tử** (`PAY-001` đến `COM-006`) cho doanh thu, trừ gói, dòng GST, hoa hồng `100%` và tip giữ hộ.[](source:ba10)
- Chín điểm nghẽn (`UNK-01` đến `UNK-09`) đều có cấu hình dự phòng để đội kỹ thuật xây dựng ngay đợt `R0-R1` mà không phải đoán mò.[](source:prd10)

@answer scope="17 màn hình MVP (A1-D6) · 42 quy tắc BA · 7 đợt phát hành (R0-R6) · 9 điểm nghẽn UNK-01..09"
@source ba10 title="Tài liệu Phân tích Nghiệp vụ & Product Blueprint v1.0 (2.148 dòng)" url=https://theheavenlounge.com/
@source prd10 title="Tài liệu Yêu cầu Sản phẩm MVP PRD v1.0 (733 dòng)" url=https://theheavenlounge.com/
@source wp22 title="Whitepaper Kiến trúc Mini-ERP Spa Úc v2.2 (3.862 dòng)" url=https://theheavenlounge.com/

## Ma trận phân quyền RBAC bảo vệ bệnh án khách hàng và dữ liệu lương như thế nào? {#ma-tran-rbac}

Cơ chế phân quyền chạy hoàn toàn ở phía máy chủ (`SEC-001`) dựa trên bộ ba `role + branch_scope + staff_id`. Quy tắc `SEC-008` tách bạch dữ liệu sức khỏe nhạy cảm (`Privacy Act 1988`) khỏi thông tin vận hành tại quầy lễ tân.[](source:ba10)

| Nhóm Tài nguyên & Thao tác | Chủ Spa (`ORG-01`) | Quản lý Chi nhánh (`BR-01 / BR-02`) | Lễ tân (`BR-01 / BR-02`) | Kỹ thuật viên (`Ca được giao`) | Khách hàng (`Hồ sơ cá nhân`) |
| --- | --- | --- | --- | --- | --- |
| Cấu hình Dịch vụ, Bảng giá & Tỷ lệ Hoa hồng (`D1, D2`) {#rbac-cfg} | Toàn quyền Đọc/Ghi {good:Toàn quyền} | Chỉ xem tại chi nhánh | Chỉ xem giá bán | Không truy cập {bad:Chặn} | Xem bảng giá công khai (`A1`) |
| Lịch hẹn & Hàng đợi `Need Attention` (`B1, B2`) {#rbac-cal} | Toàn chuỗi (2 chi nhánh) | Toàn quyền tại chi nhánh | Tạo, Đổi lịch, Check-in | Chỉ xem lịch cá nhân (`C1`) {caution:Chỉ cá nhân} | Đặt/Đổi lịch của mình (`A1, A2`) |
| Sức khỏe, Dị ứng, Chống chỉ định & SOAP (`C2`) {#rbac-soap} | Toàn quyền + Nhật ký Audit | Xem khi xử lý sự cố | **Chỉ thấy cờ cảnh báo; che bệnh án** {caution:SEC-008 Che} | Xem & Ghi cho ca được giao {good:Được phép} | Điền biểu mẫu Intake & Consent (`A3`) |
| Hồ sơ Chứng chỉ Hành nghề & Hạn sử dụng (`D2, C3`) {#rbac-cert} | Duyệt & Khóa chứng chỉ | Tải lên & Theo dõi hạn | Chỉ thấy trạng thái Đạt/Chặn | Tải chứng chỉ của mình (`C3`) | Không truy cập {bad:Chặn} |
| Thu ngân POS, Giảm giá Tiền mặt & Trừ Gói (`B5`) {#rbac-pos} | Toàn quyền + Hoàn tiền | Thu ngân + Duyệt hoàn tiền | Thu ngân + Ghi nhận Tip (`PAY-004`) | Không truy cập POS {bad:Chặn} | Xem hóa đơn & Số dư gói (`A2`) |
| Hoa hồng & Snapshot Bảng lương 2 Tuần (`D4, C3`) {#rbac-pay} | Khóa kỳ lương & Xuất file {good:Chốt kỳ} | Xem tổng hợp chi nhánh | Không truy cập bảng lương {bad:SEC-004 Chặn} | Chỉ xem ví thu nhập cá nhân (`C3`) | Không truy cập {bad:Chặn} |

@callout level=warning title="Bảo vệ Dữ liệu Y tế Nhạy cảm (SEC-008) & Nhật ký Kiểm toán Bất biến (NFR-006)"
| Theo `Privacy Act 1988`, bệnh lý, thai kỳ và ghi chú `SOAP` là dữ liệu nhạy cảm. Lễ tân chỉ thấy nhãn cảnh báo vận hành (`Cần ký Consent`, `Lưu ý dị ứng`). |
| Mọi thao tác sửa lịch, đổi người sau xác nhận, hoàn tiền, sửa gói và khóa kỳ lương đều phải ghi vào `audit_logs` bất biến (`before_json`, `after_json`, `reason`). |

## 17 màn hình MVP được phân bổ vào 4 cổng vai trò và 7 đợt phát hành (R0–R6) ra sao? {#man-hinh-17-mvp}

Phạm vi MVP (`PRD Mục 5 & Mục 8`) gói gọn trong **17 màn hình** nhằm giải quyết trọn vẹn vận hành cốt lõi mà không phình to thành hệ thống ERP cồng kềnh.[](source:prd10)

@tree id=mvp-screen-tree title="17 Màn hình MVP Phân theo 4 Cổng Vai trò (A1 đến D6)"
| root | Nền tảng Mini-ERP Spa Úc (17 Màn hình) | - |
| portalA | A. Cổng Khách hàng (3) | root |
| portalB | B. Cổng Lễ tân & Quản lý (5) | root |
| portalC | C. Cổng Kỹ thuật viên (3) | root |
| portalD | D. Cổng Chủ Spa / Admin (6) | root |
| sA1 | A1. Đặt lịch Trực tuyến (R2) | portalA |
| sA2 | A2. Lịch hẹn & Ví Voucher (R2/R4) | portalA |
| sA3 | A3. Intake & Ký Consent (R3) | portalA |
| sB1 | B1. Lịch Điều phối Đa nhánh (R2) | portalB |
| sB2 | B2. Hàng đợi Need Attention (R3) | portalB |
| sB3 | B3. Hồ sơ Khách & Gói (R1/R4) | portalB |
| sB4 | B4. Phân ca & Đi lại (R1) | portalB |
| sB5 | B5. Thu ngân POS & Chốt ca (R4) | portalB |
| sC1 | C1. Lịch làm việc Cá nhân (R2) | portalC |
| sC2 | C2. Màn hình Ca & SOAP (R3) | portalC |
| sC3 | C3. Xin nghỉ, Chứng chỉ & Ví (R1/R5) | portalC |
| sD1 | D1. Danh mục Dịch vụ & Giá (R1) | portalD |
| sD2 | D2. Ma trận Kỹ năng & Cert (R1) | portalD |
| sD3 | D3. Quản trị Gói & Sổ nợ (R4) | portalD |
| sD4 | D4. Hoa hồng & Chốt Lương (R5) | portalD |
| sD5 | D5. Xuất Kế toán & Đối soát (R5) | portalD |
| sD6 | D6. Cảnh báo Đánh giá & Audit (R6) | portalD |

| Đợt Phát hành | Mục tiêu Cốt lõi | Màn hình Bàn giao | Mã Quy tắc Nghiệp vụ Thực thi | Điều kiện Nghiệm thu (Exit Gate) |
| --- | --- | --- | --- | --- |
| `R0` · Nền tảng {#rel-r0} | Xác thực, RBAC, phạm vi chi nhánh, khung `audit_logs` | Khung đăng nhập & RBAC | `SEC-001..008`, `NFR-006`, `NFR-010` | Chặn truy cập trái phép ở mức API {good:Cổng Bảo mật} |
| `R1` · Dữ liệu gốc & Ca {#rel-r1} | Chi nhánh, dịch vụ, kỹ năng, chứng chỉ, phân ca & giờ tàu điện | `D1, D2, B4, B3 (cơ bản), C3 (chứng chỉ)` | `SKL-001..004`, `BKG-003` | Tự động chặn gán dịch vụ nếu chứng chỉ hết hạn |
| `R2` · Đặt lịch Hợp nhất {#rel-r2} | Động cơ tính slot hợp nhất, đặt lịch web & lịch điều phối lễ tân | `A1, A2 (lịch), B1, C1` | `BKG-001..007`, `NFR-001`, `NFR-009` | `0` lịch trùng khi 2 người đặt cùng lúc (`< 300ms`) {good:Khóa Slot} |
| `R3` · Nghỉ đột xuất & Consent {#rel-r3} | Hàng đợi `Need Attention`, cổng chặn Consent & ghi chú `SOAP` | `B2, A3, C2, C3 (xin nghỉ)` | `BKG-008..009`, `TRT-001..005` | Chặn `In Service` nếu chưa ký Consent (`TRT-002`) {caution:Cổng Pháp lý} |
| `R4` · POS, Gói & Sổ nợ {#rel-r4} | Thu ngân tách dòng, giảm giá tiền mặt, bán/trừ gói & chốt ca | `B5, D3, A2 (ví), B3 (gói)` | `PKG-001..006`, `PAY-001..005` | Số dư gói khớp `100%` với tổng sự kiện sổ cái |
| `R5` · Hoa hồng, Lương & Xero {#rel-r5} | Chia hoa hồng combo, khóa `Payroll Snapshot` & xuất bút toán Xero | `D4, D5, C3 (thu nhập)` | `COM-001..006`, `FIN-001..004` | Tệp lương 2 tuần bất biến; khớp tổng chốt ca |
| `R6` · Đánh giá, Tồn kho & Go-Live {#rel-r6} | Đánh giá sau dịch vụ, cảnh báo 1-2 sao, tồn kho gọn & diễn tập | `D6` + kiểm thử toàn trình | `REV-001..003`, `INV-001..003` | Vượt qua toàn bộ 12 kịch bản nghiệm thu (`PRD Mục 9`) {good:Sẵn sàng Go-Live} |

## Động cơ Kiểm tra Điều kiện (Eligibility Engine) xử lý lịch liên chi nhánh và nghỉ đột xuất ra sao? {#luong-kiem-tra-dieu-kien}

Mọi khung giờ trống và đề xuất người thay thế đều phải vượt qua 5 điều kiện cứng (`SKL-003, BKG-003, BKG-004`). Khi kỹ thuật viên báo ốm (`BKG-008`), hệ thống kiểm tra cờ `Selected Therapist` (`BKG-007`) trước khi cho phép đổi người.[](source:ba10)

@workflow id=eligibility-leave-flow title="Kiểm tra 5 Điều kiện & Xử lý Hàng đợi Nghỉ Đột xuất"
| elig | Đủ 5 điều kiện? | backend |
| travel | Chèn giờ tàu | backend |
| queue | Hàng đợi B2 | security |
| sel | Khách chọn KTV? | database |
| elig > travel | đạt | emphasis |
| elig > queue | nghỉ |
| travel > sel | giữ chỗ | emphasis |
| queue > sel | lọc thay | security |

@steps id=eligibility-leave-steps
| step-elig | Đủ 5 điều kiện? | Kiểm tra đồng thời: `Kỹ năng hoạt động + Hoàn thành đào tạo + Chứng chỉ còn hạn (Laser/HLTINF005) + Có ca làm việc + Không trùng lịch`. Thiếu bất kỳ điều kiện nào sẽ bị chặn ngay (`SKL-003`). | eligibility-leave-flow | elig |
| step-travel | Chèn giờ tàu | Khi KTV có lịch ở cả `Brunswick` và `North Melbourne` trong ngày, hệ thống tự giữ chỗ `30-45 phút` đi tàu điện (`BKG-003`). Khối này được gắn nhãn giờ công đi lại cho bảng lương. | eligibility-leave-flow | travel |
| step-queue | Hàng đợi B2 | Báo nghỉ đột xuất tuyệt đối không xóa lịch hẹn (`BKG-008`). Toàn bộ lịch bị ảnh hưởng chuyển vào hàng đợi `Need Attention` (`B2`) kèm danh sách KTV thay thế hợp lệ. | eligibility-leave-flow | queue |
| step-sel | Khách chọn KTV? | Kiểm tra `assignment_mode`. Nếu khách đã chọn đích danh KTV (`Selected Therapist` `BKG-007`), hệ thống chặn đổi ngầm và chuyển sang trạng thái chờ khách quyết định (`Waiting for Customer Decision`). | eligibility-leave-flow | sel |

## Giao dịch thu ngân nguyên tử, thuế GST và khóa bảng lương 2 tuần vận hành như thế nào? {#trinh-tu-thu-ngan-luong}

Hoàn tất một dịch vụ kích hoạt giao dịch cơ sở dữ liệu nguyên tử (`PAY-001` đến `COM-006`). Nhờ đó, doanh thu, nợ trả trước, thuế GST, hoa hồng kỹ thuật viên và tiền tip giữ hộ không bao giờ lệch nhau.[](source:ba10)

@sequence id=checkout-payroll-seq title="Cổng Consent, Giao dịch Nguyên tử 6 Bước & Khóa Bảng lương 2 Tuần"
| actor ktv | Kỹ thuật viên (C2) | frontend |
| actor pos | Lễ tân POS (B5) | frontend |
| actor core | Lõi Mini-ERP | backend |
| actor db | Sổ Cái & Lương DB | database |
| actor ext | Xero & Payroll Úc | external |
| phase | 1. Cổng Chặn Pháp lý trước Dịch vụ (TRT-002 & SKL-003) |
| ktv | core | Bấm Bắt đầu Ca làm việc (In Service) |
| core | db | Kiểm tra Hồ sơ Consent đã ký + Tuổi >= 18 + Chứng chỉ | security |
| db | ktv | Mở khóa Ca & Đồng hồ Đếm giờ 3 Chạm (W2) | return |
| phase | 2. Thu ngân Nguyên tử 6 Bước & Tách dòng (PAY-001 đến COM-005) |
| pos | core | Chốt Thanh toán (Tiền mặt/Thẻ + Trừ gói + Tip) |
| core | db | Ghi 6 sự kiện: Thu tiền + Doanh thu + Trừ nợ + GST + Hoa hồng 100% + Tip | emphasis |
| db | pos | Xuất Hóa đơn & Số dư Gói Sự kiện (NT7) | return |
| phase | 3. Chốt Ca Cuối ngày & Khóa Kỳ lương 2 Tuần (FIN-004 & COM-006) |
| core | ext | Đẩy Bút toán Tổng hợp Ngày & Tệp Payroll Input Snapshot Đã khóa | emphasis |

@steps id=checkout-payroll-steps
| step-consent | Kỹ thuật viên (C2) | Đối với dịch vụ xâm lấn, Laser/IPL hoặc phun xăm (`>= 18 tuổi`), hệ thống chặn nút `In Service` nếu khách chưa ký biểu mẫu đồng ý điện tử (`TRT-002`). | checkout-payroll-seq | ktv |
| step-pos | Lễ tân POS (B5) | Hỗ trợ thanh toán kết hợp (`Tiền mặt + Thẻ + Voucher`), áp giảm giá tiền mặt từ giá thẻ chuẩn (`PAY-001`), và tách riêng tiền tip qua thẻ (`PAY-004`). | checkout-payroll-seq | pos |
| step-atomic | Sổ Cái & Lương DB | Ghi nhận nguyên tử 6 sự kiện: tiền thu, doanh thu, giảm nợ gói (`PKG-004`) và dòng GST. Đồng thời chốt hoa hồng combo tổng `= 100%` (`COM-002`) và tiền tip giữ hộ. | checkout-payroll-seq | db |
| step-export | Xero & Payroll Úc | Đẩy bút toán tổng hợp ngày theo chi nhánh sau khi chốt ca (`FIN-002`) và xuất tệp `Payroll Input Snapshot` bất biến mỗi 2 tuần (`COM-006`). Mọi điều chỉnh sau khi khóa kỳ đều phải sang kỳ kế tiếp (`COM-005`). | checkout-payroll-seq | ext |

## Làm sao xử lý 9 điểm nghẽn chưa xác nhận (UNK-01 đến UNK-09) trước ngày Go-Live? {#diem-nghen-9-unk}

Mục `1.3` BA Blueprint và Mục `10` MVP PRD cô lập **9 câu hỏi chưa chốt (`UNK-01..09`)** thành các tham số cấu hình. Nhờ vậy, đội kỹ thuật có thể xây dựng ngay từ đợt `R0` mà không phải viết lại lõi.[](source:prd10)

| Mã UNK | Nội dung Chưa Chốt | Đợt Chặn | Thiết kế Dự phòng trong Hệ thống (`Fallback`) | Chủ sở hữu Chốt |
| --- | --- | --- | --- | --- |
| `UNK-01` {#u-01} | Phần mềm kế toán đang dùng (`Xero` hay `MYOB`) | Trước `R5` | Xây `Accounting Export Adapter` xuất chuẩn CSV Xero, CSV MYOB và bảng tổng hợp JSON | Chị Thủy / Kế toán |
| `UNK-02` {#u-02} | Phần mềm tính lương (`Employment Hero`, `KeyPay` hay `Xero Payroll`) | Trước `R5` | Chuẩn hóa bảng `payroll_snapshots` độc lập nhà cung cấp kèm bộ ánh xạ cột CSV | Chị Thủy / Kế toán |
| `UNK-03` {#u-03} | Phần cứng máy quẹt thẻ (`Tyro`, `Square`, `Stripe` hay `Zeller`) | Trước `R4` | Hỗ trợ cả 2 chế độ: gọi API thiết bị trực tiếp hoặc nhập mã `Terminal Reference ID` có đối soát chốt ca | Chị Thủy |
| `UNK-04` {#u-04} | Cơ sở tính hoa hồng (trên **giá niêm yết gốc** hay **giá thực thu sau giảm**) | Trước `R5` | Bảng quy tắc hoa hồng hỗ trợ cờ cấu hình `base_type = LIST_PRICE \| NET_COLLECTED` (`COM-001`) {caution:Bắt buộc Chốt} | Chị Thủy |
| `UNK-05` {#u-05} | Bảng tỷ lệ hoa hồng chi tiết theo từng dịch vụ và bậc KTV | Trước `R5` | Thiết kế bảng `commission_rules` có hiệu lực theo ngày (`effective_from`, `version_id`) | Chị Thủy |
| `UNK-06` {#u-06} | Chính sách đặt cọc, thời hạn hủy miễn phí và phí `No-Show` | Trước `R2` | Cấu hình tham số chính sách theo chi nhánh/dịch vụ (mặc định tắt thu cọc online ở đợt đầu) | Chị Thủy |
| `UNK-07` {#u-07} | Nhà cung cấp cổng SMS tại Úc và tên thương hiệu `ACMA Sender ID` | Trước `R2` | Xây `Notification Provider Interface` (cắm `Twilio AU` hoặc `MessageMedia` + dự phòng Email) | Chị Thủy / Sunext |
| `UNK-08` {#u-08} | Số lượng phòng, giường và máy chuyên dụng tại mỗi chi nhánh | Trước `R1` | Cho phép bật/tắt ràng buộc tài nguyên phòng/máy (`BKG-004`) trên từng dịch vụ | Quản lý Chi nhánh |
| `UNK-09` {#u-09} | Danh sách dịch vụ bắt buộc ký Consent và kiểm tra chứng chỉ | Trước `R1` | Cờ cấu hình trên danh mục dịch vụ: `requires_consent`, `min_age`, `required_cert_codes[]` | Chị Thủy / Quản lý |

## Những tiêu chí nào bắt buộc phải vượt qua trước khi nghiệm thu đưa vào vận hành? {#danh-sach-nghiem-thu-prd}

Mọi đợt bàn giao từ `R0` đến `R6` phải vượt qua bộ kiểm thử nghiệm thu trong Mục `9` của MVP PRD.[](source:prd10)

- [ ] **Không trùng lịch (`NFR-001`):** Hai yêu cầu đặt lịch đồng thời cho cùng 1 KTV và khung giờ chỉ thành công đúng 1 yêu cầu (`SELECT FOR UPDATE` + `TSTZRANGE`).
- [ ] **Tự động khóa giờ tàu điện (`BKG-003`):** Khi KTV có lịch ở cả 2 chi nhánh trong ngày, hệ thống tự chèn khối `30-45 phút` đi lại và xuất sang lương.
- [ ] **Bảo vệ khách chọn đích danh (`BKG-007 & BKG-008`):** Khi KTV báo nghỉ, lịch `Selected Therapist` chuyển vào `Need Attention` chờ khách xác nhận, tuyệt đối không đổi ngầm.
- [ ] **Cổng chặn pháp lý trước ca (`TRT-002 & SKL-003`):** Chặn gán KTV hết hạn chứng chỉ và chặn bấm `In Service` nếu khách chưa ký biểu mẫu Consent bắt buộc.
- [ ] **Toàn vẹn sổ nợ và kỳ lương (`PKG-004 & COM-006`):** Số dư gói khớp tuyệt đối với sổ cái sự kiện. Kỳ lương đã khóa không bị sửa bởi hoàn tiền về sau.

