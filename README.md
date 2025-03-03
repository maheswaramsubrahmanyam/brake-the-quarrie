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
1. **Find students who have enrolled in all available courses.**
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

## Incorrect Queries with Mistakes to Fix
1. **(Mistake: Wrong use of alias and missing GROUP BY)**
```sql
SELECT Name, AVG(Grade) AS GPA  
FROM Students s
JOIN Enrollments e ON s.StudentID = e.StudentID
HAVING GPA > 3.0;
```

2. **(Mistake: Missing JOIN condition)**
```sql
SELECT s.Name, c.CourseName
FROM Students s
JOIN Enrollments e  
JOIN Courses c ON e.CourseID = c.CourseID;
```

3. **(Mistake: NULL comparison using `=` instead of `IS NULL`)**
```sql
SELECT Name FROM Students
WHERE Email = NULL;
```

4. **(Mistake: Using aggregate function in WHERE clause instead of HAVING)**
```sql
SELECT CourseID, COUNT(StudentID) AS EnrolledStudents
FROM Enrollments
WHERE COUNT(StudentID) > 10  
GROUP BY CourseID;
```

5. **(Mistake: Using DISTINCT incorrectly inside COUNT in HAVING clause)**
```sql
SELECT c.CourseName, COUNT(DISTINCT s.StudentID) AS UniqueStudents
FROM Students s
JOIN Enrollments e ON s.StudentID = e.StudentID
JOIN Courses c ON e.CourseID = c.CourseID
GROUP BY c.CourseID
HAVING UniqueStudents > 5;
```

---

**📌 Challenge:**  
- Solve the **correct queries** and verify the outputs.  
- **Debug** the incorrect queries to get the expected results.  
