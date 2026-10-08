# Herdr Multi-Agent Development

Một skill giúp agent hiểu công việc trước khi chia việc: bắt đầu từ ngữ cảnh nghiệp vụ, tạo BigPlan đầy đủ, rồi tổ chức thực hiện theo phụ thuộc thật giữa các phần việc.

## Vì sao có skill này?

Bản thiết kế ban đầu cố bao quát toàn bộ quy trình bằng role cố định, wave cố định và cặp worker/reviewer. Cách đó tạo ra một lịch làm việc trông rất gọn, nhưng không bảo đảm agent hiểu nghiệp vụ, không chỉ ra chính xác task phụ thuộc nhau thế nào, và dễ ép các dự án khác nhau vào cùng một khuôn.

Bản này chuyển trọng tâm sang **nghiệp vụ và bằng chứng hoàn thành**. Agent tự chọn làm tuần tự, song song hay một mình dựa trên quan hệ phụ thuộc, file cùng sửa, mức rủi ro và runtime có sẵn. Herdr điều khiển terminal và agent; OpenRig là nguồn cảm hứng cho context bền vững và ownership, không phải điều kiện bắt buộc.

## Luồng làm việc

~~~mermaid
flowchart TD
    A[Người dùng + ngữ cảnh đầy đủ] --> B[Khám phá: grill-me, wayfinder, scout]
    B --> C[Chốt hướng, quyết định và điều chưa rõ]
    C --> D[BigPlan: bối cảnh, luật, thiết kế, nghiệm thu]
    D --> E[work-map.yaml: đồ thị phụ thuộc và file giao nhau]
    E --> F[task.md + session-log.md cho mỗi task]
    F --> G{Chọn cách thực hiện theo graph}
    G -->|Độc lập| H[Chạy song song an toàn]
    G -->|Có phụ thuộc| I[Chạy khi đầu vào hoặc hợp đồng đã sẵn sàng]
    G -->|Không đáng tách| J[Một luồng thực hiện]
    H --> K[Kiểm chứng bằng kết quả thực tế]
    I --> K
    J --> K
    K --> L[Kiểm tra chéo theo rủi ro và tích hợp]
    L --> M[Đóng việc bằng bằng chứng]
~~~

Sơ đồ không quy định thứ tự tuyến tính cho mọi task. Một task chỉ phải chờ khi nó thực sự cần đầu ra hoặc hợp đồng của task khác.

## BigPlan và task

BigPlan lưu toàn bộ thông tin cần để xây và kiểm tra kết quả:

- `context-and-decisions.md`: ngữ cảnh người dùng, phát hiện từ repo, quyết định, giả định và câu hỏi còn mở.
- `business-rules.md`: quy chuẩn nghiệp vụ đầy đủ, có ID ổn định, nằm ngoài task để nhiều task cùng tham chiếu.
- `solution-design.md`: thiết kế được chọn, hợp đồng và các ranh giới quan trọng.
- `work-map.yaml`: map tĩnh dạng graph, có dependency, hợp đồng, đường dẫn đọc/tạo/sửa và đầu ra nghiệm thu.
- `tasks/<id>/task.md`: nội dung và luồng cụ thể bên trong task.
- `tasks/<id>/session-log.md`: nhật ký từng phiên gồm việc đã làm, quyết định, lệnh và kết quả kiểm tra, file thay đổi, blocker và bước tiếp theo.

Không rút gọn mất luật, điều kiện, hợp đồng hoặc bằng chứng cần thiết. Một lời giao việc có thể ngắn nếu nó trỏ tới đúng file và mục đầy đủ.

## Dùng trong Herdr

Mở dự án trong Herdr, chạy agent bạn muốn dùng trong pane, rồi yêu cầu agent tạo BigPlan trước khi sửa code:

~~~text
Dùng skill herdr-multiagent-development. Hãy đọc yêu cầu và repo, làm rõ các quyết định nghiệp vụ, rồi tạo BigPlan đầy đủ. Bao gồm work-map.yaml và mỗi task có task.md cùng session-log.md. Chưa triển khai code; hãy báo đường dẫn các file plan để tôi xem.
~~~

Sau khi xem và đồng ý với plan, yêu cầu agent tiếp tục từ thư mục đó. Agent phải đọc đầy đủ các quy chuẩn liên quan, chọn task sẵn sàng theo map, và ghi kết quả thật vào session log. Lệnh Herdr thay đổi theo phiên bản; xem [Herdr docs](https://herdr.dev/docs/) và hướng dẫn runtime trước khi dùng CLI.

## Cách skill giao tiếp

Agent nên nói như một đồng nghiệp hiểu việc: rõ ràng, tự nhiên, không tâng bốc hoặc dùng câu chữ rập khuôn. Dùng ngôn ngữ người dùng đang dùng; giải thích thuật ngữ khi cần; nói rõ đâu là sự thật quan sát được, đâu là suy luận, và điều gì vẫn chưa chắc.

## English overview

This skill turns a complex software request into a complete, business-centered BigPlan and a dependency graph. Tasks have a stable specification and a sibling session log that records actual work and evidence. The graph—not fixed roles or waves—determines what can run in parallel. Herdr is a runtime option; OpenRig is an optional source of ideas for persistent context and work ownership.

