-- Tạo bảng Toys
CREATE TABLE Toys (
    ProductCode varchar(5) PRIMARY KEY,
    Name varchar(30),
    Category varchar(30),
    QtyOnHand int
);

-- Thêm dữ liệu vào bảng Toys
INSERT INTO Toys (ProductCode, Name, Category, QtyOnHand)
VALUES
    ('T001', 'Lego Set 1', 'Lego', 25),
    ('T002', 'Board Game 1', 'Board Game', 30),
    ('T003', 'Puzzle 1', 'Puzzle', 22),
    ('T004', 'Action Figure 1', 'Action Figure', 28),
    ('T005', 'Building Blocks 1', 'Building Blocks', 26),
    -- Thêm dữ liệu cho 10 sản phẩm khác ở đây
    ('T015', 'Remote Control Car 1', 'RC Car', 21);
-- Tạo thủ tục lưu trữ HeavyToys
CREATE PROCEDURE HeavyToys AS
BEGIN
    SELECT * FROM Toys
    WHERE Category = 'Action Figure' AND QtyOnHand > 500;
END;
-- Tạo thủ tục lưu trữ PriceIncrease
CREATE PROCEDURE PriceIncrease AS
BEGIN
    UPDATE Toys
    SET QtyOnHand = QtyOnHand + 10;
END;
-- Tạo thủ tục lưu trữ QtyOnHand
CREATE PROCEDURE QtyOnHand AS
BEGIN
    UPDATE Toys
    SET QtyOnHand = QtyOnHand - 5;
END;
-- Thực thi thủ tục lưu trữ HeavyToys
EXEC HeavyToys;

-- Thực thi thủ tục lưu trữ PriceIncrease
EXEC PriceIncrease;

-- Thực thi thủ tục lưu trữ QtyOnHand
EXEC QtyOnHand;
-- Xem định nghĩa của thủ tục lưu trữ HeavyToys
EXEC sp_helptext 'HeavyToys';

-- Xem định nghĩa của thủ tục lưu trữ PriceIncrease
EXEC sp_helptext 'PriceIncrease';

-- Xem định nghĩa của thủ tục lưu trữ QtyOnHand
EXEC sp_helptext 'QtyOnHand';
-- Xem định nghĩa của thủ tục lưu trữ HeavyToys
SELECT OBJECT_DEFINITION(OBJECT_ID('HeavyToys'));

-- Xem định nghĩa của thủ tục lưu trữ PriceIncrease
SELECT OBJECT_DEFINITION(OBJECT_ID('PriceIncrease'));

-- Xem định nghĩa của thủ tục lưu trữ QtyOnHand
SELECT OBJECT_DEFINITION(OBJECT_ID('QtyOnHand'));

EXEC sp_depends 'HeavyToys';
EXEC sp_depends 'PriceIncrease';
EXEC sp_depends 'QtyOnHand';

-- Chỉnh sửa thủ tục PriceIncrease
-- Chỉnh sửa thủ tục PriceIncrease
-- Chỉnh sửa thủ tục PriceIncrease
ALTER TABLE Toys
ADD UnitPrice DECIMAL(10, 2); -- Thay đổi kiểu dữ liệu và độ chính xác theo yêu cầu của bạn


ALTER PROCEDURE PriceIncrease AS
BEGIN
    UPDATE Toys
    SET QtyOnHand = QtyOnHand + 10, UnitPrice = UnitPrice + 10; -- Thay đổi UnitPrice theo yêu cầu
END;

-- Chỉnh sửa thủ tục QtyOnHand
ALTER PROCEDURE QtyOnHand AS
BEGIN
    UPDATE Toys
    SET QtyOnHand = QtyOnHand - 5, UnitPrice = UnitPrice + 10; -- Thay đổi UnitPrice theo yêu cầu
END;

-- Tạo thủ tục lưu trữ SpecificPriceIncrease

CREATE PROCEDURE SpecificPriceIncrease AS
BEGIN
    DECLARE @ProductCode varchar(5);
    DECLARE @QtyOnHand int;
    
    -- Lấy thông tin sản phẩm cần tăng giá
    SELECT TOP 1 @ProductCode = ProductCode, @QtyOnHand = QtyOnHand
    FROM Toys
    WHERE QtyOnHand > 0; -- Chỉ chọn sản phẩm có số lượng còn lại
    
    -- Tăng giá của sản phẩm dựa trên số lượng còn lại
    UPDATE Toys
    SET UnitPrice = UnitPrice + @QtyOnHand
    WHERE ProductCode = @ProductCode;
END;
-- Chỉnh sửa thủ tục lưu trữ SpecificPriceIncrease

ALTER PROCEDURE SpecificPriceIncrease
AS
BEGIN
    DECLARE @TotalUpdated INT;

    -- Lấy tổng số sản phẩm đã được cập nhật
    SELECT @TotalUpdated = COUNT(*) 
    FROM Toys
    WHERE QtyOnHand > 0; -- Chỉ cập nhật sản phẩm có số lượng còn lại

    -- Cập nhật giá của sản phẩm dựa trên số lượng còn lại
    UPDATE Toys
    SET UnitPrice = UnitPrice + QtyOnHand
    WHERE QtyOnHand > 0;

    -- Trả về tổng số bản ghi đã được cập nhật
    SELECT @TotalUpdated AS 'TotalUpdatedRecords';
END;

-- Chỉnh sửa thủ tục lưu trữ SpecificPriceIncrease
ALTER PROCEDURE SpecificPriceIncrease
AS
BEGIN
    DECLARE @TotalUpdated INT;

    -- Lấy tổng số sản phẩm đã được cập nhật
    SELECT @TotalUpdated = COUNT(*) 
    FROM Toys
    WHERE QtyOnHand > 0; -- Chỉ cập nhật sản phẩm có số lượng còn lại

    -- Gọi thủ tục lưu trữ HeavyToys để liệt kê sản phẩm nặng
    EXEC HeavyToys;

    -- Cập nhật giá của sản phẩm dựa trên số lượng còn lại
    UPDATE Toys
    SET UnitPrice = UnitPrice + QtyOnHand
    WHERE QtyOnHand > 0;

    -- Trả về tổng số bản ghi đã được cập nhật
    SELECT @TotalUpdated AS 'TotalUpdatedRecords';
END;

-- Chỉnh sửa thủ tục lưu trữ SpecificPriceIncrease với xử lý lỗi
ALTER PROCEDURE SpecificPriceIncrease
AS
BEGIN
    BEGIN TRY
        DECLARE @TotalUpdated INT;

        -- Lấy tổng số sản phẩm đã được cập nhật
        SELECT @TotalUpdated = COUNT(*) 
        FROM Toys
        WHERE QtyOnHand > 0; -- Chỉ cập nhật sản phẩm có số lượng còn lại

        -- Gọi thủ tục lưu trữ HeavyToys để liệt kê sản phẩm nặng
        EXEC HeavyToys;

        -- Cập nhật giá của sản phẩm dựa trên số lượng còn lại
        UPDATE Toys
        SET UnitPrice = UnitPrice + QtyOnHand
        WHERE QtyOnHand > 0;

        -- Trả về tổng số bản ghi đã được cập nhật
        SELECT @TotalUpdated AS 'TotalUpdatedRecords';
    END TRY
    BEGIN CATCH
        -- Xử lý lỗi ở đây
        SELECT ERROR_MESSAGE() AS 'ErrorMessage';
    END CATCH;
END;

-- Xóa bỏ thủ tục lưu trữ HeavyToys
DROP PROCEDURE IF EXISTS HeavyToys;

-- Xóa bỏ thủ tục lưu trữ PriceIncrease
DROP PROCEDURE IF EXISTS PriceIncrease;

-- Xóa bỏ thủ tục lưu trữ QtyOnHand
DROP PROCEDURE IF EXISTS QtyOnHand;

-- Xóa bỏ thủ tục lưu trữ SpecificPriceIncrease
DROP PROCEDURE IF EXISTS SpecificPriceIncrease;





