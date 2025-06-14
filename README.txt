# HỆ THỐNG ĐẶT VÉ SỰ KIỆN - HƯỚNG DẪN CHI TIẾT

## 📚 LỊCH SỬ (History)

### Tổng quan về tính năng Lịch sử
Tính năng Lịch sử cho phép người dùng xem lại tất cả các giao dịch đặt vé đã thực hiện trước đó, bao gồm:
- Lịch sử đặt vé thành công
- Lịch sử giao dịch thất bại
- Trạng thái đơn hàng theo thời gian thực
- Thông tin chi tiết từng giao dịch

### Cách truy cập Lịch sử
1. **Đăng nhập vào tài khoản**
2. **Vào menu "Tài khoản"**
3. **Chọn "Lịch sử đặt vé"**
4. **Xem danh sách các giao dịch**

### Thông tin hiển thị trong Lịch sử
- **Mã đơn hàng**: Mã định danh duy nhất cho mỗi giao dịch
- **Tên sự kiện**: Tên chính thức của sự kiện đã đặt vé
- **Ngày đặt**: Thời điểm thực hiện giao dịch
- **Ngày diễn ra**: Thời gian sự kiện sẽ diễn ra
- **Số lượng vé**: Tổng số vé đã đặt
- **Tổng tiền**: Số tiền đã thanh toán
- **Trạng thái**: 
  - ✅ Thành công
  - ⏳ Đang xử lý
  - ❌ Thất bại
  - 🔄 Đã hoàn tiền

### Tính năng nâng cao của Lịch sử
- **Lọc theo thời gian**: Xem lịch sử theo ngày, tuần, tháng
- **Tìm kiếm**: Tìm kiếm theo tên sự kiện hoặc mã đơn hàng
- **Xuất báo cáo**: Tải xuống lịch sử dưới dạng PDF/Excel
- **In vé**: In lại vé từ lịch sử (nếu đã thanh toán thành công)

### Workflow của hệ thống (Tính năng Lịch sử)
1.  **Truy cập trang:** Người dùng đăng nhập và điều hướng đến trang "Lịch sử đặt vé".
2.  **Yêu cầu dữ liệu:** Component `LichSuPage` (trong `src/pages/lichsu.jsx`) được render. Hook `useEffect` được kích hoạt.
3.  **Gọi Firebase:** Bên trong `useEffect`, hàm `getUserBookingHistory` (từ `src/Data/firebaseUtils.js`) được gọi với ID của người dùng hiện tại.
4.  **Truy vấn Database:** `getUserBookingHistory` thực hiện truy vấn đến collection 'bookings' (hoặc tương tự) trong Firebase, lọc các bản ghi theo `userId` và sắp xếp kết quả.
5.  **Nhận và Cập nhật State:** Dữ liệu lịch sử trả về từ Firebase được cập nhật vào state của component `LichSuPage` (ví dụ: `setHistory`).
6.  **Hiển thị:** Component re-render, hiển thị danh sách các giao dịch, bao gồm thông tin như mã đơn hàng, tên sự kiện, ngày đặt, trạng thái.
7.  **Tương tác người dùng (Tùy chọn):** Người dùng có thể sử dụng các tính năng lọc (theo thời gian) hoặc tìm kiếm (theo tên sự kiện, mã đơn hàng) nếu được triển khai. Các hành động này sẽ kích hoạt việc lấy lại dữ liệu hoặc lọc dữ liệu hiện có.

---

## 🎪 CHI TIẾT SỰ KIỆN (Event Details)

### Cấu trúc trang Chi tiết sự kiện
Trang chi tiết sự kiện hiển thị đầy đủ thông tin về một sự kiện cụ thể:

### 1. **Header sự kiện**
- **Ảnh bìa**: Hình ảnh chất lượng cao của sự kiện
- **Tên sự kiện**: Tiêu đề chính
- **Ngày giờ**: Thời gian diễn ra chi tiết
- **Địa điểm**: Tên venue và địa chỉ cụ thể
- **Trạng thái**: Còn vé/Hết vé/Sắp diễn ra

### 2. **Thông tin chi tiết**
- **Mô tả sự kiện**: 
  - Giới thiệu tổng quan
  - Nội dung chương trình
  - Danh sách nghệ sĩ/diễn giả
  - Thể loại sự kiện
- **Thời gian diễn ra**:
  - Ngày bắt đầu
  - Giờ bắt đầu - kết thúc
  - Thời lượng dự kiến
- **Địa điểm**:
  - Tên venue
  - Địa chỉ đầy đủ
  - Bản đồ tích hợp
  - Hướng dẫn đi lại

### 3. **Thông tin vé**
- **Loại vé có sẵn**:
  - VIP: Ghế hạng sang, ưu đãi đặc biệt
  - Thường: Ghế tiêu chuẩn
  - Sinh viên: Giá ưu đãi cho sinh viên
- **Giá vé**: 
  - Giá gốc
  - Giá sau ưu đãi (nếu có)
  - Phí dịch vụ
- **Số lượng còn lại**: Cập nhật theo thời gian thực

### 4. **Gallery**
- **Ảnh sự kiện**: Bộ sưu tập hình ảnh
- **Video trailer**: Video giới thiệu (nếu có)
- **Ảnh venue**: Hình ảnh địa điểm tổ chức

### 5. **Đánh giá và nhận xét**
- **Rating**: Điểm đánh giá trung bình
- **Reviews**: Nhận xét từ người dùng đã tham gia
- **Comments**: Bình luận và thảo luận

### 6. **Sự kiện tương tự**
- **Recommended**: Các sự kiện cùng thể loại
- **From same organizer**: Sự kiện từ cùng ban tổ chức
- **In same location**: Sự kiện tại cùng địa điểm

### Workflow của hệ thống (Tính năng Chi tiết sự kiện)
1.  **Chọn sự kiện:** Người dùng nhấp vào một sự kiện từ danh sách (trang chủ, trang danh mục).
2.  **Điều hướng:** Hệ thống điều hướng người dùng đến trang chi tiết sự kiện (`src/pages/chitietsukien.jsx`), thường kèm theo `eventId` trong URL (ví dụ: `/events/EVENT_ID_123`).
3.  **Lấy Event ID:** Component `ChiTietSuKienPage` đọc `eventId` từ params của URL.
4.  **Yêu cầu dữ liệu:** Hook `useEffect` được kích hoạt, gọi hàm `getEventDetails` (từ `src/Data/firebaseUtils.js`) với `eventId` đã lấy được.
5.  **Truy vấn Database:** `getEventDetails` truy vấn Firebase để lấy thông tin chi tiết của sự kiện.
6.  **Nhận và Cập nhật State:** Dữ liệu sự kiện (tên, mô tả, hình ảnh, loại vé, giá, số lượng còn lại, v.v.) được trả về và cập nhật vào state của component.
7.  **Hiển thị:** Component re-render, hiển thị toàn bộ thông tin chi tiết của sự kiện.
8.  **Tương tác người dùng:** Người dùng xem thông tin, chọn loại vé, số lượng và có thể nhấn nút "Thêm vào giỏ hàng".


## 🛒 GIỎ HÀNG (Shopping Cart)

### Tổng quan về Giỏ hàng
Giỏ hàng là nơi lưu trữ tạm thời các vé đã chọn trước khi thanh toán:

### 1. **Chức năng cơ bản**
- **Thêm vé**: Thêm vé từ trang chi tiết sự kiện
- **Xóa vé**: Loại bỏ vé không mong muốn
- **Cập nhật số lượng**: Thay đổi số lượng vé
- **Tính tổng tiền**: Tự động tính toán tổng chi phí

### 2. **Thông tin hiển thị trong Giỏ hàng**
- **Tên sự kiện**: Tên đầy đủ của sự kiện
- **Ngày diễn ra**: Thời gian sự kiện
- **Loại vé**: VIP/Thường/Sinh viên
- **Số lượng**: Số vé đã chọn
- **Đơn giá**: Giá mỗi vé
- **Thành tiền**: Tổng tiền cho từng loại vé

### 3. **Tính năng nâng cao**
- **Lưu giỏ hàng**: Tự động lưu khi đăng nhập
- **Thời gian giữ chỗ**: Giữ vé trong 15 phút
- **Mã giảm giá**: Áp dụng coupon/voucher
- **Ước tính phí**: Phí dịch vụ, thuế (nếu có)

### 4. **Bảo mật Giỏ hàng**
- **Session timeout**: Tự động xóa sau thời gian nhất định
- **Verification**: Xác minh tính khả dụng của vé
- **Price protection**: Bảo vệ giá trong thời gian giữ chỗ

### 5. **Tối ưu hóa trải nghiệm**
- **Responsive design**: Tương thích mọi thiết bị
- **Quick checkout**: Thanh toán nhanh 1 click
- **Save for later**: Lưu để mua sau
- **Share cart**: Chia sẻ giỏ hàng với bạn bè

### Workflow của hệ thống (Tính năng Giỏ hàng)
1.  **Thêm vào giỏ:** Từ trang chi tiết sự kiện, người dùng chọn vé và nhấn "Thêm vào giỏ hàng".
2.  **Cập nhật State:** Hành động này kích hoạt một hàm (ví dụ: `addItemToCart` từ `CartContext` hoặc `src/state.js`) để thêm thông tin vé (ID sự kiện, loại vé, số lượng, giá) vào state của giỏ hàng.
3.  **Lưu trữ (Tùy chọn):** Giỏ hàng có thể được lưu trữ tạm thời ở client-side (localStorage, sessionStorage) hoặc đồng bộ với Firebase nếu người dùng đã đăng nhập để duy trì giữa các phiên.
4.  **Xem giỏ hàng:** Người dùng điều hướng đến trang "Giỏ hàng" (`src/pages/cart.jsx`).
5.  **Hiển thị:** Component `CartPage` đọc dữ liệu từ state giỏ hàng và hiển thị danh sách các vé đã chọn, cùng với số lượng, đơn giá, thành tiền cho từng loại vé.
6.  **Thay đổi giỏ hàng:** Người dùng có thể thay đổi số lượng vé hoặc xóa vé khỏi giỏ. Mỗi hành động này sẽ gọi các hàm tương ứng để cập nhật lại state giỏ hàng.
7.  **Tính tổng tiền:** Tổng số tiền của giỏ hàng được tự động tính toán lại mỗi khi có thay đổi.
8.  **Tiến hành thanh toán:** Người dùng nhấn nút "Tiến hành thanh toán" để chuyển sang quy trình đặt vé.

---

## 🎫 ĐẶT VÉ (Ticket Booking)

### Quy trình đặt vé chi tiết

### BƯỚC 1: Tìm kiếm và chọn sự kiện
1. **Trang chủ**: Duyệt sự kiện nổi bật
2. **Danh mục**: Chọn theo thể loại
3. **Tìm kiếm**: Tìm theo tên/địa điểm/ngày
4. **Filter**: Lọc theo giá/thời gian/địa điểm

### BƯỚC 2: Xem chi tiết và chọn vé
1. **Đọc thông tin**: Xem đầy đủ chi tiết sự kiện
2. **Chọn loại vé**: VIP/Thường/Sinh viên
3. **Chọn số lượng**: Nhập số vé cần mua
4. **Thêm vào giỏ**: Click "Thêm vào giỏ hàng"

### BƯỚC 3: Kiểm tra giỏ hàng
1. **Review**: Kiểm tra thông tin vé
2. **Modify**: Chỉnh sửa nếu cần
3. **Apply coupon**: Áp dụng mã giảm giá
4. **Proceed**: Tiến hành thanh toán

### BƯỚC 4: Thông tin khách hàng
1. **Đăng nhập**: Đăng nhập hoặc tạo tài khoản mới
2. **Thông tin cá nhân**:
   - Họ tên đầy đủ
   - Số điện thoại
   - Email
   - Địa chỉ (nếu cần)
3. **Xác minh**: OTP qua SMS/Email

### BƯỚC 5: Phương thức thanh toán
1. **Chọn phương thức**:
   - 💳 Thẻ tín dụng/ghi nợ
   - 🏦 Chuyển khoản ngân hàng
   - 📱 Ví điện tử (MoMo, ZaloPay, VNPay)
   - 💵 Tiền mặt (tại cửa hàng)

2. **Nhập thông tin thanh toán**:
   - Số thẻ/tài khoản
   - Thông tin xác thực
   - Mã bảo mật

### BƯỚC 6: Xác nhận và thanh toán
1. **Review final**: Kiểm tra lần cuối
2. **Accept terms**: Đồng ý điều khoản
3. **Submit payment**: Xác nhận thanh toán
4. **Processing**: Xử lý giao dịch

### BƯỚC 7: Hoàn tất đặt vé
1. **Confirmation**: Nhận xác nhận qua email/SMS
2. **E-ticket**: Vé điện tử được gửi
3. **Order tracking**: Theo dõi trạng thái đơn hàng
4. **Customer support**: Hỗ trợ sau bán hàng



### Workflow của hệ thống (Tính năng Đặt vé)
1.  **Bắt đầu thanh toán:** Từ trang giỏ hàng, người dùng nhấn "Tiến hành thanh toán", điều hướng đến trang `src/pages/thanhtoan.jsx`.
2.  **Nhập thông tin:** Người dùng điền các thông tin cần thiết (thông tin cá nhân nếu chưa đăng nhập hoặc chưa có, địa chỉ giao hàng nếu cần, chọn phương thức thanh toán).
3.  **Xác nhận đơn hàng:** Người dùng xem lại tóm tắt đơn hàng và nhấn "Xác nhận và Đặt vé".
4.  **Gọi hàm xử lý:** Component `ThanhToanPage` gọi hàm `processPaymentAndCreateOrder` (từ `src/Data/firebaseUtils.js`) với toàn bộ chi tiết đơn hàng.
5.  **Xử lý thanh toán:**
    *   `processPaymentAndCreateOrder` kết nối với cổng thanh toán (ví dụ: Stripe, PayPal, MoMo API).
    *   Gửi thông tin thanh toán để xác thực và xử lý.
6.  **Kết quả thanh toán:**
    *   **Thành công:**
        *   **Tạo đơn hàng:** Dữ liệu đơn hàng (thông tin người dùng, vé đã đặt, tổng tiền, trạng thái "Thành công") được lưu vào Firebase (ví dụ: collection 'bookings').
        *   **Cập nhật kho vé:** Số lượng vé còn lại của các sự kiện tương ứng được cập nhật (giảm đi).
        *   **Xóa giỏ hàng:** Giỏ hàng của người dùng được xóa (thường được xử lý trong `CartContext` hoặc `state.js`).
        *   **Thông báo:** Người dùng nhận được thông báo đặt vé thành công (trên web, email, SMS) cùng với mã đơn hàng. Điều hướng đến trang xác nhận hoặc lịch sử đơn hàng.
    *   **Thất bại:**
        *   **Thông báo lỗi:** Người dùng nhận được thông báo lỗi thanh toán và được yêu cầu thử lại hoặc chọn phương thức khác. Đơn hàng không được tạo.
