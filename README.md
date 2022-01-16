# CMS
Backend routes and database for an abstract theoretical app for organizing and providing course information that relates courses, users, and assignments. The primary purpose of this app is to experiment with many-to-many relationships.

app.py contains all routes. db.py contains SQLite database, implemented with SQLAlchemy, with classes "Course", "User", and "Assignment".

The database contains five tables: "course", "user", "assignment", "tstudents", and "tinstructors". The last two tables are both join tables between "course" and "user". For each course, the "course" table stores an id, code, name, list of assignments, list of instructors, and list of students. For each user, the "user" table stores an id, name, netid, instructor courses, and student courses. Columns for both instructor courses and student courses are necessary as a user could be both an instructor and student. For each assignment, the "assignment" table stores an id, title, due date, and course id. There is a one-to-many relationship between "course" and "assignment" and two many-to-many relationships between "course" and "user", the first relating courses with instructors and the second relating courses with students. instructor and student columns in "course" table are serialized users without course field and courses column in "user" table is serialized without users to avoid redundancy and recursion.

## 8 routes implemented:

GET /api/courses/
Sucess response returns a list of all courses, serialized with all columns.

POST /api/courses/
Pass in course code and name. Success response creates a new course with empty fields for assignments, instructors, and students. Adds course to database. Returns course, serialized with all columns.
Returns error if the code or name fields are invalid.

GET /api/courses/<int:course_id>/
Success response returns course specified by id, serialized with all columns.
Returns error if id does not exist.

DELETE /api/courses/<int:course_id>/
Success response removes course from database and returns course, serialized with all columns.
Returns error if id does not exist.

POST /api/users/
Pass in name and netid. Success response creates a new user with empty fields for instructor and student courses and adds user to database. Returns user, serialized with all columns.

GET /api/users/<int:user_id>/
Success response returns user specified by id, serialized with all columns.
Returns error if id does not exist.

POST /api/courses/<int:course_id>/add/
Pass in user id and type, where type is either "instructor" or "student". Sucess response updates the specified course by adding the specified user to either its "instructors" or "students" column. Updates specified user by adding specified course to either its "instuctor_courses" or "student_courses" columns. Returns the specified course. 
Returns error if course id does not exist, or if "user_id" or "type" fields are invalid. 

POST /api/courses/<int:course_id>/assignment/
Pass in title and due date. Success response creates a new assignment with specified course in its "course_id" column. Updates specified course by adding the new assignment to its assignments column. Returns new assignment, serialized with all columns. 
Returns error if course id does not exist, or if "title" or "due_date" fields are invalid.

