# FIT4012 Lab 7 - Báo cáo 1 trang: SHA-256

## 1. Mục tiêu / Objective
Tìm hiểu thuật toán SHA-256, thực hành băm chuỗi, kiểm tra toàn vẹn file, băm mật khẩu cơ bản và ứng dụng salt để tăng cường bảo mật cho mật khẩu.

## 2. Cách làm / Approach
- Biên dịch các chương trình C++ chứa thuật toán SHA-256.
- Kiểm thử thuật toán với các chuỗi đầu vào đã biết trước kết quả (Known answer test).
- Giả lập việc thay đổi nội dung file và dùng mã băm để phát hiện sự thay đổi.
- Viết chương trình lưu trữ mật khẩu bằng SHA-256 và quan sát sự khác biệt khi thêm chuỗi ngẫu nhiên (Salt).

## 3. Kết quả / Result
- Hash của chuỗi `abc`: `ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad`
- Hash của file mẫu trước khi sửa: `5ee62dc925a9958dbd6732c570a23c7f65a8c11066e889b15068cfb4bf1a0bd9`
- Kết quả kiểm tra file sau khi sửa nội dung: `[FAIL] File was changed or expected hash is incorrect`
- Kết quả đăng nhập với mật khẩu đúng: `[PASS] Login success`
- Kết quả đăng nhập với mật khẩu sai: `[FAIL] Login failed: wrong password`
- Hai bản ghi `salt:hash` của cùng một mật khẩu có giống nhau không?: **Không**, vì mỗi lần gọi hàm đăng ký, hệ thống tự động sinh ra một giá trị salt ngẫu nhiên khác nhau.

## 4. Kết luận / Conclusion
- **SHA-256 giúp phát hiện thay đổi dữ liệu như thế nào?** Chỉ cần một sự thay đổi rất nhỏ trong file (dù chỉ lật 1 bit), mã băm đầu ra sẽ thay đổi hoàn toàn (hiệu ứng thác đổ). Do đó, đối chiếu mã băm cho phép phát hiện ngay file đã bị sửa (tampered).
- **Vì sao cần salt khi lưu hash mật khẩu?** Để tránh trường hợp hai người dùng có cùng mật khẩu thì mã băm trong database giống hệt nhau, đồng thời chống lại kiểu tấn công bằng bảng tra cứu trước (Rainbow Table).
- **Vì sao SHA-256 demo trong lab chưa nên dùng trực tiếp cho hệ thống thật?** Thuật toán SHA-256 tính toán rất nhanh. Điều này lại là điểm yếu trong bảo mật mật khẩu vì hacker có thể dùng phần cứng mạnh (GPU) để thử sai (brute-force) hàng tỷ mật khẩu mỗi giây. Hệ thống thực tế cần các hàm băm cố tình được thiết kế để chạy chậm như Argon2id hoặc bcrypt.
