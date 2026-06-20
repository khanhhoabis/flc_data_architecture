# Slide Khởi động Dự án: Khảo sát & Xây dựng Kiến trúc Dữ liệu Hiện trạng FPT Long Châu

---

## Slide 1: Khởi động Dự án
* **Tên Dự án**: Xây dựng Baseline Kiến trúc Dữ liệu - FPT Long Châu
* **Chủ đề**: Khởi động & Thống nhất Kế hoạch Khảo sát
* **Đơn vị Thực hiện**: Ban Kiến trúc Doanh nghiệp (EA Team)
* **Báo cáo bởi**: EA Consultant (Leader)
* **Ngày**: 20/06/2026

---

## Slide 2: Mục tiêu Dự án (Project Objectives)
* **Định vị & Phân loại**: Xác định toàn bộ nguồn dữ liệu nghiệp vụ cốt lõi (POS, ERP, CRM, CDP, WMS).
* **Tuân thủ Nghị định 13/2023/NĐ-CP**: Bản đồ hóa vùng lưu trữ dữ liệu cá nhân (nhạy cảm/cơ bản) của khách hàng.
* **Bản đồ luồng thông tin (Data Flow & Lineage)**: Làm rõ cách thức dữ liệu sinh ra, luân chuyển và tích hợp.
* **Đánh giá hiện trạng quản trị**: Đánh giá chất lượng dữ liệu, bảo mật và vòng đời dữ liệu.
* **Thiết lập Baseline**: Xây dựng kho tài liệu kiến trạng làm cơ sở thiết kế Target Architecture.

---

## Slide 3: Phạm vi Khảo sát (Scope)
* **Nghiệp vụ cốt lõi**:
  * Bán hàng (POS), Chuỗi cung ứng (WMS, Logistics).
  * Chăm sóc khách hàng & Trung thành (CDP, CRM).
  * Mua hàng & Quản trị sản phẩm (ERP), Omnichannel.
* **Hệ thống ứng dụng**: ERP, POS, WMS, CRM, CDP, DWH/Lakehouse.
* **Khía cạnh dữ liệu**: Database Schemas, Data Dictionary, Metadata, Cơ chế an toàn (encryption, masking, access control, audit log).

---

## Slide 4: Ban chỉ đạo & Ban dự án (Project Committee)
* **Sponsor / Product Owner (PO)**: Khanh Hoa (Head of Data / Business Sponsor).
* **EA Team**:
  * **EA Consultant (Leader)**: ea_consultant (Điều phối chung, tổng hợp Baseline).
  * **Data Architect**: data_architect (Khảo sát nguồn dữ liệu, mô hình hóa ArchiMate).
  * **Data Engineer**: data_engineer (Khảo sát luồng tích hợp, Data Lineage).
  * **Enterprise Security Architect**: ea_security_architect (Đánh giá bảo mật, Nghị định 13, WP10).
* **Đầu mối phía Doanh nghiệp (Domain Contacts)**:
  * POS & Retail Ops: Trưởng phòng POS.
  * Supply Chain / WMS: Trưởng phòng Chuỗi cung ứng.
  * CRM / CDP: Trưởng phòng Marketing & CRM.
  * IT Infra / DBA: Trưởng phòng Hạ tầng & DBA.

---

## Slide 5: Kế hoạch phân rã công việc (10 Work Packages)
1. **WP1**: Chuẩn bị & Khởi động dự án (EA Consultant) - *Đang thực hiện*
2. **WP2**: Lập ma trận RACI & Stakeholders (EA Consultant)
3. **WP3**: Khảo sát Danh mục Hệ thống & Nguồn dữ liệu (Data Architect)
4. **WP4**: Khảo sát Cấu trúc, Từ điển & Phân loại Dữ liệu (Data Architect)
5. **WP5**: Khảo sát Luồng thông tin & Tích hợp (Data Engineer)
6. **WP6**: Đánh giá Bảo mật, Quản trị & Tuân thủ NĐ 13 (Security Architect)
7. **WP7**: Mô hình hóa ArchiMate Baseline (Data Architect)
8. **WP8**: Tổng hợp Baseline & Đánh giá Gaps (EA Consultant)
9. **WP9**: Đánh giá & Ký duyệt Baseline (EA Consultant & Stakeholders)
10. **WP10**: An toàn thông tin cho hoạt động Khảo sát (Security Architect)

---

## Slide 6: Kế hoạch Truyền thông & Phối hợp
* **Kênh trao đổi chính**: Microsoft Teams / Slack nội bộ cho giao tiếp nhanh.
* **Chia sẻ tài liệu**: SharePoint/OneDrive nội bộ có bảo mật MFA (Tuyệt đối không dùng Zalo/Telegram).
* **Họp định kỳ**:
  * *Họp Ban dự án (Weekly Sync)*: Thứ Sáu hàng tuần (1h).
  * *Họp Ban chỉ đạo (Steering Committee)*: Bi-weekly (sau mỗi 2 tuần).
* **Báo cáo tiến độ**: Gửi PO hàng tuần thông qua issue update / comment.

---

## Slide 7: Nguyên tắc An toàn Thông tin Khảo sát
* **Quyền truy cập**: Chỉ cấp quyền **Read-only** tối thiểu trên môi trường Staging/Sandbox.
* **Hạn chế dữ liệu thật**: Không kết xuất hoặc lưu trữ dữ liệu thực của khách hàng (Actual/Customer Data). Chỉ thu thập siêu dữ liệu (Metadata/DDL).
* **Bảo mật tài liệu**: Mã hóa sơ đồ hạ tầng chi tiết, lưu trữ phân quyền hạn chế truy cập.
* **NDA**: Ký cam kết bảo mật trước khi tiếp cận hệ thống nghiệp vụ.

---

## Slide 8: Các bước tiếp theo (Next Steps)
* **Kích hoạt WP2**: Xác định danh sách chi tiết Data Owners & Stewards.
* **Lên lịch phỏng vấn**: Gửi lịch phỏng vấn trước 1 tuần cho các bộ phận POS, Supply Chain, CRM, IT.
* **Chuẩn bị bảng câu hỏi**: Soạn thảo trước danh sách câu hỏi khảo sát cho từng bộ phận để tối ưu hóa thời gian họp.
