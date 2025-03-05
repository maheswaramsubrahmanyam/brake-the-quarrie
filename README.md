# Break-The-Query

# SQLite Query Challenges

## Database Schema
```sql
CREATE TABLE Students (
    StudentID INTEGER PRIMARY KEY,
    Name TEXT NOT NULL,
    Age INTEGER CHECK(Age > 15),
    Gender TEXT CHECK(Gender IN ('Male', 'Female', 'Other')),
    Email TEXT UNIQUE NOT NULL,
    EnrollmentDate DATE NOT NULL
);

CREATE TABLE Courses (
    CourseID INTEGER PRIMARY KEY,
    CourseName TEXT NOT NULL,
    Credits INTEGER CHECK(Credits BETWEEN 1 AND 6)
);

CREATE TABLE Enrollments (
    EnrollmentID INTEGER PRIMARY KEY,
    StudentID INTEGER,
    CourseID INTEGER,
    Semester TEXT NOT NULL,
    Grade TEXT CHECK(Grade IN ('A', 'B', 'C', 'D', 'F', 'Incomplete')),
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);

CREATE TABLE Professors (
    ProfessorID INTEGER PRIMARY KEY,
    Name TEXT NOT NULL,
    Department TEXT NOT NULL,
    Email TEXT UNIQUE NOT NULL
);

CREATE TABLE Assignments (
    AssignmentID INTEGER PRIMARY KEY,
    CourseID INTEGER,
    AssignmentName TEXT NOT NULL,
    DueDate DATE,
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);

CREATE TABLE Submissions (
    SubmissionID INTEGER PRIMARY KEY,
    AssignmentID INTEGER,
    StudentID INTEGER,
    SubmissionDate DATE,
    Score INTEGER CHECK(Score BETWEEN 0 AND 100),
    FOREIGN KEY (AssignmentID) REFERENCES Assignments(AssignmentID),
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID)
);


-- Insert data into Students
INSERT INTO Students (StudentID, Name, Age, Gender, Email, EnrollmentDate) VALUES
(1, 'John Doe', 18, 'Male', 'john.doe@example.com', '2023-08-01'),
(2, 'Alice Smith', 20, 'Female', 'alice.smith@example.com', '2022-07-15'),
(3, 'Bob Johnson', 19, 'Male', 'bob.johnson@example.com', '2023-01-10'),
(4, 'Emma Brown', 21, 'Female', 'emma.brown@example.com', '2021-09-05'),
(5, 'Michael Davis', 22, 'Male', 'michael.davis@example.com', '2020-06-20'),
(6, 'Sophia Wilson', 18, 'Female', 'sophia.wilson@example.com', '2023-09-12'),
(7, 'James Lee', 19, 'Male', 'james.lee@example.com', '2022-05-30'),
(8, 'Olivia Martinez', 20, 'Female', 'olivia.martinez@example.com', '2021-11-18'),
(9, 'William Taylor', 21, 'Male', 'william.taylor@example.com', '2020-04-22'),
(10, 'Emily White', 19, 'Female', 'emily.white@example.com', '2023-07-09');

-- Insert data into Courses
INSERT INTO Courses (CourseID, CourseName, Credits) VALUES
(101, 'Mathematics', 4),
(102, 'Physics', 3),
(103, 'Computer Science', 5),
(104, 'History', 3),
(105, 'English Literature', 4),
(106, 'Data Science', 6),
(107, 'Software Engineering', 5),
(108, 'Artificial Intelligence', 6),
(109, 'Cyber Security', 5),
(110, 'Web Development', 4);

-- Insert data into Enrollments
INSERT INTO Enrollments (EnrollmentID, StudentID, CourseID, Semester, Grade) VALUES
(1, 1, 101, 'Fall 2023', 'A'),
(2, 2, 102, 'Spring 2023', 'B'),
(3, 3, 103, 'Fall 2023', 'C'),
(4, 4, 104, 'Spring 2023', 'A'),
(5, 5, 105, 'Fall 2023', 'B'),
(6, 6, 106, 'Spring 2023', 'A'),
(7, 7, 107, 'Fall 2023', 'C'),
(8, 8, 108, 'Spring 2023', 'D'),
(9, 9, 109, 'Fall 2023', 'F'),
(10, 10, 110, 'Spring 2023', 'A'),
(11, 1, 103, 'Fall 2023', 'B'),
(12, 2, 106, 'Spring 2023', 'A'),
(13, 3, 107, 'Fall 2023', 'B'),
(14, 4, 109, 'Spring 2023', 'C'),
(15, 5, 110, 'Fall 2023', 'A');

-- Insert data into Professors
INSERT INTO Professors (ProfessorID, Name, Department, Email) VALUES
(1, 'Dr. Smith', 'Mathematics', 'smith@example.com'),
(2, 'Dr. Johnson', 'Physics', 'johnson@example.com'),
(3, 'Dr. Williams', 'Computer Science', 'williams@example.com'),
(4, 'Dr. Brown', 'History', 'brown@example.com'),
(5, 'Dr. Davis', 'English', 'davis@example.com'),
(6, 'Dr. Wilson', 'Data Science', 'wilson@example.com'),
(7, 'Dr. Lee', 'Software Engineering', 'lee@example.com'),
(8, 'Dr. Martinez', 'AI', 'martinez@example.com'),
(9, 'Dr. Taylor', 'Cyber Security', 'taylor@example.com'),
(10, 'Dr. White', 'Web Development', 'white@example.com');

-- Insert data into Assignments
INSERT INTO Assignments (AssignmentID, CourseID, AssignmentName, DueDate) VALUES
(1, 101, 'Algebra Homework', '2023-09-10'),
(2, 102, 'Physics Lab Report', '2023-09-15'),
(3, 103, 'Programming Assignment 1', '2023-09-20'),
(4, 104, 'History Essay', '2023-09-25'),
(5, 105, 'Literature Analysis', '2023-09-30'),
(6, 106, 'Data Science Project', '2023-10-05'),
(7, 107, 'Software Design Document', '2023-10-10'),
(8, 108, 'AI Model Implementation', '2023-10-15'),
(9, 109, 'Cyber Security Report', '2023-10-20'),
(10, 110, 'Web Development Project', '2023-10-25'),
(11, 103, 'Database Assignment', '2023-09-22'),
(12, 106, 'ML Model Evaluation', '2023-10-07'),
(13, 107, 'API Development', '2023-10-12'),
(14, 109, 'Ethical Hacking Report', '2023-10-23'),
(15, 110, 'React Project', '2023-10-28');

-- Insert data into Submissions
INSERT INTO Submissions (SubmissionID, AssignmentID, StudentID, SubmissionDate, Score) VALUES
(1, 1, 1, '2023-09-09', 85),
(2, 2, 2, '2023-09-14', 78),
(3, 3, 3, '2023-09-21', 92),
(4, 4, 4, '2023-09-24', 88),
(5, 5, 5, '2023-09-29', 75),
(6, 6, 6, '2023-10-04', 90),
(7, 7, 7, '2023-10-09', 82),
(8, 8, 8, '2023-10-14', 79),
(9, 9, 9, '2023-10-19', 60),
(10, 10, 10, '2023-10-24', 95),
(11, 3, 1, '2023-09-21', 80),
(12, 6, 2, '2023-10-06', 85),
(13, 7, 3, '2023-10-11', 77),
(14, 9, 4, '2023-10-22', 83),
(15, 10, 5, '2023-10-27', 90);

```

---

## Correct Queries to Solve
1. **Get students who have never submitted an assignment but are enrolled in a course.**
```sql
-- Query 1
```

2. **Retrieve the average grade per semester for each student.**
```sql
-- Query 2
```

3. **Retrieve the names of students who have enrolled in the "Computer Science" course and have scored more than 80 in any of their submissions.**
**🔹 Hint: You need to join the Students, Enrollments, Courses, and Submissions tables.**
```sql
-- Query 3
```

4. **Find the professor names who teach courses in which at least one student has received a grade of 'A' in any semester.**
**🔹 Hint: You need to join the Professors, Courses, and Enrollments tables.**
```sql
-- Query 4
```

5. **Get the number of students who have failed at least one course.**
```sql
-- Query 5
```

---

# 🛠 SQL Debugging Challenges  

This document contains **incorrect SQL queries** along with their **mistakes**. Your task is to analyze, debug, and fix them. 🚀  

---

## ❌ Incorrect Queries with Mistakes to Fix  

6. **1️⃣ Find students who have never submitted an assignment but are enrolled in a course.**  
```sql
SELECT s.StudentID, s.Name
FROM Students s
JOIN Enrollments e ON s.StudentID = e.StudentID
WHERE s.StudentID NOT IN (
    SELECT StudentID FROM Submissions
);
```

7. **4️⃣ Find students who submitted assignments after the due date more than twice.**  
```sql
SELECT s.Name, COUNT(sub.SubmissionID) AS LateSubmissions
FROM Students s
JOIN Submissions sub ON s.StudentID = sub.StudentID
JOIN Assignments a ON sub.AssignmentID = a.AssignmentID
WHERE sub.SubmissionDate > a.DueDate
HAVING LateSubmissions > 2;
```

8. **6️⃣ Retrieve the GPA of each student who has an average grade greater than 3.0.**  
```sql
SELECT Name, AVG(Grade) AS GPA  
FROM Students s
JOIN Enrollments e ON s.StudentID = e.StudentID
HAVING GPA > 3.0;
```
# 📌 Industry-Level Complex SQL Question

## 🚀 Challenge: Retrieve Student, Course, Professor, and Submission Data

**Problem Statement:**  
Retrieve a list of all students, along with the courses they are enrolled in, their professors, and their latest assignment submission details. If a student has never submitted an assignment, display `"No Submission"` for the submission date and score. Ensure that students who are not enrolled in any course are also included in the output.

### **Requirements:**
- ✅ Use **LEFT JOIN** to include students who are not enrolled in any course.
- ✅ Use **RIGHT JOIN** (or equivalent) to ensure professors teaching courses are included, even if no students are enrolled.
- ✅ Fetch only the **latest assignment submission** per student per course (if available).
- ✅ If a student has **never submitted an assignment**, display `"No Submission"` in place of `SubmissionDate` and `Score`.

---

## ❌ SQL Query (With Mistakes to Debug)
```sql
SELECT 
    s.StudentID, 
    s.Name AS StudentName, 
    c.CourseName, 
    p.Name AS ProfessorName, 
    COALESCE(sub.SubmissionDate, 'No Submission') AS LatestSubmissionDate, 
    COALESCE(sub.Score, 'No Submission') AS LatestScore
FROM Students s
LEFT JOIN Enrollments e ON s.StudentID = e.StudentID
RIGHT JOIN Courses c ON e.CourseID = c.CourseID
LEFT JOIN Professors p ON c.CourseID = p.ProfessorID
LEFT JOIN (
    SELECT AssignmentID, StudentID, SubmissionDate, Score
    FROM Submissions
    WHERE SubmissionDate = (
        SELECT MAX(SubmissionDate) FROM Submissions sub2 
        WHERE sub2.StudentID = Submissions.StudentID
    )
) sub ON s.StudentID = sub.StudentID;
```

---

## 🛠 **Debugging Hints:**
1️⃣ **Issue 1:** Professors are linked incorrectly using `CourseID = ProfessorID`.  
2️⃣ **Issue 2:** `RIGHT JOIN` on `Courses` might not work well with `LEFT JOIN` on `Enrollments`.  
3️⃣ **Issue 3:** `MAX(SubmissionDate)` in the subquery should be used with `JOIN` instead of `WHERE`.  
4️⃣ **Issue 4:** `COALESCE` works, but `Score` is an integer; `"No Submission"` might cause data type issues.  

---

## 🎯 **Expected Output:**  
| StudentID | StudentName  | CourseName   | ProfessorName | LatestSubmissionDate | LatestScore  |
|-----------|-------------|-------------|---------------|----------------------|--------------|
| 101       | John Doe    | Data Science | Dr. Smith     | 2024-02-15          | 95           |
| 102       | Alice Brown | Web Dev      | Dr. Johnson   | No Submission       | No Submission |
| 103       | Bob White   | NULL         | NULL          | NULL                | NULL         |

---


10. **🔟 List courses that have more than 5 unique students enrolled.**  
```sql
SELECT c.CourseName, COUNT(DISTINCT s.StudentID) AS UniqueStudents
FROM Students s
JOIN Enrollments e ON s.StudentID = e.StudentID
JOIN Courses c ON e.CourseID = c.CourseID
GROUP BY c.CourseID
HAVING UniqueStudents > 5;
```

---

## 📢 **Debugging Challenge**  
- 🛠 **Fix the incorrect queries** and verify the results.  
- 🔎 **Identify missing conditions, wrong joins, and incorrect usage of aggregate functions.**  

**📌 Let's debug and improve SQL skills! 🚀**

