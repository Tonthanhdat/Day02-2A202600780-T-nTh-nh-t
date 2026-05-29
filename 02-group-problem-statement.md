## Chủ đề nhóm chọn

```text
Đọc và kiểm tra nhiều điều luật để verify output của hệ thống Legal QA.
```

---

# Phase 3 — Group Convergence

## 3.1. Danh sách 15 candidate problems

| # | Người đưa ra | Candidate problem | Actor chính | Điểm nghẽn |
|---|---|---|---|---|
| 1 | Huy | Tìm/lọc paper cho khóa luận hoặc nghiên cứu | Sinh viên nghiên cứu | Đọc abstract, lọc paper mất 25-40 phút/lần |
| 2 | Huy | Chỉnh format báo cáo đúng template | Sinh viên, thực tập sinh | Check heading/font/citation/numbering thủ công |
| 3 | Huy | Viết lại template thuật toán khi code giải thuật | Sinh viên IT | Search/copy/sửa template mất 10-20 phút |
| 4 | Dương | Tóm tắt meeting notes thành action items | Project leader, team member | Đọc notes dài và giao task mất 40-60 phút |
| 5 | Dương | Review 4-5 papers trước khi trình bày | Project leader, team member | Đọc, lọc ý, so sánh paper mất 60-120 phút |
| 6 | Dương | Ý tưởng chức năng trừu tượng, dev phải hỏi lại | Developer, project leader | Làm rõ input/output/flow/edge case |
| 7 | Tùng | Tìm lại rationale quyết định cũ trong Slack/Discord | PM, Dev, Tech Lead | Search chat không reconstruct được timeline |
| 8 | Tùng | Gom feedback đa kênh thành action items | PM Intern, PM | Gom, deduplicate feedback mất 1-1.5 giờ |
| 9 | Tùng | Tổng hợp Jira/Slack/GitHub viết Weekly Report | Coordinator, Tech Lead | Tổng hợp context và viết narrative |
| 10 | Đạt | Verify output sai của Legal QA bằng cách đọc nhiều điều luật | AI Engineer Legal QA | Đọc luật gốc, đối chiếu chéo mất 20-30 phút/query |
| 11 | Đạt | Khó hiểu vì sao retrieval chọn điều luật A thay vì B | AI Engineer RAG | Đọc top-k và đoán lệch semantic |
| 12 | Đạt | Khó tổng hợp nhiều điều luật thành answer thống nhất | AI Engineer Prompt/RAG | Đọc dò logic và sửa prompt trial-and-error |
| 13 | Khải | Gom deadline/task/lịch dạy thành kế hoạch tuần | Sinh viên nhiều vai trò | Thông tin rải rác nhiều kênh |
| 14 | Khải | Chuẩn bị tài liệu dạy thêm cá nhân hóa | Gia sư THPT | Chọn bài, soạn lời giải mất 60 phút/buổi |
| 15 | Khải | Biến chat/issue/screenshot thành checklist dev | Developer online | Requirement rải rác, mất 30 phút nối context |

## 3.2. Gom trùng / cluster

| Cluster | Candidates included | Pattern chung |
|---|---|---|
| A — Paper / Research Review | Huy #1; Dương #5 | Đọc nhiều paper, lọc ý chính, chọn nguồn phù hợp |
| B — Notes / Report / Summary | Dương #4; Tùng #9 | Tổng hợp thông tin dài/rải rác thành report/action items |
| C — Requirement / Product Context | Dương #6; Tùng #7; Tùng #8; Khải #15 | Gom context từ chat/issue/feedback thành decision/checklist/action items |

## 3.3. Shortlist

| Candidate | Vì sao vào shortlist | Rủi ro |
|---|---|---|
| Đạt #10 — Verify output Legal QA | Actor rõ, workflow rõ, bottleneck cụ thể, impact đo được | Cần sample query lỗi, chunks và luật gốc |
| Tùng #8 — Gom feedback đa kênh | Workflow thực tế, dễ đo thời gian, dễ demo | Cần dữ liệu giả lập đủ giống thật |
| Huy #1 + Dương #5 — Paper review | Pain phổ biến, nhóm hiểu domain | Scope dễ rộng |

## 3.4. Score

| Candidate | Actor rõ | Workflow rõ | Pain có evidence | Impact đo được | Làm trong lab | So sánh R/W/A | Nhóm hiểu domain | Tổng |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Đạt #10 — Verify output Legal QA | 5 | 5 | 5 | 5 | 4 | 5 | 4 | 33 |
| Tùng #8 — Gom feedback đa kênh | 5 | 5 | 4 | 5 | 4 | 4 | 4 | 31 |
| Huy #1 + Dương #5 — Paper review | 5 | 4 | 4 | 4 | 5 | 4 | 5 | 31 |

## Candidate nhóm chọn

```text
Đạt #10 — Đọc và kiểm tra nhiều điều luật để verify output của hệ thống Legal QA.
```

## Lý do chọn

```text
Candidate này có actor rõ, workflow rõ, bottleneck cụ thể và impact đo được.
AI Engineer phải mất 20-30 phút/query lỗi để đọc luật gốc, đối chiếu retrieved chunks và xác định lỗi retrieval/generation.
Bài toán phù hợp để so sánh Rule / Workflow / Agent và có thể pilot nhỏ trong lab.
```

---

# Phase 4 — Quick Validation + Research giải pháp

## 4.1. Quick validation

Nhóm chọn:

```text
Option A — Quick interviews
Option C — Log / review / ticket nội bộ
```

## Câu hỏi interview đề xuất

```text
1. Lần gần nhất bạn phải verify output sai của hệ thống QA/RAG là khi nào?
2. Bạn xử lý bằng workflow nào?
3. Bước nào mất thời gian nhất?
4. Bạn mất bao lâu để debug một query lỗi?
5. Bạn phải mở bao nhiêu nguồn/chunks/tài liệu gốc?
6. Bạn có phân biệt được lỗi retrieval hay generation không?
7. Nếu tốt hơn, bạn muốn workflow thay đổi gì?
```

## Kết quả validation tạm thời

| Nguồn | Tín hiệu xác nhận | Tín hiệu phản bác | Nhóm sửa problem thế nào |
|---|---|---|---|
| Interview | Chưa có dữ liệu thật, cần hỏi 2-3 người làm RAG/QA | Có thể không đau nếu họ đã có observability tốt | Nếu cần, mở rộng actor thành AI Engineer debug RAG trong domain cần grounding |
| Survey / poll | Không ưu tiên vì domain hẹp | Survey đại trà dễ nhiễu | Chỉ dùng làm tín hiệu phụ |
| Log / review / ticket | Problem card có workflow, bottleneck, estimate 20-30 phút/query | Evidence hiện tại vẫn self-reported | Thu hẹp problem thành debug assistant cho một query lỗi |

## 4.2. Research giải pháp đã có

| Tool / Case | Link | Họ giải quyết phần nào? | Khoảng trống |
|---|---|---|---|
| LangSmith RAG Evaluation | https://docs.langchain.com/langsmith/evaluate-rag-tutorial | Trace/evaluate retrieval và generation | Không chuyên cho legal logic/chỉ dẫn chéo |
| Ragas Metrics | https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/context_precision/ | Đánh giá context precision/retrieval quality | Metric không đủ để kết luận đúng/sai pháp lý |
| TruLens RAG Triad | https://www.trulens.org/getting_started/core_concepts/rag_triad/ | Context relevance, groundedness, answer relevance | Grounded vào chunk chưa chắc đúng với luật gốc |
| vLex Vincent AI | https://vlex.com/vincent-ai | Legal research assistant có citation | Hướng tới lawyer, không phải AI engineer debug pipeline |
| Lexis+ Protégé | https://www.lexisnexis.com/en-us/products/lexis-plus-protege.page | Legal AI workflow, citation-backed research | Sản phẩm đóng, không hỗ trợ debug RAG nội bộ |
| LegalBench-RAG | https://arxiv.org/abs/2408.10343 | Benchmark retrieval trong legal RAG | Không phải tool interactive để debug từng query lỗi |

## Kết luận Phase 4

```text
Problem có tín hiệu đáng giải quyết.
Đã có tool đánh giá RAG và legal AI, nhưng khoảng trống của nhóm là workflow hẹp:
AI-assisted verification/debug workflow cho AI Engineer khi Legal QA trả lời sai.
```

---

# Phase 5 — Workflow + Problem Statement

## 5.1. Current workflow

```text
CURRENT STATE — 8 bước, ~30 phút / query lỗi

[1. Nhận query lỗi]
Ai: Engineer | ⏱ 1'
In: test log | Out: query debug
        ↓
[2. Xem output hệ thống]
Ai: Engineer | ⏱ 3'
In: query + answer | Out: answer cần check
        ↓
[3. Kiểm tra retrieved chunks]
Ai: Engineer | ⏱ 4'
In: top-k chunks | Out: chunks liên quan
        ↓
[4. Mở luật gốc / PDF / website]
Ai: Engineer | ⏱ 5'
In: metadata | Out: luật gốc
        ↓
[5. Đọc điều khoản + dẫn chiếu] 🔴
Ai: Engineer | ⏱ 10'
In: luật gốc | Out: note pháp lý
        ↓
[6. Đối chiếu answer - chunks - luật gốc] 🔴
Ai: Engineer | ⏱ 7'
In: answer + chunks + law | Out: supported/contradicted/unclear
        ↓
[7. Phân loại lỗi]
Ai: Engineer | ⏱ 3'
In: note đối chiếu | Out: retrieval/rerank/prompt/generation
        ↓
[8. Ghi note debug + fix]
Ai: Engineer | ⏱ 5'
In: error type | Out: action sửa pipeline
```

## Bottleneck chính

```text
Bước 5-6: đọc luật gốc, hiểu điều kiện/ngoại lệ, đối chiếu answer với retrieved chunks và luật gốc.
```

## 5.2. Future workflow

```text
FUTURE STATE — 8 bước, ~10 phút / query lỗi

[1. Nhận query lỗi + output + chunks]
🟢 Human | ⏱ 1'
In: debug case | Out: case chuẩn hóa
        ↓
[2. Tách answer thành claims]
🔵 Rule/script | ⏱ 1'
In: answer | Out: list claims
        ↓
[3. Map chunks về luật gốc]
🔵 Rule/metadata | ⏱ 1'
In: chunks + metadata | Out: law source mapping
        ↓
[4. Tóm tắt luật liên quan]
🔵 AI workflow | ⏱ 2'
In: law source/chunks | Out: legal summary
        ↓
[5. Lập bảng đối chiếu]
🔵 AI workflow | ⏱ 1'
In: claims + legal summary | Out: verify table
        ↓
[6. Gợi ý loại lỗi]
🔵 AI workflow | ⏱ 1'
In: verify table | Out: suspected error type
        ↓
[7. Engineer review nguồn]
🟢 Human boundary | ⏱ 2-3'
In: verify table | Out: confirmed error
        ↓
[8. Fix pipeline hoặc fallback]
🟢 Human boundary | ⏱ 2'
In: confirmed error | Out: fix action/fallback
```

```text
🔵 = AI/Rule xử lý
🟢 = Human boundary
Fallback: AI không chắc → engineer mở luật gốc kiểm tra thủ công.
```

## Before/after impact

| Metric | Trước | Sau kỳ vọng |
|---|---:|---:|
| Số bước | 8 | 8 |
| Tổng thời gian | 20-30 phút/query | Dưới 10 phút/query |
| Số bước thủ công | 7-8 | 2-3 |
| Số nguồn mở thủ công | 3-5 | 1-2 |
| Bottleneck chính | Đọc luật và đối chiếu thủ công | Review bảng đối chiếu |
| Risk mới | Không có AI risk | AI tóm tắt sai hoặc overconfident |

## 5.3. Problem Statement v0

| Field | Nội dung |
|---|---|
| Actor | AI Engineer phát triển/debug hệ thống Legal QA/RAG |
| Workflow | Xem output, kiểm tra chunks, mở luật gốc, đối chiếu answer với luật, phân loại lỗi |
| Bottleneck | Đọc luật gốc và đối chiếu chéo answer — chunks — điều luật |
| Impact | 20-30 phút/query lỗi, làm chậm vòng lặp cải thiện hệ thống |
| Success Metric | Giảm xuống dưới 10 phút/query; giảm nguồn mở thủ công từ 3-5 xuống 1-2 |
| Boundary | AI không tự kết luận đúng/sai pháp lý; engineer review và quyết định cuối |

---

# Phase 6 — Rule / Workflow / Agent + Decision

## 6.0. Ma trận phù hợp AI

```text
Bài toán nằm ở ô: Độ mơ hồ cao + Độ phức tạp cao.
```

## Vì sao?

```text
Độ mơ hồ cao:
- Một answer có nhiều claim nhỏ.
- Claim có thể đúng một phần nhưng thiếu điều kiện/ngoại lệ.
- Cần đọc hiểu ngữ nghĩa pháp lý.

Độ phức tạp cao:
- Nhiều bước nối tiếp.
- Nhiều nguồn dữ liệu: query, answer, chunks, metadata, luật gốc, log retrieval.
- Bước sau phụ thuộc kết quả bước trước.
```

## 6.1. So sánh Rule / Workflow / Agent

| Mức | Phương án | Khi nào đủ | Rủi ro | Chọn? |
|---|---|---|---|---|
| Rule | Parse citation, map metadata, highlight keyword, checklist | Case đơn giản, citation rõ | Không hiểu ngữ nghĩa pháp lý | Không chọn làm mức chính |
| Workflow | Tách claim → map source → AI tóm tắt luật → bảng đối chiếu → engineer review | Các bước rõ, cần human-in-the-loop | AI có thể tóm tắt sai | Chọn |
| Agent | Tự tìm luật, tự phân tích, tự quyết định lỗi/fix | Khi có tool ổn định và sandbox an toàn | Rủi ro cao trong domain pháp luật | Chưa chọn |

## Mức chọn

```text
Workflow
```

## Lý do chọn

```text
Workflow đủ mạnh để giảm bottleneck đọc/đối chiếu luật, nhưng vẫn giữ boundary an toàn.
AI hỗ trợ phân tích, còn AI Engineer kiểm tra nguồn và quyết định cuối cùng.
```

## Vì sao không chọn Rule-only

```text
Rule chỉ xử lý tốt phần có cấu trúc như citation, metadata, keyword.
Rule không đủ để phát hiện thiếu điều kiện pháp lý, mâu thuẫn logic hoặc lỗi generation.
```

## Vì sao không chọn Agent

```text
Agent có rủi ro cao vì domain pháp luật cần kiểm chứng nguồn.
Giai đoạn lab chưa cần AI tự lập kế hoạch hoặc tự hành động.
```

## 6.2. Problem Statement v1

| Field | Nội dung |
|---|---|
| Actor | AI Engineer phát triển và debug hệ thống Legal QA/RAG trong domain pháp luật |
| Workflow | Khi query trả lời sai, engineer xem output, chunks, luật gốc, đối chiếu và phân loại lỗi retrieval/rerank/prompt/generation |
| Bottleneck | Đọc luật gốc và đối chiếu chéo answer — chunks — điều luật |
| Impact | 20-30 phút/query lỗi; chậm vòng lặp cải thiện hệ thống |
| Success Metric | Dưới 10 phút/query; nguồn mở thủ công 1-2; tăng tỷ lệ phân loại đúng lỗi |
| Boundary | AI không kết luận cuối cùng đúng/sai pháp lý; engineer review nguồn và quyết định fix |
| AI intervention point | Tách claim, tóm tắt luật, map evidence, lập bảng đối chiếu, gợi ý lỗi |
| Mức chọn | Workflow |
| Rủi ro & người thật kiểm tra | AI có thể tóm tắt sai/bỏ sót điều kiện; AI Engineer kiểm tra nguồn, citation, confidence label |

## 6.3. Final decision

| Câu hỏi | Yes / Not Yet / No | Ghi chú |
|---|---|---|
| Actor và workflow đã rõ chưa? | Yes | Actor là AI Engineer debug Legal QA/RAG |
| Baseline và success metric đã đo được chưa? | Not Yet | Có estimate, cần đo thật trên query mẫu |
| Có data/input đủ dùng chưa? | Not Yet | Cần 3-5 query lỗi, answer, chunks, luật gốc |
| Nếu AI sai, hậu quả có chấp nhận được không? | Yes, nếu có human review | AI chỉ hỗ trợ debug, không trả lời end-user |
| Có người review/owner vận hành không? | Yes | AI Engineer là reviewer |
| Có cách non-AI đơn giản hơn không? | Yes, nhưng không đủ | Checklist/template hỗ trợ một phần |

## Decision

```text
Go
```

## Lý do

```text
Go với pilot nhỏ vì actor/workflow rõ, bottleneck cụ thể, AI có vai trò hợp lý và có human-in-the-loop.
```

## Pilot nhỏ nhất

```text
Input:
- 3 query lỗi từ hệ thống Legal QA.
- Mỗi query có: question, current answer, top 3-5 retrieved chunks, metadata nguồn, luật gốc liên quan.

Output:
- Bảng claim trong answer.
- Claim map với chunk.
- Chunk map với điều luật gốc.
- Status: supported / contradicted / missing evidence / unclear.
- Suspected error type: retrieval / rerank / prompt / generation.
- Note để engineer quyết định fix.

Cách đo:
- So sánh thời gian debug thủ công vs debug bằng workflow.
- Mục tiêu: dưới 10 phút/query.
```

---

# Kết luận chung

```text
Nhóm chọn candidate Legal QA debugging.

Mức triển khai phù hợp: Workflow có AI hỗ trợ.

Không chọn Rule-only vì không đủ xử lý ngữ nghĩa pháp lý.
Không chọn Agent vì rủi ro cao và chưa cần AI tự hành động.

Decision: Go với pilot nhỏ 3-5 query lỗi.
