# BAI-TAP-2_SQL-SERVER-2026

# Bài kiểm tra số 2: (Sẽ kiểm tra các kiến thức 6..11)
# PHẦN MỞ ĐẦU
Thông tin sinh viên:

Họ và tên:Nguyễn Phạm Đan Linh

Mã sinh viên: K235480106095

Lớp: K59KMT.K01

Khoa: Điện tử

Môn học: Hệ Quản Trị Cơ Sở Dữ Liệu (SQL Server)

Yêu Cầu Đầu Bài

Đề tài: Quản lý khách sạn

Thực hiện xây dựng một hệ thống quản lý khách sạn hoàn chỉnh trên SQL Server, đáp ứng các tiêu chí cụ thể sau đây:

Toàn bộ quá trình thực hiện phải được ghi lại thông qua các screenshot minh họa, mỗi bức ảnh đều đi kèm với câu lệnh SQL tương ứng và chú thích rõ ràng về chức năng, mục đích xử lý, cũng như kết quả đạt được.

Bài tập được nộp dưới dạng repository GitHub (public), bao gồm hai thành phần chính: tệp README.md chứa toàn bộ nội dung, hình ảnh minh họa và giải thích chi tiết, tệp baikiemtra2.sql chứa toàn bộ mã SQL.

Đánh giá dựa trên ba yếu tố quan trọng:

Logic xử lý dữ liệu — các câu lệnh có giải quyết đúng bài toán không

Quy tắc đặt tên — các bảng, cột, hàm có tuân theo chuẩn bướu Lạc Đà không

Commit history — quá trình phát triển có rõ ràng, có thể theo dõi được không. Deadline nộp bài là cuối kỳ, sinh viên cần hoàn thành và push lên GitHub trước ngày hết hạn.

Giới Thiệu Về Hệ Thống Quản Lý Khách Sạn

Xây dựng một hệ thống quản lý khách sạn (QuanLyKhachSan) từ nền tảng SQL Server, bao gồm các chức năng chủ yếu như quản lý thông tin khách hàng, quản lý dữ liệu phòng, và quản lý các lượt đặt phòng. Mỗi khách hàng đều có hồ sơ lưu trữ với các thông tin cá nhân, số điện thoại liên hệ, ngày sinh và điểm đánh giá; mỗi phòng trong khách sạn được phân loại theo loại phòng (Standard, Superior, Deluxe, Suite), có diện tích riêng biệt và giá thuê theo ngày; các lượt đặt phòng lưu giữ mối quan hệ giữa khách hàng và phòng, cùng với thời gian nhận-trả phòng và tiền cọc.

Toàn bộ bài làm được chia thành 5 phần chính theo thứ tự tăng độ phức tạp.

Thiết Kế Cơ Sở Dữ Liệu — khởi tạo các bảng KhachHang, Phong, DatPhong với các ràng buộc toàn vẹn (Primary Key, Foreign Key, Check Constraint) và chèn dữ liệu mẫu.
Xây Dựng Function — tạo các hàm tính toán như tính số ngày ở, tính doanh thu, tìm phòng trống; những hàm này giúp tái sử dụng logic và làm sạch mã.
Xây Dựng Stored Procedure — tạo các thủ tục lưu trữ để xử lý các nghiệp vụ phức tạp như đặt phòng mới, tính hóa đơn, báo cáo doanh thu theo tháng.
Trigger Xử Lý Nghiệp Vụ — định nghĩa các trigger tự động kích hoạt khi dữ liệu thay đổi (chẳng hạn, tự động cập nhật trạng thái phòng khi có đặt phòng mới, hoặc ghi log thay đổi).
Dùng Cursor và Xử Lý Dữ Liệu — sử dụng cursor để duyệt từng bản ghi và thực hiện các xử lý tuần tự, đồng thời so sánh hiệu năng giữa phương pháp cursor và phương pháp set-based, từ đó rút ra kết luận về tối ưu hóa.
Trong quá trình thực hiện, sẽ thiết kế cơ sở dữ liệu với tên chuẩn: QuanLyKhachSan_K235480106022. Mỗi phần lệnh SQL viết ra sẽ đi kèm một ảnh screenshot chứa mã lệnh và kết quả thực thi, với các chú thích chi tiết: ảnh dùng để minh họa bước nào, câu lệnh giải quyết vấn đề gì, kết quả thể hiện thông tin gì. Thông qua bài tập này sẽ nắm vững các khái niệm cơ bản và nâng cao của SQL Server, từ DDL (Data Definition Language) đến DML (Data Manipulation Language), từ những truy vấn đơn giản đến những xử lý phức tạp bằng Function, Procedure, Trigger, và Cursor, từ đó tích lũy kỹ năng thiết kế và quản trị cơ sở dữ liệu chuyên nghiệp.

## NỘI DUNG YÊU CẦU (GỒM 5 PHẦN):

### Phần 1: Thiết kế và Khởi tạo Cấu trúc Dữ liệu (Kiến thức 6, 7) 
+ Tạo cơ sở dữ liệu

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/ee12e08d-ec38-4da8-84ce-f3497d0038e1" />
                                      
                                      Tạo cơ sở dữ liệu QUANLYKHACHSAN_K235480106095 
                                      
+ Bảng [KHACHHANG]:
  
Phân tích:
  
MaKhachHang: Primary Key (PK) → định danh duy nhất, tự tăng

TenKhachHang: NOT NULL → bắt buộc nhập

DiemDanhGia: CHECK (CK) → chỉ cho phép giá trị từ 0–10

Ý nghĩa:

Đảm bảo dữ liệu khách hàng không bị trùng và hợp lệ.

<img width="1918" height="1079" alt="image" src="https://github.com/user-attachments/assets/968faf0f-e05a-4bcd-98d4-b51e11bd6e09" />

                                      Tạo bảng KhachHang
+ Bảng [PHONG]:

Mục đích:

Quản lý thông tin phòng trong khách sạn.

Câu lệnh SQL:

CREATE TABLE [Phong] (

    [MaPhong] INT IDENTITY(1,1) PRIMARY KEY,
    
    [LoaiPhong] NVARCHAR(50),
    
    [DienTich] FLOAT,
    
    [GiaPhong] DECIMAL(10,2),
    
    [TrangThai] BIT
    
)

Phân tích:

MaPhong: Primary Key (PK)

GiaPhong: dùng DECIMAL → đảm bảo tính chính xác tiền

TrangThai: kiểu BIT

0: phòng trống

1: phòng đã thuê

Ý nghĩa:

Giúp quản lý tình trạng phòng và giá thuê rõ ràng.

Kết quả:

Bảng được tạo thành công, có thể lưu dữ liệu phòng.

  <img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/b3d1271d-9467-487b-8d86-78c0416b1748" />
  


                    


### Phần 2: Xây dựng Function (Kiến thức 8, 9) 

  + Hãy cho biết trong SQL Server có những loại function build_in (hàm có sẵn) nào, nêu 1 vài system function build_in mà em tìm hiểu được (ko cần nhiều, cần đặc sắc theo góc nhìn của em), cho SQL khai thác các hàm đó.

  + Hàm do người dùng tự viết trong SQL thường mang mục đích gì? Nó có những loại nào? Mỗi loại thường được dùng khi nào? Tại sao có nhiều system function rồi mà vẫn cần tự viết fn riêng?

  + Viết 01 Scalar Function (Hàm trả về một giá trị): Đưa ra 1 logic cho cơ sở dữ liệu của em, mà cần dùng đến function này. (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ)

    Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.

  + Viết 01 Inline Table-Valued Function: Trả về danh sách các bản ghi theo một điều kiện lọc cụ thể (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ)

    Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.

  + Viết 01 Multi-statement Table-Valued Function: Thực hiện xử lý logic phức tạp bên trong (có sử dụng biến bảng) trước khi trả về kết quả. (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ)

    Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.

### Phần 3: Xây dựng Store Procedure (Kiến thức 10) 

  + Trong SQL Server có những SP có sẵn nào? nêu 1 vài system sp mà em tìm hiểu được, giải thích cách dùng chúng.

  + Viết 01 Store Procedure đơn giản để thực hiện lệnh INSERT hoặc UPDATE dữ liệu, có kiểm tra điều kiện logic (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ)

  + Viết 01 Store Procedure có sử dụng tham số OUTPUT để trả về một giá trị tính toán (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ, SP NÀY CÓ DÙNG THAM SỐ LOẠI OUTPUT)

  + Viết 01 Store Procedure trả về một tập kết quả (Result set) từ lệnh SELECT sau khi đã join nhiều bảng. (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ)

### Phần 4: Trigger và Xử lý logic nghiệp vụ (Kiến thức 11)

  + Viết 01 Trigger để tự động làm gì đó tại 1 bảng B khi mà dữ liệu thay đổi dữ liệu ở bảng A. Logic giải quyết do sv tự nghĩ ra, sao cho thực tế và thuyết phục.

  + Thử viết Trigger cho Bảng A : Khi insert thì cập nhật dữ liệu vào bảng B; sau đó viết trigger cho bảng B để khi B được cập nhật thì cập nhật sang bảng A : Quan sát các thông báo (nếu có) của hệ thống, giải thích các thông báo đó (nếu có). Đưa ra nhật xét cuối cùng về tình trạng này.

### Phần 5: Cursor và Duyệt dữ liệu (Kiến thức 11)

  + Viết một đoạn script sử dụng CURSOR để duyệt qua danh sách của 1 câu lệnh SQL dạng SELECT, duyệt qua từng bản ghi, xử lý riêng từng bản ghi (THEO LOGIC SV TỰ ĐẶT RA: SAO CHO HỢP LÝ VÀ THUYẾT PHỤC)

  + Tìm cách không sử dụng CURSOR để giải quyết bài toán mà em đã dùng CURSOR mới giải quyết được ở trên. thử so sánh tốc độ giữa có dùng cursor và không dùng cursor (nếu cùng kết quả) thì thời gian xử lý cái nào nhanh hơn, cần ảnh chụp màn hình minh chứng.

  + Nếu vẫn tìm được cách dùng SQL để giải quyết vấn đề mà ko cần CURSOR: thử nghĩ bài toán khác, mà chỉ CURSOR mới giải quyết được, còn SQL rất khó giải quyết đc (theo logic suy nghĩ của em)

---------

## HƯỚNG DẪN TRÌNH BÀY TRÊN GITHUB
Sinh viên trình bày file README.md theo cấu trúc:

Phần mở đầu: Thông tin cá nhân, yêu cầu đầu bài, giới thiệu sơ qua về cách làm.

Phần 1: Khởi tạo bảng

[Mô tả logic]

[Code SQL]

![Ảnh chụp màn hình kết quả]

Chú thích: Ảnh này cho thấy tôi đã tạo thành công 3 bảng với các kiểu dữ liệu đúng yêu cầu...

Phần 2: ... (Tương tự cho các câu tiếp theo, mỗi phần có thể có nhiều ảnh, mỗi ảnh hãy gõ thêm chú thích)

...

Lưu ý: Mọi hành vi sao chép code hoặc ảnh chụp của người khác sẽ bị phát hiện tự động và nhận điểm 0 (cấm thi) theo quy định của môn học.

code sql lưu vết các demo trên lớp được gửi kèm trong repo này, readme.md chứa bài tập 1 được đổi tên thành bai_tap_01.md để lưu trữ, readme được tạo mới với nội dung bài tập 2. 

sv làm bài xong cập nhật link repo (public access able) vào file sau: https://docs.google.com/spreadsheets/d/1iwHJ6qSFKkS3iUjtlbCxw_0jUWC56PnCso1kcgxOEas/edit?usp=sharing  (nhớ dùng tài khoản @tnut)
