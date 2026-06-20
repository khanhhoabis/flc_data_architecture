# Danh mục Hệ thống & Nguồn dữ liệu (Data Inventory Matrix) - FPT Long Châu

Tài liệu này thuộc gói công việc **WP3: Khảo sát Danh mục Hệ thống & Nguồn dữ liệu** trong chiến dịch xây dựng Kiến trúc Dữ liệu Hiện trạng (Baseline Data Architecture) của FPT Long Châu. Tài liệu thực hiện thống kê, phân loại và lập danh mục toàn bộ các hệ thống ứng dụng nghiệp vụ, cơ sở dữ liệu vật lý (SQL, NoSQL), môi trường lưu trữ và các nguồn dữ liệu ngoại vi đang vận hành tại doanh nghiệp.

---

## 1. Phương pháp Khảo sát & Phân loại

Danh mục này được xây dựng dựa trên việc phỏng vấn các đầu mối kỹ thuật (Lead DBA, Infrastructure Head) và chủ sở hữu dữ liệu (Data Owners) đã được xác định trong [Bản đồ Stakeholders & RACI (WP2)](stakeholder_raci_map.md), đồng thời tuân thủ các nguyên tắc an toàn thông tin của [Kế hoạch Khảo sát (WP10)](project_plan.md#6-ke-hoach-quan-ly-rui-ro-khao-sat):
- **Phân loại dữ liệu cá nhân theo Nghị định 13/2023/NĐ-CP**: 
  - **DLCN Cơ bản**: Họ tên, số điện thoại, email, địa chỉ, ngày sinh, giới tính.
  - **DLCN Nhạy cảm**: Thông tin đơn thuốc, lịch sử bệnh lý, thông tin thẻ bảo hiểm y tế, dữ liệu vị trí thời gian thực.
- **Phân cấp bảo mật**: 
  - `Public` (Công khai)
  - `Internal` (Nội bộ)
  - `Confidential` (Mật - Cần bảo vệ nghiêm ngặt)
  - `Restricted` (Tối mật - Giới hạn truy cập tối đa, chứa DLCN nhạy cảm)

---

## 2. Ma trận Danh mục Hệ thống & Nguồn dữ liệu (Data Inventory Matrix)

| STT | Tên Hệ thống (Ứng dụng) | Miền Dữ liệu (Domain) | Loại CSDL Vật lý (Engine) | Môi trường Hosting | Các Thực thể/Bảng chính (Core Tables/Entities) | Phân cấp Bảo mật | Nhận diện DLCN (Nghị định 13) | Chủ sở hữu (Data Owner) & Đầu mối kỹ thuật |
| :--- | :--- | :--- | :--- | :--- | :--- | :---: | :--- | :--- |
| **1** | **POS (Point of Sale)** | Sales & Retail Operations | Microsoft SQL Server (Cluster) | On-Premises (Private Cloud - DC Long Châu) | `bills`, `bill_details`, `store_inventories`, `shifts`, `cash_registers`, `promotions` | **Confidential** | DLCN Cơ bản (SĐT khách hàng mua lẻ, tên khách hàng trên hóa đơn). | - Trưởng phòng POS<br>- DBA POS & IT Retail Ops Support |
| **2** | **SAP S/4HANA** | Purchasing & Product Master | SAP HANA (Enterprise Edition) | On-Premises (DC Long Châu) & AWS (Staging/Sandbox) | `products` (MDM), `vendors`, `purchase_orders`, `goods_receipts`, `inventory_ledger`, `accounts_payable`, `accounts_receivable` | **Confidential** | Không chứa DLCN của khách hàng. Chứa thông tin nhân viên & đối tác kinh doanh (DLCN Cơ bản). | - Trưởng phòng Mua hàng & Quản lý Sản phẩm<br>- ERP Technical Lead & DBA ERP |
| **3** | **WMS (Warehouse Management)** | Supply Chain & Logistics | Oracle Database (RAC) | On-Premises (DC Long Châu) | `warehouses`, `bins`, `stock_items`, `inbound_shipments`, `outbound_shipments`, `picking_lists`, `inventory_adjustments` | **Internal** | Không chứa DLCN của khách hàng. Chỉ chứa thông tin nhân sự kho (DLCN Cơ bản). | - Trưởng phòng Chuỗi cung ứng<br>- DBA WMS & Technical Support Engineer |
| **4** | **CRM (Customer Relationship Management)** | Customer CRM & Loyalty | PostgreSQL (HA Setup) | FPT Cloud | `customers` (Profiles), `loyalty_points`, `customer_addresses`, `membership_tiers`, `interaction_logs`, `customer_complaints` | **Confidential** | DLCN Cơ bản (Họ tên, SĐT, Email, Địa chỉ, Ngày sinh, Hạng thành viên). | - Trưởng phòng Marketing & CRM<br>- DBA CRM |
| **5** | **CDP (Customer Data Platform)** | Customer CRM & Loyalty | MongoDB (NoSQL) & ClickHouse (Analytics) | FPT Cloud & Google Cloud Platform | `customer_360`, `prescriptions` (Đơn thuốc), `medical_histories` (Bệnh lý), `device_tokens`, `customer_segments`, `clickstream_events` | **Restricted** | **DLCN Nhạy cảm**: Thông tin đơn thuốc, bệnh lý khách hàng.<br>**DLCN Cơ bản**: SĐT, Email, thiết bị kết nối. | - Trưởng phòng Marketing & CRM<br>- CDP Data Engineer |
| **6** | **Ecommerce (Web/App Backend)** | Omnichannel E-commerce | PostgreSQL & Redis (Cache) | AWS (Amazon Web Services) | `online_orders`, `shopping_carts`, `web_users`, `product_reviews`, `delivery_logs`, `payment_transactions` | **Confidential** | DLCN Cơ bản (Họ tên, SĐT giao hàng, Địa chỉ giao hàng, Lịch sử đặt hàng trực tuyến). | - Trưởng phòng Thương mại Điện tử<br>- Web/App Data Engineer & DevOps Team |
| **7** | **Data Lakehouse (BigQuery)** | Data Platform & Analytics | Google BigQuery (Data Lakehouse) | Google Cloud Platform | **Bronze Layer**: Raw replication của POS, ERP, CRM, CDP, Web/App.<br>**Silver Layer**: ODS, Cleaned Data.<br>**Gold Layer**: DWH, Dimensional Model, Data Marts | **Confidential** / **Restricted** | Chứa cả DLCN Cơ bản và Nhạy cảm trong các bảng phân tích lịch sử giao dịch và chân dung khách hàng. | - Head of Data / Product Owner<br>- Lead Data Engineer & Platform Admin |

---

## 3. Đặc tả Kỹ thuật Chi tiết từng Hệ thống & Nguồn dữ liệu

### 3.1. Hệ thống POS (Point of Sale)
- **Vai trò nghiệp vụ**: Ghi nhận toàn bộ giao dịch bán lẻ tại mạng lưới hơn 1000 nhà thuốc Long Châu trên toàn quốc. Đây là nguồn sinh dữ liệu doanh thu chính.
- **Kiến trúc dữ liệu**:
  - Mỗi cửa hàng chạy một DB local (SQL Server Express) để đảm bảo bán hàng ngoại tuyến khi mất mạng.
  - Dữ liệu giao dịch được đồng bộ thời gian thực (Real-time Replication) về cụm Database trung tâm **POS Central DB** (Microsoft SQL Server AlwaysOn Availability Groups) tại Data Center chính.
- **An toàn thông tin**:
  - Mã hóa kết nối SSL/TLS giữa Store và DC.
  - Dữ liệu nhạy cảm như số điện thoại khách hàng được che giấu một phần (Masking) ở mức ứng dụng tại quầy, chỉ hiển thị đầy đủ cho dược sĩ khi có sự xác thực OTP từ khách hàng.

### 3.2. Hệ thống SAP S/4HANA (ERP)
- **Vai trò nghiệp vụ**: Quản lý chuỗi cung ứng mua hàng, danh mục sản phẩm (Master Data Product), thông tin nhà cung cấp (Vendors), kế toán tài chính và giá vốn.
- **Kiến trúc dữ liệu**:
  - Chạy trên nền tảng **SAP HANA** tối ưu hóa bộ nhớ (In-Memory Database), giúp xử lý các truy vấn báo cáo tài chính và kiểm kê kho tổng cực nhanh.
  - Là hệ thống nguồn tối cao (Single Source of Truth) đối với thông tin danh mục thuốc, mã hoạt chất, hạn sử dụng và phân loại nhóm dược phẩm.
- **An toàn thông tin**:
  - Phân quyền chặt chẽ theo chức năng (Role-Based Access Control - RBAC) của SAP.
  - Kết nối tích hợp với các hệ thống khác hoàn toàn qua cổng API Gateway được bảo mật và giám sát.

### 3.3. Hệ thống WMS (Warehouse Management System)
- **Vai trò nghiệp vụ**: Quản lý xuất, nhập, tồn kho, vị trí ô kệ tại các kho tổng khu vực và điều phối vận chuyển dược phẩm đến các cửa hàng.
- **Kiến trúc dữ liệu**:
  - Sử dụng **Oracle Database RAC** để đảm bảo tính sẵn sàng cao và khả năng chịu lỗi.
  - Lưu lượng ghi dữ liệu cực lớn do các thiết bị quét mã vạch (Handheld) liên tục gửi tín hiệu cập nhật vị trí hàng hóa trong kho.
- **An toàn thông tin**:
  - Cơ sở dữ liệu đặt trong vùng mạng cô lập (Intranet), chỉ truy cập được từ mạng nội bộ kho hoặc VPN chuyên dụng.

### 3.4. Hệ thống CRM & CDP (Customer Relationship & Data Platform)
- **Vai trò nghiệp vụ**: Quản lý hồ sơ thành viên khách hàng, chương trình tích điểm FPT Long Châu, tương tác chăm sóc khách hàng và cá nhân hóa trải nghiệm.
- **Kiến trúc dữ liệu**:
  - **CRM**: Sử dụng **PostgreSQL** để lưu trữ thông tin cấu trúc ổn định về tài khoản, điểm thưởng và khiếu nại.
  - **CDP**: Sử dụng **MongoDB** làm kho dữ liệu phi cấu trúc thu thập hành vi clickstream, thiết bị, và **ClickHouse** phục vụ phân tích phân khúc khách hàng theo thời gian thực.
  - **Lưu ý đặc biệt**: CDP là nơi lưu trữ **Thông tin đơn thuốc số hóa** và **Lịch sử bệnh lý**. Đây là khu vực chịu sự giám sát tuân thủ nghiêm ngặt nhất của Nghị định 13.
- **An toàn thông tin**:
  - Áp dụng mã hóa dữ liệu cấp độ cột (Column-Level Encryption) cho các trường thông tin bệnh lý và số điện thoại trong database.
  - Triển khai cơ chế Quản lý Sự đồng ý (Consent Management): Dữ liệu khách hàng chỉ được thu thập và xử lý khi khách hàng đồng ý qua ứng dụng hoặc OTP. Có cơ chế hỗ trợ rút lại sự đồng ý và xóa dữ liệu khi có yêu cầu.

### 3.5. Hệ thống Omnichannel (Website/Mobile App)
- **Vai trò nghiệp vụ**: Kênh bán hàng trực tuyến, cho phép người dùng tìm kiếm sản phẩm, đặt hàng, tải lên đơn thuốc của bác sĩ và theo dõi quá trình giao hàng.
- **Kiến trúc dữ liệu**:
  - Sử dụng **PostgreSQL** chạy trên AWS (Amazon RDS) để lưu trữ đơn hàng online.
  - Sử dụng **Redis** làm bộ nhớ đệm (Cache) cho danh mục sản phẩm và trạng thái phiên làm việc để đảm bảo tốc độ phản hồi nhanh dưới 100ms.
- **An toàn thông tin**:
  - Đạt chứng chỉ bảo mật thanh toán trực tuyến (nếu tích hợp cổng thanh toán) và mã hóa toàn bộ luồng truyền tải HTTPS.

### 3.6. Hệ thống Data Lakehouse
- **Vai trò nghiệp vụ**: Trung tâm phân tích dữ liệu toàn diện (BI & Reporting), phục vụ cho Ban Giám đốc đưa ra quyết định kinh doanh, dự báo nhu cầu hàng tồn kho và tối ưu hóa vận hành.
- **Kiến trúc dữ liệu**:
  - Xây dựng trên nền tảng **Google BigQuery** theo mô hình Medallion Architecture (Bronze -> Silver -> Gold):
    - *Bronze (Raw)*: Dữ liệu thô sao chép nguyên trạng từ các hệ thống POS, ERP, CRM thông qua các tiến trình CDC (Change Data Capture) hoặc Batch ETL.
    - *Silver (Cleaned/Enriched)*: Dữ liệu đã được chuẩn hóa kiểu dữ liệu, loại bỏ trùng lặp, giải mã các trường che giấu và liên kết thông tin.
    - *Gold (Curated)*: Dữ liệu đã được mô hình hóa theo dạng hình sao (Star Schema) phục vụ trực tiếp cho PowerBI và các mô hình học máy (Machine Learning).
- **An toàn thông tin**:
  - Áp dụng cơ chế phân quyền truy cập chi tiết đến từng dòng và cột (Row-Level Security và Column-Level Security).
  - Tự động che giấu thông tin cá nhân (Data Masking) đối với các tài khoản phân tích thông thường, chỉ mở khóa giải mã cho một số nhân sự được phê duyệt đặc quyền phục vụ nghiên cứu thị trường có kiểm soát.

---

## 4. Các Nguồn Dữ liệu Ngoại vi (External Data Sources)

Ngoài các hệ thống nội bộ, FPT Long Châu còn tích hợp và tiếp nhận dữ liệu từ các nguồn bên ngoài:

1. **Hệ thống Bảo hiểm Y tế Quốc gia**: Tích hợp kiểm tra thông tin thẻ bảo hiểm và quyền lợi của khách hàng khi mua thuốc thuộc danh mục bảo hiểm chi trả (Liên kết qua cổng API của Bộ Y tế).
2. **Hệ thống Hoạt chất & Dược thư Quốc gia (Bộ Y tế)**: Cập nhật thông tin khoa học về tương tác thuốc, liều lượng khuyến cáo và cảnh báo tác dụng phụ (Cập nhật định kỳ dạng file/API).
3. **Đối tác Vận chuyển (Giao Hàng Nhanh, Grab, Lalamove)**: Trao đổi dữ liệu trạng thái đơn hàng trực tuyến và vị trí của shipper (Liên kết REST API thời gian thực).
4. **Cổng Dịch vụ công / Hệ thống Xuất hóa đơn điện tử (e-Invoice)**: Gửi thông tin giao dịch để cấp mã hóa đơn tài chính hợp lệ từ Tổng cục Thuế.

---

## 5. Đánh giá Hiện trạng Hạ tầng Dữ liệu Vật lý & Độ tin cậy (DR/HA)

Dưới góc nhìn của Data Architect, hiện trạng hạ tầng lưu trữ dữ liệu của FPT Long Châu có các đặc điểm và rủi ro kỹ thuật sau:

- **Độ sẵn sàng cao (High Availability - HA)**: Các hệ thống cốt lõi nghiệp vụ (POS Central, SAP ERP, WMS) đều được thiết kế chạy trên cụm máy chủ Cluster hoặc RAC, đảm bảo tính sẵn sàng hoạt động 99.9%. Hệ thống POS tại cửa hàng có DB local giúp hoạt động ổn định cả khi mất kết nối WAN.
- **Phòng chống thảm họa (Disaster Recovery - DR)**: Đã thiết lập cơ chế sao lưu (Backup) tự động hàng ngày và replicate dữ liệu sang Data Center dự phòng (DR Site). Tuy nhiên, cần kiểm tra định kỳ quy trình khôi phục dữ liệu (Restore) để đảm bảo thời gian phục hồi (RTO) và điểm phục hồi (RPO) đáp ứng cam kết vận hành.
- **Thách thức về Tích hợp**: Do phân tách hạ tầng giữa On-Premises (POS, ERP, WMS) và Cloud (CRM, CDP, AWS E-commerce), việc đồng bộ dữ liệu giữa các môi trường đòi hỏi băng thông lớn và kiểm soát an ninh nghiêm ngặt tại các chốt kiểm soát mạng (Firewalls, NAT Gateways). Đây sẽ là nội dung khảo sát chi tiết trong **WP5: Khảo sát Luồng thông tin & Tích hợp**.

---

## 6. Kế hoạch Triển khai Tiếp theo

Với việc hoàn thành Danh mục Hệ thống & Nguồn dữ liệu (WP3), đội ngũ EA có đầy đủ thông tin về các Database vật lý để sẵn sàng chuyển sang:
- **WP4 (Khảo sát Cấu trúc, Từ điển & Phân loại Dữ liệu)**: Tiến hành thu thập tài liệu mô tả bảng (DDL schema) và phân loại bảo mật chi tiết đến từng trường thông tin, đặc biệt là lập bản đồ định vị chính xác vị trí lưu trữ dữ liệu cá nhân nhạy cảm trong CRM, CDP và POS.
- **WP5 (Khảo sát Luồng thông tin & Tích hợp)**: Lập bản đồ các đường đi của dữ liệu (ETL/API) kết nối giữa 7 hệ thống cốt lõi trên.
