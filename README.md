# brake-the-quarrie

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

3. **List professors who have at least one student enrolled in a course they are teaching.**
```sql
-- Query 3
```

4. **Find students who submitted assignments after the due date more than twice.**
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

