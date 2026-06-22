# Phân tích Failure Analysis (Bottom-5)

Phân tích 5 câu hỏi có kết quả tệ nhất dựa trên Evaluation Pipeline bằng RAGAS.

| Câu hỏi | Metric tệ nhất | Điểm trung bình | Chẩn đoán (Diagnosis) | Đề xuất sửa chữa (Suggested Fix) |
|---------|---------------|------|----------------------|-----------------------------------|
| (Sẽ được điền sau khi có ragas_report.json) | | | | |
| (Sẽ được điền sau khi có ragas_report.json) | | | | |
| (Sẽ được điền sau khi có ragas_report.json) | | | | |
| (Sẽ được điền sau khi có ragas_report.json) | | | | |
| (Sẽ được điền sau khi có ragas_report.json) | | | | |

## Error Tree Diagnostic
Dựa theo cây lỗi sau:
- **Faithfulness thấp**: LLM bị hallucination → Cần siết chặt prompt, giảm nhiệt độ (temperature).
- **Context Recall thấp**: Truy xuất thiếu chunk quan trọng → Cần cải thiện thuật toán chunking hoặc bổ sung BM25.
- **Context Precision thấp**: Lấy lên quá nhiều chunk không liên quan → Cần cải thiện thuật toán Reranking hoặc metadata filter.
- **Answer Relevancy thấp**: Câu trả lời không khớp với câu hỏi → Cần thiết kế lại prompt template.
