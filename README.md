# Track1_Day23_2A202601215_Phùng Hồng Phước
00 — Dự án, persona, core job
- Dự án : Xây dựng 1 hệ thống AI Agents giúp viết nhiều báo cáo đầu ra từ dữ liệu đầu vào đã được xác thực.
- Persona: Quản lí 
- Core job: Cần tổng hợp lại báo cáo hàng ngày để viết ra báo cáo tuần gửi cho các phòng ban khác nhau theo những văn phong và số liệu cần gửi tới khác nhau.Mất thời gian tổng hợp, chỉnh sửa , đổi chiếu số liệu.
01 — Core Action Card (+ kết quả tự kiểm 5 tiêu chí)

Phân biệt 4 khái niệm (trước khi điền thẻ)
| Khái niệm | Câu hỏi | Áp dụng cho dự án này |
| --- | --- | --- |
| Core job | User đang cố hoàn thành việc gì? | Tổng hợp báo cáo ngày → viết báo cáo tuần gửi đúng phòng ban, đúng số liệu và văn phong, không mất thời gian đối chiếu thủ công |
| Core action | User làm gì trong sản phẩm để tiến tới giá trị? | Duyệt số liệu + xuất một báo cáo tuần cho một phòng ban nhận cụ thể |
| Core value | User nhận được lợi ích gì? | Có bản báo cáo tuần sẵn sàng gửi: số liệu khớp nguồn đã xác thực, đúng format/văn phong phòng ban, tiết kiệm thời gian tổng hợp–chỉnh sửa |
| Core value event | Sự kiện nào chứng minh value đã xảy ra? | `weekly_report_approved_exported` — user xác nhận draft và xuất thành công (AI sinh draft chưa đủ) |

Core Action Card
| Thành phần | Câu trả lời của bạn |
| --- | --- |
| Target user | Quản lý vận hành / quản lý bộ phận — người phải gom báo cáo ngày thành báo cáo tuần gửi các phòng ban |
| Core job | “Tôi mất quá nhiều thời gian tổng hợp báo cáo ngày, chỉnh sửa văn phong và đối chiếu số liệu để viết báo cáo tuần gửi đúng từng phòng ban.” |
| Core action | Duyệt và xuất một báo cáo tuần cho một đối tượng nhận (phòng ban) từ dữ liệu đầu vào đã xác thực |
| Object | Một bản báo cáo tuần (draft đã gắn nguồn số liệu + template/văn phong của phòng ban nhận) |
| Preconditions | (1) Có bộ dữ liệu/báo cáo ngày đã được xác thực trong hệ thống; (2) Đã chọn chu kỳ tuần và phòng ban nhận; (3) Hệ thống đã tạo được draft báo cáo tuần để user soi |
| Completion rule | Action hoàn tất khi user xác nhận draft (số liệu/văn phong ổn) **và** xuất thành công file/bản gửi (download hoặc đánh dấu sẵn sàng gửi). Chỉ “AI đã generate” hoặc “user mở draft” **không** tính hoàn tất |
| Core value | Có báo cáo tuần đúng số liệu, đúng văn phong phòng ban, sẵn sàng gửi — giảm thời gian tổng hợp, chỉnh sửa và đối chiếu |
| Evidence of value | User xuất báo cáo sau khi duyệt với ít chỉnh tay lớn; báo cáo gửi đi không bị phòng ban trả về vì sai số / sai format; chu kỳ tuần đó không phải làm lại từ đầu ngoài hệ thống |
| Candidate event | `weekly_report_approved_exported` (props gợi ý: `user_id`, `week_id`, `department_id`, `source_report_count`, `edit_intensity`, `export_format`) |

Tự kiểm 5 tiêu chí
| Tiêu chí | Đạt? | Giải thích ngắn |
| --- | --- | --- |
| Gần core value | Có | Duyệt + xuất = user đã có bản gửi được; gần value hơn nhiều so với chỉ hỏi AI hay xem draft |
| Có thể lặp lại | Có | Lặp theo nhịp tuần và theo từng phòng ban cần báo cáo khác nhau |
| Có thể quan sát | Có | Có thời điểm rõ: approve + export success → event fire |
| Có ý nghĩa | Có | Nhiều báo cáo được duyệt–xuất đúng nghĩa sản phẩm đang thay thế quy trình tổng hợp thủ công; tăng “mở app” thì không |
| Có thể tác động | Có | Team cải thiện chất lượng draft, đối chiếu số liệu, template theo phòng ban, UX duyệt/xuất → tăng tỉ lệ hoàn tất |

Kết quả tự kiểm: **5/5** — giữ core action này.

Vì sao không phải “mở app” / “hỏi AI”?
- Mở app / đăng nhập / hỏi AI chỉ là thao tác giao diện hoặc bước trung gian; chưa chứng minh quản lý đã có báo cáo tuần gửi được.
- Value chỉ xảy ra khi user **chấp nhận số liệu + xuất** bản cho một phòng ban cụ thể — đó mới là hành vi gần core job nhất và đo được.

GATE 1: Đạt — có actor (quản lý), object (báo cáo tuần theo phòng ban), completion rule (approve + export); 5/5 tiêu chí; không nhầm với thao tác UI.
02 — Action Nature Card + kết luận cadence

### Action Nature Card
| Thành phần | Câu trả lời của bạn |
| --- | --- |
| Actor | Quản lý (người chịu trách nhiệm duyệt và xuất báo cáo). |
| Intent | Cần gửi báo cáo tổng kết tuần cho các phòng ban liên quan đảm bảo chính xác, đúng hạn mà không tốn công tổng hợp thủ công. |
| Trigger | Sự kiện thời gian (đến hạn nộp báo cáo cuối tuần/đầu tuần) và Hệ thống kích hoạt (notification báo đã gom đủ dữ liệu báo cáo ngày và sinh xong draft). |
| Effort | Thấp. Chủ yếu là công sức đọc lướt, kiểm tra độ hợp lý của số liệu/văn phong và ấn xác nhận (giảm đáng kể so với việc tự tổng hợp). |
| Value timing | Ngay lập tức. User có ngay bản báo cáo hoàn chỉnh sẵn sàng gửi sau khi click xuất/xác nhận. |
| State | Bản draft chuyển thành bản chính thức (exported/ready to send), dữ liệu báo cáo ngày chuyển sang trạng thái "đã được tổng hợp". |
| Dependency | Phụ thuộc mạnh vào nguồn cung dữ liệu (các báo cáo ngày phải được nộp và xác thực đầy đủ) và phụ thuộc thời điểm (đúng chu kỳ tuần). |
| Repeat condition | Khi một chu kỳ tuần mới kết thúc, hoặc khi có yêu cầu xuất thêm báo cáo cho một phòng ban nhận khác. |

### Kết luận cadence
- **Dạng hành vi:** Hành vi theo chu kỳ (chu kỳ tuần).

**Kết luận:**
Đối với **quản lý**, core action **duyệt và xuất báo cáo tuần** thường xuất hiện **định kỳ hàng tuần** vì **quy trình vận hành của tổ chức yêu cầu tổng kết và nộp báo cáo theo chu kỳ kết thúc tuần**. Do đó, nhịp đo phù hợp là **Weekly** ở cấp **User (Quản lý)**.
03 — Metric System

### 1. Activation metric
- **Start event:** `first_department_configured` (Quản lý thiết lập xong ít nhất một luồng nhận dữ liệu ngày và một phòng ban nhận báo cáo tuần).
- **Activation event:** `weekly_report_approved_exported` (Quản lý duyệt và xuất thành công bản báo cáo tuần đầu tiên).
- **Time window:** Trong vòng **14 ngày** kể từ Start event (để chờ dữ liệu ngày thu thập đủ và bao phủ ít nhất 1-2 chu kỳ tuần).

### 2. Engagement metric
- **Frequency:** Số lượng báo cáo tuần được xuất thành công trên mỗi quản lý trong một chu kỳ tuần (VD: 1 quản lý xuất gửi 3 phòng ban khác nhau).
- **Depth:** Tỉ lệ số lượng báo cáo xuất thành công trên tổng số phòng ban đã thiết lập (VD: setup gửi 4 phòng, thực tế hệ thống giúp gửi đủ cả 4).

### 3. North Star Metric (NSM) + leading + counter
- **North Star Metric:** Số lượng báo cáo tuần (Unit of value) được *duyệt/xuất với mức độ chỉnh sửa thủ công dưới 10%* (Quality threshold) lặp lại *đều đặn hàng tuần* (Frequency).
- **Leading indicators:**
  - *(1) Tỷ lệ gom đủ 100% báo cáo ngày trước chiều thứ 6:* Vì đủ nguyên liệu đầu vào thì draft mới chất lượng, có draft tốt mới xuất được.
  - *(2) Tỷ lệ click mở xem draft báo cáo tuần trong vòng 24h kể từ khi hệ thống báo có:* Tương tác sớm với draft dự báo khả năng chốt và xuất thành công rất cao.
  - *(3) Thời gian chỉnh sửa draft trung bình (Edit time):* Dưới 5 phút chứng tỏ AI làm rất sát ý, giảm friction cho user.
- **Counter-metric:** 
  - *Tỷ lệ báo cáo bị trả về / khiếu nại sai số liệu từ phòng ban nhận:* Báo cáo được xuất nhiều nhưng toàn sai số chứng tỏ AI bị hallucinate, phá vỡ core value thực sự của sản phẩm.

04 — Retention Definition

### Retention Definition Card
| Thành phần | Câu trả lời của bạn |
| --- | --- |
| Unit | User (Cụ thể: Quản lý hoặc Người làm tổng hợp). |
| Cohort entry | `first_weekly_report_exported` (User hoàn thành core action lần đầu tiên). |
| Return event | `weekly_report_approved_exported` (Duyệt và xuất báo cáo). |
| Window | Weekly bracket (W1, W2, W3...). Khớp hoàn toàn với nhịp tự nhiên. |
| Threshold | ≥ 1 lần trong window (Mỗi tuần thực hiện xuất ít nhất 1 báo cáo). |
| Segment | Quản lý vận hành có nghiệp vụ làm báo cáo tổng hợp gửi các phòng ban khác. |
05 — Product Loop (2 chu kỳ + metric hypothesis)

### Product Loop (Loại: Workflow)
**Chu kỳ 1:**
- **Natural trigger:** Chiều thứ Sáu, quản lý có nhiệm vụ phải gửi báo cáo tổng kết tuần cho phòng ban liên quan.
- **Core action:** Mở bản draft do AI sinh từ các báo cáo ngày đã xác thực, duyệt qua số liệu, sửa văn phong (nếu cần) và ấn Xuất (Approve & Export).
- **Immediate value:** Có ngay bản báo cáo hoàn chỉnh, đúng số liệu sẵn sàng gửi đi; tiết kiệm được hàng giờ đồng hồ so với việc copy-paste thủ công.
- **Saved state / investment:** Báo cáo được lưu trữ; các báo cáo ngày chuyển thành "đã tổng hợp". Quan trọng nhất: AI ghi nhận lịch sử chỉnh sửa (những câu từ user đã sửa) làm investment.

**Chu kỳ 2:**
- **Next natural trigger:** Chiều thứ Sáu tuần tiếp theo (hoặc chuyển sang gửi báo cáo cho phòng ban khác).
- **Core action tiếp theo:** Quản lý lại mở draft báo cáo tuần mới để duyệt và xuất.
- **Repeat value:** Nhờ investment từ chu kỳ 1, AI đã tự động áp dụng đúng văn phong quản lý thích. Draft chuẩn hơn, quản lý duyệt cực nhanh (có khi không cần sửa chữ nào), giá trị tiết kiệm thời gian càng tăng mạnh.

### Metric Hypothesis
**Nếu loop này hoạt động, metric** *Thời gian chỉnh sửa draft trung bình (Edit time)* **sẽ thay đổi theo hướng** *giảm dần (tiệm cận mức rất nhỏ)* **trong** *4 tuần đầu tiên*, **vì** *AI liên tục học được định dạng và văn phong từ "saved state" (lịch sử sửa chữa) của các chu kỳ duyệt trước đó*.

06 — Tracking nhanh

### Bảng Events
| Tên event | Ý nghĩa | Thời điểm ghi nhận | Metric sử dụng |
| --- | --- | --- | --- |
| `department_configured` | Quản lý thiết lập xong luồng báo cáo tuần cho 1 phòng ban nhận. | Ngay khi API tạo cấu hình phòng ban báo cáo trả về success 200. | Start event (Activation), Depth (Engagement) |
| `daily_report_verified` | Một báo cáo ngày được nạp và xác thực thành công. | Khi trạng thái của dữ liệu ngày trong DB đổi thành "verified". | Leading indicator (Tỷ lệ nguyên liệu đầu vào) |
| `weekly_draft_viewed` | Quản lý click mở xem bản draft báo cáo tuần. | Khi giao diện chi tiết của bản draft báo cáo tuần load xong trên UI. | Leading indicator (Tỷ lệ mở xem trong 24h) |
| `weekly_report_approved_exported` | Báo cáo tuần chính thức được chốt và xuất ra. | Khi API trả về trạng thái xuất file thành công (kèm props: `edit_time`, `edit_%`). | NSM, Activation event, Return event (Retention) |
| `report_rejected` | Báo cáo đã xuất bị phát hiện sai lệch (từ phía người nhận hoặc quản lý tự thấy). | Khi quản lý chủ động click nút "Báo cáo lỗi/Reject" cho bản đã xuất. | Counter-metric |

### Acceptance Criteria
1. **Chỉ ghi nhận khi hành vi hoàn tất thật sự (không bắt thao tác UI vô nghĩa):**
   Với event `weekly_report_approved_exported`, hệ thống chỉ bắn event duy nhất 1 lần khi API export trả về kết quả tạo file thành công. Tải lại trang (reload UI), mạng lag bấm nút Export 2 lần, hoặc ấn nút "Tải lại file" cho bản đã xuất không được sinh ra event trùng lặp (tránh làm NSM bị ảo).
2. **Đo thời gian chỉnh sửa (Edit time) chính xác:**
   Với thông số `edit_time` đi kèm trong event xuất báo cáo, hệ thống chỉ đếm khi tab đang `active` và có tương tác (chuột/bàn phím). Nếu không có tương tác quá 3 phút, bộ đếm phải tạm dừng (pause) để tránh trường hợp user mở draft rồi đi ăn trưa làm hỏng Leading indicator.

07 — Revision & Phase 5 (Tự soi lỗi & nộp)

### Kết quả tự kiểm (GATE 5)
1. [x] **Core action không phải thao tác giao diện/hệ thống:** Core action là "Duyệt và xuất báo cáo". Đây là lúc user chính thức chấp nhận kết quả của AI và tạo ra file thật để gửi đi, không phải chỉ xem draft hay click UI đơn thuần.
2. [x] **Activation không phải xem hướng dẫn:** Activation event gắn thẳng vào `weekly_report_approved_exported` đầu tiên, tức là user đã "chạm" được core value thật sự.
3. [x] **Frequency không cao hơn nhu cầu thật:** Nhịp tự nhiên được chốt là Weekly (chu kỳ tuần), hoàn toàn khớp với thực tế chu kỳ báo cáo công ty. Không ép user dùng daily.
4. [x] **Loop có reason to return ngoài notification:** Lý do quay lại mạnh mẽ nhất là nghiệp vụ ép buộc (phải nộp báo cáo) VÀ AI có "saved state" (học văn phong từ lịch sử) giúp những lần làm sau càng lúc càng nhàn.
5. [x] **Retention không dùng chung một window cho mọi cadence:** Window được đặt là "Weekly bracket", đồng bộ hoàn hảo với cadence Weekly ở Phase 2.
6. [x] **Mọi event đều map về một metric:** Cả 5 events định nghĩa trong Phase 4 đều trỏ thẳng về Start event, NSM, Leading hoặc Counter metric tương ứng.
7. [x] **Metric nào cũng có event để tính nó:** Toàn bộ metrics định nghĩa trong Phase 3 đều được nuôi bằng properties có trong bảng Tracking nhanh (VD: Edit Time cho Leading Indicator).

**Kết luận cuối cùng:** Bài làm mạch lạc, không mắc các lỗi thiết kế kinh điển. Chuỗi logic giữ vững từ đầu đến cuối: `Dự án tổng hợp báo cáo` -> `Persona Quản lý` -> `Core Action: Duyệt/Xuất báo cáo tuần` -> `Cadence: Weekly` -> `Retention: Weekly` -> `NSM: Báo cáo xuất thành công ít sửa đổi` -> `Events: Đo đạc chính xác hành vi export`.
