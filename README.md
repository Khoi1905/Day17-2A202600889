# Day17-2A202600743-BuiNgocKhanh

## Thông tin học viên

| Trường | Điền |
|---|---|
| Mã học viên | 2A202600743 |
| Họ tên | Bùi Ngọc Khánh |
| Dự án đang làm | Trợ Lý Đặt Xe An Toàn (V-VoiceRide) |
| Vai trò trong dự án | AI Product Strategy — phân tích JTBD, giả thuyết sản phẩm và thiết kế phương án kiểm chứng |

## Build-up Chain

| Hạng mục | Link / ghi chú |
|---|---|
| Day 16 artifact 02 | [JTBD Project Analysis](https://github.com/BNK2610/Day16-2A202600743-BuiNgocKhanh-Track01-Lab-Assignment/blob/main/worksheet/02-jtbd-project-analysis.md) |
| Present board cá nhân | [Mở present-board.html](./artifacts/present-board.html) |
| Phần của tôi trên board | Toàn bộ board là phần trình bày cá nhân của Bùi Ngọc Khánh |

**Công cụ dùng để làm studio:** Markdown, HTML/CSS và trình duyệt.

**Link present board / frame / file:** [`artifacts/present-board.html`](./artifacts/present-board.html)

> Ghi chú về bằng chứng: bài này kế thừa kết quả JTBD Day 16 và tài liệu dự án đã phân tích trong quá trình làm bài. Dự án chưa có user research thực tế, vì vậy các mô tả về nhu cầu và hành vi người dùng vẫn là hypothesis cần kiểm chứng. Các artifact dưới đây là thiết kế test, không phải kết quả test.

## Bước 0 — JTBD Checkpoint

| Câu hỏi | Điền |
|---|---|
| Core JTBD hiện tại là gì? | Sắp xếp phương tiện để di chuyển từ điểm đón đến đúng điểm đến một cách an toàn và tự chủ khi khả năng nhập hoặc kiểm tra thông tin bị hạn chế. |
| Current workflow hiện tại là gì? | Xác định nhu cầu chuyến đi → tìm/chọn địa chỉ → chuẩn bị thông tin → kiểm tra → đặt chuyến → theo dõi xe → sửa khi có sai sót → nhận diện đúng xe. User hiện có thể tự dùng app gọi xe, gọi tổng đài hoặc nhờ người thân đặt hộ. |
| Pain step đau nhất là gì? | `Locate`: xác định đúng điểm đón/điểm đến; và `Modify`: sửa đúng phần bị sai mà không phải làm lại toàn bộ. |
| AI leverage point nằm ở bước nào? | AI hiểu cách diễn đạt địa chỉ và yêu cầu sửa bằng tiếng Việt tự nhiên ở `Locate` và `Modify`; geocoding, read-back và state machine kiểm soát quyết định an toàn. |
| Điểm còn mơ hồ nhất trước khi chọn hypothesis? | Chưa biết người dùng mục tiêu có thực sự muốn tự sắp xếp chuyến đi và chịu trách nhiệm xác nhận thông tin, hay họ vẫn muốn người thân/tổng đài làm thay. |

## Bước 1 — Hypothesis Card

### Hypothesis được chọn

> Khi cần đi bệnh viện, người lớn tuổi hoặc ít thành thạo ứng dụng đặt xe có nhu cầu trực tiếp tham gia sắp xếp và tự xác nhận thông tin chuyến đi, thay vì luôn giao toàn bộ thao tác và trách nhiệm cho người thân hoặc tổng đài.

| Câu hỏi | Điền |
|---|---|
| Hypothesis này thuộc phần nào của dự án? | Desirability/value risk: nhu cầu tự chủ của primary persona. Việc họ thích voice hơn một accessible form là hypothesis thứ hai, chưa kết luận trong bài này. |
| Nếu nó sai thì chuyện gì sập hoặc yếu đi? | Primary persona và value proposition “tự chủ” yếu đi. Dự án có thể phải đổi job executor sang người chăm sóc hoặc trở thành công cụ assisted-booking cho người thân/tổng đài thay vì trợ lý trực tiếp cho người dùng cuối. |
| Vì sao chọn hypothesis này? | Kế hoạch hiện tại đề xuất một MVP bốn tuần với nhiều dependency, nhưng chưa có user research chứng minh user thật sự muốn trực tiếp thực hiện và chịu trách nhiệm xác nhận job. Đây là assumption đứng trước lựa chọn interaction và feasibility: nếu không có nhu cầu tự chủ, build voice tốt vẫn khó tạo adoption. |
| Dấu hiệu ban đầu ủng hộ hypothesis | User kể được tình huống gần đây họ muốn tự đặt nhưng phải nhờ người khác; chọn một phương án tự thực hiện thay vì giao toàn bộ cho người thân; hoàn thành scenario mà không giao quyền thao tác cho người hỗ trợ; tự kiểm tra/sửa/xác nhận và đánh giá cảm giác kiểm soát từ 4/5. |
| Dấu hiệu phản bác hypothesis | User ưu tiên để người thân/tổng đài chịu trách nhiệm; từ chối tự xác nhận thông tin; chỉ chọn trợ lý khi có người hỗ trợ bên cạnh; nhu cầu chính là giá, tài xế hoặc thanh toán chứ không phải thao tác và sửa thông tin. |

## Bước 2 — Current Approach Snapshot

### Dự án đang định làm gì?

Dự án đang định xây một responsive web app mobile-first hoàn chỉnh trong bốn tuần, gồm:

- đăng ký/đăng nhập và phân quyền User/Admin;
- voice input, STT/TTS tiếng Việt và transcript;
- LLM trích xuất điểm đón, điểm đến, loại xe và yêu cầu correction;
- geocoding thật trong phạm vi Hà Nội;
- state machine, read-back và câu xác nhận bắt buộc;
- booking giả lập, thông tin tài xế/xe và năm trạng thái chuyến;
- trang quản trị user/booking, bộ 20 scenario, metrics và deployment.

### Vì sao current approach đang đắt hoặc chậm?

| Loại chi phí | Current approach phải đầu tư |
|---|---|
| Time | Bốn tuần build, tích hợp, kiểm thử và polish |
| Scope | Auth, admin, voice, AI, geocoding, booking lifecycle và accessibility |
| Dependency | STT, TTS, LLM, geocoding API, database, hosting |
| Design | Nhiều conversation state, error state, correction và safety guardrail |
| Code | Frontend, backend/API, state machine, persistence và quyền truy cập |
| Data/test | 20 scenario, ground truth transcript, mock driver/vehicle và metric logging |
| People | Product, design, engineering và người tham gia usability test |

Current approach có nguy cơ kiểm chứng “team có thể build hay không” trước khi biết “user có muốn tự làm job này hay không”.

### Câu hỏi bắt buộc

> **Còn cách nào rẻ hơn để test hypothesis này không?**

Có. Ba hướng dưới đây cùng kiểm tra nhu cầu trực tiếp tham gia và tự xác nhận chuyến đi, nhưng chưa cần tích hợp STT, LLM, geocoding, auth hay backend. Preference đối với voice chỉ được ghi nhận như insight phụ, không được dùng thay cho kết luận về nhu cầu tự chủ.

## Bước 3 — Ba Cách Rẻ Hơn

### Cách A — Storyboard “Tự đi bệnh viện”

| Câu hỏi | Điền |
|---|---|
| Tên hướng test A | Storyboard “Tự đi bệnh viện” |
| Loại artifact | Storyboard 6 khung + interview prompts |
| Người dùng sẽ thấy gì? | Một câu chuyện từ lúc cần đi bệnh viện, gặp khó với app, cân nhắc nhờ con, thử trợ lý, sửa địa chỉ và tự xác nhận chuyến. |
| Phía sau sẽ làm gì? | Người điều phối kể ít nhất có thể, yêu cầu user diễn giải từng khung, liên hệ với chuyến đi gần nhất và chỉ ra đoạn giống/không giống đời thật. Không trình diễn AI thật. |
| Nó đang test hypothesis nào? | Người dùng có nhu cầu trực tiếp tham gia, kiểm tra và chịu trách nhiệm xác nhận chuyến thay vì mặc định giao toàn bộ job cho người thân. |
| Vì sao rẻ hơn current approach? | Chỉ cần một trang storyboard và 20–30 phút phỏng vấn; không cần code, API hay dữ liệu hệ thống. |
| Nó giúp học được gì? | Bối cảnh thật, động lực tự chủ, alternative hiện tại, ai đang chịu trách nhiệm và phần nào của concept tạo hoặc làm giảm niềm tin. |
| Nó chưa giúp học được gì? | Không chứng minh user thao tác được, không đo STT/LLM accuracy và dễ có social-desirability bias. |
| Artifact A nằm ở đâu? | [Present board — Test A](./artifacts/present-board.html#test-a) |

### Cách B — Fake-door Choice Test

| Câu hỏi | Điền |
|---|---|
| Tên hướng test B | “Bạn muốn đặt chuyến theo cách nào?” |
| Loại artifact | Fake-door mobile screen + forced-choice follow-up |
| Người dùng sẽ thấy gì? | Một tình huống đi bệnh viện và ba lựa chọn được mô tả với độ dài/lợi ích cân bằng: tự thao tác trên app, tự thao tác với trợ lý hội thoại, hoặc nhờ người thân thao tác. Thứ tự ba lựa chọn được xoay giữa participant. |
| Phía sau sẽ làm gì? | Ghi nhận lựa chọn đầu tiên, lý do, kỳ vọng và điều kiện khiến user đổi lựa chọn. Primary signal là tỷ lệ chọn một trong hai phương án tự thực hiện so với giao job cho người thân; lựa chọn app hay voice là secondary signal. |
| Nó đang test hypothesis nào? | Khi đứng trước alternative thật, user có chủ động giữ vai trò thực hiện/xác nhận job hay giao toàn bộ thao tác và trách nhiệm cho người thân. |
| Vì sao rẻ hơn current approach? | Một màn hình tĩnh/clickable; không cần chức năng voice, booking thật hoặc backend. |
| Nó giúp học được gì? | Preference giữa tự thực hiện và nhờ người khác, lý do giữ/giao trách nhiệm; đồng thời thu thêm secondary insight về app so với voice. |
| Nó chưa giúp học được gì? | Click không đồng nghĩa usage/adoption; cách viết option có thể tạo framing bias; chưa kiểm tra trải nghiệm dài. |
| Artifact B nằm ở đâu? | [Present board — Test B](./artifacts/present-board.html#test-b) |

### Cách C — Wizard-of-Oz Safety Concierge

| Câu hỏi | Điền |
|---|---|
| Tên hướng test C | Safety Concierge |
| Loại artifact | Wizard-of-Oz flow + mobile screens + operator script |
| Người dùng sẽ thấy gì? | Một màn hình giống trợ lý đang nghe, hỏi thiếu thông tin, đọc lại, cho sửa từng phần và yêu cầu câu xác nhận rõ. |
| Phía sau sẽ làm gì? | Người vận hành nghe câu nói, tự điền slot vào control sheet, chọn phản hồi có sẵn và đọc/phát phản hồi như hệ thống. Booking và tài xế đều là dữ liệu mock. |
| Nó đang test hypothesis nào? | User có trực tiếp tiếp tục flow, tự kiểm tra, tự sửa và tự xác nhận hay chuyển quyền thao tác/trách nhiệm cho người hỗ trợ. |
| Vì sao rẻ hơn current approach? | Mô phỏng STT, LLM, geocoding và state machine bằng con người; chỉ cần prototype vài màn hình và script. |
| Nó giúp học được gì? | Hành vi gần thật: tỷ lệ tự hoàn thành, số lần giao thiết bị/nhờ thao tác thay, correction chủ động, phát hiện lỗi trong read-back, explicit confirmation và cảm giác kiểm soát. |
| Nó chưa giúp học được gì? | Không chứng minh technical feasibility, latency hay accuracy của AI thật; operator có thể phản hồi tốt hơn hệ thống tương lai. |
| Artifact C nằm ở đâu? | [Present board — Test C](./artifacts/present-board.html#test-c) |

## Bước 4 — Checklist Artifact

| Cách | Đã có artifact? | Artifact là gì? | Link |
|---|---|---|---|
| A | Có | Storyboard 6 khung và interview prompts | [Test A](./artifacts/present-board.html#test-a) |
| B | Có | Fake-door mobile choice screen và follow-up | [Test B](./artifacts/present-board.html#test-b) |
| C | Có | Wizard-of-Oz flow, user screens và operator script | [Test C](./artifacts/present-board.html#test-c) |

## Bước 5 — So Sánh Nhanh

| Tiêu chí | Cách A — Storyboard | Cách B — Fake door | Cách C — Wizard of Oz |
|---|---|---|---|
| Nhanh hơn current approach ở đâu? | Có thể chuẩn bị và chạy trong một ngày | Có thể dựng trong vài giờ | Có thể dựng và chạy thử trong 1–2 ngày |
| Rẻ hơn ở đâu? | Không code/API | Chỉ cần một màn hình clickable | Không cần AI/backend; vận hành thủ công |
| Gần hành vi thật tới đâu? | Thấp–trung bình | Trung bình | Cao nhất trong ba cách |
| Học được rõ nhất | Bối cảnh, động lực và alternative | Preference và value proposition | Hành vi, correction, trust và điểm bỏ cuộc |
| Giới hạn lớn nhất | User nói điều “nghe hợp lý” | Click không phải adoption | Operator bias, không test feasibility |

**Cách thuyết phục nhất khi present:** C — Wizard-of-Oz, vì cho thấy một flow gần thật mà chưa cần build hệ thống.

**Cách dễ bị phản biện nhất:** B — Fake-door, vì lựa chọn trên màn hình có thể bị framing bias và chưa tạo commitment thật.

### Trình tự chạy test đề xuất

1. Chạy A trước để kiểm tra context và sửa ngôn ngữ concept.
2. Chạy B để so sánh preference với alternative.
3. Chỉ chạy C với những participant có nhu cầu hoặc hành vi phù hợp, nhằm quan sát flow gần thật.

### Tiêu chí quyết định sau test

| Kết quả | Quyết định gợi ý |
|---|---|
| Nhu cầu tự chủ yếu, đa số muốn người thân chịu trách nhiệm | Xem lại primary user/job executor trước khi build voice-first MVP |
| Có nhu cầu tự chủ nhưng không chọn voice | Khám phá accessible form, assisted mode hoặc caregiver flow |
| Chọn concept nhưng bỏ cuộc trong WoZ | Sửa workflow/read-back/correction trước khi tích hợp AI |
| Hoàn thành WoZ với ít trợ giúp và trust ≥ 4/5 | Tiến tới technical spike cho STT/LLM/state machine |

## Bước 6 — Present Board Cá Nhân

| Câu hỏi | Điền |
|---|---|
| Đã gom đủ 5 khối lên board chưa? | Rồi: Hypothesis, Current Approach, Test A, Test B, Test C và trade-off. |
| Artifact quan trọng nhất khi present? | Wizard-of-Oz Safety Concierge, vì nó thể hiện cách kiểm tra hành vi gần thật mà chưa build AI stack. |
| Chỗ nào trên board dễ dài chữ? | Current approach và giới hạn từng test; khi present chỉ nói headline và dùng README cho chi tiết. |

## Bước 7 — Present Cá Nhân

### Link sẽ mở

[Present Board — Day 17](./artifacts/present-board.html)

### Câu mở đầu 30 giây

> Dự án của tôi định xây một trợ lý đặt xe bằng giọng nói cho người lớn tuổi, nhưng trước khi tích hợp STT, LLM, geocoding và state machine, có một câu hỏi rẻ hơn cần trả lời: người dùng có thật sự muốn tự sắp xếp chuyến đi hay vẫn muốn người thân làm và chịu trách nhiệm thay? Tôi chọn hypothesis này và thiết kế ba cách test tăng dần độ gần với hành vi thật.

### Script present 3–4 phút

**0:00–0:30 — JTBD checkpoint**

> Lát cắt của tôi là người lớn tuổi hoặc ít rành app cần tự đi bệnh viện. Job của họ không phải “dùng AI”, mà là sắp xếp phương tiện để đi đúng nơi, an toàn và tự chủ. Hai bước đau được giả định là xác định địa chỉ và sửa thông tin sai.

**0:30–1:00 — Hypothesis**

> Assumption nguy hiểm nhất là họ thực sự muốn tự làm job này. Nếu họ muốn người thân hoặc tổng đài chịu trách nhiệm, một voice flow dù hoạt động tốt vẫn khó tạo adoption.

**1:00–1:35 — Current approach**

> Kế hoạch hiện tại đề xuất MVP bốn tuần với auth, admin, STT, TTS, LLM, geocoding, state machine và booking giả lập. Cách này phù hợp để chứng minh hệ thống, nhưng quá lớn để trả lời câu hỏi desirability đầu tiên.

**1:35–2:50 — Ba cheaper tests**

> Test A là storyboard interview để tìm lại hành vi gần nhất, động lực tự chủ và alternative thật. Test B là fake-door choice test: primary signal là user chọn tự thực hiện hay giao job cho người thân; app so với voice chỉ là secondary signal. Test C là Wizard-of-Oz: user trải qua một flow gần thật nhưng phía sau người vận hành đóng vai STT, LLM và state machine. Ba cách cùng test một hypothesis, nhưng lần lượt tăng từ điều user kể, điều user chọn đến điều user thực sự làm.

**2:50–3:25 — Trade-off**

> Storyboard rẻ nhất nhưng dễ có social-desirability bias. Fake-door cho signal lựa chọn nhưng click chưa phải adoption. Wizard-of-Oz gần hành vi thật nhất nhưng không chứng minh feasibility kỹ thuật và có operator bias.

**3:25–3:45 — Xin phản biện**

> Tôi muốn lớp phản biện vào hai điểm: ba test đã thực sự tách nhu cầu tự chủ khỏi preference đối với voice chưa, và các behavioral metrics của Test C có đủ phân biệt mong muốn giữ trách nhiệm với việc chỉ làm theo hướng dẫn hay không?

### Hai câu hỏi muốn lớp phản biện

1. Sau khi tách preference đối với voice ra khỏi hypothesis chính, ba test hiện tại đã đo đúng nhu cầu trực tiếp tham gia và tự xác nhận job chưa?
2. Những hành vi nào trong Wizard-of-Oz vẫn có thể bị hiểu nhầm là “user làm theo hướng dẫn” thay vì thực sự muốn giữ trách nhiệm?

## Bước 8 — Ghi Lại Sau Present

| Câu hỏi | Ghi sau review |
|---|---|
| Phản biện quan trọng nhất vừa nhận là gì? | Hypothesis ban đầu gộp hai niềm tin khác nhau: user muốn trực tiếp làm job và user sẽ chọn voice safety-first. Nếu test thất bại sẽ không biết nhu cầu tự chủ không tồn tại hay chỉ interaction voice chưa phù hợp. |
| Trong ba cách test, cách nào bị hỏi nhiều nhất? | Test B — Fake-door, vì cách mô tả trợ lý ban đầu hấp dẫn và chi tiết hơn alternative, dễ tạo framing bias; click cũng chưa phải adoption. |
| Cần chỉnh gì trên board/artifact? | Thu hẹp hypothesis vào nhu cầu trực tiếp tham gia/tự xác nhận; coi voice preference là secondary hypothesis; cân bằng copy và xoay thứ tự option trong Test B; bổ sung behavioral metrics cụ thể cho Test C. |
| Điều gì muốn giữ nguyên sau phản biện? | Giữ current approach snapshot và chuỗi ba test tăng dần từ điều user kể → chọn → làm. Giữ Wizard-of-Oz là artifact chính vì cho signal hành vi gần thật nhất mà chưa cần build AI stack. |

### Thay đổi đã áp dụng sau review

- Tách “muốn tự chủ” khỏi “chọn voice”.
- Đổi primary signal của Test B thành `tự thực hiện` so với `giao job cho người thân`.
- Yêu cầu cân bằng copy và xoay thứ tự ba lựa chọn trong Test B.
- Chốt metrics Test C: tự hoàn thành, số lần giao thiết bị/nhờ thao tác, correction chủ động, phát hiện lỗi read-back, explicit confirmation và control rating.

## Checklist Trước Khi Nộp

- [x] Tôi đã checkpoint lại JTBD.
- [x] Tôi đã chọn đúng một hypothesis.
- [x] Tôi đã ghi lại current approach hiện tại của dự án.
- [x] Tôi đã nghĩ ra ít nhất ba cách rẻ hơn để test cùng hypothesis.
- [x] Tôi đã làm artifact cho cả ba cách.
- [x] Tôi đã gom thành present board.
- [x] Tôi đã chuẩn bị phần present cá nhân.
- [x] Tôi đã present trên lớp và bổ sung phản biện của giảng viên/lớp 

## Chốt Trong 60 Giây

1. Tôi test hypothesis rằng người dùng mục tiêu thật sự muốn trực tiếp tham gia và tự xác nhận chuyến đi thay vì luôn giao toàn bộ job cho người thân; preference đối với voice là hypothesis thứ hai.
2. Current approach là một MVP bốn tuần với nhiều API và thành phần kỹ thuật, quá lớn để kiểm tra desirability đầu tiên.
3. Ba cách rẻ hơn là storyboard interview, fake-door choice test và Wizard-of-Oz safety concierge.
4. Ba artifact lần lượt giúp kiểm tra điều user kể, điều user chọn và điều user thực sự làm trước khi team quyết định build AI stack.
