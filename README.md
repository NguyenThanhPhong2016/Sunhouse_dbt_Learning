# Sunhouse_dbt_Learning

# Mô tả cấu trúc và cách sử dụng source code dbt ETL PIPELINE

## Mục Lục

1. [Giới thiệu](#1-giới-thiệu)
2. [Cấu trúc thư mục dự án](#2-cấu-trúc-thư-mục-dự-án)
    - [analyses](#analyses)
    - [dags](#dags)
    - [macros](#macros)
    - [models](#models)
    - [tests](#tests)
    - [scripts_create_table](#scripts_create_table)
    - [seeds](#seeds)
    - [variables_and_prameters](#variables_and_prameters)
    - [dbt_project.yml](#dbt_project.yml)
    - [profiles](#profiles)
    - [Dockerfile](#Dockerfile)
3. [Tài liệu tham khảo](#4-tài-liệu-tham-khảo)

---

# 1. Giới thiệu

Tài liệu này nhằm cung cấp hướng dẫn toàn diện về cách viết code cho luồng ETL sử dụng công cụ DBT. Qua tài liệu này, bạn sẽ tìm hiểu cách cấu hình các thành phần quan trọng của dự án DBT, bao gồm các macros, models, tests....

DBT là viết tắt của Data Build Tool, là công cụ mã nguồn mở hỗ trợ quá trình transform data trở nên nhanh và trở nên dễ dàng hơn.

Các nội dung chính trong tài liệu sẽ bao gồm:
- Cấu trúc thư mục dự án
- Hướng dẫn sử dụng các tệp trong thư mục

Hướng dẫn này sẽ giúp bạn từng bước thiết lập một môi trường dbt trên máy cá nhân. Cách viết và sửa code cho luồng ETL từ cơ bản đến nâng cao. Từ đó, có thể xử lý được các tình huống xảy ra trong quá trình vận hành luồng ETL hằng ngày.

---

# 2. Cấu trúc thư mục dự án

## analyses
TThư mục analyses/ là nơi bạn lưu trữ các truy vấn phân tích hoặc báo cáo tùy chỉnh mà bạn muốn chạy ngoài quy trình ETL chính.

Các truy vấn trong thư mục này có thể không phải là một phần của quá trình biến đổi dữ liệu chính thức nhưng rất hữu ích cho việc kiểm tra, phân tích nhanh hoặc báo cáo.

Các tệp .sql trong thư mục này không được chạy tự động bằng lệnh dbt run, bạn sẽ cần chạy chúng một cách thủ công khi cần.

Tham khảo cách sử dụng tại [đây](https://docs.getdbt.com/docs/build/analyses)

## macros
Thư mục macros/ chứa các macro - những khối mã SQL mà bạn có thể tái sử dụng trong các mô hình và snapshot.

dbt sử dụng Jinja để xây dựng các macro, giúp bạn có thể tạo ra những truy vấn SQL động và tiết kiệm thời gian bằng cách tái sử dụng mã.

Các macro có thể được định nghĩa trong các tệp .sql và được gọi ở bất kỳ đâu trong dự án dbt của bạn.

Tham khảo cách sử dụng tại [đây](https://docs.getdbt.com/docs/build/jinja-macros)

## models
Thư mục này dành cho các tệp SQL xác định các chuyển đổi dữ liệu. Các chuyển đổi này thường là một phần của quy trình lập mô hình dữ liệu.

TThư mục models/ là nơi chứa các mô hình (models) của dbt. Đây là phần cốt lõi của một dự án dbt, nơi bạn viết các truy vấn SQL để định nghĩa các mô hình dữ liệu của mình.

Mô hình là những bảng hoặc view được xây dựng từ dữ liệu thô (raw data) trong kho dữ liệu. Bạn viết các tệp .sql để thực hiện các truy vấn biến đổi và sắp xếp dữ liệu cho các báo cáo hoặc ứng dụng.

Các tệp .sql trong thư mục này chính là các truy vấn SQL mà dbt sẽ sử dụng để tạo ra các bảng trong cơ sở dữ liệu của bạn.

Tham khảo cách sử dụng tại [đây](https://docs.getdbt.com/reference/model-configs)

## tests

Thư mục tests/ chứa các kiểm thử dữ liệu tùy chỉnh mà bạn muốn thực hiện.

dbt cung cấp các kiểm thử mặc định như unique, not_null, relationships, nhưng bạn cũng có thể tạo các kiểm thử SQL tùy chỉnh tại thư mục này.

Các tệp .sql trong thư mục này chứa các truy vấn để kiểm tra xem các điều kiện bạn mong đợi có được thỏa mãn hay không.

Tham khảo cách sử dụng tại [đây](https://docs.getdbt.com/docs/build/data-tests)

## snapshots
Thư mục snapshots/ là nơi bạn tạo các bản snapshot của dữ liệu.

Snapshot là một ảnh chụp của một bảng tại một thời điểm cụ thể, giúp bạn theo dõi thay đổi dữ liệu theo thời gian. Snapshot rất hữu ích khi bạn cần lưu giữ các lịch sử thay đổi của dữ liệu.

Mỗi tệp trong thư mục này chứa cú pháp đặc biệt của dbt để định nghĩa cách snapshot dữ liệu.

Tham khảo cách sử dụng tại [đây](https://docs.getdbt.com/docs/build/snapshots)

## seeds
Thư mục seeds/ là nơi chứa các tệp CSV chứa dữ liệu tĩnh mà bạn muốn load vào cơ sở dữ liệu.

dbt hỗ trợ việc load dữ liệu từ các tệp CSV vào cơ sở dữ liệu, các tệp CSV này sẽ được lưu trong thư mục seeds/.
Các tệp CSV này có thể được sử dụng làm dữ liệu bổ sung cho quá trình biến đổi.

Tham khảo cách sử dụng tại [đây](https://docs.getdbt.com/docs/build/seeds)

## logs
Thư mục logs/ chứa các tệp log ghi lại quá trình chạy dbt.

Các tệp log này bao gồm thông tin về các mô hình đã được thực thi, thời gian chạy, và các lỗi nếu có.
Thư mục này giúp bạn dễ dàng xem lại lịch sử chạy và phát hiện lỗi trong quá trình phát triển dự án.

## targets
Thư mục target/ là nơi dbt lưu trữ các tệp output sau khi chạy.

Bao gồm các kết quả của các lệnh như dbt run, dbt test, và dbt docs generate.

Các bảng tạm (temporary tables), các tài liệu và tệp log bổ sung sẽ được lưu ở đây.

Tham khảo cách sử dụng tại [đây](https://docs.getdbt.com/reference/commands/compile)

## dbt_project.yml
Tệp dbt_project.yml là tệp cấu hình chính cho dự án dbt của bạn.

Trong tệp này, bạn định nghĩa các thiết lập cho dự án như tên mô hình, các thư mục chứa mô hình, và các thông số tùy chỉnh.
Bạn cũng có thể định nghĩa cấu hình chạy cho từng môi trường như dev, prod, v.v.

Tham khảo cách sử dụng tại [đây](https://docs.getdbt.com/reference/dbt_project.yml)

## profiles
Có khả năng chứa các cấu hình hồ sơ, có thể cho các môi trường khác nhau (ví dụ: phát triển, dàn dựng, sản xuất).

Tham khảo cách sử dụng tại [đây](https://docs.getdbt.com/docs/core/connect-data-platform/connection-profiles)

## scripts_create_table
Thư mục này có thể chứa các tập lệnh SQL để tạo trước các bảng cần thiết trong cơ sở dữ liệu.

## variables_and_prameters
Thư mục này có thể lưu trữ các tệp cấu hình hoặc tập lệnh xác định các biến và tham số được sử dụng trong toàn bộ dự án.

## Dockerfile
Tệp này chứa các hướng dẫn để xây dựng Docker image cho dự án, có thể được sử dụng để tạo môi trường phát triển hoặc sản xuất nhất quán.

Tham khảo cách sử dụng tại [đây](https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/)

## dags
Thường được sử dụng trong các dự án liên quan đến Apache Airflow, thư mục này sẽ chứa DAG xác định quy trình công việc.

Tham khảo cách sử dụng tại [đây](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dags.html)

---

# 4. Tài liệu tham khảo

Danh sách các tài liệu tham khảo liên quan:
- [Tài liệu dbt](https://docs.getdbt.com/docs/)
- [Source code github dbt trino](https://github.com/starburstdata/dbt-trino)

