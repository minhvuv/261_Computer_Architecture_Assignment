# PHÂN CÔNG CÔNG VIỆC NHÓM — CA ASSIGNMENT

**Môn học:** Computer Architecture Lab (CO2008)  
**Đề tài:** Checkers Boardgame with MARS MIPS Assembly  
**Ngôn ngữ / môi trường:** MIPS Assembly, chạy trên MARS MIPS Simulator  
**Số thành viên:** 03 (đúng giới hạn tối đa 3 người trong đề)

## 1. Thành viên và phạm vi công việc

| Thành viên | Module phụ trách chính | Phần việc và đầu ra | Tỷ trọng đề xuất |
|---|---|---|---|
| **Vũ Như Hoàng Minh** | **Board, Terminal UI & Main Loop** | Khởi tạo bàn cờ 8×8 (32 ô có thể đi), hiển thị trạng thái sau mỗi lượt trên MARS, đọc ô bắt đầu/ô đích, thông báo lỗi; xây dựng `main` điều phối luồng chơi. | ~33% |
| **Hàng Khang Định** | **Move Validation & Promotion** | Kiểm tra người chơi và quyền sở hữu quân; nước đi chéo hợp lệ, ô đến trống, hướng đi của quân thường, đi hai hướng của King; xử lý phong cấp; phối hợp quy tắc đổi lượt. | ~33% |
| **Dương Thế Tuấn Kiệt** | **Capture, Chain Capture & Win Conditions** | Ăn quân bằng nhảy qua quân đối phương; xóa quân bị ăn; phát hiện và bắt buộc tiếp tục chuỗi ăn; kiểm tra thắng khi đối phương hết quân; xây dựng test cho tình huống phức tạp. | ~34% |

> **Nguyên tắc:** Mỗi thành viên chịu trách nhiệm thiết kế, lập trình, tự kiểm thử và giải thích được module của mình. Cả nhóm cùng kiểm thử tích hợp và review code. Tỷ trọng là gợi ý phân chia công việc nội bộ, **không phải thang điểm của giảng viên**.

## 2. Công việc chi tiết theo người

### A. Vũ Như Hoàng Minh — Board, UI & Main Loop

**Công việc bắt buộc**

- [ ] Thiết kế biểu diễn bàn cờ: 32 ô chơi được trên bàn 8×8; chuẩn hóa cách đánh số ô **1–32** và ánh xạ giữa ô/row/column.
- [ ] Khởi tạo bàn cờ: 12 quân đỏ (`r`) và 12 quân đen (`b`); quân đen đi trước.
- [ ] Viết hàm in bàn cờ **mỗi lượt** lên terminal MARS; phân biệt `r`, `b`, `R`, `B`, `0` và ô không thể đi.
- [ ] Viết lời nhắc nhập **starting square** và **landing square**, đọc dữ liệu nguyên từ terminal.
- [ ] Xử lý nhập ngoài khoảng, phản hồi nước đi không hợp lệ và yêu cầu nhập lại (gọi hàm kiểm tra của Định).
- [ ] Xây dựng vòng lặp `main`: hiển thị bàn cờ → nhập → kiểm tra → thực hiện nước đi → xử lý chuỗi ăn / thắng → đổi lượt.
- [ ] Đảm bảo game chạy liên tục đến khi có người thắng và kết thúc chương trình.

**Đầu ra cần bàn giao**

- Các routine: `init_board`, `print_board`, `read_move`, `main` (tên có thể đổi theo thống nhất nhóm).
- Test: bàn cờ khởi tạo đúng; ký tự in đúng; thông báo lượt đúng; nhập sai có thể nhập lại; sau mỗi nước đi in trạng thái mới.

### B. Hàng Khang Định — Move Validation & Promotion

**Công việc bắt buộc**

- [ ] Viết kiểm tra ô nguồn có quân và quân đó thuộc **người đang đi**.
- [ ] Kiểm tra ô đích nằm trong các ô hợp lệ, đang trống và khác ô nguồn.
- [ ] Kiểm tra quân thường đi **1 ô chéo về phía trước**; quân King đi **1 ô chéo về cả hai phía**.
- [ ] Phân biệt nước đi thường và nước ăn quân (phối hợp interface với Kiệt); không cho nước đi sai quy tắc.
- [ ] Xử lý **promotion** khi quân thường chạm hàng cuối; cập nhật `b → B` hoặc `r → R`.
- [ ] Bảo đảm nước đi của King vẫn chỉ có tầm di chuyển tương ứng quân thường, không đi xa nhiều ô tùy ý.
- [ ] Cung cấp kết quả hợp lệ/không hợp lệ cho `main`; ghi rõ lý do lỗi nếu nhóm muốn thông báo chi tiết.

**Đầu ra cần bàn giao**

- Các routine: `validate_move`, `is_own_piece`, `promote_if_needed`.
- Test: nước đi đúng/sai hướng; ô đích bị chiếm; chọn quân đối phương; chọn ô trống; King đi lùi; phong cấp hai màu.

### C. Dương Thế Tuấn Kiệt — Capture, Chain Capture & Winning

**Công việc bắt buộc**

- [ ] Kiểm tra nước **ăn quân**: nhảy chéo qua đúng một quân đối phương và đáp xuống ô trống.
- [ ] Cập nhật bàn cờ sau khi ăn: di chuyển quân đang đi và xóa quân đối phương.
- [ ] Sau khi ăn, tìm các bước ăn tiếp từ **chính quân vừa di chuyển**.
- [ ] Nếu còn bước ăn, **bắt buộc** người chơi tiếp tục chuỗi ăn (nhập tiếp ô đích); chỉ kết thúc lượt khi không còn nước ăn nối tiếp.
- [ ] Hỗ trợ quân King ăn về trước hoặc sau; khi được phong King từ một lượt ăn, cho phép tiếp tục ăn lùi ngay nếu có thể (theo đề).
- [ ] Kiểm tra điều kiện thắng **tối thiểu**: đối phương không còn quân trên bàn; thông báo bên thắng và kết thúc game.
- [ ] Tạo các tình huống test: ăn 1 quân, ăn liên tiếp nhiều quân, đường ăn bị chặn, ăn đến hàng cuối được phong King rồi ăn tiếp.

**Đầu ra cần bàn giao**

- Các routine: `validate_capture`, `apply_capture`, `has_follow_up_capture`, `check_winner`.
- Test: xóa đúng quân; không thể nhảy qua quân mình; không thể đáp vào ô đã có quân; buộc ăn nối tiếp; kết thúc ván đúng.

## 3. Quy tắc tối thiểu phải hoàn thành (theo đề)

- [ ] Chạy bằng **MARS MIPS Assembly** (không dùng C/C++/Python làm sản phẩm chính).
- [ ] Bàn cờ English Checkers **8×8**, chỉ 32 ô chơi được; hai người thay lượt, đen trước.
- [ ] In trạng thái bàn cờ trên terminal MARS **mỗi lượt**.
- [ ] Nhập ô xuất phát/ô đến và **kiểm tra tính hợp lệ**; nước sai phải được nhập lại.
- [ ] Quân thường di chuyển chéo tiến; King đi và ăn chéo theo hai hướng, với tầm ngắn như quân thường.
- [ ] Có ăn quân, xóa quân bị ăn và **bắt buộc ăn tiếp nếu có chuỗi ăn**.
- [ ] Có phong King khi quân đến hàng cuối; xử lý tiếp chuỗi ăn sau phong cấp như mô tả trong đề.
- [ ] Phát hiện ít nhất trường hợp **đối phương hết quân**, công bố người thắng rồi kết thúc.

**Lưu ý quan trọng của đề:** Ở bản tối thiểu, **không bắt buộc** người chơi phải chọn ăn ngay khi có khả năng ăn ở đầu lượt; tuy nhiên **đã ăn rồi thì phải tiếp tục chuỗi ăn nếu có**. Ép phải ăn ngay từ đầu lượt là **tính năng nâng cao**, không phải yêu cầu tối thiểu.

## 4. Quy ước kỹ thuật để ghép code MIPS

Đây là **đề xuất nội bộ** nhằm tránh xung đột khi ghép bài; không phải quy chuẩn do đề đưa ra.

### 4.1. Dữ liệu dùng chung

| Tên | Kiểu đề xuất | Ý nghĩa |
|---|---|---|
| `board` | `32 × .byte` | Mảng 32 ô chơi được; ký tự `'0'`, `'r'`, `'R'`, `'b'`, `'B'` |
| `current_player` | `.byte` | `'b'` hoặc `'r'` |
| Số ô nhập | Integer `1..32` | Chuẩn hóa về index `0..31` trước khi truy cập mảng |

- **Minh** xác định và gửi cho cả nhóm bảng ánh xạ `square (1..32) ↔ (row, col)` trước khi code module di chuyển.
- Không tự ý giả định `index + 4`, `index - 4` luôn là một bước chéo hợp lệ: phải kiểm tra hàng, cột, hướng và biên bàn cờ.
- Nếu dùng `jal`, tất cả routine có gọi routine con phải lưu/khôi phục `$ra` và các thanh ghi cần bảo toàn theo quy ước nhóm.

### 4.2. Giao diện hàm được đề xuất

| Routine | Người làm | Input / Output thống nhất |
|---|---|---|
| `init_board` | Minh | Khởi tạo `board`, `current_player` |
| `print_board` | Minh | Đọc `board`, in lên terminal |
| `read_move` | Minh | Trả 2 số ô nhập (`$v0`: nguồn, `$v1`: đích) |
| `validate_move` | Định | `$a0`: ô nguồn; `$a1`: ô đích; `$a2`: cờ chỉ cho ăn tiếp; `$v0`: `0` sai, `1` đi thường, `2` ăn quân; **chưa sửa board** |
| `promote_if_needed` | Định | `$a0`: ô quân vừa đáp; cập nhật `board` nếu đạt hàng cuối |
| `apply_capture` | Kiệt | `$a0`: ô nguồn; `$a1`: ô đích; cập nhật board, xóa quân bị ăn |
| `has_follow_up_capture` | Kiệt | `$a0`: vị trí mới của quân vừa ăn; `$v0`: `1` nếu còn bước ăn, `0` nếu không |
| `check_winner` | Kiệt | `$v0`: `0` chưa kết thúc, `1` đen thắng, `2` đỏ thắng |
| `main` | Minh (cả nhóm review) | Điều phối các routine, đổi lượt chỉ khi chuỗi ăn đã kết thúc |

**Tách trách nhiệm:** Định kiểm tra một nước đi có hợp lệ hay không; Kiệt xử lý ăn quân và tìm chuỗi ăn. Hai người thống nhất trước cách phát hiện nước nhảy qua quân địch. Nước đi thường do `main` cập nhật board, hoặc tách thêm routine `apply_normal_move` do Định viết.

### 4.3. Ghép file

- Mỗi người có thể viết/kiểm thử routine riêng, nhưng cần ghép thành **một chương trình chạy được trong MARS** và bảo đảm không trùng nhãn (label) / symbol.
- Thống nhất cách truyền tham số qua `$a0–$a3`, trả kết quả qua `$v0/$v1`, quy tắc lưu thanh ghi và tên biến toàn cục.
- Chỉ `main` quyết định chuyển `current_player`; trong lúc chain capture, giữ nguyên người chơi.
- Có thể quản lý source trên Git theo nhánh mỗi người; cách tổ chức file và nơi nộp **chưa được đề quy định**.

## 5. Tính năng cộng điểm (chỉ làm sau khi đủ phần bắt buộc)

Theo mục **Extra tasks** của PDF, bản tối thiểu chỉ có thể đạt tối đa **9/10** (hoặc **8/10** với Honors). Những mục dưới đây là **gợi ý**, không có số điểm cộng cố định cho từng mục.

| Ưu tiên | Tính năng nâng cao | Người phụ trách đề xuất |
|---|---|---|
| 1 | Thông báo lỗi chi tiết; hiển thị bàn cờ đẹp hơn | **Minh** |
| 2 | Bắt buộc ăn quân khi có thể **ngay từ đầu lượt** | **Định + Kiệt** |
| 3 | Phát hiện thắng do **đối phương hết nước đi hợp lệ** (bị chặn) | **Kiệt**, Định hỗ trợ thuật toán |
| 4 | Tùy chọn chơi lại mà không thoát MARS | **Minh** |
| 5 | Đề nghị hòa / chấp nhận hòa | **Minh + Kiệt** |
| 6 | Xoay bàn cờ / đổi hệ tọa độ theo phía người chơi | **Minh + Định** |

Bàn 10×10 và luật chơi biến thể cũng được PDF liệt kê trong mục Advanced rulesets, nhưng nên để sau cùng do ảnh hưởng lớn đến cấu trúc dữ liệu và bộ luật.

## 6. Kế hoạch phối hợp và kiểm thử

| Mốc nội bộ (không phải hạn nộp của giảng viên) | Minh | Định | Kiệt | Kết quả chung |
|---|---|---|---|---|
| **M1 — Chốt thiết kế** | Chuẩn bàn cờ + mapping 1–32 + UI | Quy tắc nước đi + hàm trả kết quả | Quy tắc ăn + chiến lược chain/win | Thống nhất API và test cases |
| **M2 — Hoàn thành module** | Có thể chơi/nhập/in cơ bản | Validate + promotion | Capture + chain + win | Từng routine có test riêng |
| **M3 — Tích hợp** | Ghép `main`, điều phối | Sửa lỗi luật đi và King | Sửa lỗi chain, ăn quân và thắng | Chạy trọn ván trên MARS |
| **M4 — Hoàn thiện** | Làm sạch output, README | Review logic, bổ sung test | Test tình huống biên | Demo ổn định, giải thích được code |

### Bộ test tích hợp tối thiểu

- [ ] Khởi tạo đủ quân, các vị trí đúng, **Black** chơi trước.
- [ ] Đi thường hợp lệ làm cập nhật bàn cờ và chuyển lượt.
- [ ] Nguồn trống / quân của đối phương / đi sai hướng / đích bị chiếm → bị từ chối và nhập lại.
- [ ] Ăn quân thường hợp lệ → quân đối thủ bị xóa.
- [ ] Ăn nhiều bước liên tiếp → **không** cho kết thúc lượt trước khi hết chuỗi.
- [ ] Quân thường không được đi lùi, King được đi lùi nhưng không đi xa quá luật.
- [ ] Quân lên hàng cuối phong cấp thành King; nếu vừa ăn để phong cấp và còn nước ăn lùi thì tiếp tục được.
- [ ] Khi đối thủ hết quân → thông báo đúng người thắng, chương trình dừng.
- [ ] Tình huống đầu lượt có nước ăn nhưng người chơi chọn nước đi thường vẫn hợp lệ trong **bản tối thiểu**.

## 7. Báo cáo, nộp bài và lưu ý từ đề

- **Tài liệu giao nhiệm vụ:** *CA_Assignment.pdf*, phần **Requirements** (trang 10–12), **Extra tasks** (trang 13–14), **General** (trang 15 theo số trang trong PDF; tương ứng trang cuối của tệp).
- **Chấm điểm chú trọng:** chức năng, độ dễ đọc của source code, cấu trúc phần mềm, hiểu biết MIPS Assembly và kỹ năng báo cáo/thuyết trình.
- **Sử dụng AI để hỗ trợ code:** đề **không cấm**, nhưng yêu cầu hiểu và chủ động thiết kế chương trình.
- **Chống đạo văn:** không sao chép từ nhóm khác.
- **Nộp gì, nộp ở đâu, có báo cáo/demo/vấn đáp hay không, hạn nộp:** **PDF không quy định**; giảng viên sẽ thông báo riêng qua kênh khác. Không xem các mốc M1–M4 ở trên là deadline chính thức.

### Phân công phần giải trình / báo cáo (nếu giảng viên yêu cầu)

- **Minh:** Kiến trúc chung, cấu trúc dữ liệu bàn cờ, nhập/xuất terminal, luồng chính.
- **Định:** Thuật toán xác thực nước đi, quy tắc quân thường/King, phong cấp.
- **Kiệt:** Thuật toán ăn quân, ăn liên tiếp, xác định kết thúc trận, minh họa test.
- **Cả nhóm:** Kịch bản demo, mô tả cách chạy MARS, thảo luận lỗi và giới hạn chương trình.

---

**Nguồn:** *CA_Assignment.pdf* — Ho Chi Minh City University of Technology, Faculty of Computer Science and Engineering, Computer Architecture Lab (CO2008), “Checkers boardgame with MARS MIPS Assembly”, bản ghi *April 2025*. Phân công, API, mốc nội bộ và người phụ trách tính năng nâng cao là **đề xuất của nhóm**, không phải yêu cầu nguyên văn trong đề.
