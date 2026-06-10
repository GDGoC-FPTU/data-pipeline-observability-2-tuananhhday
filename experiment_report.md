# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202600758  
**Name:** Nguyễn Tuấn Anh  
**Email:** tuananhnguyen28112005@gmail.com  
**Date:** 10/06/2026

---

## 1. Kết quả thí nghiệm

Chạy `agent_simulation.py` với 2 bộ dữ liệu và ghi lại kết quả:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200.0. | 9 | Dữ liệu đã được xử lý qua ETL nên `category` hợp lệ, `price` hợp lệ, Agent tìm đúng sản phẩm thuộc nhóm electronics có giá cao nhất trong dữ liệu sạch. |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 2 | Dữ liệu rác có outlier rất lớn trong nhóm electronics nên Agent bị kéo sang câu trả lời sai và không thực tế. |

---

## 2. Phân tích & nhận xét (Analysis)

### Tại sao Agent trả lời sai khi dùng Garbage Data?

Agent trả lời sai khi dùng Garbage Data vì chất lượng dữ liệu đầu vào không được kiểm soát. Trong file dữ liệu rác có nhiều vấn đề như duplicate ID, sai kiểu dữ liệu khi `price` là chữ `"ten dollars"`, giá trị null, và đặc biệt là outlier `Nuclear Reactor` với giá `999999` trong category electronics. Agent simulation chỉ lọc theo category và chọn sản phẩm có price cao nhất, nên outlier này làm kết quả bị sai lệch. Nếu pipeline không validate và transform dữ liệu trước, Agent có thể đưa ra câu trả lời nghe có vẻ hợp lý nhưng thực tế lại không phù hợp với ngữ cảnh.

---

## 3. Kết luận

**Quality Data > Quality Prompt?** Đồng ý.

Prompt tốt có thể giúp Agent hiểu câu hỏi rõ hơn, nhưng nếu dữ liệu nguồn bị sai, thiếu, trùng lặp hoặc có outlier thì câu trả lời vẫn có thể sai. Vì vậy, cần có ETL pipeline và data observability để làm sạch, kiểm tra và theo dõi chất lượng dữ liệu trước khi đưa dữ liệu vào Agent.
