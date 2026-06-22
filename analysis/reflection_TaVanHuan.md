# Reflection - Ta Van Huan

## Phần 1: Mapping bài giảng
| Lecture Concept | Module | Hàm cụ thể | Observation |
|----------------|--------|-------------|-------------|
| Semantic chunking | M1 | `chunk_semantic()` | Threshold nhóm các câu có cosine similarity cao lại với nhau, giữ được context tốt hơn split theo paragraph. |
| BM25 + Dense fusion | M2 | `reciprocal_rank_fusion()` | RRF giải quyết được trường hợp BM25 bắt keyword tốt nhưng Dense lại hiểu ngữ nghĩa tốt, khi merge lại cải thiện kết quả rõ rệt. |
| Cross-encoder reranking | M3 | `CrossEncoderReranker.rerank()` | Latency có thể tăng đôi chút nhưng precision của top-k context tăng đáng kể nhờ attention toàn cục giữa query và document. |
| RAGAS 4 metrics | M4 | `evaluate_ragas()` | Metric Context Recall dễ bị thấp nếu chunking và search không lấy được đủ context; Answer Relevancy phụ thuộc nhiều vào prompt. |
| Contextual embeddings | M5 | `_enrich_single_call()` | Giảm retrieval failure bằng cách sinh summary, tạo câu hỏi giả định và thêm metadata ngay trước khi embedding. |

## Phần 2: Khó khăn & giải quyết
- **Lỗi gặp phải**: `UnicodeEncodeError: 'charmap' codec can't encode characters...` và `ModuleNotFoundError: No module named 'qdrant_client'`
- **Cách debug**: Lỗi do print các ký tự emoji trong log trên hệ điều hành Windows với encoding mặc định không hỗ trợ. Kèm theo là việc quên chưa cài đặt đủ requirements trong môi trường. Cả hai lỗi được fix bằng cách thiết lập `PYTHONIOENCODING="utf-8"` và chạy lệnh `pip install -r requirements.txt`.
- **Kiến thức bổ sung**: Hiểu rõ hơn về cách hệ thống RAGAS sử dụng model đánh giá, và việc quản lý môi trường (environment variables) khi chạy pipeline trên các nền tảng khác nhau.

## Phần 3: Action Plan cho project
### Dự án: Hệ thống RAG hỗ trợ nhân sự nội bộ (HR Policy RAG)
- **RAG pipeline hiện tại**: Đang sử dụng Basic chunking (split by paragraph), và chỉ có Dense search cơ bản.
- **Known issues**: Trả lời sai các câu hỏi liên quan đến policy phức tạp hoặc các thuật ngữ đồng nghĩa. Context lấy lên có thể bị cắt ngang khiến LLM sinh ra thông tin không chính xác.

### Plan áp dụng
1. [x] **Chunking strategy**: Sử dụng Hierarchical chunking kết hợp Semantic chunking. Vì các policy thường dài, Parent chunk giúp giữ lại toàn bộ context, Child chunk giúp tăng precision khi query.
2. [x] **Search**: Hybrid (BM25 + Dense) kết hợp với RRF. Sẽ sử dụng `bge-m3` embedding để bắt semantics tiếng Việt tốt hơn, đồng thời giữ BM25 cho exact matching.
3. [x] **Reranking**: Bổ sung mô hình `bge-reranker-v2-m3` để filter lại top chunks, giảm rủi ro hallucination.
4. [x] **Evaluation**: Tích hợp RAGAS để đo lường định kì Answer Relevancy và Faithfulness mỗi khi thay đổi prompt hoặc file dữ liệu HR.
5. [x] **Enrichment**: Áp dụng Contextual Prepend và HyQA - bổ sung ngữ cảnh chung cho mỗi đoạn cắt và sinh sẵn các câu hỏi tiềm năng để rút ngắn khoảng cách semantics.

### Timeline
- Tuần 1: Hoàn thiện data pipeline: xử lý PDF/MD bằng Semantic Chunking + Enrichment.
- Tuần 2: Xây dựng Qdrant vector DB và API cho Hybrid Search, Reranking.
- Tuần 3: Tích hợp Frontend, đánh giá benchmark 100 câu hỏi test bằng RAGAS.
- Tuần 4: Chỉnh sửa các threshold (RRF k, chunk size), tối ưu và deploy system.
