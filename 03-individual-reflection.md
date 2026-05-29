# 03 — Individual Reflection

## Đóng góp của Đạt trong nhóm

| Hoạt động | Đạt đã làm gì? | Kết quả |
|---|---|---|
| Scan cá nhân | Đóng góp đề xuất hướng Legal QA/RAG Debugging từ trải nghiệm code thực tế của bản thân. | Nhóm có nguồn ý tưởng dồi dào, sát với pipeline thực tế. |
| Pitch | Thuyết trình sắc bén về bottleneck debug mất 20-30 phút/query lỗi và tác động tiêu cực của nó lên iteration cycle của kỹ sư. | Thuyết phục thành công nhóm đưa đề tài vào shortlist lựa chọn. |
| Challenge | Chất vấn nhóm về rủi ro pháp lý và chi phí API khổng lồ nếu chọn giải pháp Agent tự trị thay vì Workflow. | Giúp nhóm tỉnh táo, giữ vững tư duy thực tế và không bị cuốn theo các công nghệ quá rộng. |
| Gom trùng / cluster | Nhận diện pattern chung của các lỗi liên quan đến debug RAG và gom thành cluster Verification. | Giúp nhóm có cái nhìn hệ thống hóa về toàn bộ pipeline lỗi. |
| Chọn candidate | Đưa ra ma trận chấm điểm và lập luận chặt chẽ để nhóm chốt candidate số 10 làm đề tài chính. | Đạt được sự đồng thuận 100% của cả nhóm để hội tụ về bài toán debug chéo. |
| Validation / research | Nghiên cứu sâu các công cụ RAG evaluation (Ragas, TruLens) và chỉ ra khoảng trống về mặt explainability tiếng Việt. | Định hình rõ ràng giải pháp RAG Verification hỗ trợ riêng cho AI Engineer. |
| Workflow | Phối hợp với các thành viên thiết kế và vẽ sơ đồ hộp ASCII cho quy trình Before/After. | Cung cấp cho nhóm một bản thiết kế quy trình trực quan, làm rõ bước nghẽn và ranh giới kiểm soát. |
| Problem Statement | Đóng góp viết chi tiết ranh giới (boundary) kiểm soát để đảm bảo an toàn hệ thống. | Bản Problem Statement v1 đạt độ chặt chẽ cao, tránh bẫy solution-first. |
| Rule / Workflow / Agent | Lập luận so sánh và thuyết phục nhóm chọn giải pháp mức Workflow. | Thống nhất được kiến trúc giải pháp an toàn, an tâm giữ con người làm trung tâm kiểm duyệt. |
| Decision | Thống nhất quyết định Go và xây dựng kịch bản thử nghiệm nhỏ (Pilot) chi tiết. | Nhóm có kế hoạch hành động thực tế, có tiêu chuẩn rollback rõ ràng. |

## Bảng dùng AI trong reflection

| Phase | Tôi dùng AI để làm gì? | AI hữu ích ở đâu? | AI sai/hời hợt ở đâu? | Tôi sửa |
|---|---|---|---|---|
| Scan | Gợi ý cách diễn đạt problem và kiểm tra workflow có đủ actor/input/output không. | Giúp cấu trúc hóa ý tưởng nhanh. | AI dễ viết chung chung nếu không có context thật. | Bổ sung ví dụ, thời gian ước lượng và pain thật của mình. |
| Problem Card | Gợi ý format problem card và câu chữ ngắn gọn. | Giúp card rõ hơn: actor, bottleneck, metric. | AI có thể phóng đại impact hoặc viết như solution. | Giữ lại phần mình thật sự trải nghiệm, bỏ phần quá chung. |
| Workflow | Gợi ý before/after workflow. | Giúp chia bước rõ hơn. | AI có thể tự thêm bước không có trong thực tế. | Chỉnh lại theo workflow nhóm thực sự thảo luận. |
| Research | Gợi ý tool/pattern tương tự như RAG evaluation, legal AI, citation-backed answer. | Giúp nhóm thấy thị trường đã có giải pháp liên quan. | AI có thể đưa claim nếu không verify link. | Chỉ giữ nguồn có link và claim kiểm tra được. |
| Problem Statement | Kiểm tra 6 field: actor, workflow, bottleneck, impact, metric, boundary. | Giúp phát hiện field còn mơ hồ. | AI có xu hướng viết lại thay vì phản biện. | Tự giữ decision của nhóm và chỉ dùng AI như checklist. |
| Rule / Workflow / Agent | Gợi ý so sánh 3 mức. | Giúp nhóm thấy Workflow phù hợp hơn Agent. | AI có thể thiên về Agent vì nghe “AI hơn”. | Nhóm tự đặt boundary pháp lý và chọn Workflow. |
| Decision | Gợi ý tiêu chí Go/Not Yet/No-Go. | Giúp quyết định có lý do rõ. | AI không biết data thật của nhóm đã đủ chưa. | Ghi rõ “Go với pilot nhỏ”, nhưng baseline/data vẫn cần đo thật. |

## Bài học của Đạt

* Một problem tốt trong AI không phải là thứ gì đó nghe đao to búa lớn hay đi theo xu hướng (hype), mà phải có actor chịu đau thực tế, workflow hiện hành vẽ được và bottleneck định lượng được rõ ràng bằng con số.
* Vẽ chi tiết workflow Before/After giúp ta nhìn rõ ranh giới giữa những phần mà rule/script có thể giải quyết cực kỳ tốt (như map metadata, citation) và phần thực sự cần đến năng lực xử lý ngôn ngữ tự nhiên của AI (verify logic, tóm tắt dẫn chiếu chéo).
* Agent tự trị không phải là đích đến mặc định cho mọi hệ thống. Trong một domain đòi hỏi tính chính xác tuyệt đối như pháp luật, giải pháp mức Workflow kết hợp cơ chế human-in-the-loop (con người kiểm duyệt đầu ra) là lựa chọn khôn ngoan nhất để kiểm soát rủi ro hallucination và tối ưu hóa chi phí API.
* Khi làm việc nhóm, việc bị challenge (phản biện) không phải để bài xích ý tưởng của nhau, mà là cơ hội tuyệt vời để nhìn ra các góc khuất thực tế (như hiệu lực văn bản cũ/mới, các kịch bản ngoại lệ) nhằm thiết kế một boundary (ranh giới) an toàn và chặt chẽ hơn cho sản phẩm.

Nếu làm lại:

```text
Tôi sẽ chủ động yêu cầu nhóm thu thập ít nhất 10 query lỗi thực tế kèm retrieved chunks để chạy đo lường baseline thật ngay từ đầu, thay vì vội vàng chấp nhận các con số ước lượng (estimates) lý thuyết, giúp phần validation và thiết kế success metric có sức thuyết phục thực tiễn cao hơn.
```
