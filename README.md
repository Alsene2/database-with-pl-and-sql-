
mysql> create database pl_sql;
Query OK, 1 row affected (0.01 sec)

mysql> use pl_sql;
Database changed
mysql> CREATE TABLE Students (
    ->     student_id INT PRIMARY KEY,
    ->     first_name VARCHAR(50) NOT NULL,
    ->     last_name VARCHAR(50) NOT NULL,
    ->     email VARCHAR(100) UNIQUE
    -> );
Query OK, 0 rows affected (0.04 sec)

mysql> CREATE TABLE Courses (
    ->     course_id INT PRIMARY KEY,
    ->     course_name VARCHAR(100) NOT NULL,
    ->     credits INT NOT NULL
    -> );
Query OK, 0 rows affected (0.03 sec)

mysql>
mysql> CREATE TABLE Enrollments (
    ->     enrollment_id INT PRIMARY KEY,
    ->     student_id INT,
    ->     course_id INT,
    ->     grade CHAR(2),
    ->     FOREIGN KEY (student_id) REFERENCES Students(student_id),
    ->     FOREIGN KEY (course_id) REFERENCES Courses(course_id)
    -> );
Query OK, 0 rows affected (0.04 sec)

mysql> -- INNER JOIN
mysql> SELECT s.first_name, s.last_name, c.course_name
    -> FROM Students s
    -> INNER JOIN Enrollments e ON s.student_id = e.student_id
    -> INNER JOIN Courses c ON e.course_id = c.course_id;
Empty set (0.01 sec)

mysql> -- LEFT JOIN
mysql> SELECT s.first_name, s.last_name, c.course_name
    -> FROM Students s
    -> LEFT JOIN Enrollments e ON s.student_id = e.student_id
    -> LEFT JOIN Courses c ON e.course_id = c.course_id;
Empty set (0.00 sec)

mysql> -- RIGHT JOIN
mysql> SELECT s.first_name, s.last_name, c.course_name
    -> FROM Students s
    -> RIGHT JOIN Enrollments e ON s.student_id = e.student_id
    -> RIGHT JOIN Courses c ON e.course_id = c.course_id;
Empty set (0.00 sec)

mysql> -- FULL OUTER JOIN simulation in MySQL
mysql> SELECT s.first_name, s.last_name, c.course_name
    -> FROM Students s
    -> LEFT JOIN Enrollments e ON s.student_id = e.student_id
    -> LEFT JOIN Courses c ON e.course_id = c.course_id
    ->
    -> UNION
    ->
    -> SELECT s.first_name, s.last_name, c.course_name
    -> FROM Students s
    -> RIGHT JOIN Enrollments e ON s.student_id = e.student_id
    -> RIGHT JOIN Courses c ON e.course_id = c.course_id;
Empty set (0.01 sec)

mysql> INSERT INTO Students (student_id, first_name, last_name, email) VALUES
    -> (1, 'uwase', 'juliene', 'alice@gmail.com'),
    -> (2, 'ishimwe', 'ciella', 'ishaciel@gmail.com'),
    -> (3, 'irakoze', 'benjamin', 'charlie@gmail.com'),
    -> (4, 'manishimwe', 'arsennn', 'ishimwearss@gmail.com');
Query OK, 4 rows affected (0.02 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> INSERT INTO Courses (course_id, course_name, credits) VALUES
    -> (101, 'Database Systems', 3),
    -> (102, 'Computer Networks', 4),
    -> (103, 'Operating Systems', 3),
    -> (104, 'Information management', 4);
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ' 4)' at line 5
mysql> INSERT INTO Courses (course_id, course_name, credits) VALUES
    -> (101, 'Database Systems', 3),
    -> (102, 'Computer Networks', 4),
    -> (103, 'Operating Systems', 3),
    -> (104, 'Information Management', 4);
Query OK, 4 rows affected (0.01 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> -- INNER JOIN
mysql> SELECT s.first_name, s.last_name, c.course_name
    -> FROM Students s
    -> INNER JOIN Enrollments e ON s.student_id = e.student_id
    -> INNER JOIN Courses c ON e.course_id = c.course_id;
Empty set (0.00 sec)

mysql> -- LEFT JOIN
mysql> SELECT s.first_name, s.last_name, c.course_name
    -> FROM Students s
    -> LEFT JOIN Enrollments e ON s.student_id = e.student_id
    -> LEFT JOIN Courses c ON e.course_id = c.course_id;
+------------+-----------+-------------+
| first_name | last_name | course_name |
+------------+-----------+-------------+
| uwase      | juliene   | NULL        |
| ishimwe    | ciella    | NULL        |
| irakoze    | benjamin  | NULL        |
| manishimwe | arsennn   | NULL        |
+------------+-----------+-------------+
4 rows in set (0.00 sec)

mysql> -- RIGHT JOIN
mysql> SELECT s.first_name, s.last_name, c.course_name
    -> FROM Students s
    -> RIGHT JOIN Enrollments e ON s.student_id = e.student_id
    -> RIGHT JOIN Courses c ON e.course_id = c.
    -> ^C
mysql> -- RIGHT JOIN
mysql> SELECT s.first_name, s.last_name, c.course_name
    -> FROM Students s
    -> RIGHT JOIN Enrollments e ON s.student_id = e.student_id
    -> RIGHT JOIN Courses c ON e.course_id = c.course_id;
+------------+-----------+------------------------+
| first_name | last_name | course_name            |
+------------+-----------+------------------------+
| NULL       | NULL      | Database Systems       |
| NULL       | NULL      | Computer Networks      |
| NULL       | NULL      | Operating Systems      |
| NULL       | NULL      | Information Management |
+------------+-----------+------------------------+
4 rows in set (0.00 sec)

mysql> -- FULL OUTER JOIN simulation in MySQL
mysql> SELECT s.first_name, s.last_name, c.course_name
    -> FROM Students s
    -> LEFT JOIN Enrollments e ON s.student_id = e.student_id
    -> LEFT JOIN Courses c ON e.course_id = c.course_id
    ->
    -> UNION
    ->
    -> SELECT s.first_name, s.last_name, c.course_name
    -> FROM Students s
    -> RIGHT JOIN Enrollments e ON s.student_id = e.student_id
    -> RIGHT JOIN Courses c ON e.course_id = c.course_id;
+------------+-----------+------------------------+
| first_name | last_name | course_name            |
+------------+-----------+------------------------+
| uwase      | juliene   | NULL                   |
| ishimwe    | ciella    | NULL                   |
| irakoze    | benjamin  | NULL                   |
| manishimwe | arsennn   | NULL                   |
| NULL       | NULL      | Database Systems       |
| NULL       | NULL      | Computer Networks      |
| NULL       | NULL      | Operating Systems      |
| NULL       | NULL      | Information Management |
+------------+-----------+------------------------+
8 rows in set (0.00 sec)

mysql> CREATE INDEX idx_student_name ON Students(last_name);
Query OK, 0 rows affected (0.11 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> CREATE VIEW StudentCourseReport AS
    -> SELECT s.first_name, s.last_name, c.course_name, e.grade
    -> FROM Students s
    -> INNER JOIN Enrollments e ON s.student_id = e.student_id
    -> INNER JOIN Courses c ON e.course_id = c.course_id;
Query OK, 0 rows affected (0.01 sec)
