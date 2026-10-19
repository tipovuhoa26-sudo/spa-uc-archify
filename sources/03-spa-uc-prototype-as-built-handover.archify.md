# Bộ 3 file HTML Prototype As-Built (8.904 dòng mã) đã làm được gì và cần thay thế gì khi lên Cloud MVP? {#spa-uc-prototype-as-built}

Tái sử dụng **giao diện UX và từ điển dữ liệu** từ 3 file HTML (`8.904 dòng`), nhưng thay `localStorage` bằng cơ sở dữ liệu máy chủ chung.[](source:asbuilt)

- Cả 3 cổng (`Kỹ thuật viên`, `Khách hàng`, `Chủ Spa & Lễ tân`) hiện lưu vào **3 khóa `localStorage` tách biệt**, chưa liên thông với nhau.[](source:asbuilt)
- Cổng Kỹ thuật viên (`2.866 dòng`) đã hoàn thiện **chuỗi nâng cấp 7 tuần (`W1` đến `W7`)**: đồng hồ 3 chạm tại giường, thẻ `SOAP`, đặt lại 1 chạm và che PII.[](source:ktvhtml)
- Chế độ chuyển vai trò trên trình duyệt của file Admin (`Thuy_Spa_Chay_Thu.html`) chỉ phục vụ trình diễn và phải thay bằng xác thực máy chủ (`SEC-001..008`).[](source:admhtml)
- Mười hai giới hạn kỹ thuật của bản chạy thử đã có lộ trình ánh xạ 1-1 sang các đợt phát hành `R0` đến `R6` của MVP.[](source:asbuilt)

@answer scope="3 file HTML5 · 8.904 dòng mã · ~398 KB · 3 khóa localStorage độc lập · nâng cấp W1-W7"
@source asbuilt title="Báo cáo Kiểm toán 3 File HTML Prototype As-Built (894 dòng)" url=https://theheavenlounge.com/
@source ktvhtml title="Thuy_Spa_Ky_Thuat_Vien.html (2.866 dòng / 147,7 KB)" url=https://theheavenlounge.com/
@source cushtml title="Thuy_Spa_Khach_Hang.html (3.023 dòng / 125,0 KB)" url=https://theheavenlounge.com/
@source admhtml title="Thuy_Spa_Chay_Thu.html (3.015 dòng / 125,3 KB)" url=https://theheavenlounge.com/

## Cấu trúc kỹ thuật hiện tại của 3 file HTML chạy thử độc lập như thế nào? {#kiem-ke-3-file-html}

Bộ chạy thử gồm 3 tệp HTML5 độc lập viết bằng Vanilla ES6+ JavaScript đóng gói trong hàm `IIFE`. Mỗi tệp tự nhúng bảng màu CSS, dữ liệu mẫu JSON, bộ quản lý trạng thái và hàm vẽ giao diện riêng.[](source:asbuilt)

@architecture id=asbuilt-isolated-arch title="Kiến trúc 3 File HTML Prototype Hiện trạng (3 Kho localStorage Cô lập)"
| ktvHtml | Thuy_Spa_Ky_Thuat_Vien.html | frontend |
| cusHtml | Thuy_Spa_Khach_Hang.html | frontend |
| admHtml | Thuy_Spa_Chay_Thu.html | frontend |
| lsKtv | localStorage (KTV v1) | database |
| lsCus | localStorage (Khách v1) | database |
| lsAdm | localStorage (Admin v1) | database |
| ktvHtml > lsKtv | | emphasis |
| cusHtml > lsCus | |
| admHtml > lsAdm | |

@steps id=asbuilt-isolated-steps
| step-ktv | Thuy_Spa_Ky_Thuat_Vien.html | `2.866 dòng` (`147.700 bytes`). Triển khai 5 tab Kỹ thuật viên (`Công việc hôm nay`, `Lịch & Đổi ca`, `Hồ sơ & Ghi chú`, `Hoa hồng & Thu nhập`, `Xin nghỉ & Hỗ trợ`) cùng nâng cấp `W1-W7`. | asbuilt-isolated-arch | ktvHtml |
| step-cus | Thuy_Spa_Khach_Hang.html | `3.023 dòng` (`125.035 bytes`). Triển khai 6 tab Khách hàng (`Trang chủ`, `Đặt lịch`, `Lịch hẹn của tôi`, `Gói & Thẻ quà tặng`, `Hồ sơ & Cam kết`, `Đánh giá & Hỗ trợ`) với luồng đặt lịch 7 bước. | asbuilt-isolated-arch | cusHtml |
| step-adm | Thuy_Spa_Chay_Thu.html | `3.015 dòng` (`125.274 bytes`). Triển khai 7 tab Quản trị & Lễ tân (`Tổng quan`, `Lịch hẹn & Điều phối`, `Khách hàng & Liệu trình`, `Đội ngũ & Chứng chỉ`, `Thu ngân & Hoa hồng`, `Tồn kho & Đánh giá`, `Pháp lý Úc`). | asbuilt-isolated-arch | admHtml |

| Tên File Prototype | Dung lượng & Dòng mã | Khóa `localStorage` | Dữ liệu Mẫu Nhúng sẵn | Tính năng Tương tác Nổi bật |
| --- | --- | --- | --- | --- |
| `Thuy_Spa_Ky_Thuat_Vien.html` {#file-ktv} | `147,7 KB` / `2.866 dòng` | `thuy-spa-technician-trial-v1` | `4` KTV (`Minh Anh, Lan Chi, Thảo Nguyên, Bích Ngọc`), `7` ca, `5` khách | Đồng hồ 3 chạm (`W2`), thẻ `SOAP` (`W3`), đặt lại 1 chạm (`W4`), ví hoa hồng (`W5`), báo nghỉ (`W6`), che PII (`W7`) {good:Hoàn chỉnh nhất} |
| `Thuy_Spa_Khach_Hang.html` {#file-cus} | `125,0 KB` / `3.023 dòng` | `thuy-spa-customer-trial-v1` | `2` chi nhánh, `7` dịch vụ, `3` khách mẫu (`Emma, Linh, Olivia`), gói | Phễu đặt lịch 7 bước (`Chọn đích danh` vs `Tự động`), ví voucher, ký Consent điện tử, đánh giá `1-5 sao` {caution:Cô lập Trạng thái} |
| `Thuy_Spa_Chay_Thu.html` {#file-adm} | `125,3 KB` / `3.015 dòng` | `thuy-spa-offline-trial-v1` | `2` chi nhánh, `4` nhân sự, `7` lịch hẹn, `6` vật tư, `4` trụ cột pháp lý | Bộ chuyển vai trò (`Chủ Spa, Quản lý, Lễ tân, KTV`), hàng đợi `Cần xử lý`, POS tách dòng, xem trước BAS/Lương {caution:Xuất File Giả lập} |

## Chuỗi nâng cấp 7 tuần (W1 đến W7) trên Cổng Kỹ thuật viên đã bổ sung những gì? {#nang-cap-w1-w7}

Cổng Kỹ thuật viên (`Thuy_Spa_Ky_Thuat_Vien.html`) đã trải qua 7 đợt nâng cấp (`W1` đến `W7`) tối ưu cho thao tác một tay trên điện thoại tại giường trị liệu.[](source:ktvhtml)

@workflow id=ktv-w1-w7-flow title="Chuỗi Nâng cấp 7 Tuần trên Cổng Kỹ thuật viên (W1 đến W7)"
| w1 | W1-W2: Đếm giờ | frontend |
| w3 | W3-W4: Thẻ SOAP | backend |
| w5 | W5: Ví hoa hồng | database |
| w7 | W6-W7: Che PII | security |
| w1 > w3 | chạm | emphasis |
| w1 > w5 | tính |
| w3 > w7 | lưu | emphasis |
| w5 > w7 | khóa | security |

@steps id=ktv-w1-w7-steps
| step-w12 | W1-W2: Đếm giờ | `W1` xây hàng đợi ca làm việc kèm cảnh báo dị ứng, thai kỳ và trạng thái Consent. `W2` thêm chế độ tại giường 3 chạm (`Bắt đầu -> Đếm ngược -> Hoàn tất`) và chặn nếu thiếu Consent (`TRT-002`). | ktv-w1-w7-flow | w1 |
| step-w34 | W3-W4: Thẻ SOAP | `W3` thay ô nhập liệu dài bằng các thẻ chọn nhanh `SOAP` (`Da khô`, `Kích ứng nhẹ`, `Áp lực vừa`) và mẫu dặn dò sau ca. `W4` thêm nút đề xuất lịch hẹn tiếp theo (`+2 tuần`, `+4 tuần`) chỉ với 1 chạm. | ktv-w1-w7-flow | w3 |
| step-w5 | W5: Ví hoa hồng | Hiển thị rõ doanh thu dịch vụ, tỷ lệ chia combo (`100%` hoặc `50%/50%`) và tiền tip thẻ. Đồng thời kèm phụ cấp tàu điện và luồng khiếu nại số liệu. | ktv-w1-w7-flow | w5 |
| step-w67 | W6-W7: Che PII | `W6` tính trước các ca bị ảnh hưởng khi KTV báo nghỉ đột xuất và gợi ý đồng nghiệp thay thế. `W7` che số điện thoại, email khách hàng và tự động khóa màn hình sau `5 phút` không thao tác. | ktv-w1-w7-flow | w7 |

## Làm sao khắc phục 12 giới hạn kỹ thuật của Prototype khi xây dựng Cloud MVP? {#gioi-han-12-prototype}

Mục `6` báo cáo As-Built ghi nhận **12 giới hạn kỹ thuật** của 3 file HTML. Bảng dưới đây ánh xạ từng điểm vào kiến trúc đích (`R0-R6`).[](source:asbuilt)

| Mã | Giới hạn trong Prototype Hiện tại | Hậu quả Vận hành Thực tế | Giải pháp Kiến trúc trong Cloud MVP (`R0-R6`) | Đợt Sửa |
| --- | --- | --- | --- | --- |
| `L01` {#l-01} | 3 khóa `localStorage` cô lập giữa 3 file HTML | Khách đặt lịch ở file Khách hàng không hiện sang file Lễ tân hay KTV {bad:Đứt Luồng} | Hợp nhất trên PostgreSQL 16 chung kèm sự kiện thời gian thực (`WebSocket / SSE`) | `R0-R2` |
| `L02` {#l-02} | Dữ liệu mất khi xóa bộ nhớ trình duyệt hoặc đổi máy | Không thể vận hành thật trên nhiều máy tính bảng và quầy thu ngân | Lưu trữ bền vững trên AWS Sydney (`ap-southeast-2`) kèm sao lưu tự động hằng ngày | `R0` |
| `L03` {#l-03} | Chuyển vai trò (`Chủ Spa / Lễ tân / KTV`) ngay trên trình duyệt | Bất kỳ ai mở file cũng xem được lương, doanh thu và bệnh án {bad:Vi phạm SEC-001} | Xác thực JWT máy chủ, kiểm tra RBAC theo phạm vi chi nhánh và che bệnh án (`SEC-008`) | `R0` |
| `L04` {#l-04} | Chưa có khóa chống trùng lịch đồng thời (`Race Condition`) | Hai khách đặt cùng 1 giờ trên 2 thiết bị sẽ tạo lịch trùng | Khóa dòng `SELECT FOR UPDATE` kết hợp ràng buộc `PostgreSQL TSTZRANGE` (`NFR-001`) {good:Khóa Nguyên tử} | `R2` |
| `L05` {#l-05} | Khối đi tàu điện `30-45p` mới kiểm tra bằng mảng tĩnh trong bộ nhớ | Dễ lệch khi đổi ca hoặc kéo thả lịch liên chi nhánh | Động cơ kiểm tra lịch hợp nhất (`NT1, BKG-003`) tự động tạo và thu hồi khối đi lại | `R1-R2` |
| `L06` {#l-06} | Ký Consent mới chỉ là hộp đánh dấu (`Checkbox`) | Thiếu bằng chứng pháp lý khi xảy ra tranh chấp y khoa hoặc kiểm tra | Lưu chữ ký điện tử, phiên bản biểu mẫu, mốc giờ, IP và khóa cứng với ca làm việc (`TRT-002`) | `R3` |
| `L07` {#l-07} | Số dư gói và thẻ quà tặng đang trừ trực tiếp vào biến số | Không thể truy vết lịch sử hoặc hoàn tác khi hủy lịch/hoàn tiền | Sổ cái sự kiện chỉ ghi thêm (`Append-Only Ledger` `NT7, PKG-004`) | `R4` |
| `L08` {#l-08} | Thanh toán thẻ và hoàn tiền mới là nút bấm giả lập | Không đối soát được với máy quẹt thẻ EFTPOS thực tế tại quầy | Tích hợp cổng `Tyro / Square / Stripe` kèm khóa lũy đẳng (`Idempotency Key` `NFR-003`) | `R4` |
| `L09` {#l-09} | Nút xuất `BAS/GST` và `Payroll` mới chỉ hiện hộp thông báo (`Toast`) | Kế toán vẫn phải nhập tay sang Xero và Employment Hero | Bộ xuất chuẩn CSV/API cho Xero (`FIN-002`) và `Payroll Input Snapshot` bất biến (`COM-006`) {good:Tự động hóa} | `R5` |
| `L10` {#l-10} | Tin nhắn SMS nhắc lịch và báo đổi KTV mới chỉ ghi nhật ký màn hình | Khách không nhận được SMS thật trên điện thoại | Tích hợp cổng SMS Úc (`Twilio AU / MessageMedia`) có kiểm tra đồng ý nhận tin (`Spam Act 2003`) | `R2-R3` |
| `L11` {#l-11} | Chưa lưu vết trước/sau (`before_json / after_json`) trên máy chủ | Không kiểm toán được ai đã sửa giá, hoa hồng hoặc hoàn tiền | Bảng `audit_logs` chỉ ghi thêm ở tầng máy chủ (`NFR-006`) | `R0-R6` |
| `L12` {#l-12} | Chưa có cột `organisation_id` cho mô hình đa chuỗi | Khó nhân bản nếu Sunext thương mại hóa cho `15-20 spa` (`Q0`) | Gắn `organisation_id` và `branch_id` trên toàn bộ các bảng cốt lõi (`NFR-010`) | `R0` |

## Đội kỹ thuật nên tái sử dụng những tài sản nào từ Prototype khi bắt tay vào code MVP? {#danh-sach-tai-su-dung}

Đội phát triển cần trích xuất trực tiếp các quy tắc và từ điển dữ liệu đã kiểm chứng dưới đây sang dự án Cloud MVP.[](source:asbuilt)

- [ ] **Giữ nguyên mã định danh miền:** Dùng bộ mã `BR-01, BR-02`, `STF-01..04`, `SRV-01..07`, `PKG-01..03` và `GC-204` làm dữ liệu mẫu (`Seed Data`) cho đợt `R1`.
- [ ] **Chuyển hàm kiểm tra 5 điều kiện sang Backend:** Đưa logic `Kỹ năng + Đào tạo + Chứng chỉ + Ca + Giờ tàu điện` vào dịch vụ `SlotValidator` (`NFR-009`).
- [ ] **Giữ nguyên bố cục tại giường (`W1-W7`):** Tái sử dụng đồng hồ 3 chạm (`W2`), thẻ `SOAP` (`W3`) và nút đặt lại 1 chạm (`W4`) cho màn hình `C2`.
- [ ] **Giữ nguyên phễu đặt lịch 7 bước:** Chuyển giao diện chọn `Selected Therapist` vs `Any Therapist` (`BKG-007`) từ `Thuy_Spa_Khach_Hang.html` sang màn hình `A1`.
- [ ] **Nâng cấp hàng đợi `Cần xử lý` thành màn hình `B2`:** Giữ giao diện xếp hạng người thay thế khi KTV báo nghỉ và nối vào PostgreSQL thời gian thực.

