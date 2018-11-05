# Q2 Write a query that shows the student's name, the courses the student is taking and the professors that teach that course. 

SELECT student_name, se.course_no,last_name AS Prof_last_name
FROM students st, student_enrollment se, teach th
WHERE st.student_no = se.student_no
AND se.course_no = th.course_no
ORDER BY student_name, se.course_no, last_name

# OR:

SELECT student_name, se.course_no, p.last_name 
FROM students s 
INNER JOIN student_enrollment se 
    ON s.student_no = se.student_no
INNER JOIN teach t 
    ON se.course_no = t.course_no
INNER JOIN professors p 
    ON t.last_name = p.last_name
ORDER BY student_name;

# Q4 In question 3 you discovered why there is repeating data. How can we eliminate this redundancy? Let's say we only care to see a single professor teaching a course and we don't care for all the other professors that teach the particular course. Write a query that will accomplish this so that every record is distinct.
* HINT: Using the DISTINCT keyword will not help. :-)
  
* Note: Added student_no to show diff between Michael no. 1 and Michael no. 7
SELECT student_name, te.course_no, pr.last_name  AS professor
FROM students s LEFT JOIN  student_enrollment se
ON s.student_no = se.student_no
JOIN courses co ON se.course_no = co.course_no
JOIN teach te ON co.course_no = te.course_no
JOIN professors pr ON te.last_name = pr.last_name
WHERE pr.last_name IN (SELECT last_name FROM teach t1 WHERE t1.course_no = te.course_no LIMIT 1 )
ORDER BY student_name, se.course_no

# Q6 In the video lectures, we've been discussing the employees table and the departments table. Considering those tables, write a query that returns employees whose salary is above average for their given department.

SELECT * FROM employees e
WHERE salary > (SELECT AVG(salary) FROM employees e1 
WHERE e1.department = e.department )

# Q7 Write a query that returns ALL of the students as well as any courses they may or may not be taking. 

SELECT s.student_no, student_name, course_no
FROM students s LEFT JOIN student_enrollment se
    ON s.student_no = se.student_no