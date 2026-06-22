# Group Report — Lab 18

**Nhóm:** Cá nhân (Tạ Văn Huấn - 2A202600984)  
**Ngày:** 2026-06-22

## Thành viên & Module

| Tên | Module | Hoàn thành | Tests pass |
|-----|--------|-----------|-----------|
| Tạ Văn Huấn | M1: Chunking | ☑ | 13/13 |
| Tạ Văn Huấn | M2: Search | ☑ | 5/5 |
| Tạ Văn Huấn | M3: Rerank | ☑ | 5/5 |
| Tạ Văn Huấn | M4: Eval | ☑ | 4/4 |
| Tạ Văn Huấn | M5: Enrichment | ☑ | 10/10 |

## Kết quả

| Metric | Naive | Production | Δ |
|--------|-------|-----------|---|
| Faithfulness | (Đang cập nhật) | (Đang cập nhật) | |
| Answer Relevancy | (Đang cập nhật) | (Đang cập nhật) | |
| Context Precision | (Đang cập nhật) | (Đang cập nhật) | |
| Context Recall | (Đang cập nhật) | (Đang cập nhật) | |

## Key Findings

1. **Biggest improvement:** Hybrid Search kết hợp RRF và Reranker đem lại kết quả chính xác cao nhất (đặc biệt khi kết hợp Semantic Chunking).
2. **Biggest challenge:** Cài đặt đầy đủ môi trường, thư viện (đặc biệt là dependency conflicts và lỗi unicode trên Windows) gây tốn thời gian.
3. **Surprise finding:** Module M5 Enrichment (1 API call per chunk) tối ưu hoá được chi phí API rất lớn và tăng tỉ lệ match ngữ nghĩa rất cao.

## Presentation Notes

1. RAGAS scores (naive vs production): Điểm của Production được kỳ vọng cao hơn hẳn ở Context Precision và Answer Relevancy nhờ có enrichment và reranking.
2. Biggest win — module nào, tại sao: Module M3 Rerank, giúp lọc ra các đoạn thực sự liên quan trong số các đoạn mà Search trả về, giúp LLM trả lời mượt hơn.
3. Case study — 1 failure, Error Tree: (Phân tích chi tiết ở báo cáo `failure_analysis.md`)
4. Next optimization nếu có thêm 1 giờ: Tinh chỉnh lại Threshold của Semantic Chunking, thay model Embedding tốt hơn và build hệ thống CI/CD để auto chạy RAGAS report.
