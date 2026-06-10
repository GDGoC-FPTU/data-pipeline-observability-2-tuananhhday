[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112759&assignment_repo_type=AssignmentRepo)

# Day 10 Lab: Data Pipeline & Data Observability

**Student ID:** 2A202600758  
**Student Email:** tuananhnguyen28112005@gmail.com  
**Name:** Nguyễn Tuấn Anh

---

## Mô tả

Bài lab này xây dựng một quy trình ETL đơn giản để xử lý dữ liệu sản phẩm từ file `raw_data.json`. Pipeline thực hiện các bước: đọc dữ liệu, kiểm tra chất lượng dữ liệu, loại bỏ bản ghi không hợp lệ, chuẩn hóa dữ liệu, thêm thông tin quan sát được và lưu kết quả ra file `processed_data.csv`.

Ngoài phần ETL, bài lab còn có phần Data Observability thông qua stress test Agent với hai bộ dữ liệu: dữ liệu sạch (`processed_data.csv`) và dữ liệu rác (`garbage_data.csv`). Mục tiêu là quan sát tác động của chất lượng dữ liệu đến câu trả lời của Agent.

---

## Cách chạy (How to Run)

### 1. Cài đặt thư viện

```bash
pip install pandas pytest
```

### 2. Chạy ETL Pipeline

```bash
python solution.py
```

Sau khi chạy thành công, chương trình sẽ tạo file `processed_data.csv`.

### 3. Tạo dữ liệu rác

```bash
python generate_garbage.py
```

Lệnh này tạo file `garbage_data.csv`, chứa các lỗi dữ liệu như duplicate ID, sai kiểu dữ liệu, giá trị null và outlier.

### 4. Chạy Agent Simulation / Stress Test

```bash
python agent_simulation.py
```

Script này so sánh phản hồi của Agent khi dùng dữ liệu sạch và dữ liệu rác.

### 5. Chạy kiểm thử tự động

```bash
python -m pytest tests/test_autograder.py -v
```

---

## Cấu trúc thư mục

```text
solution.py             # ETL Pipeline script
raw_data.json           # Dữ liệu gốc
processed_data.csv      # Output sau khi xử lý ETL
generate_garbage.py     # Script tạo dữ liệu rác
garbage_data.csv        # Dữ liệu rác dùng cho stress test
agent_simulation.py     # Script mô phỏng Agent
experiment_report.md    # Báo cáo thí nghiệm
README.md               # File mô tả bài làm
tests/test_autograder.py # Bộ test chấm tự động
```

---

## Kết quả

Khi chạy `python solution.py`, pipeline xử lý dữ liệu như sau:

- Tổng số bản ghi ban đầu: 5
- Số bản ghi hợp lệ được giữ lại: 3
- Số bản ghi bị loại: 2
- Lý do bị loại: `price <= 0` hoặc thiếu `category`

Khi chạy stress test bằng `python agent_simulation.py`:

- Với Clean Data, Agent chọn `Laptop` với giá `$1200.0`.
- Với Garbage Data, Agent chọn sai `Nuclear Reactor` với giá `$999999` do dữ liệu có outlier.

Kết quả này cho thấy chất lượng dữ liệu ảnh hưởng trực tiếp đến độ chính xác của Agent.
