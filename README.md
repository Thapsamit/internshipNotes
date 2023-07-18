


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




#### Open source project 


BigBlueButton: https://github.com/bigbluebutton/bigbluebutton
Jitsi: https://github.com/jitsi/jitsi
Open edX: https://github.com/edx/edx-platform
Moodle: https://github.com/moodle/moodle
Mediasoup: https://github.com/versatica/mediasoup
You can explore these repositories to find the source code, documentation, and community discussions related to each project. Keep in mind that each project may have multiple repositories for different components and modules. Always refer to the official documentation provided in the repository for detailed instructions on installation, configuration, and usage.










### TECH STACK


Based on your requirements, here's a suggested tech stack:

Frontend:

Angular or React for building the web application user interface.
Socket.io for real-time communication features like chat and live interaction during classes.
Miro API integration for the interactive whiteboard and drawing capabilities.
Backend:

Node.js with Express.js or Django for handling the server-side logic.
MongoDB or PostgreSQL for the database to store class recordings, user data, and other relevant information.
Nginx or Apache for handling the video streaming and serving static files.
FFmpeg for video encoding and recording video streams.
Video Streaming:

WebRTC for live video streaming between the teacher and students during classes.
MediaSoup or Janus as WebRTC media servers to handle the video streams.
Notifications:

Use Twilio or Nexmo API for sending SMS notifications to students before class.
Use an email service like SendGrid or AWS SES for sending email notifications.
Authentication & Authorization:

Use JWT (JSON Web Tokens) for authentication and authorization of users.
Passport.js for handling user authentication in Node.js.
Scheduling & Calendar:

Use a scheduling library like FullCalendar for managing live class schedules.
Moment.js or Date-fns for date and time manipulation.
Feedback & Rating System:

Implement a rating system where students can provide feedback and rate the classes.
Store the ratings and feedback in the database.
Single Sign-On (SSO):

Implement SSO for seamless login between the main website and the video portal.
Cost & Scalability:

Consider using cloud-based services like AWS or Google Cloud for hosting and scaling the application as per the user load.
Note: This is just a suggested tech stack based on the requirements you provided. Depending on the specific needs of your application and your team's expertise, you may make adjustments or explore other technologies that suit your project better. Additionally, it's always good to conduct a detailed feasibility study and evaluate the pros and cons of each technology before finalizing the tech stack for your project.














