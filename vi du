USE AdventureWorks2022
GO
CREATE PROCEDURE sp_DisplayEmployeesHireYear
@HireYear int
AS
SELECT * FROM HumanResources.Employee
WHERE DATEPART(YY, HireDate)=@HireYear
GO
EXECUTE sp_DisplayEmployeesHireYear 2008
GO


CREATE PROCEDURE sp_EmployeesHireYearCount
@HireYear int,
@Count int OUTPUT
AS
SELECT @Count=COUNT(*) FROM HumanResources.Employee
WHERE DATEPART(YY, HireDate)=@HireYear
GO
DECLARE @Number int
EXECUTE sp_EmployeesHireYearCount 2008, @Number OUTPUT
PRINT @Number
GO

CREATE PROCEDURE sp_EmployeesHireYearCount2
@HireYear int
AS
DECLARE @Count int
SELECT @Count=COUNT(*) FROM HumanResources.Employee
WHERE DATEPART(YY, HireDate)=@HireYear
RETURN @Count
GO

DECLARE @Number int
EXECUTE @Number = sp_EmployeesHireYearCount2 2008
PRINT @Number
GO

CREATE TABLE #Students
(
RollNo varchar(6) CONSTRAINT PK_Students PRIMARY KEY,
FullName nvarchar(100),
Birthday datetime constraint DF_StudentsBirthday DEFAULT 
DATEADD(yy, -18, GETDATE())
)
GO
--Tạo thủ tục lưu trữ tạm để chèn dữ liệu vào bảng tạm
CREATE PROCEDURE #spInsertStudents
@rollNo varchar(6),
@fullName nvarchar(100),
@birthday datetime
AS BEGIN
IF(@birthday IS NULL)
SET @birthday=DATEADD(YY, -18, GETDATE())
INSERT INTO #Students(RollNo, FullName, Birthday)
VALUES(@rollNo, @fullName, @birthday)
END
go

EXEC #spInsertStudents 'A12345', 'abc', NULL
EXEC #spInsertStudents 'A54321', 'abc', '12/24/2011'
SELECT * FROM #Students
--Tạo thủ tục lưu trữ tạm để xóa dữ liệu từ bảng tạm theo RollNo
CREATE PROCEDURE #spDeleteStudents
@rollNo varchar(6)
AS BEGIN
DELETE FROM #Students WHERE RollNo=@rollNo
END
--Xóa dữ liệu sử dụng thủ tục lưu trữ
EXECUTE #spDeleteStudents 'A12345'
GO

CREATE PROCEDURE Cal_Square @num int=0 AS
BEGIN
RETURN (@num * @num);
END
GO
--Chạy thủ tục lưu trữ
DECLARE @square int;
EXEC @square = Cal_Square 10;
PRINT @square;
GO

SELECT 
OBJECT_DEFINITION(OBJECT_ID('HumanResources.uspUpdateEmployeePersonalInfo')) AS DEFINITION
--Xem định nghĩa thủ tục lưu trữ bằng
SELECT definition FROM sys.sql_modules
WHERE 
object_id=OBJECT_ID('HumanResources.uspUpdateEmployeePersonalInfo')
GO

sp_depends 'HumanResources.uspUpdateEmployeePersonalInfo'
GO

USE AdventureWorks2022
GO
--Tạo thủ tục lưu trữ sp_DisplayEmployees
CREATE PROCEDURE sp_DisplayEmployees AS
SELECT * FROM HumanResources.Employee
GO

ALTER PROCEDURE sp_DisplayEmployees AS
SELECT * FROM HumanResources.Employee
WHERE Gender='F'
GO
--Chạy thủ tục lưu trữ sp_DisplayEmployees
EXEC sp_DisplayEmployees
GO
--Xóa một thủ tục lưu trữ
DROP PROCEDURE sp_DisplayEmployees
GO
--
CREATE PROCEDURE sp_EmployeeHire
AS
BEGIN
--Hiển thị
EXECUTE sp_DisplayEmployeesHireYear 1999
DECLARE @Number int
EXECUTE sp_EmployeesHireYearCount 1999, @Number OUTPUT
PRINT N'Số nhân viên vào làm năm 1999 là: ' + 
CONVERT(varchar(3),@Number)
END
GO
--Chạy thủ tục lưu trữ
EXEC sp_EmployeeHire 
GO

ALTER PROCEDURE sp_EmployeeHire
@HireYear int
AS
BEGIN
BEGIN TRY
EXECUTE sp_DisplayEmployeesHireYear @HireYear
DECLARE @Number int
--Lỗi xảy ra ở đây có thủ tục sp_EmployeesHireYearCount chỉ
truyền 2 tham số mà ta truyền 3
EXECUTE sp_EmployeesHireYearCount @HireYear, @Number OUTPUT, 
'123'
PRINT N'Số nhân viên vào làm năm là: ' + 
CONVERT(varchar(3),@Number)
END TRY
BEGIN CATCH
PRINT N'Có lỗi xảy ra trong khi thực hiện thủ tục lưu trữ'
END CATCH
PRINT N'Kết thúc thủ tục lưu trữ'
END
GO

EXEC sp_EmployeeHire 1999
--Xem thông báo lỗi bên Messages không phải bên Result
GO
--Thay đổi thủ tục lưu trữ sp_EmployeeHire sử dụng hàm @@ERROR
ALTER PROCEDURE sp_EmployeeHire
@HireYear int
AS
BEGIN
EXECUTE sp_DisplayEmployeesHireYear @HireYear
DECLARE @Number int
EXECUTE sp_EmployeesHireYearCount @HireYear, @Number OUTPUT, 
'123'
IF @@ERROR <> 0
PRINT N'Có lỗi xảy ra trong khi thực hiện thủ tục lưu trữ'
PRINT N'Số nhân viên vào làm năm là: ' + 
CONVERT(varchar(3),@Number)
END
GO
--Chạy thủ tục sp_EmployeeHire
EXEC sp_EmployeeHire 1999


