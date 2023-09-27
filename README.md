# Overview
- Developer: Phong Nguyen.
- Target release: 1 October, 2023.
- Epic: Building Data Pipelines for VN-Stocks
- Coach: Huy Do.

# Objective
- Purpose to join the stock market effeciently, get all stock data in a year, and automatically update latest data to predict and warning when the following index decreasing

# Goals
- Practices to work with data pipeline
- Practices to use the GCP service
- Output the result to another stackholder

# Overall requirement 
## VN Stock: API source **TCBS** và **SSI**
**Compute Engine (Ubuntu 20.04):**
- Dev thêm một module stock_subscribe.py cho phép bạn lựa chọn những mã cổ phiếu mà bạn quan tâm. Liên tục cập nhật chỉ số các mã sau mỗi một giờ (trong khung giờ giao dịch, ngoài khung giờ không chạy module này) và publish lên cloud pub/sub
**Cloud Storage**
- Tạo một bucket lưu dữ liệu chứng khoán
- Toàn bộ dữ liệu trong một năm trở lại đây được thu thập và lưu trên một file
- Dữ liệu chỉ số cổ phiếu hàng ngày sẽ được thu thập và lưu ở một file riêng với mỗi file là dữ liệu chứng khoán của toàn bộ mã cổ phiếu trong ngày.
**Dataproc:**
- Setup Spark, đọc dữ liệu từ Cloud Storage rồi tính toán lấy ra những mã cổ phiếu tăng trưởng ổn định nhất trong 3 tháng. Định nghĩa ổn định là chỉ số index trung bình là tăng lên và biên độ dao động không quá 10%. Sau đó kết quả được ghi vào BigQuery.
**Cloud Pub/Sub:**
Ghi nhận các mã cổ phiếu mà người dùng subscribe, được publish lên từ Compute Engine
**Cloud Function:**
- Trigger khi có dữ liệu mới trên GCS thì cập nhật vào BigQuery
- Trigger khi có dữ liệu mới publish trên Cloud pub/sub thì sẽ đọc và ghi mới vào BigQuery
**BigQuery:**
Thiết kế Data Warehouse để lưu trữ thông tin của tất cả mã cổ phiếu trong hơn 1 năm
**Data Studio:**
- Hiển thị line chart của các mã cổ phiếu mà bạn subscribe, cập nhật theo giờ
- Hiển thị line chart của những mã cổ phiếu tăng trưởng ổn định trong 3 tháng
**Email:**
- Gửi email thông báo khi có mã cổ phiếu nào đang subscribe giảm 10% giá trị so với giá bạn đặt kỳ vọng
- Gửi email báo khi có lỗi về luồng dữ liệu (Có kịch bản test)
**Airflow:**
- Pipeline lấy toàn bộ dữ liệu thị trường vào 16h hàng ngày
- Pipeline lấy chỉ số các mã cổ phiếu bạn subscribe sau mỗi 1h và publish lên Pub/Sub
- Pipeline tạo dashboard trên Data Studio, cập nhật sau mỗi một giờ
**Note:**
- Khi có lỗi xảy ra, luồng dữ liệu tự retry 3 lần
- Mỗi lần retry cách nhau 5 phút
- Gửi email báo lỗi khi hệ thống không retry thành công (có kịch bản để test trường hợp này)

**Reference**
https://thinhvu.com/2022/09/22/vnstock-api-tai-du-lieu-chung-khoan-python/
https://ilhamaulanap.medium.com/data-lake-with-pyspark-through-dataproc-gcp-using-airflow-d3d6517f8168
https://selectfrom.dev/automate-your-data-warehouse-with-airflow-on-gcp-b48dfe51360f
https://www.youtube.com/watch?v=jS2laqcPXuM

# Architecture

# Issues

# Suggestions for using those data

# Improving if have time need to do