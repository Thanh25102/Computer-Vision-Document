# Tính Tổng Thời Gian Ở Văn Phòng

## Giới Thiệu

Chức năng tính tổng thời gian ở văn phòng của **Computer Vision** giúp quản trị viên (admin) có thể quản lý và theo dõi thời gian làm việc của nhân viên một cách chi tiết và chính xác. Hệ thống cho phép cấu hình giờ làm việc hành chánh và thời gian nghỉ trong ngày, đồng thời tự động tính toán thời gian làm việc dựa trên các sự kiện check-in và check-out của nhân viên.

## Cách Thức Tính Thời Gian

### Cấu Hình Giờ Làm Việc Và Thời Gian Nghỉ

1. **Cấu Hình Giờ Làm Việc Hành Chánh**:
   - Admin có thể cấu hình giờ bắt đầu và kết thúc của ca làm việc hành chánh.
   - Ví dụ: Giờ làm việc hành chánh bắt đầu từ 8:00 sáng và kết thúc lúc 5:00 chiều.

2. **Cấu Hình Thời Gian Nghỉ**:
   - Admin có thể thiết lập các khoảng thời gian nghỉ trong ngày.
   - Ví dụ: Thời gian nghỉ trưa từ 12:00 trưa đến 1:00 chiều.

## Quy Trình Tính Thời Gian

1. **Check-In và Check-Out Liên Tục**:
   - Nhân viên có thể check-in và check-out nhiều lần trong ngày.
   - Hệ thống sẽ tính toán thời gian dựa trên các cặp check-in và check-out liên tục.

2. **Xử Lý Các Cặp Check-In/Check-Out**:
   - Nếu có nhiều check-in liên tục, hệ thống sẽ lấy lần check-in đầu tiên.
   - Nếu có nhiều check-out liên tục, hệ thống sẽ lấy lần check-out đầu tiên.
   - Ví dụ: Nếu nhân viên check-in lúc 8:00, 8:05, và 8:10, hệ thống sẽ lấy lần check-in lúc 8:00. Tương tự, nếu nhân viên check-out lúc 11:55, 12:00, và 12:05, hệ thống sẽ lấy lần check-out lúc 11:55.

3. **Tính Thời Gian Của Các Cặp Check-In/Check-Out**:
   - Hệ thống sẽ tạo các cặp check-in/check-out liên tục để tính thời gian làm việc.
   - Ví dụ: Nếu nhân viên check-in lúc 8:00 và check-out lúc 11:55, sau đó check-in lại lúc 1:00 chiều và check-out lúc 5:00 chiều, hệ thống sẽ tạo hai cặp (8:00-11:55 và 1:00-5:00).

4. **Loại Bỏ Thời Gian Nghỉ**:
   - Thời gian nằm trong khoảng nghỉ do admin cấu hình sẽ không được tính vào tổng thời gian làm việc.
   - Ví dụ: Thời gian từ 12:00 trưa đến 1:00 chiều (giờ nghỉ trưa) sẽ không được tính.

## Sơ Đồ Quy Trình Tính Thời Gian 

```mermaid
graph TD;
    A[Cấu Hình Giờ Làm Việc] --> B{Nhân viên Check-in/Check-out Nhiều Lần}
    B -->|Nhiều Check-in liên tục| C[Lấy Check-in Đầu Tiên]
    B -->|Nhiều Check-out liên tục| D[Lấy Check-out Đầu Tiên]
    C --> E[Tạo Cặp Check-in/Check-out]
    D --> E[Tạo Cặp Check-in/Check-out]
    E --> F[Tính Thời Gian Của Cặp]
    F --> G{Có Thời Gian Nghỉ?}
    G -->|Có| H[Loại Bỏ Thời Gian Nghỉ]
    G -->|Không| I[Tính Tổng Thời Gian]
    H --> I[Tính Tổng Thời Gian]
   ```
    
## Ví Dụ Minh Họa

### Cấu Hình Của Admin
- **Giờ làm việc hành chánh**: 8:00 sáng - 5:00 chiều
- **Thời gian nghỉ trưa**: 12:00 trưa - 1:00 chiều

### Ví Dụ 1: Check-In/Check-Out Liên Tục

#### Hoạt Động Check-In/Check-Out Trong Ngày {id="ho-t-ng-check-in-check-out-trong-ng-y_1"}
- **Check-in**: 8:00 sáng
- **Check-in**: 8:05 sáng
- **Check-out**: 11:55 sáng
- **Check-out**: 12:00 trưa

#### Tính Toán Thời Gian {id="t-nh-to-n-th-i-gian_1"}
- **Cặp 1**: 8:00 sáng - 11:55 sáng (3 giờ 55 phút)

#### Tổng Thời Gian Làm Việc {id="t-ng-th-i-gian-l-m-vi-c_1"}
- **Tổng thời gian**: 3 giờ 55 phút

### Ví Dụ 2: Check-In/Check-Out Trong Thời Gian Nghỉ

#### Hoạt Động Check-In/Check-Out Trong Ngày {id="ho-t-ng-check-in-check-out-trong-ng-y_2"}
- **Check-in**: 8:00 sáng
- **Check-out**: 11:55 sáng
- **Check-in**: 12:05 trưa
- **Check-out**: 1:00 chiều
- **Check-in**: 1:05 chiều
- **Check-out**: 5:00 chiều

#### Tính Toán Thời Gian {id="t-nh-to-n-th-i-gian_2"}
- **Cặp 1**: 8:00 sáng - 11:55 sáng (3 giờ 55 phút)
- **Cặp 2**: 1:05 chiều - 5:00 chiều (3 giờ 55 phút)

#### Tổng Thời Gian Làm Việc {id="t-ng-th-i-gian-l-m-vi-c_2"}
- **Tổng thời gian**: 3 giờ 55 phút + 3 giờ 55 phút = 7 giờ 50 phút

### Ví Dụ 3: Check-In/Check-Out Trong Và Ngoài Thời Gian Nghỉ

#### Hoạt Động Check-In/Check-Out Trong Ngày
- **Check-in**: 8:00 sáng
- **Check-out**: 11:55 sáng
- **Check-in**: 12:00 trưa
- **Check-out**: 12:30 trưa
- **Check-in**: 1:00 chiều
- **Check-out**: 5:00 chiều

#### Tính Toán Thời Gian
- **Cặp 1**: 8:00 sáng - 11:55 sáng (3 giờ 55 phút)
- **Cặp 2**: 1:00 chiều - 5:00 chiều (4 giờ)

#### Tổng Thời Gian Làm Việc
- **Tổng thời gian**: 3 giờ 55 phút + 4 giờ = 7 giờ 55 phút

Thời gian làm việc của nhân viên trong ngày sẽ được tính là 7 giờ 55 phút, không bao gồm thời gian nghỉ trưa từ 12:00 trưa đến 1:00 chiều.

---

*Computer Vision - Giải Pháp Chấm Công Và Quản Lý Thời Gian Hiệu Quả Với AI Và Thị Giác Máy Tính Tích Hợp VCloud AI*
