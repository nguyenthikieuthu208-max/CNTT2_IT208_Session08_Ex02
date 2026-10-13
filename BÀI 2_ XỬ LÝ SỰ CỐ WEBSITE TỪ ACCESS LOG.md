***BÀI 2: XỬ LÝ SỰ CỐ WEBSITE TỪ ACCESS LOG***

Phần 1: Phân tích sự nhầm lẫn của An

1\. Sự nhầm lẫn về mặt tư duy của An:

An đang bị nhầm lẫn nghiêm trọng giữa hai khái niệm: "Tìm kiếm tệp tin/thư mục" và "Tìm kiếm nội dung chữ bên trong tệp tin".

Lỗi 404 và chữ ERROR là các dữ liệu văn bản nằm bên trong ruột của tệp access.log, chứ không phải là tên của một tệp tin nằm trên ổ cứng. Do đó, việc An sử dụng lệnh quét tên tệp sẽ không bao giờ trả về kết quả mong muốn.

2\. Sự khác biệt bản chất giữa Get-ChildItem và Select-String:

Get-ChildItem: Đóng vai trò tương đương lệnh dir trên CMD hoặc ls trên Linux. Lệnh này chỉ có nhiệm vụ duy nhất là liệt kê, tìm kiếm các tên tệp tin, tên thư mục hoặc đường dẫn dựa trên bộ lọc \-Filter từ bên ngoài ổ đĩa. Nó hoàn toàn không có khả năng "đọc" nội dung bên trong file.

Select-String: Đóng vai trò tương đương lệnh findstr trên CMD hoặc grep trên Linux. Lệnh này chuyên dùng để mở tệp tin ra, đọc từng dòng chữ bên trong và sử dụng thuật toán tìm kiếm (Pattern Matching) để trích xuất chính xác các dòng có chứa từ khóa được yêu cầu.

Phần 2: Câu lệnh chuẩn trên PowerShell để lọc lỗi

Trên PowerShell, lệnh Select-String mặc định không phân biệt chữ hoa hay chữ thường (Case-insensitive) và tự động hiển thị số thứ tự dòng kèm theo kết quả trả về mà không cần lập trình viên phải gõ thêm bất kỳ cờ hiệu mở rộng nào (như /i /n của CMD hay \-i \-n của Bash).

Để lọc chính xác từ khóa error bên trong tệp access.log, câu lệnh chuẩn mực và ngắn gọn nhất trên PowerShell là:

powershell  
Select-String \-Path "access.log" \-Pattern "error"

Nếu bạn muốn lọc đồng thời cả 2 lỗi 404 và ERROR cùng một lúc (như bối cảnh yêu cầu):

Hệ thống PowerShell hỗ trợ truyền vào một mảng từ khóa ngăn cách bằng dấu phẩy. Bạn gõ câu lệnh nâng cao sau:

powershell  
Select-String \-Path "access.log" \-Pattern "404", "error"

Kết quả hiển thị thực tế trên màn hình:

Hệ thống sẽ trả về định dạng: Tên\_file : Số\_dòng : Nội\_dung\_dòng\_log, giúp kỹ sư DevOps ngay lập tức biết chính xác lỗi xảy ra ở dòng thứ bao nhiêu để mở file ra sửa lỗi thần tốc.

