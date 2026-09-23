
### 1. Nguyên lý TDD và chu trình Red–Green–Refactor

**TDD là gì?**

- TDD(Test-Driven Development)là phương pháp viết test cho hành vi mong muốn trước khi viết code đáp ứng hành vi đó.
- Test giúp xác định rõ đầu vào, kết quả mong muốn và các trường hợp lỗi.
- Phát triển theo từng bước nhỏ, chạy test thường xuyên để nhận phản hồi.
- Viết test sau khi hoàn thành chức năng vẫn hữu ích, nhưng không đồng nghĩa đã áp dụng TDD.

**Chu trình Red–Green–Refactor:**

- **Red:**
  - Viết test cho một hành vi chưa được đáp ứng.
  - Chạy test và quan sát thất bại.
  - Kiểm tra test thất bại đúng nguyên nhân, không phải do cấu hình hoặc import sai.

- **Green:**
  - Viết code vừa đủ để đáp ứng yêu cầu.
  - Chạy lại để test mới và các test hiện có đều đạt.
  - Không thêm chức năng ngoài phạm vi hiện tại.

- **Refactor:**
  - Cải thiện cấu trúc code: đặt tên rõ hơn, tách hàm hoặc loại bỏ lặp.
  - Giữ nguyên hành vi.
  - Chạy lại test sau khi chỉnh sửa.


### 2. Các cấp độ kiểm thử

**Unit test — Kiểm thử đơn vị**

- **Phạm vi:** một đơn vị logic nhỏ, chẳng hạn hàm tạo ticket.
- **Mục đích:** kiểm tra quy tắc nghiệp vụ, phát hiện lỗi nhanh, hỗ trợ refactor.
- **Dùng khi:** kiểm tra validation, tính toán, điều kiện và trường hợp biên.
- **Ví dụ:**
  - Tiêu đề rỗng bị từ chối.
  - Ticket mới có trạng thái `open`.
  - Cập nhật đúng ticket, giữ nguyên ticket khác.
- Không bắt buộc dùng mock nếu hàm có thể kiểm tra trực tiếp.

**Integration test — Kiểm thử tích hợp**

- **Phạm vi:** sự phối hợp giữa các thành phần.
- **Mục đích:** phát hiện lỗi truyền dữ liệu, đọc/ghi hoặc kết nối giữa các lớp.
- **Dùng khi:** kiểm tra file, database, API hoặc các module kết hợp.
- **Ví dụ:**
  - Lưu ticket vào file JSON thật.
  - Đọc lại file.
  - So sánh dữ liệu đọc được với dữ liệu đã lưu.

**End-to-end test — Kiểm thử đầu cuối**

- **Phạm vi:** luồng hoàn chỉnh qua giao diện sử dụng.
- **Mục đích:** xác nhận người dùng thực hiện được tác vụ quan trọng.
- **Dùng khi:** kiểm tra các luồng CLI từ nhận lệnh đến lưu dữ liệu và trả kết quả.
- **Ví dụ:**
  - Chạy lệnh tạo ticket.
  - Chạy lệnh xem ticket trong một tiến trình mới.
  - Kiểm tra dữ liệu, exit code và thông báo lỗi.
- Thường tốn thời gian hơn test nhỏ; không cần áp dụng cho mọi biến thể đầu vào.

### 3. Kiểm thử CLI

**Kiểm tra lệnh:**

- Lệnh hợp lệ thực hiện đúng chức năng.
- Lệnh không tồn tại được báo lỗi.
- Thiếu hoặc thừa đối số được xử lý rõ ràng.
- `--help` hiển thị hướng dẫn sử dụng.
- Ví dụ: gọi `create` mà không có tiêu đề phải báo thiếu dữ liệu.

**Kiểm tra dữ liệu đầu vào:**

- Tiêu đề không rỗng hoặc chỉ chứa khoảng trắng.
- ID đúng định dạng quy định.
- Trạng thái thuộc danh sách được hỗ trợ.
- Nội dung có dấu hoặc nhiều từ được tiếp nhận đúng.
- Ví dụ: ID `abc` phải bị từ chối nếu hệ thống yêu cầu số nguyên dương.

**Kiểm tra lưu trữ file:**

- Dữ liệu được lưu và đọc lại đúng.
- Dữ liệu vẫn tồn tại khi chạy chương trình lần sau.
- Cập nhật một ticket không làm thay đổi ticket khác.
- Phân biệt file chưa tồn tại với file bị hỏng.
- JSON hỏng phải được báo lỗi, không tự ghi đè.
- Dùng file tạm riêng khi kiểm thử tự động.

**Kiểm tra xử lý lỗi:**

- Thông báo giúp người dùng hiểu vấn đề.
- Thành công trả exit code `0`; thất bại trả mã khác `0`.
- Kết quả thông thường ở `stdout`; thông báo lỗi ở `stderr`.
- Thao tác bị từ chối không làm thay đổi dữ liệu cũ.
- Không thông báo thành công khi lưu file thất bại.

### 4. Kiểm chứng code AI

**Test giúp kiểm chứng code AI bằng cách:**

- Đối chiếu hành vi thực tế với yêu cầu.
- Phát hiện trường hợp AI bỏ sót.
- Cung cấp bằng chứng cụ thể để yêu cầu sửa code.
- Xác nhận bản sửa đã khắc phục lỗi.
- Phát hiện lỗi hồi quy khi thay đổi làm hỏng chức năng cũ.

**Ví dụ:**

- Yêu cầu: không chấp nhận tiêu đề trắng.
- Code AI chỉ kiểm tra chuỗi rỗng, bỏ sót `"   "`.
- Test với `"   "` phát hiện chương trình vẫn tạo ticket.
- Bổ sung validation.
- Chạy lại test đầu vào sai và test tạo ticket hợp lệ.

**Điểm cần chú ý:**

- Kiểm tra cả test do AI viết.
- Expected result phải dựa trên yêu cầu nghiệp vụ.
- Không sửa kỳ vọng đúng chỉ để test đạt.
- Không coi lời AI nói “test passed” là kết quả thực thi.
- Test đạt không bảo đảm mọi trường hợp đều đúng.

### 5. Những lỗi kiểm thử phổ biến và cách tránh

- **Assertion yếu:**
  - Lỗi: chỉ kiểm tra ticket tồn tại.
  - Cách tránh: kiểm tra cụ thể tiêu đề, trạng thái và các trường thuộc yêu cầu.

- **Chỉ kiểm tra trường hợp thành công:**
  - Lỗi: bỏ qua dữ liệu rỗng, ID sai hoặc file hỏng.
  - Cách tránh: bổ sung trường hợp lỗi và trường hợp biên.

- **Kiểm thử quá mức:**
  - Lỗi: dùng E2E cho mọi quy tắc nhỏ hoặc lặp lại quá nhiều kiểm tra.
  - Cách tránh: chọn cấp độ test phù hợp với rủi ro.

- **Kiểm tra chi tiết triển khai:**
  - Lỗi: bắt buộc code dùng một vòng lặp hoặc helper cụ thể.
  - Cách tránh: kiểm tra hành vi và kết quả quan sát được.

- **Test phụ thuộc nhau:**
  - Lỗi: test sau cần dữ liệu do test trước tạo.
  - Cách tránh: mỗi test tự chuẩn bị và dọn dữ liệu riêng.

- **Mock quá nhiều:**
  - Lỗi: thay thế cả phần hành vi cần kiểm tra.
  - Cách tránh: giữ logic cần kiểm tra chạy thật; có test cho ranh giới thực tế.

- **Tin tưởng hoàn toàn vào AI:**
  - Lỗi: chấp nhận code và test cùng dựa trên giả định sai.
  - Cách tránh: đối chiếu yêu cầu, review assertion và kiểm chứng thực tế.

## Phần 2. Áp dụng ba workflow AI

### 1. Layered Questioning — Đặt câu hỏi theo từng lớp

#### Research — Hỏi kiến thức nền tảng


- “TDD là gì?”
- “Chu trình Red–Green–Refactor là gì?”
- “Unit test, integration test và end-to-end test khác nhau như thế nào?”
- “Repository pattern là gì và tại sao nên tách `TicketRepository` khỏi `TicketService`?”
- “File JSON có những ưu điểm và hạn chế gì khi dùng làm nơi lưu trữ ticket?”

#### Brief — Đưa bối cảnh dự án

> Tôi đang xây dựng một Ticket Manager CLI bằng Java. Kiến trúc gồm CLI, `TicketService` và `TicketRepository`. `TicketRepository` lưu dữ liệu vào file JSON. Các lệnh chính là `add`, `list`, `get`, `update`, `close` và `delete`. Hãy đề xuất cách áp dụng TDD cho kiến trúc này.

Tiếp tục hỏi:

- “Với lệnh `add`, tôi nên viết test nào trước?”
- “Với lệnh `list` và `get`, cần kiểm tra những trường hợp nào?”
- “Với lệnh `update` và `close`, cần kiểm tra vòng đời trạng thái ra sao?”
- “Với lệnh `delete`, có nên cho phép xóa ticket đã `CLOSED` không?”

#### Example — Yêu cầu ví dụ nhỏ

> Hãy viết một unit test Java cho trường hợp tạo ticket với tiêu đề chỉ chứa khoảng trắng. Test phải thể hiện rõ input, expected exception và lý do business rule này cần tồn tại.

Sau đó hỏi tiếp:

- “Hãy viết implementation tối thiểu để test này pass.”
- “Test này có kiểm tra được `null` và chuỗi rỗng chưa?”
- “Hãy bổ sung test cho tiêu đề trùng và độ dài tối đa.”

#### Validation — Kiểm tra lại đề xuất

- “Implementation này còn bỏ sót edge case nào?”
- “Nếu file JSON chưa tồn tại thì kết quả là gì?”
- “Nếu file JSON bị hỏng thì chương trình nên báo lỗi hay tạo lại file?”
- “Hãy lập bảng mapping giữa từng business rule và test case tương ứng.”

### 2. Solution Exploration — Khám phá giải pháp

#### Bước 1: Đặt câu hỏi về vấn đề cần giải quyết

> Ticket Manager CLI nên lưu dữ liệu bằng File JSON, SQLite hay PostgreSQL? Dự án có quy mô nhỏ, chạy chủ yếu trên máy cá nhân và cần dễ test.

#### Bước 2: Yêu cầu AI đưa ra nhiều phương án

> Hãy so sánh File JSON, SQLite và PostgreSQL theo các tiêu chí: độ phức tạp triển khai, khả năng query, hiệu năng, khả năng mở rộng và mức độ dễ kiểm thử.

| Giải pháp | Ưu điểm | Nhược điểm |
| --- | --- | --- |
| File JSON | Đơn giản, dễ triển khai, dễ đọc và debug | Hiệu năng giảm khi dữ liệu lớn, khó query phức tạp |
| SQLite | Có transaction, hỗ trợ query và database nhẹ | Cần thiết kế schema và setup phức tạp hơn JSON |
| PostgreSQL | Mạnh, ổn định, phù hợp production và nhiều người dùng | Cần cài đặt và vận hành database server |

#### Bước 3: Hỏi AI phản biện từng phương án

- “Nếu chọn File JSON, dữ liệu có bị mất khi hai thao tác ghi xảy ra liên tiếp không?”
- “Nếu chọn SQLite, phần setup test có phức tạp hơn đáng kể không?”
- “PostgreSQL có thực sự cần thiết cho một CLI nhỏ chạy trên máy cá nhân không?”
- “Có thể thay đổi storage trong tương lai mà không sửa `TicketService` không?”

#### Bước 4: Đưa ra quyết định

Chọn **File JSON** vì quy mô dự án nhỏ, không cần query phức tạp, không cần cài đặt database server và dễ dùng file tạm để setup/teardown giữa các test.

Đặt interface để tách business logic khỏi cách lưu trữ:

```java
public interface TicketRepository {
    Ticket save(Ticket ticket);
    Optional<Ticket> findById(String id);
    List<Ticket> findAll();
    Ticket update(Ticket ticket);
    void delete(String id);
}
```

Kiểm tra lại:

> Nếu sau này chuyển từ JSON sang SQLite, phần nào của hệ thống cần thay đổi và phần nào phải giữ nguyên?

### 3. Iterative Refinement — Cải tiến qua nhiều vòng

#### Vòng 1: Yêu cầu implementation ban đầu

> Hãy viết hàm Java `updateStatus(Ticket ticket, Status newStatus)` để cập nhật trạng thái ticket.

```java
public void updateStatus(Ticket ticket, Status newStatus) {
    ticket.setStatus(newStatus);
}
```

#### Vòng 2: Review implementation

> Hãy review implementation trên theo các business rule sau: ticket chỉ được chuyển `OPEN → IN_PROGRESS → CLOSED`, không được nhảy cóc trạng thái, không được cập nhật ticket đã `CLOSED`, và không chấp nhận giá trị `null`.

Các vấn đề phát hiện được:

- Chưa kiểm tra `ticket` hoặc `newStatus` là `null`.
- Cho phép chuyển từ `OPEN` sang `CLOSED`.
- Cho phép cập nhật ticket đã `CLOSED`.
- Chưa kiểm tra trạng thái mới có hợp lệ hay không.

#### Vòng 3: Yêu cầu chỉnh sửa

> Hãy sửa hàm trên để kiểm tra `null`, chỉ cho phép `OPEN → IN_PROGRESS` và `IN_PROGRESS → CLOSED`, đồng thời từ chối mọi thay đổi trên ticket đã `CLOSED`.

```java
public void updateStatus(Ticket ticket, Status newStatus) {
    if (ticket == null || newStatus == null) {
        throw new IllegalArgumentException("Ticket and status are required");
    }

    Status currentStatus = ticket.getStatus();

    if (currentStatus == Status.CLOSED) {
        throw new IllegalStateException("Closed ticket cannot be updated");
    }

    boolean validTransition =
            (currentStatus == Status.OPEN && newStatus == Status.IN_PROGRESS)
            || (currentStatus == Status.IN_PROGRESS && newStatus == Status.CLOSED);

    if (!validTransition) {
        throw new IllegalStateException("Invalid status transition");
    }

    ticket.setStatus(newStatus);
}
```

#### Vòng 4: Viết test và kiểm chứng

Yêu cầu AI đề xuất test case:

- `OPEN → IN_PROGRESS`: được phép.
- `IN_PROGRESS → CLOSED`: được phép.
- `OPEN → CLOSED`: bị từ chối vì nhảy cóc trạng thái.
- `CLOSED → OPEN` hoặc `CLOSED → IN_PROGRESS`: bị từ chối.
- Ticket hoặc trạng thái mới là `null`: phải ném `IllegalArgumentException`.
- Trạng thái không thay đổi khi transition thất bại.

