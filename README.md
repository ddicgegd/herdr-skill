# Herdr Multi-Agent Development

Một skill giúp agent hiểu công việc trước khi chia việc: bắt đầu từ ngữ cảnh nghiệp vụ, khảo sát đúng code/config/service liên quan, tạo BigPlan đầy đủ, rồi tổ chức thực hiện theo phụ thuộc thật giữa các phần việc.

## Vì sao có skill này?

Bản thiết kế ban đầu cố bao quát toàn bộ quy trình bằng role cố định, wave cố định và cặp worker/reviewer. Cách đó tạo ra một lịch làm việc trông gọn, nhưng không bảo đảm agent hiểu nghiệp vụ, không chỉ ra chính xác task phụ thuộc nhau thế nào, và dễ ép các dự án khác nhau vào cùng một khuôn.

Bản này chuyển trọng tâm sang **nghiệp vụ, bằng chứng hoàn thành và tích hợp an toàn**. Agent tự chọn làm tuần tự, song song hay một mình dựa trên dependency thật, file cùng sửa, rủi ro và năng lực runtime. Không áp đặt danh sách vai trò hay tên agent.

Herdr là runtime mặc định. Khi tạo plan, skill cũng tạo `herdr-runbook.md` riêng với lệnh đã kiểm chứng. File này có thể xóa nếu muốn chuyển sang runtime khác; BigPlan và task vẫn giữ nguyên vì mô tả nghiệp vụ chứ không phụ thuộc vào CLI.

## Luồng làm việc

~~~mermaid
flowchart TD
    A[Người dùng + ngữ cảnh đầy đủ] --> B[Khám phá: grill-me, wayfinder, scout]
    B --> C[Khảo sát code, style từng service, cấu hình và dịch vụ bên thứ ba]
    C --> D[Chốt hướng, quyết định và điều cần hỏi]
    D --> E[BigPlan: luật, thiết kế, checklist kiểm chứng theo service]
    E --> F[work-map.yaml: dependency, xung đột file, TODO handoff, việc sẵn sàng]
    F --> G[Task: task.md + session-log.md]
    G --> H[Tạo nhánh nghiệp vụ trước khi sửa code]
    H --> I{Chọn việc READY theo graph}
    I -->|Độc lập| J[Thực hiện song song an toàn]
    I -->|Thiếu đầu vào| K[Chờ handoff; làm việc READY khác]
    J --> L[Chạy checklist phù hợp + ghi bằng chứng]
    K --> I
    L --> M[Tích hợp vào nhánh nghiệp vụ]
    M --> N[Kiểm tra luồng kết hợp và mọi TODO trong phạm vi]
    N --> O[Hoàn tất khi có bằng chứng]
    P[Herdr mặc định + herdr-runbook.md riêng] --> I
~~~

Sơ đồ không ép mọi task chạy tuần tự. Một task chỉ chờ khi nó thật sự cần đầu ra hoặc hợp đồng từ task khác.

## BigPlan và task

BigPlan lưu đủ thông tin để xây, kiểm tra và tích hợp kết quả:

- `context-and-decisions.md`: ngữ cảnh người dùng, phát hiện từ repo, cấu hình, phong cách code theo từng service, phụ thuộc bên thứ ba, quyết định, giả định và câu hỏi còn mở.
- `business-rules.md`: quy chuẩn nghiệp vụ đầy đủ, có ID ổn định, nằm ngoài task để nhiều task cùng tham chiếu.
- `solution-design.md`: thiết kế được chọn, hợp đồng và các ranh giới quan trọng.
- `work-map.yaml`: graph tĩnh có dependency, contract, TODO handoff, đường dẫn đọc/tạo/sửa và kết nối tới kiểm chứng.
- `tasks/<id>/task.md`: kết quả, đường đi cụ thể, phạm vi file, tiêu chuẩn code theo service, checklist phù hợp và điều kiện nghiệm thu.
- `tasks/<id>/session-log.md`: nhật ký từng phiên gồm việc đã làm, lệnh và kết quả kiểm tra, file thay đổi, handoff, blocker và bước tiếp theo.
- `validation-and-integration.md`: checklist theo từng task/service/dependency và kiểm tra luồng kết hợp.
- `herdr-runbook.md`: file riêng chứa lệnh Herdr đã xác minh để điều phối, xem trạng thái, chờ/tiếp tục và ghi báo cáo.

Không rút gọn mất luật, điều kiện, hợp đồng hoặc bằng chứng cần thiết. Worker có thể vừa triển khai vừa chạy checklist của task; việc đó không thay thế kiểm tra tích hợp sau khi ghép các phần.

## Dùng trong Herdr

Mở dự án trong Herdr, chạy agent bạn muốn dùng trong pane, rồi yêu cầu tạo BigPlan trước khi sửa code:

~~~text
Dùng skill herdr-multiagent-development. Hãy đọc yêu cầu và repo, làm rõ các quyết định nghiệp vụ, khảo sát code/config và các dịch vụ bên thứ ba liên quan, rồi tạo BigPlan đầy đủ. Bao gồm work-map.yaml, từng task có task.md cùng session-log.md, validation-and-integration.md và herdr-runbook.md với lệnh Herdr đã kiểm chứng. Chưa triển khai code; hãy báo đường dẫn plan và các vấn đề cấu hình/dependency cần tôi quyết định.
~~~

Sau khi plan được duyệt, yêu cầu agent tiếp tục từ thư mục đó. Agent điều phối phải đọc runbook, tạo nhánh nghiệp vụ trước khi sửa code, dùng Herdr để dispatch và theo dõi task READY/WAITING/BLOCKED, tích hợp kết quả và xác minh luồng kết hợp. Lệnh Herdr thay đổi theo phiên bản; runbook phải dựa trên tài liệu và CLI đang cài, không dựa trên lệnh ghi nhớ.

## Cách skill giao tiếp

Agent nên nói như một đồng nghiệp hiểu việc: rõ ràng, tự nhiên, không tâng bốc hoặc dùng câu chữ rập khuôn. Dùng ngôn ngữ người dùng đang dùng; giải thích thuật ngữ khi cần; nói rõ đâu là sự thật quan sát được, đâu là suy luận, và điều gì vẫn chưa chắc.

## English overview

This skill turns a complex software request into a complete, business-centered BigPlan and dependency graph. It scouts real service conventions, configuration, and external dependencies; creates per-task verification evidence; keeps initiative work on a new branch; and integrates task outcomes safely. Herdr is the default runtime, with its CLI guide isolated in a removable `herdr-runbook.md`.
