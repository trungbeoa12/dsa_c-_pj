# Luyện DSA với C++ trên VS Code

Thư mục học cấu trúc dữ liệu và giải thuật (DSA) bằng C++17 trên macOS.

## 1. Các file và thư mục

| Đường dẫn | Dùng để làm gì? |
| --- | --- |
| `bai-tap/co-ban/` | Chứa bài tập cơ bản |
| `bai-tap/mang-vector/` | Chứa bài tập về mảng và vector |
| `templates/khungcode.cpp` | Khung code để tạo bài mới |
| `input.txt` | Dữ liệu đầu vào cho bài đang chạy |
| `output.txt` | Kết quả do chương trình ghi ra |
| `.vscode/tasks.json` | Cấu hình biên dịch (build) |
| `.vscode/launch.json` | Cấu hình chạy gỡ lỗi (debug) bằng CodeLLDB |

**Thư mục gốc** là thư mục chứa `bai-tap/`, `templates/`, `input.txt` và `output.txt`. Hãy mở cả thư mục này trong VS Code.

## 2. Chuẩn bị môi trường

- Công cụ biên dịch C++: trên macOS, chạy `xcode-select --install` nếu chưa cài Command Line Tools.
- Extension **C/C++** (`ms-vscode.cpptools`): hỗ trợ viết code C++.
- Extension **CodeLLDB** (`vadimcn.vscode-lldb`): hỗ trợ gỡ lỗi.

## 3. Quy trình làm một bài tập

1. Tạo bản sao của `templates/khungcode.cpp` trong thư mục chủ đề phù hợp, ví dụ `bai-tap/mang-vector/bai1.cpp`.
2. Viết phần lời giải trong file vừa tạo.
3. Thay nội dung `input.txt` bằng dữ liệu kiểm tra của bài đó.
4. Đảm bảo cả file `.cpp` và `input.txt` đã được lưu trước khi chạy. Tab còn dấu chấm thường là file còn thay đổi chưa lưu.
5. Biên dịch và chạy chương trình theo mục bên dưới.
6. Mở `output.txt` để xem kết quả và so sánh với kết quả mong đợi.

## 4. Biên dịch và chạy bằng Terminal

Mở Terminal trong VS Code tại **thư mục gốc**. Ví dụ, để chạy `bai-tap/mang-vector/bai1.cpp`:

```bash
g++ -std=c++17 -Wall -Wextra -g bai-tap/mang-vector/bai1.cpp -o bai-tap/mang-vector/bai1 && ./bai-tap/mang-vector/bai1
```

Ý nghĩa các phần trong lệnh:

| Thành phần | Ý nghĩa |
| --- | --- |
| `g++` | Gọi trình biên dịch C++ |
| `-std=c++17` | Sử dụng chuẩn C++17 |
| `-Wall -Wextra` | Bật thêm cảnh báo để dễ phát hiện lỗi |
| `-g` | Thêm thông tin phục vụ gỡ lỗi |
| `bai-tap/mang-vector/bai1.cpp` | File code cần biên dịch |
| `-o bai-tap/mang-vector/bai1` | Đặt đường dẫn và tên file thực thi |
| `&&` | Chỉ chạy lệnh tiếp theo nếu biên dịch thành công |
| `./bai-tap/mang-vector/bai1` | Chạy chương trình vừa biên dịch |

Khi chuyển sang bài khác, thay cả đường dẫn file `.cpp`, đường dẫn sau `-o` và đường dẫn sau `./` cho đúng bài.

### Tạo lệnh `run` trên macOS

Có thể thêm hàm sau vào file cấu hình zsh để biên dịch và chạy bài bằng một lệnh. Việc này cần thực hiện một lần trên mỗi máy Mac:

1. Mở file cấu hình: `nano ~/.zshrc`.
2. Thêm đoạn sau vào cuối file:

   ```zsh
   run() {
       if [ "$#" -ne 1 ]; then
           echo "Cach dung: run duong-dan/file.cpp"
           return 1
       fi

       local src="$1"
       local exe="${src%.cpp}"

       g++ -std=c++17 -Wall -Wextra -g "$src" -o "$exe" &&
           "$PWD/$exe"
   }
   ```

3. Lưu file (`Ctrl + O`, `Enter`, `Ctrl + X` trong nano), rồi nạp cấu hình: `source ~/.zshrc`.
4. Trong Terminal, chuyển đến thư mục gốc của project và chạy, ví dụ: `run bai-tap/set/set4.cpp`.

Hàm `run` cần `g++`; cài Xcode Command Line Tools bằng `xcode-select --install` nếu máy chưa có. Các cấu hình `.zshrc` chỉ áp dụng trên máy hiện tại. Không cần chép các dòng có đường dẫn riêng của máy như Maven, MySQL hoặc thiết lập prompt.

## 5. Dùng phím tắt trong VS Code

- **Build: `Cmd + Shift + B`**. Với task chỉ biên dịch, thao tác này tạo file thực thi nhưng chưa chạy chương trình, nên `output.txt` chưa có kết quả mới.
- **Debug: `F5`** (hoặc `Fn + F5`, tùy bàn phím). Đặt breakpoint bằng cách bấm cạnh số dòng để chương trình dừng tại đó và kiểm tra giá trị biến.

Các phím tắt phụ thuộc vào nội dung `tasks.json` và `launch.json`. Cấu hình chạy/debug cần dùng thư mục gốc làm thư mục làm việc (`cwd` là `${workspaceFolder}`) để đọc đúng `input.txt` và ghi đúng `output.txt`.

## 6. Cách dùng input.txt và output.txt

Trong khung C++, cần có `#include <cstdio>` để dùng `freopen`. Đặt đoạn sau ở đầu hàm `main()`, trước khi đọc hoặc in dữ liệu:

```cpp
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
```

- `cin` đọc dữ liệu từ `input.txt` thay vì bàn phím.
- `cout` ghi kết quả vào `output.txt` thay vì Terminal.
- Chế độ `"w"` xóa nội dung cũ của `output.txt` khi mở file thành công. Mỗi lần chạy sẽ ghi kết quả mới.
- Hai file dùng chung cho các bài tập chạy lần lượt. Chuyển bài thì thay dữ liệu trong `input.txt`; không cần tự điền kết quả vào `output.txt`.
- Đường dẫn được tính từ **thư mục làm việc khi chạy chương trình**, không phải thư mục chứa file `.cpp`. Vì vậy, hãy chạy từ thư mục gốc.

`#ifndef ONLINE_JUDGE` có nghĩa là chỉ dùng hai lệnh `freopen` khi macro `ONLINE_JUDGE` chưa được định nghĩa. Không phải hệ thống chấm bài nào cũng định nghĩa macro này; hãy kiểm tra yêu cầu nơi nộp bài, hoặc bỏ hai lệnh `freopen` khi nộp.

### Ví dụ dữ liệu

Nếu chương trình đọc `n`, sau đó đọc `n` số nguyên và in lại chúng, `input.txt` có thể là:

```text
5
3 1 2 4 120
```

Ở đây, `5` là số lượng phần tử. Năm số phía sau là dữ liệu cần đọc. Kết quả mong đợi trong `output.txt`:

```text
3 1 2 4 120
```

## 7. Khi output trống hoặc không đúng

1. Kiểm tra file `.cpp` và `input.txt` đã được lưu chưa.
2. Kiểm tra Terminal có báo lỗi biên dịch không. Biên dịch lỗi thì lệnh dùng `&&` sẽ không chạy chương trình.
3. Kiểm tra đã chạy chương trình chưa, hay mới chỉ build.
4. Kiểm tra code có dùng `cout` để in kết quả không. Chỉ đọc dữ liệu bằng `cin` sẽ không tạo kết quả đầu ra.
5. Kiểm tra `input.txt` đúng định dạng và đủ dữ liệu mà code yêu cầu chưa.
6. Chạy `pwd` để xem thư mục làm việc và `ls` để kiểm tra có `input.txt` tại đó không.

**Ghi nhớ:** sửa code hoặc dữ liệu → lưu thay đổi → biên dịch và chạy → xem `output.txt`.
