# Kế hoạch Khảo sát Hiện trạng Kiến trúc Dữ liệu đã phê duyệt (Approved Survey Plan)

Tài liệu này ghi nhận phiên bản kế hoạch khảo sát hiện trạng kiến trúc dữ liệu (Baseline Data Architecture) của FPT Long Châu đã được phê duyệt chính thức bởi Product Owner và sẵn sàng đưa vào triển khai thực tế.

---

## 1. Trạng thái và Thông tin Phê duyệt
- **Người phê duyệt**: Product Owner (Khanh Hoa).
- **Ngày phê duyệt**: 20/06/2026.
- **Quyết định**: Phê duyệt toàn bộ nội dung WBS (10 Work Packages) bao gồm các điều chỉnh bổ sung về Bảo mật, Quản trị Dữ liệu và Tuân thủ Nghị định 13/2023/NĐ-CP.
- **Tài liệu tham chiếu gốc**: [Kế hoạch Khảo sát Hiện trạng (Baseline Data Architecture)](project_plan.md).

---

## 2. Phạm vi Khảo sát đã Thống nhất
- **Nghiệp vụ (Business Operations)**: POS (Cửa hàng), CRM/CDP (Marketing/Khách hàng), ERP (Mua hàng/Tài chính), WMS/Logistics (Kho vận/Cung ứng).
- **Kỹ thuật (Technology/Application)**:
  - Database vật lý: Các cơ sở dữ liệu của ERP, POS, WMS, CDP, CRM và hệ thống lưu trữ Data Lakehouse/DWH.
  - Luồng tích hợp dữ liệu: Các cổng API thời gian thực, các tiến trình Batch Job, ETL/ELT và Message Queues kết nối giữa các hệ thống.
  - An toàn thông tin: Các cơ chế mã hóa (at rest/in transit), che giấu dữ liệu (data masking), phân quyền IAM/RBAC và ghi nhật ký kiểm toán (audit logs).
  - Tuân thủ Nghị định 13: Định vị khu vực lưu trữ Dữ liệu Cá nhân nhạy cảm (đơn thuốc, bệnh lý khách hàng), cơ chế Consent Management và DPIA.

---

## 3. Phân công Vai trò chịu trách nhiệm theo Work Packages (WPs)

| Mã WP | Tên Work Package | Thành viên chịu trách nhiệm | Trạng thái hiện tại |
| :--- | :--- | :--- | :--- |
| **WP1** | Chuẩn bị & Khởi động dự án | ea_consultant | **Hoàn thành (Done)** |
| **WP2** | Xác định & Lập bản đồ Stakeholders (RACI) | ea_consultant | **Đang thực hiện (In Progress)** |
| **WP3** | Khảo sát Danh mục Hệ thống & Nguồn dữ liệu | data_architect | Chờ kích hoạt (Backlog) |
| **WP4** | Khảo sát Cấu trúc, Từ điển & Phân loại Dữ liệu | data_architect | Chờ kích hoạt (Backlog) |
| **WP5** | Khảo sát Luồng thông tin & Tích hợp (Data Lineage) | data_engineer | Chờ kích hoạt (Backlog) |
| **WP6** | Đánh giá Bảo mật, Quản trị & Tuân thủ Nghị định 13 | ea_security_architect | Chờ kích hoạt (Backlog) |
| **WP7** | Mô hình hóa Kiến trúc Dữ liệu Hiện trạng (ArchiMate) | data_architect | Chờ kích hoạt (Backlog) |
| **WP8** | Tổng hợp Baseline & Đánh giá Gaps ban đầu | ea_consultant | Chờ kích hoạt (Backlog) |
| **WP9** | Đánh giá & Ký duyệt Baseline | ea_consultant | Chờ kích hoạt (Backlog) |
| **WP10**| Đảm bảo An toàn thông tin cho Hoạt động Khảo sát | ea_security_architect | Chờ kích hoạt (Backlog) |

---

## 4. Nguyên tắc Triển khai Kỹ thuật an toàn (WP10)
Để đảm bảo an toàn thông tin trong suốt quá trình khảo sát, Ban dự án cam kết tuân thủ các quy tắc sau:
1. **Quyền truy cập hạn chế**: Chỉ xin cấp tài khoản **Read-only** trên môi trường Sandbox/Staging của doanh nghiệp. Tuyệt đối không yêu cầu quyền write/edit dữ liệu hoặc quyền admin hệ thống.
2. **Không trích xuất Actual Data**: Đội ngũ khảo sát chỉ thu thập thông tin cấu trúc (DDL schema, metadata, data dictionary) và sơ đồ tích hợp. Tuyệt đối không tải về máy cá nhân hoặc trích xuất dữ liệu thực của khách hàng hay giao dịch kinh doanh.
3. **Mã hóa tài liệu nhạy cảm**: Toàn bộ sơ đồ kiến trúc chi tiết, cổng mạng IP, danh mục API và tài liệu bảo mật phải được lưu trữ trên SharePoint nội bộ có phân quyền và bảo mật MFA.
