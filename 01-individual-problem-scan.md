# 01 — Individual Problem Scan
Bối cảnh: Xây dựng và đánh giá (Evaluation/Debug) hệ thống QA Pháp luật & RAG Pipeline.

## Scan rộng

| # | Lăng kính | Problem quan sát được | Ai đang đau? | Dấu hiệu thật (Tần suất, thời gian mất, hậu quả...) |
|---|---|---|---|---|
| 1 | Lặp lại | Khi hệ thống QA trả lời sai, phải đọc cả đoạn luật được retrieve để xem có đúng context hay không. | AI Engineer | Phải đọc lại từ 5-10 đoạn luật tương tự nhau trên mỗi query lỗi. |
| 2 | Tốn thời gian | Văn bản luật dài và nhiều điều khoản chéo khiến việc kiểm tra relevance của retrieval rất chậm. | AI Engineer | Mất khoảng ~15 phút cho mỗi query để đối chiếu thủ công. |
| 3 | AI tốt hơn | Khó hiểu lý do vì sao hệ thống chọn điều luật A thay vì B (hai điều luật có độ tương đồng embedding gần nhau nhưng ý nghĩa pháp lý khác nhau). | AI Engineer | Embedding rất gần (high similarity score) nhưng ngữ nghĩa thực tế khác nhau hoàn toàn. |
| 4 | Pain từ người khác | Người dùng hỏi mơ hồ (ví dụ: "tôi có được tự ý nghỉ việc không") nhưng luật quy định cần context rất cụ thể (loại hợp đồng, thời gian báo trước). | End-user, AI Engineer | Query thiếu thông tin -> Hệ thống trả về kết quả mơ hồ hoặc sai lệch. |
| 5 | Tốn thời gian | Phải đọc nhiều điều khoản chéo trong cùng một văn bản hoặc nhiều văn bản để hiểu đầy đủ ngữ cảnh (luật không nằm ở một điều duy nhất). | AI Engineer | Phải lần mò liên kết từ 2-3 điều luật liên quan mới ra câu trả lời đầy đủ. |
| 6 | Lặp lại | Khi test nhiều query lỗi cùng lúc, phải kiểm tra thủ công xem câu trả lời mới sinh ra có thực sự chính xác theo luật không. | AI Engineer | Phải đối chiếu lặp đi lặp lại từng câu trả lời với ground truth (đáp án chuẩn). |
| 7 | AI tốt hơn | Khó tổng hợp nhiều điều luật rời rạc thành một câu trả lời thống nhất, đúng logic pháp lý (không bị mâu thuẫn giữa luật cũ và mới). | AI Engineer | LLM trả lời bị thiếu ý, mâu thuẫn logic hoặc trích dẫn sai văn bản gốc. |
| 8 | Pain từ người khác | Người dùng không chuyên không hiểu ngôn ngữ pháp lý trang trọng nên không thể tự đánh giá output của AI là đúng hay sai. | End-user | User cần một câu giải thích bình dân hóa nhưng hệ thống chỉ hiển thị nguyên văn luật thô. |
| 9 | Tốn thời gian | Khi debug hệ thống, phải liên tục cross-check thủ công giữa câu trả lời của model và nội dung file luật gốc. | AI Engineer | Phải mở đồng thời nhiều tab trình duyệt chứa văn bản luật gốc để tra cứu chéo. |
| 10 | Lặp lại | Phải đọc đi đọc lại cùng một văn bản luật khi gặp các query tương tự nhau vì hệ thống không có cơ chế ghi nhớ context debug. | AI Engineer | Tốn thêm 10-15 phút lặp lại quy trình đọc hiểu cho cùng một vấn đề đã xử lý trước đó. |

---

Vì sao phần scan này mạnh:

- Mỗi vấn đề đều xác định rõ đối tượng chịu đau (Actor) là AI Engineer/Data Engineer/End-user và đi kèm "Dấu hiệu thật" định lượng rất chi tiết (thời gian mất, tần suất lặp, lỗi hệ thống cụ thể).
- Đặc biệt bám sát thực tế phát triển dự án AI chuyên sâu: Nỗi đau thực sự được bóc tách từ khía cạnh "Explainability" (tính giải thích được của mô hình) và độ phức tạp của logic pháp lý chéo, chứ không chỉ dừng lại ở các bài toán debug code hay hạ tầng RAG kỹ thuật chung chung.
- Không bị rơi vào bẫy "Solution-first" (chưa nghĩ giải pháp chatbot/agent vội vàng), mà tập trung hoàn toàn vào việc mổ xẻ quy trình hiện hành (Current workflow) và chỉ ra đúng nút thắt (Bottleneck) lớn nhất của con người.

---

## Top 3

| Rank | Problem | Vì sao chọn | Điều còn chưa chắc |
|---|---|---|---|
| 1 | Khi hệ thống QA pháp luật trả lời sai, AI engineer phải đọc nhiều điều luật liên quan để verify output(problem 1) | Deep + đúng domain pháp lý phức tạp, tốn thời gian debug lớn nhất. | Đo lường độ tin cậy của phần giải thích tự động thế nào. |
| 2 | AI Engineer khó hiểu vì sao hệ thống retrieval chọn điều luật A thay vì B(problem 3) | Bài toán cốt lõi về Explainability của AI RAG. | Khó hiển thị trực quan các trọng số semantic weights cho dev hiểu. |
| 3 | AI Engineer khó thiết kế prompt/workflow để tổng hợp nhiều điều luật thành câu trả lời thống nhất, không mâu thuẫn(problem 7) | Bài toán khó về khả năng Reasoning & Synthesis của LLM. | Độ trễ (latency) khi chạy multi-step reasoning kiểm tra logic chéo. |

---

## Problem Card #1 — Kiểm tra điều luật để verify output hệ thống QA

**Problem:**  
Khi hệ thống QA pháp luật trả lời sai hoặc không chuẩn, AI engineer phải đọc và kiểm tra thủ công nhiều điều luật liên quan để xác nhận tính đúng đắn, khiến việc debug tốn rất nhiều thời gian.

**Actor:**  
AI Engineer phát triển hệ thống Legal QA.

**Thời điểm / Bối cảnh:**  
Giai đoạn debug, đánh giá chất lượng hệ thống sau khi chạy thử các bộ câu hỏi kiểm thử (Test queries).

**Current workflow:**  
1. Nhận query kiểm thử bị lỗi.
2. Xem câu trả lời (output) hiện tại của hệ thống.
3. Xem các đoạn luật (chunks) được hệ thống retrieve lên.
4. Mở văn bản luật gốc (file PDF hoặc website vbpl.vn).
5. Đọc các điều khoản liên quan được dẫn chiếu trong đoạn luật đó.
6. So sánh ngữ nghĩa, logic giữa luật gốc và câu trả lời của AI.
7. Xác định chính xác lỗi nằm ở bước nào (do retrieve sai hay do LLM sinh sai logic).
8. Tiến hành sửa code/prompt/embedding.

**Bottleneck:**  
Bước 4, 5 & 6 - Đọc hiểu luật gốc (văn phong pháp lý phức tạp), tự liên kết và xâu chuỗi nhiều điều khoản dẫn chiếu chéo để đối chiếu (mất khoảng 20-30 phút cho mỗi query lỗi).

**Impact:**  
Làm chậm chu kỳ thử nghiệm (iteration cycle) của mô hình. Mỗi ngày một kỹ sư chỉ kiểm thử tối đa được 10-15 query lỗi, gây nghẽn tiến độ ra mắt tính năng.

**Success metric:**  
Giảm thời gian kiểm thử và debug mỗi query từ 30 phút xuống dưới 10 phút. Giảm số lượng văn bản luật gốc cần mở đọc thủ công từ 3-5 văn bản xuống chỉ còn 1-2 văn bản.

**Non-AI alternative:**  
Đánh dấu (highlight) thủ công các điều luật quan trọng trong file làm việc, viết tay bảng ghi chú liên kết giữa các điều luật thường xuyên gặp lỗi.

**AI hypothesis:**  
AI hỗ trợ tóm tắt các điều luật chéo liên quan, giải thích lý do Rerank chọn đoạn này, và tự phát hiện điểm lệch logic/mâu thuẫn giữa luật gốc và câu trả lời của LLM. Dev review bản phân tích của AI để ra quyết định fix code nhanh hơn.

**Quick gut:**  
Workflow.

### Draft current workflow

```text
CURRENT STATE: ~30 phút / query debug

+---------------+     +-----------------------+     +---------------------+
| 1. Nhận query | --> | 2. Đọc output/chunks  | --> | 3. Tìm mở luật gốc  |
|   (1 phút)    |     |   (4 phút)            |     |   (5 phút)          |
+---------------+     +-----------------------+     +---------------------+
                                                               |
                                                               v
+---------------+     +-----------------------+     +---------------------+
| 8. Fix code   | <-- | 7. Xác định lỗi       | <-- | 4, 5, 6. Đọc hiểu   |
|               |     |   (5 phút)            |     | & đối chiếu chéo    |
|               |     |                       |     | (15 p) *BOTTLENECK* |
+---------------+     +-----------------------+     +---------------------+
```

### Draft future workflow

```text
FUTURE STATE: ~10 phút / query debug

+---------------+     +-----------------------+     +---------------------+
| 1. Nhận query | --> | 2. AI tự động tóm tắt | --> | 3. Dev review bảng  |
|   (1 phút)    |     | chéo & verify (3 p)   |     | phân tích (3 phút)  |
+---------------+     +-----------------------+     +---------------------+
                                                               |
                                                               v
                      +-----------------------+     +---------------------+
                      | Fix hệ thống          | <-- | 4. Xác nhận lỗi     |
                      |                       |     |   (2 phút)          |
                      +-----------------------+     +---------------------+
```

Fallback: AI verify sai/thiếu logic -> Click deep-link mở PDF luật gốc có sẵn trên UI debug để tự đối chiếu thủ công.

---

## Problem Card #2 — Explainability Gap — Khó hiểu lý do hệ thống chọn điều luật A thay vì B

**Problem:**  
AI Engineer mất rất nhiều thời gian phân tích log để hiểu tại sao bộ truy xuất (retrieval) lại ưu tiên điều luật A thay vì điều luật B khi cả hai có chỉ số embedding tương đồng nhưng ý nghĩa pháp lý khác nhau hoàn toàn.

**Actor:**  
AI Engineer tối ưu RAG/retrieval pipeline

**Thời điểm / Bối cảnh:**  
Khi điều chỉnh (fine-tune) mô hình Embedding hoặc Reranker để cải thiện độ chính xác tìm kiếm.

**Current workflow:**  
1. Phát hiện trường hợp hệ thống retrieve sai điều luật (ví dụ: lấy điều luật cũ đã hết hiệu lực hoặc lấy sai đối tượng áp dụng).
2. Xuất bảng điểm tương đồng (cosine similarity scores) của Top 10 kết quả.
3. Đọc thủ công nội dung của các điều luật trong Top 10.
4. Cố gắng suy luận xem từ khóa hoặc ngữ cảnh nào trong query đã làm lệch vector embedding.
5. Viết testcase bổ sung và chạy lại bộ benchmark.

**Bottleneck:**  
Bước 3 & 4 - Không có công cụ trực quan hóa (visualize) hoặc giải thích các trọng số ngữ nghĩa (semantic weights) tác động đến kết quả tìm kiếm của AI, dev phải đoán mò suy luận của mô hình.

**Impact:**  
Gây khó khăn lớn trong việc cải thiện độ chính xác tìm kiếm (search precision). Không biết mô hình đang hiểu sai ở đặc trưng ngữ nghĩa nào để tinh chỉnh prompt hoặc embedding.

**Success metric:**  
Giảm thời gian tìm ra nguyên nhân lệch ngữ nghĩa từ 60 phút xuống dưới 15 phút; có bản phân tích trực quan các từ khóa/ngữ cảnh đóng vai trò quyết định trong việc chấm điểm vector.

**Non-AI alternative:**  
In log điểm số ra file CSV, dùng Excel để lọc và vẽ biểu đồ phân phối điểm số thủ công.

**AI hypothesis:**  
Sử dụng một mô hình LLM giải thích (Explainer Model) để phân tích so sánh sự khác biệt ngữ nghĩa giữa Query, Điều A và Điều B, chỉ ra các từ khóa mang tính quyết định luật pháp làm thay đổi hướng vector.

**Quick gut:**  
Workflow.

### Draft current workflow

```text
CURRENT STATE: 85 phút

+-------------------+     +-------------------------+     +--------------------------+
| 1. Phát hiện lỗi  | --> | 2. Trích similarity log | --> | 3, 4. Đọc so sánh nghĩa  |
|   (5 phút)        |     |   (10 phút)             |     | & đoán mò lý do          |
+-------------------+     +-------------------------+     | (60 p) *BOTTLENECK*      |
                                                          +--------------------------+
                                                                       |
                                                                       v
                                                          +--------------------------+
                                                          | 5. Viết testcase chạy lại|
                                                          |   (10 phút)              |
                                                          +--------------------------+
```

### Draft future workflow

```text
FUTURE STATE: 10 phút

+-------------------+     +-------------------------+     +--------------------------+
| 1. Phát hiện lỗi  | --> | 2. AI Explainer phân    | --> | 3. Dev hiểu nguyên nhân  |
|   (2 phút)        |     | tích trực quan (3 p)    |     | & định hướng prompt (5 p)|
+-------------------+     +-------------------------+     +--------------------------+
```

Fallback: AI Explainer giải thích không rõ -> Dev quay lại trích xuất log thủ công và so sánh nghĩa như quy trình cũ.

---

## Problem Card #3 — Khó tổng hợp thông tin chéo giữa các điều luật thành câu trả lời thống nhất

**Problem:**  
AI Engineer khó thiết kế prompt/workflow để tổng hợp nhiều điều luật thành câu trả lời thống nhất, không mâu thuẫn

**Actor:**  
AI Engineer thiết kế Prompt & Agentic Workflow.

**Thời điểm / Bối cảnh:**  
Khi cấu hình giai đoạn Sinh (Generation) trong RAG để xử lý các câu hỏi phức tạp cần kết hợp nhiều văn bản pháp luật.

**Current workflow:**  
1. Đưa 3-5 đoạn văn bản luật được retrieve vào prompt context.
2. Thử nghiệm viết Prompt để hướng dẫn LLM tổng hợp thông tin.
3. Chạy thử nghiệm trên 20 kịch bản câu hỏi phức tạp.
4. Đọc kỹ từng câu trả lời để phát hiện các lỗi: LLM lấy điều kiện của luật A áp dụng cho hậu quả của luật B, hoặc bỏ sót các điều kiện loại trừ.
5. Sửa đi sửa lại Prompt (Prompt engineering) theo cách thử-và-sai (trial-and-error).

**Bottleneck:**  
Đọc dò logic từng câu trả lời và sửa prompt trial-and-error mất khoảng 65 phút/trial

**Impact:**  
Gây rủi ro pháp lý cao cho người dùng cuối nếu hệ thống đưa ra câu trả lời sai lệch hoặc mâu thuẫn logic. Quá trình phát triển prompt tốn nhiều thời gian và không ổn định.

**Success metric:**  
Tăng tỷ lệ câu trả lời tổng hợp đúng logic pháp lý từ 50% lên trên 90%; rút ngắn số lần lặp Prompt Engineering từ 15 lần thử xuống còn dưới 3 lần.

**Non-AI alternative:**  
Viết các bộ luật lệ (hardcoded rules) để ghép các đoạn text lại với nhau (nhưng cấu trúc câu sẽ vô cùng cứng nhắc và không tự nhiên).

**AI hypothesis:**  
Thiết kế cấu trúc suy luận đa bước (Multi-step Reasoning Chain hoặc Chain-of-Thought) cho LLM: Đầu tiên phân rã các điều kiện của từng luật -> lập ma trận logic chéo -> sinh câu trả lời nháp -> dùng LLM tự kiểm tra sự nhất quán logic chéo trước khi xuất output.

**Quick gut:**  
Workflow.

### Draft current workflow

```text
CURRENT STATE: 100 phút / trial

+--------------------+     +-------------------------+     +--------------------------+
| 1. Lắp prompt      | --> | 2. Chạy 20 kịch bản test| --> | 3, 4. Đọc dò logic từng  |
|   (10 phút)        |     |   (15 phút)             |     | câu & sửa Prompt thử-sai |
+--------------------+     +-------------------------+     |   (65 p) *BOTTLENECK*    |
                                                           +--------------------------+
                                                                        |
                                                                        v
                                                           +--------------------------+
                                                           | 5. Chạy benchmark lại    |
                                                           |   (10 phút)              |
                                                           +--------------------------+
```

### Draft future workflow

```text
FUTURE STATE: 15 phút / trial

+--------------------+     +-------------------------+     +--------------------------+
| 1. Setup multi-step| --> | 2. AI tự động verify    | --> | 3. Dev duyệt nhanh kết   |
| reasoning (5 phút) |     | logic & nhất quán (5 p) |     | quả tổng hợp (5 phút)    |
+--------------------+     +-------------------------+     +--------------------------+
```

Fallback: AI verify logic không tự tin -> Dev phải so sánh thủ công câu trả lời với các điều luật được trích dẫn để đảm bảo tính chính xác.

---

## Chọn card muốn pitch nhất

Card tôi muốn pitch nhất: Card #1 - Kiểm tra điều luật để verify output hệ thống QA

Vì sao: Đây là vấn đề chân thực, đau đớn nhất đối với bất kỳ AI Engineer nào làm trong lĩnh vực Pháp luật. Nó không chỉ đơn thuần là việc sửa lỗi code, mà chạm đúng vào nút thắt lớn nhất của ngành AI x Luật chính là khả năng giải thích được (Explainability) và sự phức tạp của các mối liên kết pháp lý chéo. Đề tài này rất sâu, độc đáo và cực kỳ thu hút sự thảo luận từ nhóm.

Câu hỏi tôi muốn nhóm challenge: Làm thế nào để định nghĩa một "Success Metric" chính xác để đo lường khả năng "giải thích logic" của AI khi hỗ trợ dev debug? Làm sao để chứng minh việc dùng AI hỗ trợ giải thích giúp dev phát hiện lỗi nhanh hơn so với đọc luật gốc truyền thống?
