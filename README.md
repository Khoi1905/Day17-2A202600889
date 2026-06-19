# Day17-2A202600889-Tran-Duc-Dang-Khoi

## Thông tin học viên

| Trường | Nội dung |
|---|---|
| Mã học viên | 2A202600889 |
| Họ tên | Trần Đức Đăng Khôi |
| Dự án đang làm | Trợ Lý Đặt Xe An Toàn (V-VoiceRide) |
| Vai trò trong dự án | AI Product Strategy — phân tích JTBD, product hypothesis và thiết kế phương án kiểm chứng |

## Build-up Chain

| Hạng mục | Link / ghi chú |
|---|---|
| Day 16 artifact 02 | [JTBD Project Analysis](https://github.com/Khoi1905/Day16-2A202600889-Tran-Duc-Dang-Khoi/blob/main/worksheet/02-jtbd-project-analysis.md) |
| Present board cá nhân | [Mở present-board.html](./artifacts/present-board.html) |
| Phần của tôi trên board | Toàn bộ board là phần trình bày cá nhân của Trần Đức Đăng Khôi |

**Công cụ dùng để làm studio:** Markdown, HTML/CSS và trình duyệt.

**Cách mở board:** clone hoặc tải repo, sau đó mở [`artifacts/present-board.html`](./artifacts/present-board.html) bằng trình duyệt.

> **Ranh giới bằng chứng:** Bài này kế thừa phân tích JTBD Day 16 và current approach của dự án. Nhóm chưa có kết quả user research thực tế. Ba artifact dưới đây là thiết kế kiểm chứng và mock interface phục vụ present, không phải kết quả test hay bằng chứng rằng hypothesis đã đúng.

## Bước 0 — JTBD Checkpoint

| Câu hỏi | Nội dung |
|---|---|
| Core JTBD hiện tại là gì? | Sắp xếp phương tiện để di chuyển từ điểm đón đến đúng điểm đến một cách an toàn và tự chủ khi khả năng nhập hoặc kiểm tra thông tin bị hạn chế. |
| Current workflow hiện tại là gì? | Xác định nhu cầu chuyến đi → tìm/chọn địa chỉ → chuẩn bị thông tin → kiểm tra → yêu cầu chuyến → theo dõi xe → sửa khi có sai sót → nhận diện đúng xe. Hiện user có thể tự dùng app gọi xe, gọi tổng đài hoặc nhờ người thân đặt hộ. |
| Pain step đau nhất là gì? | `Locate`: xác định đúng điểm đón/điểm đến; và `Modify`: sửa đúng phần bị sai mà không phải làm lại toàn bộ. |
| AI leverage point nằm ở bước nào? | AI hiểu cách diễn đạt địa chỉ và yêu cầu sửa bằng tiếng Việt tự nhiên tại `Locate` và `Modify`; geocoding, read-back và state machine kiểm soát quyết định an toàn. |
| Điểm còn mơ hồ nhất trước khi chọn hypothesis? | Chưa biết luồng hội thoại có thực sự giúp user hoàn thành kịch bản với ít trợ giúp hơn và cảm thấy kiểm soát, an toàn hơn so với form ứng dụng hay không. |

## Bước 1 — Hypothesis Card

### Hypothesis được chọn

> Nếu người lớn tuổi hoặc ít thành thạo ứng dụng đặt xe có thể cung cấp và sửa thông tin chuyến đi bằng hội thoại tiếng Việt, đồng thời được nghe lại và xác nhận rõ trước khi tạo booking, thì họ sẽ hoàn thành kịch bản tự sắp xếp chuyến đi với ít trợ giúp hơn và cảm thấy kiểm soát, an toàn hơn so với khi tự thao tác trên form ứng dụng.

| Câu hỏi | Nội dung |
|---|---|
| Hypothesis này thuộc phần nào của dự án? | Usability + trust risk tại hai bước `Locate` và `Modify`, với read-back/explicit confirmation là guardrail. |
| Nếu nó sai thì chuyện gì yếu đi? | Value proposition của voice-first flow yếu đi. Team cần ưu tiên accessible form, caregiver mode hoặc assisted-booking thay vì đầu tư sâu vào STT/LLM cho hội thoại. |
| Vì sao chọn hypothesis này? | Đây là bản hypothesis cuối của Day 16 và có thể kiểm tra trước khi build toàn bộ AI stack. Nó tách kết quả người dùng cần đạt khỏi câu hỏi team có thể tích hợp công nghệ hay không. |
| Dấu hiệu dự kiến ủng hộ | User hoàn thành scenario hợp lệ, tự phát hiện/sửa thông tin sai, cần ít trợ giúp hơn form và đánh giá control/trust từ 4/5. |
| Dấu hiệu dự kiến phản bác | User cần trợ giúp tương đương hoặc nhiều hơn form, không phát hiện lỗi read-back, không sửa được đúng slot hoặc không muốn tự xác nhận. |

### Tiêu chí kiểm chứng dự kiến

- Completion rate của conversational flow đạt ít nhất **80%**.
- Correction success đạt ít nhất **70%** mà không cần người điều phối thao tác thay.
- Control/trust rating đạt ít nhất **4/5**.
- Số lần yêu cầu trợ giúp thấp hơn khi thực hiện cùng scenario trên form ứng dụng.

> Các con số trên là ngưỡng ra quyết định dự kiến, chưa phải kết quả test.

## Bước 2 — Current Approach Snapshot

### Dự án đang định làm gì?

Current approach là kế hoạch xây responsive web app mobile-first trong bốn tuần, gồm:

- đăng ký/đăng nhập và phân quyền User/Admin;
- voice input, STT/TTS tiếng Việt và transcript;
- LLM trích xuất điểm đón, điểm đến, loại xe và correction intent;
- geocoding thật trong phạm vi Hà Nội;
- state machine, read-back và explicit confirmation;
- booking giả lập, thông tin tài xế/xe và vòng đời chuyến;
- trang quản trị user/booking, bộ scenario đánh giá, metrics và deployment.

### Vì sao current approach đang đắt hoặc chậm?

| Loại chi phí | Current approach phải đầu tư |
|---|---|
| Time | Bốn tuần build, tích hợp, kiểm thử và polish |
| Scope | Auth, admin, voice, AI, geocoding, booking lifecycle và accessibility |
| Dependency | STT, TTS, LLM, geocoding API, database và hosting |
| Design | Conversation state, error state, correction và safety guardrail |
| Code | Frontend, backend/API, state machine, persistence và quyền truy cập |
| Data/test | Scenario, transcript ground truth, mock driver/vehicle và metric logging |
| People | Product, design, engineering, operator và participant |

Current approach có nguy cơ kiểm chứng khả năng build cả hệ thống trước khi biết conversational flow có tạo kết quả tốt hơn form hay không.

### Câu hỏi bắt buộc

> **Còn cách nào rẻ hơn để test hypothesis này không?**

Có. Ba hướng dưới đây cùng kiểm tra completion, correction, mức trợ giúp và cảm giác kiểm soát, nhưng chưa cần STT, LLM, geocoding, auth hay backend hoạt động thật.

## Bước 3 — Ba Cách Rẻ Hơn

### Cách A — Before/After Storyboard

| Câu hỏi | Nội dung |
|---|---|
| Tên hướng test A | “Sửa điểm đến mà không làm lại từ đầu” |
| Loại artifact | Before/after storyboard + interview prompts |
| Người dùng sẽ thấy gì? | Hai hành trình cùng một scenario: form app buộc quay lại nhiều bước; conversational flow đọc lại summary, cho sửa đúng điểm đến và yêu cầu xác nhận rõ. |
| Phía sau sẽ làm gì? | Người điều phối yêu cầu participant kể lại điều đang xảy ra, chỉ ra điểm họ sẽ cần trợ giúp, mô tả thông tin cuối cùng và chấm mức kiểm soát dự kiến. Không giới thiệu một flow là “tốt hơn”. |
| Nó đang test hypothesis nào? | Hội thoại có read-back/correction giúp user hiểu trạng thái, cần ít trợ giúp hơn và cảm thấy kiểm soát hơn form. |
| Vì sao rẻ hơn current approach? | Một storyboard và script 20–30 phút; không code, API, dữ liệu hay AI thật. |
| Nó giúp học được gì? | Concept có dễ hiểu không, điểm nào tạo/mất niềm tin, user dự đoán cần trợ giúp ở đâu và cách họ hiểu read-back. |
| Nó chưa giúp học được gì? | Không chứng minh hiệu năng thao tác thật, latency, STT/LLM accuracy hoặc adoption. |
| Artifact A nằm ở đâu? | [Present board — Test A](./artifacts/present-board.html#test-a) |

### Cách B — Paper A/B Task Walkthrough

| Câu hỏi | Nội dung |
|---|---|
| Tên hướng test B | “Cùng một chuyến, hai cách hoàn thành” |
| Loại artifact | Paper/click-through prototype A/B + scorecard |
| Người dùng sẽ thấy gì? | Hai prototype dùng cùng dữ liệu và scenario: form nhập liệu nhiều bước và conversational flow có summary, correction, confirmation. |
| Phía sau sẽ làm gì? | Participant thực hiện cả hai flow; thứ tự được đảo giữa participant. Facilitator chỉ phản hồi theo script, ghi completion, thời gian, help requests, lỗi phát hiện, correction success và control rating. |
| Nó đang test hypothesis nào? | Conversational flow giúp hoàn thành scenario với ít trợ giúp hơn, đồng thời duy trì mức hiểu và kiểm soát tốt hơn form. |
| Vì sao rẻ hơn current approach? | Chỉ cần các màn hình tĩnh và facilitator chuyển state thủ công; chưa cần voice, backend hoặc booking thật. |
| Nó giúp học được gì? | So sánh hành vi định hướng giữa hai flow trên cùng tác vụ: số bước, điểm bối rối, trợ giúp, phát hiện lỗi và confidence. |
| Nó chưa giúp học được gì? | Mẫu nhỏ chỉ cho directional signal; chưa kiểm tra chất lượng nhận dạng giọng nói và môi trường sử dụng thật. |
| Artifact B nằm ở đâu? | [Present board — Test B](./artifacts/present-board.html#test-b) |

### Cách C — Wizard-of-Oz Error Recovery

| Câu hỏi | Nội dung |
|---|---|
| Tên hướng test C | “Nghe sai cổng bệnh viện” |
| Loại artifact | Wizard-of-Oz flow + mobile screens + operator script |
| Người dùng sẽ thấy gì? | Trợ lý đang nghe, hỏi thiếu thông tin, đọc lại một summary có lỗi “cổng chính”, nhận correction “cổng số 2”, đọc lại và yêu cầu xác nhận. |
| Phía sau sẽ làm gì? | Operator đóng vai STT/LLM/geocoding theo response library, cố ý đưa một lỗi đã định trước, cập nhật đúng slot khi user sửa và ghi hành vi trên scorecard. Booking/tài xế đều là mock. |
| Nó đang test hypothesis nào? | User có thể phát hiện, sửa và xác nhận thông tin bằng hội thoại với ít trợ giúp, đồng thời giữ cảm giác kiểm soát và an toàn. |
| Vì sao rẻ hơn current approach? | Con người mô phỏng AI stack; chỉ cần vài màn hình, script và bảng ghi nhận. |
| Nó giúp học được gì? | Hành vi gần thật: completion, correction success, help requests, phát hiện lỗi read-back, explicit confirmation và control/trust rating. |
| Nó chưa giúp học được gì? | Không chứng minh feasibility, latency hoặc accuracy của AI thật; operator có thể phản hồi tốt hơn hệ thống tương lai. |
| Artifact C nằm ở đâu? | [Present board — Test C](./artifacts/present-board.html#test-c) |

## Bước 4 — Checklist Artifact

| Cách | Đã có artifact? | Artifact là gì? | Link |
|---|---|---|---|
| A | Có | Before/after storyboard và interview prompts | [Test A](./artifacts/present-board.html#test-a) |
| B | Có | Hai task flow prototype và scorecard | [Test B](./artifacts/present-board.html#test-b) |
| C | Có | Wizard-of-Oz error-recovery flow và operator script | [Test C](./artifacts/present-board.html#test-c) |

## Bước 5 — So Sánh Nhanh

| Tiêu chí | A — Storyboard | B — Paper A/B | C — Wizard of Oz |
|---|---|---|---|
| Nhanh hơn current approach ở đâu? | Dựng và chạy trong một ngày | Dựng trong một ngày, không tích hợp | Dựng/chạy thử trong 1–2 ngày |
| Rẻ hơn ở đâu? | Không code/API | Màn hình tĩnh, facilitator đổi state | Không AI/backend; operator vận hành |
| Gần hành vi thật tới đâu? | Thấp–trung bình | Trung bình | Cao nhất trong ba cách |
| Học được rõ nhất | Comprehension và perceived control | So sánh completion/help giữa hai flow | Error recovery, trust và confirmation |
| Giới hạn lớn nhất | Social-desirability bias | Prototype không có voice thật | Operator bias, không test feasibility |

**Cách thuyết phục nhất khi present:** C — Wizard-of-Oz Error Recovery, vì cho thấy hành vi phát hiện và sửa lỗi gần thật mà chưa cần build AI stack.

**Cách dễ bị phản biện nhất:** A — Before/After Storyboard, vì visual/copy có thể làm conversational flow trông thuận lợi hơn và câu trả lời vẫn là self-report.

### Trình tự chạy test đề xuất

1. Chạy A để kiểm tra cách user hiểu concept và hiệu chỉnh ngôn ngữ.
2. Chạy B để so sánh hai flow trên cùng scenario và xác định điểm cần trợ giúp.
3. Chạy C để quan sát error recovery bằng hội thoại gần hành vi thật hơn.

### Decision rules sau test

| Kết quả | Quyết định gợi ý |
|---|---|
| Completion/correction không đạt ngưỡng hoặc cần trợ giúp không giảm | Dừng đầu tư voice-first; sửa workflow hoặc ưu tiên accessible form |
| Hoàn thành tốt nhưng control/trust thấp | Sửa read-back, trạng thái và explicit confirmation trước technical spike |
| Perceived value tốt nhưng WoZ thất bại ở correction | Thu hẹp intent/slot và chuẩn hóa recovery flow |
| Completion ≥80%, correction ≥70%, control/trust ≥4/5 và ít trợ giúp hơn form | Tiến tới technical spike cho STT/LLM/geocoding/state machine |

## Bước 6 — Present Board Cá Nhân

| Câu hỏi | Nội dung |
|---|---|
| Đã gom đủ năm khối lên board chưa? | Rồi: Hypothesis, Current Approach, Test A, Test B, Test C và phần trade-off. |
| Artifact quan trọng nhất khi present? | Wizard-of-Oz Error Recovery, vì nó thể hiện trực tiếp read-back, correction và explicit confirmation. |
| Chỗ nào trên board dễ dài chữ? | Current approach và operator script; khi present chỉ nói headline, dùng README cho chi tiết. |

## Bước 7 — Present Cá Nhân

### Link sẽ mở

[Present Board — Day 17](./artifacts/present-board.html)

### Câu mở đầu 30 giây

> Dự án của tôi định xây trợ lý đặt xe bằng hội thoại cho người lớn tuổi, nhưng trước khi tích hợp STT, LLM và geocoding, tôi cần trả lời một câu hỏi nhỏ hơn: read-back và correction bằng hội thoại có thật sự giúp user hoàn thành tác vụ với ít trợ giúp hơn và cảm thấy kiểm soát hơn form ứng dụng không?

### Script present 3–4 phút

**0:00–0:30 — JTBD checkpoint**

> Lát cắt của tôi là người lớn tuổi hoặc ít rành app cần tự sắp xếp chuyến đi bệnh viện. Job không phải “dùng AI”, mà là đi đúng nơi, an toàn và tự chủ khi khó nhập hoặc kiểm tra thông tin. Hai bước đau nhất là Locate và Modify.

**0:30–1:00 — Hypothesis**

> Tôi kiểm tra liệu conversational flow có read-back, sửa từng phần và xác nhận rõ có giúp user cần ít trợ giúp hơn, đồng thời kiểm soát và an toàn hơn form hay không.

**1:00–1:35 — Current approach**

> MVP bốn tuần gồm auth, voice, STT/TTS, LLM, geocoding, state machine và booking mock. Cách này quá lớn để trả lời riêng câu hỏi usability và trust trước khi build.

**1:35–2:50 — Ba cheaper tests**

> Test A là before/after storyboard để kiểm tra cách user hiểu hai journey. Test B cho user walkthrough cùng scenario trên form và hội thoại, rồi so completion, help requests và control. Test C dùng Wizard-of-Oz với lỗi “cổng chính” để xem user có phát hiện, sửa thành “cổng số 2” và tự xác nhận hay không.

**2:50–3:25 — Trade-off**

> Storyboard nhanh nhất nhưng thiên về self-report. Paper A/B cho so sánh trực tiếp nhưng chưa có voice thật. Wizard-of-Oz gần hành vi nhất nhưng có operator bias và chưa chứng minh feasibility.

**3:25–3:45 — Xin phản biện**

> Tôi muốn lớp phản biện liệu ba artifact đã tách được perceived control khỏi task performance chưa, và lỗi cài sẵn trong Wizard-of-Oz có đủ đại diện cho rủi ro Locate/Modify hay không.

### Hai câu hỏi muốn lớp phản biện

1. Bộ metrics completion, correction, help requests và control/trust đã đủ để kết luận “tốt hơn form” ở mức directional chưa?
2. Trong Wizard-of-Oz, cần thêm lỗi hoặc tình huống nào để tránh chỉ kiểm tra một happy path có correction đơn giản?

## Bước 8 — Ghi Lại Sau Present

| Câu hỏi | Ghi chú |
|---|---|
| Phản biện quan trọng nhất vừa nhận là gì? |  |
| Trong ba cách test, cách nào bị hỏi nhiều nhất? |  |
| Cần chỉnh gì trên board/artifact? |  |
| Điều gì muốn giữ nguyên sau phản biện? |  |

## Checklist Trước Khi Nộp

- [x] Tôi đã checkpoint lại JTBD.
- [x] Tôi đã chọn đúng một hypothesis.
- [x] Tôi đã ghi lại current approach hiện tại của dự án.
- [x] Tôi đã nghĩ ra ít nhất ba cách rẻ hơn để test cùng hypothesis.
- [x] Tôi đã làm artifact cho cả ba cách.
- [x] Tôi đã gom thành present board.
- [x] Tôi đã chuẩn bị phần present cá nhân.
- [x] Tôi đã tạo sẵn mục ghi chú sau present.

## Chốt Trong 60 Giây

1. Tôi test hypothesis rằng conversational flow có read-back/correction giúp user hoàn thành tác vụ với ít trợ giúp hơn và cảm thấy kiểm soát, an toàn hơn form.
2. Current approach là MVP bốn tuần với nhiều API và dependency, quá lớn để kiểm tra riêng usability/trust đầu tiên.
3. Ba cách rẻ hơn là before/after storyboard, paper A/B task walkthrough và Wizard-of-Oz error recovery.
4. Ba artifact lần lượt kiểm tra điều user hiểu, cách user thực hiện hai flow và hành vi phát hiện/sửa lỗi trước khi team quyết định build AI stack.
