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
CREATE TABLE [KhachHang] (
    [MaKhachHang] INT IDENTITY(1,1) PRIMARY KEY,
    [TenKhachHang] NVARCHAR(100) NOT NULL,
    [SoDienThoai] VARCHAR(15),
    [NgaySinh] DATE,
    [DiemDanhGia] INT CHECK (DiemDanhGia BETWEEN 0 AND 10)
)
  
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
                Tạo bảng Phong
+ Bảng [DatPhong]
  Mục đích:
Quản lý các lượt đặt phòng của khách hàng.

Câu lệnh SQL:

CREATE TABLE [DatPhong] (
    [MaDatPhong] INT IDENTITY(1,1) PRIMARY KEY,
    [MaKhachHang] INT,
    [MaPhong] INT,
    [NgayNhan] DATETIME,
    [NgayTra] DATETIME,
    [TienCoc] DECIMAL(10,2),

    FOREIGN KEY ([MaKhachHang]) REFERENCES [KhachHang]([MaKhachHang]),
    FOREIGN KEY ([MaPhong]) REFERENCES [Phong]([MaPhong])
)

Phân tích:

MaDatPhong: Primary Key (PK)
MaKhachHang: Foreign Key (FK) → liên kết bảng KhachHang
MaPhong: Foreign Key (FK) → liên kết bảng Phong

Ý nghĩa:

Đảm bảo chỉ có thể đặt phòng khi khách hàng và phòng tồn tại
Tăng tính toàn vẹn dữ liệu (không có dữ liệu “mồ côi”)

Kết quả:
Bảng được tạo thành công và liên kết đúng với các bảng khác.
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/97fde3c6-5b78-4182-b539-b2f559c46c06" />      
            Tạo bảng DatPhong
            + Chèn dữ liệu
            
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/db5cd0ba-ee9b-4747-9177-b3d663fb4f5f" />

  


                    


### Phần 2: Xây dựng Function (Kiến thức 8, 9) 

  + Hãy cho biết trong SQL Server có những loại function build_in (hàm có sẵn) nào, nêu 1 vài system function build_in mà em tìm hiểu được (ko cần nhiều, cần đặc sắc theo góc nhìn của em), cho SQL khai thác các hàm đó.
    
CÁC LOẠI BUILT-IN FUNCTION TRONG SQL SERVER

## Trong SQL Server, các hàm có sẵn (Built-in Functions) được chia thành các nhóm chính sau:

 1. Hàm xử lý chuỗi (String Functions)
Ví dụ: LEN(), UPPER(), LOWER(), CONCAT()

 3. Hàm ngày giờ (Date & Time Functions)
Ví dụ: GETDATE(), DATEDIFF(), DATEADD()

 5. Hàm toán học (Mathematical Functions)
Ví dụ: ABS(), ROUND(), CEILING()

 7. Hàm hệ thống (System Functions)
Ví dụ: NEWID(), ISNULL(), CAST()

 9. Hàm tổng hợp (Aggregate Functions)
Ví dụ: COUNT(), SUM(), AVG(), MAX(), MIN()

## MỘT SỐ BUILT-IN FUNCTION TIÊU BIỂU (THEO GÓC NHÌN CÁ NHÂN)

+ NEWID(): Tạo mã định danh duy nhất (GUID) và thường dùng để xáo trộn (random) dữ liệu

-- Lấy ngẫu nhiên 1 khách hàng để gợi ý dịch vụ
SELECT TOP 1 MaKhachHang, TenKhachHang
FROM [KhachHang]
ORDER BY NEWID();

Giải thích:
NEWID(): tạo ra một giá trị GUID ngẫu nhiên. Khi dùng trong ORDER BY, mỗi dòng sẽ được gán một giá trị ngẫu nhiên → giúp xáo trộn dữ liệu và lấy ra bản ghi bất kỳ.

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/00613313-1bac-4565-8ebb-a13be53ea059" />
                Lấy ngẫu nhiên một khách hàng trong hệ thống để phục vụ cho việc gợi ý hoặc kiểm thử dữ liệu.
 + DATEDIFF(): Tính khoảng cách giữa 2 mốc thời gian (rất quan trọng trong bài toán khách sạn)
-- Tính số ngày khách đã ở
SELECT 
    MaDatPhong,
    NgayNhan,
    NgayTra,
    DATEDIFF(DAY, NgayNhan, NgayTra) AS SoNgayO
FROM [DatPhong]
WHERE NgayTra IS NOT NULL;
Giải thích:
DATEDIFF() dùng để tính khoảng thời gian giữa 2 ngày. Trong hệ thống khách sạn, hàm này được dùng để tính số ngày khách lưu trú → làm cơ sở để tính tiền phòng.

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/3950161b-e118-4b80-b91a-4cc5bd8754bc" />

                        Tính số ngày khách đã ở dựa trên ngày nhận và ngày trả phòng.     
+ COALESCE(): Lấy giá trị đầu tiên khác NULL trong danh sách
-- Nếu khách chưa trả phòng thì lấy ngày hiện tại
SELECT 
    MaDatPhong,
    NgayNhan,
    NgayTra,
    COALESCE(NgayTra, GETDATE()) AS NgayTraThucTe
FROM [DatPhong];

Giải thích:
COALESCE() trả về giá trị đầu tiên khác NULL trong danh sách. Trong bài toán khách sạn, nếu khách chưa trả phòng (NgayTra = NULL) thì sẽ thay bằng ngày hiện tại (GETDATE()) để tiếp tục tính toán.

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/dc86c226-78b4-44b7-a1af-911e4696864a" />
      Xử lý dữ liệu NULL của cột NgayTra bằng cách thay thế bằng thời gian hiện tại để đảm bảo tính toán không bị gián đoạn.
      
 ## + Hàm do người dùng tự viết trong SQL thường mang mục đích gì? Nó có những loại nào? Mỗi loại thường được dùng khi nào? Tại sao có nhiều system function rồi mà vẫn cần tự viết fn riêng?
 ## Hàm do người dùng tự định nghĩa (User-Defined Functions – UDF)
a) Khái niệm và mục đích

Trong SQL Server, ngoài các hàm có sẵn, người dùng có thể tự xây dựng các hàm riêng nhằm phục vụ cho những yêu cầu xử lý dữ liệu đặc thù của hệ thống.

Về bản chất, UDF là một khối lệnh SQL có tên, nhận tham số đầu vào và trả về kết quả (dạng giá trị hoặc bảng).

Việc sử dụng UDF mang lại nhiều lợi ích:

Giúp chuẩn hóa logic xử lý (tránh viết lặp nhiều lần)
Làm cho câu lệnh SQL dễ đọc, rõ ràng hơn
Tăng khả năng tái sử dụng trong toàn bộ hệ thống
Hỗ trợ bảo trì và nâng cấp thuận tiện

+ Trong bài toán quản lý khách sạn, các nghiệp vụ như:

tính tiền phòng
truy xuất lịch sử đặt phòng
thống kê hoạt động
đều rất phù hợp để triển khai dưới dạng UDF
b) Các loại hàm và cách sử dụng

Trong SQL Server, UDF được chia thành 3 nhóm chính:

 1. Scalar Function

Đây là loại hàm đơn giản nhất, trả về một giá trị duy nhất (kiểu số, chuỗi, ngày...).

 Đặc điểm:

Có thể dùng trong SELECT như một cột
Thực hiện tính toán trên từng bản ghi

 Khi sử dụng:

Khi cần xử lý logic tính toán độc lập
Ví dụ: tính tổng tiền của một lần đặt phòng
 2. Inline Table-Valued Function (ITVF)

Đây là hàm trả về một tập kết quả dạng bảng, được xây dựng từ một câu lệnh SELECT duy nhất.

 Đặc điểm:

Không sử dụng biến trung gian
Hiệu năng cao (gần giống VIEW nhưng linh hoạt hơn)

 Khi sử dụng:

Khi cần truy vấn dữ liệu có điều kiện động
Ví dụ: danh sách lịch sử đặt phòng theo từng khách
 3. Multi-statement Table-Valued Function (MSTVF)

Đây là loại hàm nâng cao, cũng trả về bảng nhưng cho phép xử lý nhiều bước logic bên trong.

 Đặc điểm:

Sử dụng biến bảng (@Table)
Có thể dùng IF, UPDATE, vòng lặp

 Khi sử dụng:

Khi bài toán cần xử lý nhiều bước hoặc điều kiện phức tạp
Ví dụ: báo cáo thống kê, phân loại dữ liệu
c) Vì sao vẫn cần UDF khi đã có Built-in Function?

Các hàm có sẵn (Built-in) như:

DATEDIFF()
GETDATE()
ISNULL()

chỉ giải quyết các thao tác cơ bản, mang tính tổng quát.

Tuy nhiên, trong thực tế:

Mỗi hệ thống có quy tắc nghiệp vụ riêng
Các yêu cầu thường là kết hợp nhiều bước xử lý
Không thể biểu diễn bằng một hàm đơn lẻ

 Ví dụ:

Built-in chỉ giúp tính số ngày

Nhưng:

Tổng tiền = số ngày × giá phòng
→ đây là logic nghiệp vụ → phải tự xây dựng hàm

Do đó, UDF được sử dụng để:

Đóng gói logic phức tạp thành một đơn vị độc lập
Tái sử dụng trong nhiều truy vấn
Đảm bảo tính nhất quán của hệ thống

  ## + Viết 01 Scalar Function (Hàm trả về một giá trị): Đưa ra 1 logic cho cơ sở dữ liệu của em, mà cần dùng đến function này. (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ). Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.

  🔥 Scalar Function – Tính tiền phòng sau khi trừ tiền cọc
📌 Ý tưởng (logic tự nghĩ )

Trong hệ thống khách sạn, ngoài việc tính tổng tiền phòng, cần xác định số tiền khách còn phải thanh toán sau khi đã trừ tiền đặt cọc.

👉 Công thức:

Số tiền phải trả = (Số ngày ở × Giá phòng) – Tiền cọc

🔄 Luồng xử lý
Bước 1: Nhận vào MaDatPhong
Bước 2: Lấy dữ liệu:
Ngày nhận
Ngày trả (nếu NULL → lấy GETDATE)
Giá phòng
Tiền cọc
Bước 3: Tính số ngày ở (tối thiểu 1 ngày)
Bước 4: Tính tổng tiền
Bước 5: Trừ tiền cọc
Bước 6: Trả về kết quả
✅ Code
CREATE FUNCTION fn_TienConLaiPhaiTra (@MaDatPhong INT)
RETURNS MONEY
AS
BEGIN
    DECLARE @NgayNhan DATETIME
    DECLARE @NgayTra DATETIME
    DECLARE @GiaPhong MONEY
    DECLARE @TienCoc MONEY
    DECLARE @SoNgay INT
    DECLARE @TongTien MONEY
    DECLARE @ConLai MONEY

    -- Lấy dữ liệu
    SELECT 
        @NgayNhan = d.NgayNhan,
        @NgayTra = ISNULL(d.NgayTra, GETDATE()),
        @GiaPhong = p.GiaPhong,
        @TienCoc = d.TienCoc
    FROM DatPhong d
    JOIN Phong p ON d.MaPhong = p.MaPhong
    WHERE d.MaDatPhong = @MaDatPhong

    -- Tính số ngày (ít nhất 1 ngày)
    SET @SoNgay = DATEDIFF(DAY, @NgayNhan, @NgayTra)
    IF @SoNgay = 0 SET @SoNgay = 1

    -- Tính tổng tiền
    SET @TongTien = @SoNgay * @GiaPhong

    -- Trừ tiền cọc
    SET @ConLai = @TongTien - ISNULL(@TienCoc, 0)

    RETURN @ConLai
END
GO
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/c86103b3-970d-450d-8d8c-4ee83917bc0b" />
      Tạo hàm 
🔍 Khai thác hàm
-- Xem số tiền còn phải trả của từng đặt phòng
SELECT 
    MaDatPhong,
    MaKhachHang,
    MaPhong,
    NgayNhan,
    NgayTra,
    TienCoc,
    dbo.fn_TienConLaiPhaiTra(MaDatPhong) AS TienConLai
FROM DatPhong
<img width="1910" height="1079" alt="image" src="https://github.com/user-attachments/assets/9e2581cf-0da4-4e8b-94c7-fe7f82b2191f" />
Hàm fn_TienConLaiPhaiTra được xây dựng để tính số tiền khách còn phải thanh toán sau khi đã trừ tiền đặt cọc. Hàm sử dụng DATEDIFF để tính số ngày ở và ISNULL để xử lý trường hợp khách chưa trả phòng hoặc chưa có tiền cọc.

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
