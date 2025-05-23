```sql
create database QuanLyBanHang;
use QuanLyBanHang;

create table Customer (
  cID varchar(20) primary key,
  cName varchar(50) not null,
  cAge int check (cAge >= 1)
);

create table Orders (
  oID varchar(20) primary key,
  cID varchar(20) not null,
  oDate datetime,
  oTotalPrice float not null,
  foreign key (cID) references Customer(cID) on delete cascade
);

create table Product (
  pID varchar(20) primary key,
  pName varchar(50) not null,
  pPrice float not null
);

create table OrderDetail (
  oID varchar(20) not null,
  pID varchar(20) not null,
  odQTY int not null,
  primary key (oID, pID),
  foreign key (oID) references Orders(oID) on delete cascade,
  foreign key (pID) references Product(pID) on delete cascade
);
INSERT INTO Customer (cID, cName, cAge) VALUES
('1', 'Minh Quan', 10),
('2', 'Ngoc Oanh', 20),
('3', 'Hong Ha', 50);
INSERT INTO Product (pID, pName, pPrice) VALUES
('1', 'May Giat', 3.0),
('2', 'Tu Lanh', 5.0),
('3', 'Dieu Hoa', 7.0),
('4', 'Quat', 1.0),
('5', 'Bep Dien', 2.0);

INSERT INTO Orders (oID, cID, oDate, oTotalPrice) VALUES
('1', '1', '2006-03-21 00:00:00', 0.0),
('2', '2', '2006-03-23 00:00:00', 0.0),
('3', '1', '2006-03-16 00:00:00', 0.0);

INSERT INTO OrderDetail (oID, pID, odQTY) VALUES
('1', '1', 3),
('1', '3', 7),
('1', '4', 2),
('2', '1', 1),
('3', '1', 8),
('2', '5', 4),
('2', '3', 3);
SELECT * FROM Customer;
SELECT * FROM Product;
SELECT * FROM Orders;
SELECT * FROM OrderDetail;
* 1 Hiển thị các thông tin gồm oID, oDate, oTotalPrice của tất cả các hóa đơn trong bảng Orders.
SELECT
    o.oID,
    o.oDate,
    o.oTotalPrice
FROM
    Orders o;
* a1 Danh sách các khách hàng đã mua hàng:
   
    SELECT DISTINCT
    c.cID,
    c.cName
FROM
    Customer c
JOIN
    Orders o ON c.cID = o.cID;
    
* a2 Danh sách sản phẩm được mua bởi các khách   
SELECT
    c.cName AS CustomerName,
    p.pName AS ProductName,
    od.odQTY AS QuantityBought
FROM
    Customer c
JOIN
    Orders o ON c.cID = o.cID  
JOIN
    OrderDetail od ON o.oID = od.oID
JOIN
    Product p ON od.pID = p.pID
ORDER BY
    c.cName, p.pName;
    
 * Yêu cầu 3: Hiển thị tên những khách hàng không mua bất kỳ một sản phẩm nào.
  SELECT
    c.cName
FROM
    Customer c
LEFT JOIN
    Orders o ON c.cID = o.cID
WHERE
    o.oID IS NULL;  
    
 * 4:Hiển thị mã hóa đơn, ngày bán và giá tiền của từng hóa đơn
 SELECT
    o.oID AS MaHoaDon,
    o.oDate AS NgayBan,
    SUM(od.odQTY * p.pPrice) AS GiaTienHoaDon
FROM
    Orders o
JOIN
    OrderDetail od ON o.oID = od.oID
JOIN
    Product p ON od.pID = p.pID
GROUP BY
    o.oID, o.oDate
ORDER BY
    o.oID;
