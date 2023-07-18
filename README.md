


# Reqs


### Db Architecture:-

Sure, let's create a basic database architecture for the given product with the specified features. Please note that this is a simplified example, and in a real-world scenario, you may need to consider additional factors like data volumes, performance requirements, security, and high availability. The following is a conceptual representation of the database architecture:

#### Users Table:

Store information about users, including students and teachers.
Columns: user_id (Primary Key), username, email, password, role (student/teacher), phone_number, etc.


#### Classes Table:

Store details about each class, including live classes and recorded lectures.
Columns: class_id (Primary Key), class_name, class_date, class_time, teacher_id (Foreign Key), etc.


#### Whiteboard Slides Table:

Store whiteboard slides information for each class, including PDFs.
Columns: slide_id (Primary Key), class_id (Foreign Key), slide_title, slide_url, etc.


#### Class Recordings Table:

Store details of class recordings.
Columns: recording_id (Primary Key), class_id (Foreign Key), recording_title, recording_date, recording_url, etc.


#### Notifications Table:

Store email and SMS notifications for class reminders.
Columns: notification_id (Primary Key), user_id (Foreign Key), notification_type (email/SMS), notification_content, notification_time, etc.


#### Polls Table:

Store information about polls conducted during live classes.
Columns: poll_id (Primary Key), class_id (Foreign Key), poll_question, poll_options, poll_responses, etc.
#### Chat Messages Table:

Store messages sent during live classes.
Columns: message_id (Primary Key), class_id (Foreign Key), user_id (Foreign Key), message_content, message_time, etc.


#### Uploaded Media Table:

Store media uploaded during live classes.
Columns: media_id (Primary Key), class_id (Foreign Key), user_id (Foreign Key), media_type (pdf/image/other), media_url, etc.


#### Scheduling Calendar Table:

Store details of scheduled live classes.
Columns: schedule_id (Primary Key), class_id (Foreign Key), schedule_date, schedule_time, teacher_id (Foreign Key), etc.


#### Feedback/Rating Table:

Store feedback and ratings for videos and live classes.
Columns: feedback_id (Primary Key), class_id (Foreign Key), user_id (Foreign Key), feedback_text, rating, etc.
#### Blocked Users Table:

Store information about users who are banned or blocked.
Columns: blocked_user_id (Primary Key), user_id (Foreign Key), blocked_reason, etc.


#### Single Sign-On (SSO) Table:

Store authentication tokens for single sign-on between the video portal and insp website.
Columns: sso_id (Primary Key), user_id (Foreign Key), auth_token, expiry_time, etc.
This is just a basic outline of the database architecture. Depending on your specific requirements and use cases, you may need to add more tables or modify the structure accordingly. Additionally, consider database indexing, foreign key constraints, and proper data normalization to ensure data integrity and optimal performance.
































