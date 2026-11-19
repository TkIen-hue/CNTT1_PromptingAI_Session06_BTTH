# Bài tập thực hành tổng hợp
## Prompt:
- Tôi là một sinh viên PTIT đang ôn tập môn Lập trình C++.
Các kiến thức:
- Đa hình cho phép các đối tượng thuộc các lớp khác nhau phản ứng với cùng một thông điệp theo những cách khác nhau.
- Đa hình động được thực hiện qua cơ chế Ghi đè phương thức (Method Overriding) và Liên kết động (Dynamic Binding / Virtual Table).
- Bạn là người nghe tò mò chưa từng học lập trình, yêu cầu tôi giải thích bằng các từ ngữ đời thường.
- Yêu cầu tôi dùng phép ẩn dụ đồ vật quen thuộc. Nếu tôi bí, hãy đặt câu hỏi gợi mở, tuyệt đối không khen xã giao.

- **Rubric:**
- Tôi là sinh viên đang tạo code thuộc PTIT cho môn Lập trình C++. Hãy đánh giá bài nộp của sinh viên dựa trên ma trận Rubric 4 tiêu chí sau:
- Correctness (40%): Code chạy đúng, không lỗi.
- Security, Robustness (20%): Kiểm tra đầu vào.
- Clean Code (20%): Tuân thủ chuẩn trình bày C++.
- Efficiency (20%): Thuật toán tối ưu, không thừa.

- Bạn vào vai 1 thầy giáo PTIT khắt khe, công tâm. Tuyệt đối không khen xã giao. Mọi điểm trừ phải đi kèm vị trí dòng lỗi và có giải pháp khắc phục.
- Một số ràng buộc:
1. Đặt temperature = 0.0 để triệt tiêu chấm điểm ngẫu nhiên.
2. Bắt buộc trả về kết quả theo cấu trúc.

- Đoạn code mẫu:
```
#include <iostream>
#include <cstdlib>

using namespace std;

int main() 
{
    double toan = rand() % 11;
    double ly = rand() % 11;
    double hoa = rand() % 11;

    double dtb = (toan + ly + hoa) / 3;

    cout << "Toan: " << toan << endl;
    cout << "Ly: " << ly << endl;
    cout << "Hoa: " << hoa << endl;
    cout << "Diem trung binh: " << dtb << endl;

    if (dtb > 8
) {
        cout << "Xep loai: Gioi" << endl;
    } 
else if (dtb >= 7
) {
        cout << "Xep loai: Kha" << endl;
    } 
else if (dtb >= 5
) {
        cout << "Xep loai: Trung binh" << endl;
    } 
else
 {
        cout << "Xep loai: Yeu" << endl;
    }

    return 0;
}
```
- Bài của sinh viên:
```
#include <iostream>
#include <cstdlib>
#include <ctime>

using namespace std
;

int main() 
{
    srand(time(
0
));

    double toan = rand() % 11
;
    double ly = rand() % 11
;
    double hoa = rand() % 11
;

    double dtb = (toan + ly + hoa) / 3
;

    string xeploai = (dtb > 8)  ? "Gioi"
 :
                     (dtb >= 
7) ? "Kha"
  :
                     (dtb >= 
5) ? "Trung binh" : "Yeu"
;

    cout << "Toan: " << toan << endl
;
    cout << "Ly: " << ly << endl
;
    cout << "Hoa: " << hoa << endl
;
    cout << "Diem trung binh: " << dtb << endl
;
    cout << "Xep loai: " << xeploai << endl
;

    return 0
;
}
```
## Kết quả:
- Nghe thì có vẻ hơi trừu tượng đấy. Cậu thử lấy một đồ vật quen thuộc hàng ngày làm ví dụ xem: khi gửi "cùng một thông điệp" tới những đồ vật khác nhau thì chúng phản ứng khác nhau như thế nào? Và làm sao đến lúc thực sự dùng mới biết đồ vật đó sẽ làm gì thay vì chốt sẵn từ đầu?

### 1. Correctness (3.5 / 4.0)

- Chương trình có seed ngẫu nhiên bằng srand(time(0)) và gộp logic xếp loại vào biến xeploai, tuy nhiên vẫn còn thiếu header chuẩn và cách chia số thực chưa rõ ràng.

- Điểm trừ -0.5 tại Dòng 24 (string xeploai = ...): Thiếu #include <string>. Dù một số trình biên dịch kéo gián tiếp qua <iostream>, việc phụ thuộc vào include bắc cầu là lỗi tính khả chuyển trong chuẩn C++.

- Giải pháp khắc phục: Thêm #include <string> ở đầu file (sau dòng 3) và dùng std::string hoặc giữ using namespace std; kèm header đầy đủ.

### 2. Security, Robustness (1.4 / 2.0)

- Có khởi tạo hạt giống ngẫu nhiên srand(time(0)) để tránh lặp chuỗi số giả ngẫu nhiên, nhưng thiếu kiểm soát kiểu ép/giá trị biên và chưa bảo vệ trước các tình huống bất thường của dữ liệu đầu vào/nguồn ngẫu nhiên.

- Điểm trừ -0.3 tại Dòng 10–12 (srand(time(0));): time(0) trả về time_t (thường là kiểu số nguyên có dấu 64-bit), ép ngầm sang unsigned int cho srand mà không kiểm soát kiểu tường minh; ngoài ra không có cơ chế kiểm tra tính hợp lệ của giá trị điểm nằm trong miền [0, 10].

- Giải pháp khắc phục: Ép kiểu tường minh srand(static_cast<unsigned int>(time(nullptr))); và bổ sung hàm/điều kiện kiểm tra miền điểm hợp lệ [0.0, 10.0] nếu mở rộng sang nhận dữ liệu từ người dùng hoặc nguồn ngoài.

- Điểm trừ -0.3 tại Dòng 21 (double dtb = (toan + ly + hoa) / 3;): Chia cho hằng số nguyên 3 thay vì 3.0 dễ gây nhầm lẫn về kiểu toán học khi đọc mã và dễ sai lệch ngữ nghĩa nếu sửa kiểu biến phía trước.

- Giải pháp khắc phục: Viết double dtb = (toan + ly + hoa) / 3.0; để thể hiện rõ phép chia số thực.

### 3. Clean Code (0.8 / 2.0)

- Trình bày mã nguồn lộn xộn, ngắt dòng tùy tiện, tách dấu chấm phẩy ; xuống dòng riêng lẻ và lồng toán tử ba ngôi ? : nhiều tầng làm giảm khả năng đọc hiểu.

- Điểm trừ -0.6 tại các Dòng 6, 15, 17, 19, 22, 31, 34, 36, 38, 40, 42, 45 (dấu ; đứng riêng một dòng): Đặt dấu chấm phẩy ở dòng mới vi phạm nghiêm trọng chuẩn trình bày C++.

- Giải pháp khắc phục: Đặt dấu ; ngay cuối câu lệnh trên cùng một dòng.

- Điểm trừ -0.6 tại các Dòng 10–12 và Dòng 24–31 (ngắt dòng tùy tiện và toán tử ba ngôi lồng nhau): Tách 0 xuống dòng 11, bẻ dòng ngẫu nhiên ở dtb >=  (dòng 26, 29) và lồng 3 cấp ? : khiến mã khó bảo trì.

- Giải pháp khắc phục: Viết srand(static_cast<unsigned int>(time(nullptr))); trên một dòng; thay chuỗi ? : lồng nhau bằng cấu trúc if / else if / else hoặc hàm xepLoai(double dtb) rõ ràng.

### 4. Efficiency (1.7 / 2.0)

- Thuật toán có độ phức tạp 𝑂(1) về cả thời gian và không gian, không có vòng lặp thừa.

- Điểm trừ -0.3 tại Dòng 33–41 (dùng endl liên tục trong xuất dữ liệu): std::endl vừa xuống dòng vừa ép flush bộ đệm xuất (std::cout), làm giảm hiệu năng I/O không cần thiết khi in nhiều dòng.

- Giải pháp khắc phục: Dùng '\n' thay cho endl ở các dòng xuất thông thường (Dòng 33–41), chỉ dùng '\n' hoặc flush khi thực sự cần đồng bộ bộ đệm ngay lập tức.