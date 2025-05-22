```sql
CREATE DATABASE QuanLySinhVien;
 
 USE QuanLySinhVien;
 
CREATE TABLE Class(
         ClassID INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
          ClassName VARCHAR(60) NOT NULL,
          StartDate DATETIME NOT NULL,
          Status BIT

);
CREATE TABLE Student(
          StudentId INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
          StudentName VARCHAR(30) NOT NULL,
          Address VARCHAR(50),
          Phone VARCHAR(20),
          Status BIT,
          ClassId INT NOT NULL,
          FOREIGN KEY (ClassId) REFERENCES Class (ClassID)

);

CREATE TABLE Subject(
         SubId INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
         SubName VARCHAR(30) NOT NULL,
         Credit TINYINT NOT NULL DEFAULT 1 CHECK ( Credit >= 1 ),
         Status BIT DEFAULT 1
);

CREATE TABLE Mark(
        MarkId INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
        SubId INT NOT NULL,
        StudentId INT NOT NULL,
        Mark FLOAT DEFAULT 0 CHECK ( Mark BETWEEN 0 AND 100),
        ExamTimes TINYINT DEFAULT 1,
        UNIQUE (SubId, StudentId),
        FOREIGN KEY (SubId) REFERENCES Subject(SubId),
        FOREIGN KEY (StudentId) REFERENCES Student(StudentId)
 );

insert into Class (ClassName, StartDate, Status)
values ('A1','2008-12-20',1),
('A2','2008-12-22',1),
('B3',current_date,0);

insert into Student (StudentName, Address, Phone, Status, ClassId)
values ('Hung','Ha Noi','0912113113',1,1),
('Hoa','Hai Phong',null,1,1),
('Manh','HCM','0123123123',0,2);

 insert into Subject
 values (1,'CF', 5, 1),
  (2, 'C', 6, 1),
 (3, 'HDJ', 5, 1),
 (4, 'RDBMS', 10, 1);

insert into Mark (SubId, StudentId, Mark, ExamTimes)
values (1, 1, 8, 1),
 (1, 2, 10, 2),
 (2, 1, 12, 1);

select * from Student;

select * from Student
where Status = true;

select * from Subject where Credit < 10;

select S.StudentID, S.StudentName, C.ClassName
from Student S join Class C on S.ClassID = C.ClassID;

select S.StudentID,S.StudentName,C.ClassName
from Student S join Class C on S.ClassID = C.ClassID
where C.ClassName = 'A1';

select S.StudentID, S.StudentName, Sub.SubName , M.Mark
from Student S Join Mark M on S.StudentID = M.StudentID join Subject Sub on M.SubID = Sub.SubID;

select S.StudentId, S.StudentName, Sub.SubName, M.Mark
from Student S join Mark M on S.StudentId = M.StudentId join Subject Sub on M.SubId = Sub.SubId
where Sub.SubName = 'CF';

select * from Student
where StudentName LIKE 'h%';

select * from Class
where month(StartDate) = 12;

select * from Subject
where Credit between 3 and 5;

SELECT *
FROM Subject
WHERE Credit >= 3 AND Credit <= 5;

select * from Student 
where StudentName = 'Hung';

UPDATE Student
SET ClassId = 2
WHERE StudentId = 1;

select 
S.StudentName, Sub.SubName, M.Mark
from Mark M
join Student S ON M.StudentId = S.StudentId
join Subject Sub ON M.SubId = Sub.SubId 
order by M.Mark desc,S.StudentName asc;