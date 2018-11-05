# Q1. Write a query that finds students who do not take CS180.

* Note: added student_no to show difference between Michael with student_no 1 and 7 where neccessary.

SELECT distinct student_name, s.student_no
FROM students s LEFT JOIN student_enrollment se
ON s.student_no = se.student_no
WHERE s.student_no NOT IN (
SELECT student_no FROM student_enrollment
WHERE course_no = 'CS180')

# 2. Write a query to find students who take CS110 or CS107 but not both.

SELECT distinct student_name, s.student_no
FROM students s JOIN student_enrollment se
ON s.student_no = se.student_no
WHERE se.student_no IN (
SELECT student_no FROM student_enrollment WHERE course_no IN ( 'CS110', 'CS107')
EXCEPT
( SELECT student_no FROM student_enrollment
  WHERE student_no IN
      (SELECT student_no FROM student_enrollment WHERE course_no = 'CS110')
       AND student_no IN
      (SELECT student_no FROM student_enrollment WHERE course_no = 'CS107') ) )
ORDER BY student_no

# 3. Write a query to find students who take CS220 and no other courses.

SELECT distinct student_name
FROM students s JOIN student_enrollment se
ON s.student_no = se.student_no
WHERE se.course_no = 'CS220'
EXCEPT
SELECT distinct student_name
FROM students s JOIN student_enrollment se
ON s.student_no = se.student_no
WHERE se.course_no != 'CS220'

# 4. Write a query that finds those students who take at most 2 courses. Your query should exclude students that don't take any courses as well as those  that take more than 2 course. 

SELECT distinct student_name, se.student_no
FROM students s JOIN student_enrollment se
ON s.student_no = se.student_no
WHERE se.student_no IN (
SELECT student_no FROM
(SELECT student_no, COUNT(*) no_of_courses FROM student_enrollment
  GROUP BY student_no) a
WHERE no_of_courses IN (1,2) )
ORDER BY student_no

# 5. Write a query to find students who are older than at most two other students.

SELECT student_name FROM (
SELECT student_name, age,
RANK() OVER (ORDER BY age)
FROM students ) a
WHERE rank <= 3
ORDER BY student_no