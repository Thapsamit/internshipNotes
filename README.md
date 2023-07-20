


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



### Real time communication 



Using Socket.io is not a requirement for setting up a basic WebRTC-based live streaming platform. WebRTC itself provides the necessary real-time communication capabilities between the broadcaster (you) and the viewers. However, Socket.io can be beneficial in certain scenarios, depending on your specific requirements and use cases.

Socket.io is a library that enables real-time, bidirectional communication between web clients and servers. It uses WebSocket as the primary transport protocol but also provides fallback options like long polling for browsers that do not support WebSocket.

Here are some cases where Socket.io might be useful in your live streaming platform:

Signaling Server: WebRTC requires a signaling server to exchange session information between participants before establishing a direct peer-to-peer connection. Socket.io can serve as the signaling server to facilitate this process.

Chat and Interaction: If you want to implement real-time chat or interactive features between you and your viewers (e.g., questions, polls, etc.), Socket.io can be a convenient choice.

Broadcast Status Updates: Socket.io can be used to notify viewers of changes in the live stream status, such as when the stream starts, stops, or experiences technical difficulties.

Real-Time Analytics: If you need to gather real-time analytics or metrics about your live stream viewers, Socket.io can help you send this information back to the server in real-time.

Cross-Browser Support: WebSocket is supported by most modern browsers, but some older browsers may not fully support it. Socket.io provides a fallback mechanism, making it easier to achieve cross-browser compatibility.

Keep in mind that integrating Socket.io can add some complexity to your project, and it might not be necessary for a simple live streaming setup. If you're looking for basic real-time video streaming without interactive features, WebRTC alone should suffice.

However, if you have specific use cases that require real-time interactions and server-client communication beyond what WebRTC offers, Socket.io could be a valuable addition to your tech stack. Ultimately, the decision to use Socket.io depends on your project's requirements and your team's familiarity with the technology.






#### Strategy to allow 300 plus participants in the call?

For a large-scale application with around 300+ participants, using a mesh architecture in WebRTC would not be the most efficient approach due to the limitations mentioned earlier (high bandwidth consumption, CPU usage, and scalability issues). Instead, you should consider using one of the following more scalable topologies:

Selective Forwarding Unit (SFU):
In an SFU-based architecture, each participant sends their audio, video, and data streams to a centralized media server known as the SFU. The SFU then forwards the streams to all other participants. This approach significantly reduces the number of connections each participant needs to maintain and offloads much of the media handling to the server.

Advantages:

Lower bandwidth consumption: Each participant only sends one stream to the SFU.
Reduced CPU usage: Participants only need to handle one outbound stream.
Better scalability: SFUs are designed to handle a large number of participants efficiently.
Multipoint Control Unit (MCU):
An MCU is another type of centralized media server that mixes and forwards audio, video, and data streams. Unlike the SFU, the MCU actively processes and mixes the streams before sending them to the participants. This architecture works well for applications that require advanced media processing, such as video conferencing with dynamic layout changes.

Advantages:

Advanced media processing: The MCU can provide features like layout changes, screen sharing, and more.
Centralized control: The MCU has full control over the media streams, enabling more fine-tuned manipulation.
Peer-to-Peer Data Channels with SFU/ MCU for Media Streams:
In this hybrid approach, you can use a mesh-like topology for the data channels (for non-media communication) between participants, while using an SFU or MCU for handling the media streams. This approach combines the benefits of both approaches and is commonly used in large-scale WebRTC applications.



#### Approach to use SFU - Selective forwarding Unit

To implement an SFU, you would typically use media server libraries or platforms that provide SFU capabilities, such as:

mediasoup: A popular media server library designed for WebRTC applications, including SFU functionality.

GitHub: https://github.com/versatica/mediasoup
Official Website: https://mediasoup.org/
Janus Gateway: An open-source general-purpose WebRTC server with support for SFU, conferencing, and other real-time communication use cases.

GitHub: https://github.com/meetecho/janus-gateway
Kurento: An open-source WebRTC media server that provides SFU capabilities and additional media processing features.

GitHub: https://github.com/Kurento/kurento-media-server

#### Refer below for SFU

- https://bloggeek.me/webrtcglossary/sfu/

### Implement SFU 

Using a Selective Forwarding Unit (SFU) involves setting up a media server that receives media streams from participants in a communication session and selectively forwards those streams to other participants. SFUs are commonly used in large-scale WebRTC applications to reduce bandwidth consumption and CPU load on clients.

To use an SFU, you'll need to follow these general steps:

Choose an SFU Library or Service:
There are several SFU libraries and services available that provide SFU capabilities for WebRTC. Some popular options include mediasoup, janus-gateway, Kurento, and more. Choose one that best fits your requirements and tech stack.

Set up the SFU Backend:
Depending on the SFU library you choose, you'll need to set up the backend (media server) on your server infrastructure. This involves installing the necessary dependencies and configuring the media server.

Implement Signaling:
As with any WebRTC-based application, you'll need to implement signaling to exchange session metadata and negotiation messages between participants and the SFU. This could involve using WebSocket or Socket.io to facilitate communication between clients and the SFU backend.

Handle Media Stream Negotiation:
When participants join a session, you'll need to handle media stream negotiation between each participant and the SFU. This involves setting up RTCPeerConnection for each participant and exchanging SDPs and ICE candidates through the signaling channel.

Send Media Streams to the SFU:
Once participants establish WebRTC connections with the SFU, they will send their media streams (audio, video) to the SFU. The SFU will handle these media streams and manage their forwarding to other participants in the session.

Receive Media Streams from the SFU:
Similarly, participants will also receive media streams from the SFU, allowing them to view and hear the media streams of other participants in the session.

Handle Data Channels (Optional):
If you need to exchange additional data between participants, you can create WebRTC data channels on the connections between the clients and the SFU.

Implement Error Handling and Clean-up:
Properly handle errors and clean up resources, such as closing connections, when participants leave the session.




#### Chose from websocket or socket.io

When choosing between WebSocket and Socket.io as a signaling server for your WebRTC application, there are several factors to consider based on your specific requirements and use case. Let's compare both options to help you make an informed decision:

WebSocket:

Real WebSocket Protocol:
WebSocket is a standard protocol defined by the IETF (Internet Engineering Task Force). It provides a full-duplex communication channel over a single TCP connection, allowing low-latency, bidirectional communication between the client and server.

Lightweight and Low Overhead:
Since WebSocket is a low-level protocol, it has minimal overhead compared to higher-level libraries like Socket.io. This makes it suitable for scenarios where performance is critical.

Simple and Direct Integration:
Using WebSocket directly allows you to have more control over the signaling process. It's a great choice if you want a lightweight, custom solution tailored to your specific needs.

Socket.io:

WebSocket-Based but with Enhancements:
Socket.io is built on top of WebSocket and provides additional features like fallbacks (e.g., long polling) for older browsers that don't support WebSocket, making it more resilient in environments with varying browser support.

Real-Time Functionality:
Socket.io simplifies real-time communication between clients and servers by handling the management of connections, rooms, broadcasting messages, and other real-time functionalities.

Broad Browser Support:
Socket.io works with a wide range of browsers, including older ones that may not have native WebSocket support.

Choosing the Right Option:

Performance vs. Ease of Use:
If performance and low overhead are critical factors for your application, using WebSocket directly might be the better choice. If you prioritize ease of development and need real-time features like broadcasting, room management, and fallbacks, Socket.io provides a more convenient option.

Browser Support:
Consider the browsers you need to support. WebSocket has good native support in modern browsers, but if you need to cater to older browsers, Socket.io can handle fallbacks for non-WebSocket-capable browsers.

Customization:
If you need a highly customized signaling process or if you're building a custom signaling server, using WebSocket directly allows you to have more control over the implementation.

Third-Party Services:





### Why to use MediaSoup 


Mediasoup is an open-source WebRTC media server that provides real-time communication capabilities for audio, video, and data streaming. It is designed to be highly flexible, scalable, and efficient, making it an excellent choice for building large-scale video conferencing, audio conferencing, or any real-time communication applications.

Here are some key features and reasons to use Mediasoup:

Scalability: Mediasoup is built on top of Node.js and uses modern WebRTC technologies. It employs a Selective Forwarding Unit (SFU) architecture, allowing it to scale efficiently to handle a large number of participants in a conference. It offloads the media processing tasks to the clients (participants), reducing the server's processing burden.

Customizable and Lightweight: Mediasoup is designed to be modular and allows developers to customize its behavior to suit their specific use cases. You can choose to implement specific media processing logic or codecs based on your requirements, making it a lightweight and efficient solution.

Support for Simulcast: Simulcast is a technique that allows sending multiple versions of the same video stream at different resolutions and bitrates. It enables Mediasoup to adapt to varying network conditions and device capabilities, providing a better video streaming experience.

WebRTC Compliance: Mediasoup follows WebRTC specifications, ensuring compatibility with WebRTC-enabled browsers and clients. This makes it easy to integrate with various platforms and frameworks.

Secure: Security is crucial for any real-time communication platform. Mediasoup has built-in mechanisms to handle secure communications and can be integrated with your existing authentication and authorization systems.

Active Community: Mediasoup has an active and growing community of developers who contribute to its development and provide support through forums and other channels.

Media Device Management: Mediasoup provides APIs for managing media devices (cameras, microphones, speakers) to give users control over their media input and output during a conference.

Continuous Updates: The development of Mediasoup is ongoing, and updates are regularly released to improve performance, stability, and security.

Keep in mind that while Mediasoup is a powerful tool for building real-time communication applications, it does require a good understanding of WebRTC and media server concepts. It may have a steeper learning curve compared to some other out-of-the-box solutions, but it offers greater flexibility and customization options.

Before choosing Mediasoup or any other media server solution, carefully consider your project's requirements, the complexity of the application, and the expertise of your development team. It's essential to evaluate different media server options to determine which one best fits your specific use case and technical needs.











If you prefer a hosted solution for signaling, there are third-party WebRTC signaling services available that use WebSocket or Socket.io. Evaluate their features, scalability, and pricing to see if they meet your requirements.





### How to use mediaSoup with nodejs 
```js
// server.js

const express = require('express');
const app = express();
const http = require('http').createServer(app);
const io = require('socket.io')(http);
const mediasoup = require('mediasoup');

const PORT = 3001;

let worker; // Mediasoup worker
let router; // Mediasoup router
const producers = new Map(); // Store active producers

(async () => {
  worker = await mediasoup.createWorker();
  const mediaCodecs = mediasoup.defaultRouterOptions.mediaCodecs;
  router = await worker.createRouter({ mediaCodecs });

  http.listen(PORT, () => {
    console.log(`Server listening on port ${PORT}`);
  });
})();

// Set up a WebSocket server using Socket.io
io.on('connection', (socket) => {
  socket.on('join', async (roomId, userId) => {
    socket.join(roomId);

    const transport = await router.createWebRtcTransport({
      listenIps: [{ ip: '127.0.0.1', announcedIp: 'your-public-ip' }], // Replace 'your-public-ip' with your server's public IP
      enableUdp: true,
      enableTcp: true,
    });

    // Send the transport information back to the client
    socket.emit('transportInfo', {
      transportOptions: {
        id: transport.id,
        iceParameters: transport.iceParameters,
        iceCandidates: transport.iceCandidates,
        dtlsParameters: transport.dtlsParameters,
      },
    });

    // Handle client disconnection
    socket.on('disconnect', () => {
      if (producers.has(userId)) {
        const producer = producers.get(userId);
        producer.close();
        producers.delete(userId);

        io.to(roomId).emit('userLeft', userId);
      }
    });

    // Handle incoming media stream from the client
    socket.on('produce', async (data) => {
      const producer = await transport.produce({
        kind: data.kind,
        rtpParameters: data.rtpParameters,
      });

      producers.set(userId, producer);

      // Send the producer ID back to the client
      socket.emit('producerId', producer.id);

      // Inform all other participants about the new producer
      socket.to(roomId).emit('newRemoteProducer', producer.id);
    });
  });
});
```



