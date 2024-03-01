


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


## Media soup architecture

![mediasoup](https://mediasoup.org/images/mediasoup-v3-architecture-01.svg)



### Media soup setup with nodejs

```js
// server.js

const express = require('express');
const app = express();
const http = require('http').createServer(app);
const io = require('socket.io')(http);
const mediasoup = require('mediasoup');

const PORT = 3001;

let worker;
let router;
let producerTransport;
const consumers = new Map();

(async () => {
  worker = await mediasoup.createWorker();
  const mediaCodecs = mediasoup.defaultRouterOptions.mediaCodecs;
  router = await worker.createRouter({ mediaCodecs });

  http.listen(PORT, () => {
    console.log(`Server listening on port ${PORT}`);
  });
})();

io.on('connection', (socket) => {
  socket.on('joinRoom', async (roomId) => {
    try {
      socket.join(roomId);

      // Create a WebRTC transport for sending media
      producerTransport = await router.createWebRtcTransport({
        listenIps: [{ ip: '0.0.0.0', announcedIp: YOUR_PUBLIC_IP }], // Replace YOUR_PUBLIC_IP with your server's public IP
        enableUdp: true,
        enableTcp: true,
      });

      // Send the transport information back to the client
      socket.emit('transportInfo', {
        transportOptions: {
          id: producerTransport.id,
          iceParameters: producerTransport.iceParameters,
          iceCandidates: producerTransport.iceCandidates,
          dtlsParameters: producerTransport.dtlsParameters,
        },
      });
    } catch (error) {
      console.error('Error joining room:', error);
    }
  });

  socket.on('produce', async (data) => {
    try {
      const { kind, rtpParameters } = data;

      const producer = await producerTransport.produce({ kind, rtpParameters });
      socket.emit('producerId', producer.id);

      // Inform all other participants about the new producer
      socket.to(socket.rooms).emit('newProducer', { producerId: producer.id, kind });
    } catch (error) {
      console.error('Error producing media:', error);
    }
  });

  socket.on('consume', async (data) => {
    try {
      const { producerId, rtpCapabilities } = data;

      if (!producerTransport) {
        return;
      }

      const consumerTransport = await router.createWebRtcTransport({
        listenIps: [{ ip: '0.0.0.0', announcedIp: YOUR_PUBLIC_IP }], // Replace YOUR_PUBLIC_IP with your server's public IP
        enableUdp: true,
        enableTcp: true,
      });

      const producer = await producerTransport.consume({
        producerId,
        rtpCapabilities,
        paused: false,
      });

      const consumer = await consumerTransport.consume({
        producerId: producer.id,
        rtpCapabilities,
        paused: false,
      });

      consumers.set(consumer.id, consumer);

      // Send the transport information back to the client
      socket.emit('consume', {
        consumerId: consumer.id,
        producerId: producer.id,
        kind: producer.kind,
        rtpParameters: consumer.rtpParameters,
      });

      consumer.on('transportclose', () => {
        consumers.delete(consumer.id);
      });
    } catch (error) {
      console.error('Error consuming media:', error);
    }
  });

  socket.on('resume', (data) => {
    const { consumerId } = data;
    const consumer = consumers.get(consumerId);
    if (consumer) {
      consumer.resume();
    }
  });

  socket.on('pause', (data) => {
    const { consumerId } = data;
    const consumer = consumers.get(consumerId);
    if (consumer) {
      consumer.pause();
    }
  });

  socket.on('disconnect', () => {
    // Handle client disconnection, cleanup, etc.
  });
});


```



### media soup basic setup node js

```js
const express = require('express');
const http = require('http');
const socketIO = require('socket.io');
const mediasoup = require('mediasoup');

const app = express();
const server = http.createServer(app);
const io = socketIO(server);

const mediaCodecs = [
  {
    kind: 'audio',
    mimeType: 'audio/opus',
    clockRate: 48000,
    channels: 2,
  },
  {
    kind: 'video',
    mimeType: 'video/VP8',
    clockRate: 90000,
    parameters: {
      'x-google-start-bitrate': 1000,
    },
  },
];

let worker;
let router;

(async () => {
  worker = await mediasoup.createWorker({
    logLevel: 'warn',
    rtcMinPort: 10000,
    rtcMaxPort: 10100,
  });

  const mediaCodecsRouter = mediaCodecs.map((codec) => ({
    ...codec,
    remoteRtpCapabilities: worker.rtpCapabilities,
  }));

  router = await mediasoup.createRouter({ mediaCodecs: mediaCodecsRouter });
})();

io.on('connection', (socket) => {
  console.log('Client connected');

  socket.on('getRouterRtpCapabilities', (data, callback) => {
    callback(router.rtpCapabilities);
  });

  socket.on('createWebRtcTransport', async (data, callback) => {
    const { direction } = data;
    const transport = await router.createWebRtcTransport({ listenIps: ['127.0.0.1'] });

    transport.on('dtlsstatechange', (event) => {
      if (event.dtlsState === 'closed') {
        transport.close();
      }
    });

    callback({
      transportOptions: {
        id: transport.id,
        iceParameters: transport.iceParameters,
        iceCandidates: transport.iceCandidates,
        dtlsParameters: transport.dtlsParameters,
      },
    });
  });

  socket.on('connectTransport', async (data, callback) => {
    const { transportId, dtlsParameters } = data;
    const transport = router.getTransportById(transportId);
    if (!transport) {
      console.error('Transport not found');
      return callback({ error: 'Transport not found' });
    }

    await transport.connect({ dtlsParameters });

    callback({ connected: true });
  });
});

const port = 3000;
server.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});


```


### Why webRTC transport required of media soup?


mediasoup WebRTCTransport is a crucial component in WebRTC-based media server applications like mediasoup. It serves as a transport channel that facilitates real-time communication between a WebRTC endpoint (such as a web browser) and the media server. WebRTCTransport is an essential part of the WebRTC media handling process and is needed for several reasons:

Media Stream Transport: WebRTCTransport is responsible for transporting media streams, including audio and video, between the client (web browser) and the media server. It handles the secure and reliable transmission of Real-Time Transport Protocol (RTP) and Real-Time Control Protocol (RTCP) packets over the network.

ICE Candidate Exchange: WebRTCTransport assists in the exchange of Interactive Connectivity Establishment (ICE) candidates between the client and the media server. ICE candidates help establish a direct peer-to-peer connection between clients, enabling them to find the best network path for media communication.

DTLS Handshake: WebRTCTransport performs the Datagram Transport Layer Security (DTLS) handshake, which is an essential part of the WebRTC connection setup process. DTLS establishes secure encryption and authentication for media streams.

NAT Traversal: WebRTCTransport helps in Network Address Translation (NAT) traversal, allowing clients behind firewalls or NAT routers to establish a direct connection with the media server, even in complex network environments.

Secure Data Transmission: WebRTCTransport ensures that media streams and other data are transmitted securely over the internet, protecting them from unauthorized access and tampering.

Connection Management: WebRTCTransport handles the state of the connection, including its opening, closing, and any state changes that occur during the communication session.

Support for Simulcast and SVC: In more advanced use cases, WebRTCTransport supports Simulcast and Scalable Video Coding (SVC), allowing the media server to adapt the video quality based on the client's capabilities and network conditions.

In the context of mediasoup, when a client wants to send or receive media streams, it requests the server to create a WebRTCTransport. Once the transport is established, it can be used to send and receive media packets and handle the signaling required for establishing a WebRTC connection.



### Send media stream from client to server

--In the mediasoup-client library, the createSendTransport method is used to create a WebRTC transport for sending media streams from the client (sender) to the mediasoup server (SFU). This transport is often referred to as the "send transport" or "producer transport."

Here's what the createSendTransport method does and why it's essential:

Creates a Send Transport: The createSendTransport method creates a WebRTC transport optimized for sending media streams. It is specifically designed for producers who want to send their audio and video streams to the mediasoup server.

ICE and DTLS Setup: The transport information received from the server during the joinRoom process contains ICE candidates and DTLS parameters. These parameters are used to set up the ICE (Interactive Connectivity Establishment) and DTLS (Datagram Transport Layer Security) components of the WebRTC connection.

Candidate Gathering: The transport automatically starts gathering local ICE candidates, which are network addresses that the client can use to communicate with the server. The ICE gathering process finds the best network path for establishing a direct peer-to-peer connection or traversing NAT/firewall devices.

Encrypted Communication: The transport uses DTLS to establish a secure encrypted communication channel between the client and the mediasoup server. DTLS ensures that the media streams are transmitted securely over the internet.

Media Stream Production: Once the createSendTransport method is called and the transport is set up, the client can use the transport to produce media streams from its microphone and camera. These media streams are then sent to the mediasoup server, which acts as an SFU and forwards the streams to other participants in the room.

### Creating Transport 

Creating Transports
Both mediasoup-client and libmediasoupclient need separate WebRTC transports for sending and receiving. Typically, the client application creates those transports in advance, before even wishing to send or receive media.

For sending media:

A WebRTC transport must be first created in the mediasoup router: router.createWebRtcTransport().
And then replicated in the client side application: device.createSendTransport().
The client application must subscribe to the “connect” and “produce” events in the local transport.
For receiving media:

A WebRTC transport must be first created in the mediasoup router: router.createWebRtcTransport().
And then replicated in the client side application: device.createRecvTransport().
The client application must subscribe to the “connect” event in the local transport.
If SCTP (AKA DataChannel in WebRTC) are desired on those transports, enableSctp must be enabled in them (with proper numSctpStreams) and other SCTP related settings


### Producing Media


Producing Media
Once the send transport is created, the client side application can produce multiple audio and video tracks on it.

The application obtains a track (e.g. by using the navigator.mediaDevices.getUserMedia() API).
It calls transport.produce() in the local send transport.
The transport will emit “connect” if this is the first call to transport.produce().
The transport will emit “produce” so the application will transmit the event parameters to the server and will create a Producer instance in server side.
Finally transport.produce() will resolve with a Producer instance in client side




### Consumer Media 

Consuming Media
Once the receive transport is created, the client side application can consume multiple audio and video tracks on it. However the order is the opposite (here the consumer must be created in the server first).

The client application signals its device.rtpCapabilities to the server (it may have done it in advance).
The server application should check whether the remote device can consume a specific producer (this is, whether it supports the producer media codecs). It can do it by using the router.canConsume() method.
Then the server application calls transport.consume() in the WebRTC transport the client created for receiving media, thus generating a server side Consumer.
As explained in the transport.consume() documentation, it's strongly recommended to create the server side consumer with paused: true and resume it once created in the remote endpoint.
The server application transmits the consumer information and parameters to the remote client application, which calls transport.consume() in the local receive transport.
The transport will emit “connect” if this is the first call to transport.consume().
Finally transport.consume() will resolve with a Consumer instance in client side




### Node js server code for consume, produce,

```js

// server.js
const express = require('express');
const http = require('http');
const { Server } = require('socket.io');
const mediasoup = require('mediasoup');

const app = express();
const httpServer = http.createServer(app);
const io = new Server(httpServer);

const PORT = process.env.PORT || 3001;

let worker;
let router;
const producers = new Map();

(async () => {
  worker = await mediasoup.createWorker();
  const mediaCodecs = mediasoup.getSupportedRtpCapabilities().codecs;
  router = await worker.createRouter({ mediaCodecs });
})();

io.on('connection', (socket) => {
  socket.on('createProducerTransport', async (_, callback) => {
    const transport = await router.createWebRtcTransport({ listenIps: [{ ip: '0.0.0.0', announcedIp: 'your-public-ip' }] });
    producers.set(transport.id, transport);
    const params = {
      id: transport.id,
      iceParameters: transport.iceParameters,
      iceCandidates: transport.iceCandidates,
      dtlsParameters: transport.dtlsParameters,
    };
    callback(params);
  });

  socket.on('connectProducerTransport', async ({ transportId, dtlsParameters }, callback) => {
    const transport = producers.get(transportId);
    if (!transport) {
      callback({ error: 'Producer transport not found' });
      return;
    }

    await transport.connect({ dtlsParameters });
    callback({});
  });

  socket.on('produce', async ({ transportId, kind, rtpParameters }, callback) => {
    const transport = producers.get(transportId);
    if (!transport) {
      callback({ error: 'Producer transport not found' });
      return;
    }

    const producer = await transport.produce({ kind, rtpParameters });
    callback({ id: producer.id });
  });

  socket.on('createConsumerTransport', async (_, callback) => {
    const transport = await router.createWebRtcTransport({ listenIps: [{ ip: '0.0.0.0', announcedIp: 'your-public-ip' }] });
    const params = {
      id: transport.id,
      iceParameters: transport.iceParameters,
      iceCandidates: transport.iceCandidates,
      dtlsParameters: transport.dtlsParameters,
    };
    callback(params);
  });

  socket.on('connectConsumerTransport', async ({ transportId, dtlsParameters }, callback) => {
    const transport = consumers.get(transportId);
    if (!transport) {
      callback({ error: 'Consumer transport not found' });
      return;
    }

    await transport.connect({ dtlsParameters });
    callback({});
  });

  socket.on('consume', async ({ transportId, producerId, rtpCapabilities }, callback) => {
    const transport = consumers.get(transportId);
    if (!transport) {
      callback({ error: 'Consumer transport not found' });
      return;
    }

    if (!router.canConsume({ producerId, rtpCapabilities })) {
      callback({ error: 'Cannot consume this producer' });
      return;
    }

    const consumer = await transport.consume({ producerId, rtpCapabilities });
    callback({ id: consumer.id, kind: consumer.kind, rtpParameters: consumer.rtpParameters });
  });
});

httpServer.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});



```

### create room join room 


```js


// server.js

// ... (previous code)

const rooms = new Map();

io.on('connection', (socket) => {
  socket.on('createRoom', async (callback) => {
    const roomId = generateUniqueId(); // Function to generate a unique Room ID
    if (rooms.has(roomId)) {
      callback({ error: 'Room already exists' });
      return;
    }

    rooms.set(roomId, {
      peers: new Map(),
    });

    const rtpCapabilities = router.rtpCapabilities;
    callback({ roomId, rtpCapabilities });
  });

  socket.on('joinRoom', async (roomId, userId, callback) => {
    const room = rooms.get(roomId);
    if (!room) {
      callback({ error: 'Room not found' });
      return;
    }

    const transport = await router.createWebRtcTransport();
    room.peers.set(userId, {
      socketId: socket.id,
      transport,
      consumers: new Map(),
    });

    const transportOptions = {
      id: transport.id,
      iceParameters: transport.iceParameters,
      iceCandidates: transport.iceCandidates,
      dtlsParameters: transport.dtlsParameters,
    };

    callback({ peers: Array.from(room.peers.keys()), transportOptions });

    socket.on('disconnect', () => {
      const peer = room.peers.get(userId);
      if (peer) {
        peer.transport.close();
        room.peers.delete(userId);
        removeConsumers(room, userId);
        io.to(roomId).emit('peerLeft', userId);
      }
    });
  });

  // ... (previous code)
});
```

### React code

```js

// App.js
import React, { useEffect, useRef, useState } from 'react';
import io from 'socket.io-client';
import { Device } from 'mediasoup-client';

const socket = io('http://localhost:3001'); // Replace with your server URL

function App() {
  const videoRef = useRef();
  const [roomId, setRoomId] = useState('');
  const [userId, setUserId] = useState('');
  const [producerTransport, setProducerTransport] = useState(null);
  const [consumerTransport, setConsumerTransport] = useState(null);
  const [producer, setProducer] = useState(null);
  const [consumers, setConsumers] = useState([]);

  useEffect(() => {
    socket.on('connect', () => {
      setUserId(socket.id);
    });

    socket.on('newProducer', handleNewProducer);
    socket.on('newConsumer', handleNewConsumer);

    return () => {
      socket.off('newProducer', handleNewProducer);
      socket.off('newConsumer', handleNewConsumer);
    };
  }, []);

  const handleNewProducer = async (producerData) => {
    const consumer = await createConsumer(producerData);
    setConsumers((prevConsumers) => [...prevConsumers, consumer]);
  };

  const handleNewConsumer = (consumerData) => {
    // Handle new consumer, if needed
  };

  const createConsumer = async (producerData) => {
    const { id, kind, rtpParameters } = producerData;

    const transportId = consumerTransport.id;
    const { id: consumerId, kind: consumerKind, rtpParameters: consumerRtpParameters } = await sendRequest(
      'consume',
      { transportId, producerId: id, rtpCapabilities: device.rtpCapabilities }
    );

    const consumer = await consumerTransport.consume({
      id: consumerId,
      producerId: id,
      kind,
      rtpParameters: consumerRtpParameters,
    });

    return consumer;
  };

  const sendRequest = async (type, data) => {
    return new Promise((resolve) => {
      socket.emit(type, data, (response) => {
        if (response.error) {
          console.error(`Error in ${type}: ${response.error}`);
          return resolve(null);
        }

        resolve(response);
      });
    });
  };

  const initializeProducerTransport = async () => {
    const { id, iceParameters, iceCandidates, dtlsParameters } = await sendRequest('createProducerTransport', {});
    const transport = device.createSendTransport({
      id,
      iceParameters,
      iceCandidates,
      dtlsParameters,
    });

    transport.on('connect', async ({ dtlsParameters }, callback, errback) => {
      sendRequest('connectProducerTransport', { transportId: id, dtlsParameters }, callback);
    });

    setProducerTransport(transport);
  };

  const initializeConsumerTransport = async () => {
    const { id, iceParameters, iceCandidates, dtlsParameters } = await sendRequest('createConsumerTransport', {});
    const transport = device.createRecvTransport({
      id,
      iceParameters,
      iceCandidates,
      dtlsParameters,
    });

    transport.on('connect', async ({ dtlsParameters }, callback, errback) => {
      sendRequest('connectConsumerTransport', { transportId: id, dtlsParameters }, callback);
    });

    setConsumerTransport(transport);
  };

  const startProducing = async () => {
    const stream = await navigator.mediaDevices.getUserMedia({ audio: true, video: true });
    videoRef.current.srcObject = stream;

    const { id, kind, rtpParameters } = await producerTransport.produce({
      track: stream.getVideoTracks()[0],
    });

    setProducer({ id, kind, rtpParameters });
  };

  return (
    <div>
      <input
        type="text"
        value={roomId}
        onChange={(e) => setRoomId(e.target.value)}
        placeholder="Enter Room ID"
      />
      <button onClick={initializeProducerTransport}>Initialize Producer Transport</button>
      <button onClick={initializeConsumerTransport}>Initialize Consumer Transport</button>
      <button onClick={startProducing}>Start Producing</button>
      <video ref={videoRef} autoPlay playsInline muted />
      {consumers.map((consumer) => (
        <video key={consumer.id} autoPlay playsInline ref={(video) => (video.srcObject = consumer.track.stream)} />
      ))}
    </div>
  );
}

const device = new Device();

export default App;

```

### Create room join room 

```js

// App.js

// ... (previous code)

function App() {
  // ... (previous code)

  const createRoom = () => {
   socket.emit('createRoom', (response) => {
      if (response.error) {
        console.error('Error creating room:', response.error);
      } else {
        console.log('Room created:', response.roomId);
        setRoomId(response.roomId);
        initializeProducerTransport();
        initializeConsumerTransport();
      }
    });
  };

  const joinRoom = () => {
    if (roomId && userId) {
      socket.emit('joinRoom', roomId, userId, (response) => {
        if (response.error) {
          console.error('Error joining room:', response.error);
        } else {
          console.log('Joined room:', roomId);
          setPeers(response.peers);
          initializeProducerTransport();
          initializeConsumerTransport();
        }
      });
    }
  };

  return (
    <div>
      <input
        type="text"
        value={roomId}
        onChange={(e) => setRoomId(e.target.value)}
        placeholder="Enter Room ID"
      />
      <button onClick={createRoom}>Create Room</button>
      <button onClick={joinRoom}>Join Room</button>
      {/* ... (previous code) */}
    </div>
  );
}

// ... (previous code)
```

#### WHy createwebrtctransport required

In the SFU (Selective Forwarding Unit) architecture, you create a WebRTC transport for each participant in the room when they join the room. This transport is used to send and receive media streams between the server and the participant. The createWebRtcTransport function is called on the server-side when a new participant joins the room, allowing them to establish a WebRTC connection with the server for media exchange.


### why we need to create and connect producer transport?

In the SFU (Selective Forwarding Unit) architecture, the producer transport is responsible for sending media streams (e.g., audio and video) from the user's device to the SFU server. It acts as the outbound communication channel for the user's media.

We need to create and connect the producer transport for the following reasons:

Media Publishing: The producer transport is essential for enabling the user to publish their media streams to the SFU server. When a user wants to start streaming their audio and video to the room, a producer transport needs to be created so that their media can be sent to the SFU server.

SFU Forwarding: Once the media is captured by the user's device, it is sent through the producer transport to the SFU server. The SFU server acts as an intermediary, forwarding the media to all other participants in the room. Each participant, including the original sender, receives the media from the SFU server.

Scalability: The SFU architecture is designed for scalability, where the server is responsible for forwarding media streams between participants. By using producer transports, the SFU server can efficiently manage media distribution without the need for direct peer-to-peer connections between participants. This allows for better handling of large-scale group video conferences.

Adaptability: The producer transport provides a way to handle changes in the user's media, such as resolution, codecs, or bandwidth. If the user's device encounters changes in network conditions or device capabilities, the producer transport can adapt and adjust the media transmission accordingly.

In summary, the producer transport is necessary for sending media streams from the user's device to the SFU server in an SFU architecture. It enables media publishing, allows the SFU server to forward media to all participants, and ensures scalability and adaptability in group video conferencing scenarios.


### why we need to create and connect consumer transport?



In the SFU (Selective Forwarding Unit) architecture, the consumer transport is responsible for receiving media streams from other participants in the room. It acts as the inbound communication channel for the user's device to receive media.

We need to create and connect the consumer transport for the following reasons:

Media Subscribing: The consumer transport is essential for enabling the user's device to subscribe to media streams from other participants in the room. When a user joins a room and wants to view/hear media from other participants, a consumer transport needs to be created so that their device can receive media from the SFU server.

SFU Forwarding: In the SFU architecture, the SFU server acts as an intermediary that receives media streams from each participant and selectively forwards them to other participants. When a user subscribes to a media stream, the SFU server sends that media stream to the user's device through the consumer transport.

Scalability: The consumer transport allows the SFU server to manage media distribution efficiently. By using consumer transports, the SFU server can deliver media streams to multiple users without requiring direct peer-to-peer connections between participants. This enables the SFU architecture to handle large-scale group video conferences.

Media Handling: The consumer transport allows the user's device to receive media streams in a controlled manner. It provides mechanisms for handling media reception, buffering, and adaptation based on network conditions and device capabilities.

Selective Subscribing: In the SFU architecture, each user can selectively subscribe to specific media streams based on their preferences. For example, a user may choose to subscribe to audio streams only or video streams from specific participants. The consumer transport facilitates this selective subscribing process.








### How to handle emoji ?


import React, { useState, useRef } from 'react';
import EmojiPicker from 'emoji-picker-react';

const LiveStreamApp = () => {
  const [chosenEmojis, setChosenEmojis] = useState([]);
  const inputRef = useRef(null);

  const handleEmojiClick = (event, emojiObject) => {
    const emoji = emojiObject.emoji;
    insertAtCursor(emoji);
  };

  const insertAtCursor = (emoji) => {
    const input = inputRef.current;
    const start = input.selectionStart;
    const end = input.selectionEnd;
    const value = input.value;

    const newValue = value.substring(0, start) + emoji + value.substring(end);
    setChosenEmojis((prevEmojis) => [...prevEmojis, emoji]);
    input.focus();
    input.value = newValue;
    input.setSelectionRange(start + emoji.length, start + emoji.length);
  };

  const handleBackspace = (e) => {
    if (e.key === 'Backspace') {
      const input = inputRef.current;
      const start = input.selectionStart;
      const end = input.selectionEnd;
      const value = input.value;

      if (start === end && start > 0) {
        // If the selection is collapsed and the cursor is not at the beginning
        const emojiToRemove = value.substring(start - 2, start);
        const newValue = value.substring(0, start - 2) + value.substring(start);
        setChosenEmojis((prevEmojis) => prevEmojis.filter((emoji) => emoji !== emojiToRemove));
        input.value = newValue;
        input.setSelectionRange(start - 2, start - 2);
      } else if (start !== end) {
        // If there is a selected range of text
        const newValue = value.substring(0, start) + value.substring(end);
        setChosenEmojis((prevEmojis) => {
          const emojisToRemove = value.substring(start, end).match(/[\uD800-\uDBFF][\uDC00-\uDFFF]|./g);
          return prevEmojis.filter((emoji) => !emojisToRemove.includes(emoji));
        });
        input.value = newValue;
        input.setSelectionRange(start, start);
      }
    }
  };

  const handleSentChatMessage = (e) => {
    if (e.key === 'Enter') {
      const message = e.target.value;
      // Here, you can send the message (including emojis) to the server using your WebSocket implementation
      console.log('Sending message:', message);
      setChosenEmojis([]);
      inputRef.current.value = '';
    }
  };

  return (
    <div>
      <h1>Live Stream</h1>
      <div className="emoji-picker">
        {/* Render the EmojiPicker component */}
        <EmojiPicker onEmojiClick={handleEmojiClick} />
      </div>
      <div className="chosen-emojis">
        {chosenEmojis.map((emoji, index) => (
          <span key={index} onClick={() => handleRemoveEmoji(index)}>
            {emoji}
          </span>
        ))}
      </div>
      <div
        className="input-wrapper"
        ref={inputRef}
        contentEditable
        onInput={(e) => {
          setChosenEmojis(getEmojisFromContentEditable(e.currentTarget));
          // Clear the input value to avoid duplicating content
          e.currentTarget.textContent = '';
        }}
        onKeyDown={handleBackspace}
        onPaste={(e) => {
          e.preventDefault();
          const text = e.clipboardData.getData('text/plain');
          document.execCommand('insertText', false, text);
        }}
      ></div>
    </div>
  );
};

const getEmojisFromContentEditable = (element) => {
  const emojis = element.innerText.match(/[\uD800-\uDBFF][\uDC00-\uDFFF]|./g);
  return emojis ? emojis.filter((char) => char.length === 2) : [];
};

export default LiveStreamApp;

### How to check if a text message contains emojis or not 
```js
const containsEmoji = (text) => {
  const emojiRegex = /[\u{1F600}-\u{1F64F}\u{1F300}-\u{1F5FF}\u{1F680}-\u{1F6FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{1F900}-\u{1F9FF}\u{1F1E6}-\u{1F1FF}]/u;
  return emojiRegex.test(text);
};
```

```js
const message = "Hello! 😃👋";
console.log(containsEmoji(message)); // true

const plainTextMessage = "This is a plain text message.";
console.log(containsEmoji(plainTextMessage)); // false

```











In summary, the consumer transport is necessary for receiving media streams from other participants in the room in an SFU architecture. It enables media subscribing, allows the SFU server to forward media streams to the user's device, ensures scalability in group video conferencing scenarios, and provides mechanisms for handling media reception and adaptation.


### Ffmpeg to do recording  (WRONG APPROACH)

```js
const producerToRecordingMap = {};

io.on("connection", (socket) => {
  console.log("A user connected!");

  socket.on("start-recording", ({ producerId }) => {
    // Check if the producer exists and is not already being recorded
    if (producerToRecordingMap[producerId] || !producerTransport.producers.has(producerId)) {
      return;
    }

    const producer = producerTransport.producers.get(producerId);
    const fileName = `${producer.appData}-${Date.now()}.webm`;
    const outputPath = `/path/to/your/output/folder/${fileName}`;

    // Create a new FFmpeg process to start recording
    const recordingProcess = ffmpeg(producer.track)
      .output(outputPath)
      .on("end", () => {
        console.log("Recording ended!");
        delete producerToRecordingMap[producerId];
      })
      .on("error", (err) => {
        console.error("Error while recording:", err);
        delete producerToRecordingMap[producerId];
      })
      .run();

    producerToRecordingMap[producerId] = recordingProcess;
  });

  socket.on("disconnect", () => {
    console.log("A user disconnected!");
  });
});

```

### FFMPEG 

```js

const { createMediasoupWorker, Router, WebRtcTransport } = require("mediasoup");
const ffmpeg = require("fluent-ffmpeg");

const startMediasoupWorker = async () => {
  const worker = await createMediasoupWorker();
  const router = await worker.createRouter();

  const producerToRecordingMap = {};

  // Handle socket connection
  io.on("connection", (socket) => {
    console.log("A user connected!");

    // Handle start-recording event from the frontend
    socket.on("start-recording", async ({ producerId }) => {
      // Check if the producer exists and is not already being recorded
      if (!router.producers.has(producerId)) {
        console.log("Producer not found or already recording");
        return;
      }

      const producer = router.producers.get(producerId);

      // Create a new WebRTC transport to receive RTP packets from the producer
      const producerTransport = await createWebRtcTransport(router);

      // Consume the producer's media and send it to the consumer transport
      const consumer = await producerTransport.consume({ producerId });

      // Create a new WebRTC transport to send RTP packets to FFmpeg
      const ffmpegTransport = await createWebRtcTransport(router);

      // Start recording the media stream using FFmpeg
      const outputPath = `/path/to/your/output/folder/${producerId}-${Date.now()}.webm`;
      const args = ["-protocol_whitelist", "pipe,rtpfile", "-i", "pipe:0", outputPath];
      const recordingProcess = ffmpeg(consumer.track)
        .outputOptions(args)
        .on("end", () => {
          console.log("Recording ended!");
          delete producerToRecordingMap[producerId];
        })
        .on("error", (err) => {
          console.error("Error while recording:", err);
          delete producerToRecordingMap[producerId];
        })
        .save(outputPath);

      // Save the recording process and transports in the producerToRecordingMap
      producerToRecordingMap[producerId] = {
        producerTransport,
        ffmpegTransport,
        recordingProcess,
      };

      // Emit a message to the frontend indicating that recording has started
      socket.emit("recording-started", { producerId, outputPath });
    });

    // Handle stop-recording event from the frontend
    socket.on("stop-recording", async ({ producerId }) => {
      const recordingInfo = producerToRecordingMap[producerId];
      if (recordingInfo) {
        const { producerTransport, ffmpegTransport, recordingProcess } = recordingInfo;
        await producerTransport.close();
        await ffmpegTransport.close();
        recordingProcess.kill();
        delete producerToRecordingMap[producerId];
        console.log(`Recording for producer ${producerId} stopped.`);
      }
    });

    socket.on("disconnect", () => {
      console.log("A user disconnected!");
    });
  });
};

startMediasoupWorker().catch((err) => {
  console.error("Error starting Mediasoup worker:", err);
});




### recording audio and video stream

In a scenario where you want to record both audio and video streams of a real-time communication session using FFmpeg in the backend, and you need to handle cases where the audio stream is added or resumed after the recording has started, you'll need to implement a strategy to manage these streams dynamically.

Here's a general approach you could follow:

Start Recording Video and Audio:
When the producer starts sharing their screen, you can initiate a FFmpeg process to record the video stream and the audio stream (if available). If the audio stream is not yet available, you can record a silent audio placeholder.

Monitor for Audio Stream Start:
Continuously monitor the communication stream for the audio stream to start. This could be done using the real-time communication protocol you're using (WebRTC, RTP, etc.).

Adjust Audio in Recording:
When the audio stream starts, you'll need to adjust the existing recorded file to include the audio. This process can involve:

Creating a new FFmpeg process to record the audio stream.
Merging the previously recorded video (without audio) and the new audio recording to create a new file with synchronized audio and video.
The FFmpeg command to merge audio and video could look something like:

css
Copy code
ffmpeg -i video_without_audio.mp4 -i audio.mp3 -c:v copy -c:a aac -strict experimental output_with_audio.mp4
Managing Timing and Synchronization:
When merging the audio and video streams, you need to ensure that they are synchronized properly. This might involve using timestamps or other synchronization mechanisms to align the audio and video streams correctly.

Handle Cases of Disconnection or Interruption:
Consider scenarios where the audio stream is interrupted or disconnected during recording. You'll need to handle such cases gracefully, ensuring that your recording logic can adapt to changes in real-time communication conditions.

Testing and Validation:
Thoroughly test your solution to ensure that the audio and video synchronization works correctly, and that the merged recording maintains good quality.

Remember that managing real-time communication streams and dynamic changes during recording can be complex. This approach requires a good understanding of the communication protocol, FFmpeg usage, and synchronization techniques. Depending on your requirements and the complexity of your application, you might also want to explore specialized media recording libraries or solutions that handle such scenarios more seamlessly.

```

```js
import React, { useEffect, useRef } from 'react';

const YourComponent = ({ videoRef }) => {
  const canvasRef = useRef(null);
  const canvasWidth = '100%'; // Set to a percentage or fixed value if needed
  const canvasHeight = '100%'; // Set to a percentage or fixed value if needed

  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');

    const parentContainer = canvas.parentElement;

    const renderVideoToCanvas = () => {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      console.log('Capture screen share', canvas.width, canvas.height);

      const videoWidth = videoRef.current.videoWidth;
      const videoHeight = videoRef.current.videoHeight;

      const aspectRatio = videoWidth / videoHeight;

      // Calculate the dimensions to fit the video inside the canvas while preserving the aspect ratio
      let destinationWidth = parentContainer.clientWidth;
      let destinationHeight = destinationWidth / aspectRatio;

      if (destinationHeight > parentContainer.clientHeight) {
        destinationHeight = parentContainer.clientHeight;
        destinationWidth = destinationHeight * aspectRatio;
      }

      const destinationX = (parentContainer.clientWidth - destinationWidth) / 2;
      const destinationY = (parentContainer.clientHeight - destinationHeight) / 2;

      ctx.drawImage(videoRef.current, destinationX, destinationY, destinationWidth, destinationHeight);
      animateFrameId = window.requestAnimationFrame(renderVideoToCanvas);
    };

    // Set canvas dimensions based on the parent container's size
    canvas.width = parentContainer.clientWidth;
    canvas.height = parentContainer.clientHeight;

    animateFrameId = window.requestAnimationFrame(renderVideoToCanvas);

    // Cleanup function to cancel animation frame when component unmounts
    return () => {
      window.cancelAnimationFrame(animateFrameId);
    };
  }, [videoRef]);

  return (
    <canvas
      ref={canvasRef}
      width={canvasWidth}
      height={canvasHeight}
      style={{
        position: 'absolute',
        width: '100%',
        height: '100%',
        objectFit: 'contain',
      }}
    ></canvas>
  );
};

export default YourComponent;

```


```js


const getScreenShareFeed = async () => {
  try {
    const canvas = canvasRef.current; // Assuming you have a canvas reference
    const stream = canvas.captureStream(); // Capture the canvas stream

    if (screenShareRef.current) {
      screenShareRef.current.srcObject = stream;

      // call produce method using producerTransport to send this media to everybody else in real time

      const track = stream.getTracks()[0]; // Get the first track (video track)

      if (producerTransport) {
        // if there is producerTransport
        if (producerScreenShare) {
          await producerScreenShare.replaceTrack({ track });
          setScreenShareStream(stream);
          setIsScreenShare(true);
          return;
        }

        const producerScreenShareRec = await producerTransport.produce({
          track,
          appData: {
            streamType: staticVariables.screenShare,
            isTeacher: true,
          },
        });

        setProducerScreenShare(producerScreenShareRec);
        setScreenShareStream(stream);
        setIsScreenShare(true);
      }
    }
  } catch (err) {
    console.log("Screen share feed error = ", err);
  }
};

```

## How to get thumbnail using mediaconvert

```js
const mediaconvertconfig = (inputLocation, outputLocation, thumbnailOutputLocation) => {
  const params = {
    Queue: process.env.MEDIACONVERT_QUEUE,
    Role: process.env.MEDIACONVERT_ROLE,
    Settings: {
      TimecodeConfig: {
        Source: "ZEROBASED",
      },
      OutputGroups: [
        {
          CustomName: "convert-video-to-dash",
          Name: "DASH ISO",
          Outputs: [
            {
              Preset: "System-Ott_Dash_Mp4_Avc_4x3_480x360p_15Hz_0.4Mbps",
              NameModifier: "_output1",
            },
            {
              ContainerSettings: {
                Container: "MPD",
              },
              AudioDescriptions: [
                {
                  AudioSourceName: "Audio Selector 1",
                  CodecSettings: {
                    Codec: "AAC",
                    AacSettings: {
                      Bitrate: 96000,
                      CodingMode: "CODING_MODE_2_0",
                      SampleRate: 48000,
                    },
                  },
                },
              ],
              NameModifier: "_output2",
            },
            {
              Preset: "System-Ott_Dash_Mp4_Avc_16x9_1280x720p_30Hz_5.0Mbps",
              NameModifier: "_output3",
            },
            {
              Preset: "System-Ott_Dash_Mp4_Avc_16x9_1920x1080p_30Hz_8.5Mbps",
              NameModifier: "_output4",
            },
          ],
          OutputGroupSettings: {
            Type: "DASH_ISO_GROUP_SETTINGS",
            DashIsoGroupSettings: {
              SegmentLength: 30,
              Destination: outputLocation,
              Encryption: {
                PlaybackDeviceCompatibility: "CENC_V1",
                SpekeKeyProvider: {
                  ResourceId: generateRandomId(20),
                  SystemIds: [
                    process.env.PLAYREADY_SYSTEM_ID,
                    process.env.WIDEVINE_SYSTEM_ID,
                  ],
                  Url: process.env.API_GATEWAY_SPEKE_URL,
                },
              },
              FragmentLength: 2,
            },
          },
        },
        {
          CustomName: "thumbnails",
          Name: "Thumbnails",
          Outputs: [
            {
              ContainerSettings: {
                Container: "RAW",
              },
              VideoDescription: {
                ScalingBehavior: "DEFAULT",
                TimecodeInsertion: "DISABLED",
                AntiAlias: "ENABLED",
                Sharpness: 50,
                CodecSettings: {
                  Codec: "FRAME_CAPTURE",
                  FrameCaptureSettings: {
                    FramerateNumerator: 1,
                    FramerateDenominator: 5,
                    MaxCaptures: 100,
                    Quality: 80,
                    Resolution: "ORIGINAL",
                    S3DestinationSettings: {
                      AccessControl: "PUBLIC_READ",
                      Bucket: thumbnailOutputLocation,
                      StorageClass: "STANDARD",
                    },
                  },
                },
              },
              NameModifier: "_thumbnails",
            },
          ],
          OutputGroupSettings: {
            Type: "FILE_GROUP_SETTINGS",
            FileGroupSettings: {
              Destination: thumbnailOutputLocation,
            },
          },
        },
      ],
      Inputs: [
        {
          AudioSelectors: {
            "Audio Selector 1": {
              DefaultSelection: "DEFAULT",
            },
          },
          VideoSelector: {},
          TimecodeSource: "ZEROBASED",
          FileInput: inputLocation,
        },
      ],
    },
    AccelerationSettings: {
      Mode: "DISABLED",
    },
    StatusUpdateInterval: "SECONDS_60",
    Priority: 0,
  };
  return params;
};

module.exports = mediaconvertconfig;
```

## How to create project insp bottom navigator 

``` 
function BottomTabNavigation() {
  return (
    <Tab.Navigator
      screenOptions={{
        headerShown: false,
        tabBarShowLabel: false,
        tabBarStyle: {
          backgroundColor: "rgba(60, 141, 188, 1)",
          position: "absolute",
          bottom: 15,
          marginHorizontal: 15,
          borderRadius: 15,
          height: 70,
        },
      }}
    >
      <Tab.Screen
        name="Restaurant"
        component={RestaurantScreen}
        options={{
          tabBarIcon: ({ color, size, focused }) => (
            <View
              style={{
                // centring Tab Button...
                position: "relative",
                backgroundColor: focused ? "white" : "transparent",
                padding: 6,
                borderRadius: 10,
              }}
            >
              {focused && (
                <View
                  style={{
                    position: "absolute",
                    top: -5,
                    left: "50%",
                    borderRadius: 10,
                    width: 10,
                    height: 20,
                    transform: [{ translateX: -23 }],
                    backgroundColor: "white",
                  }}
                />
              )}

              <Icon
                name="home-outline"
                size={30}
                color={focused ? "rgba(60, 141, 188, 1)" : "white"}
              />
            </View>
          ),
        }}
      />
      <Tab.Screen
        name="Settings"
        component={SettingsScreen}
        options={{
          tabBarIcon: ({ color, size, focused }) => (
            <View
              style={{
                // centring Tab Button...
                position: "relative",
                backgroundColor: focused ? "white" : "transparent",
                padding: 6,
                borderRadius: 10,
              }}
            >
              {focused && (
                <View
                  style={{
                    position: "absolute",
                    top: -5,
                    left: "50%",
                    borderRadius: 10,
                    width: 10,
                    height: 20,
                    transform: [{ translateX: -23 }],
                    backgroundColor: "white",
                  }}
                />
              )}

              <Icon
                name="home-outline"
                size={30}
                color={focused ? "rgba(60, 141, 188, 1)" : "white"}
              />
            </View>
          ),
        }}
      />
      <Tab.Screen
        name="Maps"
        component={MapsScreen}
        options={{
          tabBarIcon: ({ color, size, focused }) => (
            <View
              style={{
                // centring Tab Button...
                position: "relative",
                backgroundColor: focused ? "white" : "transparent",
                padding: 6,
                borderRadius: 10,
              }}
            >
              {focused && (
                <View
                  style={{
                    position: "absolute",
                    top: -5,
                    left: "50%",
                    borderRadius: 10,
                    width: 10,
                    height: 20,
                    transform: [{ translateX: -23 }],
                    backgroundColor: "white",
                  }}
                />
              )}

              <Icon
                name="home-outline"
                size={30}
                color={focused ? "rgba(60, 141, 188, 1)" : "white"}
              />
            </View>
          ),
        }}
      />
      <Tab.Screen
        name="Profile"
        component={ProfileScreen}
        options={{
          tabBarIcon: ({ color, size, focused }) => (
            <View
              style={{
                // centring Tab Button...
                position: "relative",
                backgroundColor: focused ? "white" : "transparent",
                padding: 6,
                borderRadius: 10,
              }}
            >
              {focused && (
                <View
                  style={{
                    position: "absolute",
                    top: -5,
                    left: "50%",
                    borderRadius: 10,
                    width: 10,
                    height: 20,
                    transform: [{ translateX: -23 }],
                    backgroundColor: "white",
                  }}
                />
              )}

              <Icon
                name="home-outline"
                size={30}
                color={focused ? "rgba(60, 141, 188, 1)" : "white"}
              />
            </View>
          ),
        }}
      />
    </Tab.Navigator>
  );
}
```


## pakcage.locl


```js

{
  "name": "inspbackend",
  "version": "1.0.0",
  "lockfileVersion": 3,
  "requires": true,
  "packages": {
    "": {
      "name": "inspbackend",
      "version": "1.0.0",
      "license": "ISC",
      "dependencies": {
        "@socket.io/redis-adapter": "^8.2.1",
        "aws-sdk": "^2.1446.0",
        "bcrypt": "^5.1.0",
        "body-parser": "^1.20.2",
        "cookie-parser": "^1.4.6",
        "cors": "^2.8.5",
        "crypto-js": "^4.1.1",
        "dotenv": "^16.3.1",
        "express": "^4.18.2",
        "express-fileupload": "^1.4.0",
        "googleapis": "^126.0.1",
        "jsonwebtoken": "^9.0.2",
        "mediasoup": "^3.12.8",
        "moment-timezone": "^0.5.43",
        "mysql": "^2.18.1",
        "mysql2": "^3.6.0",
        "node-schedule": "^2.1.1",
        "nodemailer": "^6.9.4",
        "pdf-lib": "^1.17.1",
        "pg": "^8.11.1",
        "pg-hstore": "^2.3.4",
        "redis": "^4.6.13",
        "request-ip": "^3.3.0",
        "sequelize": "^6.32.1",
        "socket.io": "^4.7.2",
        "sync-request": "^6.1.0",
        "tree-kill": "^1.2.2",
        "twilio": "^4.15.0",
        "uuid": "^9.0.0"
      },
      "devDependencies": {
        "nodemon": "^3.1.0",
        "sequelize-cli": "^6.6.1"
      }
    },
    "node_modules/@mapbox/node-pre-gyp": {
      "version": "1.0.11",
      "resolved": "https://registry.npmjs.org/@mapbox/node-pre-gyp/-/node-pre-gyp-1.0.11.tgz",
      "integrity": "sha512-Yhlar6v9WQgUp/He7BdgzOz8lqMQ8sU+jkCq7Wx8Myc5YFJLbEe7lgui/V7G1qB1DJykHSGwreceSaD60Y0PUQ==",
      "dependencies": {
        "detect-libc": "^2.0.0",
        "https-proxy-agent": "^5.0.0",
        "make-dir": "^3.1.0",
        "node-fetch": "^2.6.7",
        "nopt": "^5.0.0",
        "npmlog": "^5.0.1",
        "rimraf": "^3.0.2",
        "semver": "^7.3.5",
        "tar": "^6.1.11"
      },
      "bin": {
        "node-pre-gyp": "bin/node-pre-gyp"
      }
    },
    "node_modules/@one-ini/wasm": {
      "version": "0.1.1",
      "resolved": "https://registry.npmjs.org/@one-ini/wasm/-/wasm-0.1.1.tgz",
      "integrity": "sha512-XuySG1E38YScSJoMlqovLru4KTUNSjgVTIjyh7qMX6aNN5HY5Ct5LhRJdxO79JtTzKfzV/bnWpz+zquYrISsvw==",
      "dev": true
    },
    "node_modules/@pdf-lib/standard-fonts": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/@pdf-lib/standard-fonts/-/standard-fonts-1.0.0.tgz",
      "integrity": "sha512-hU30BK9IUN/su0Mn9VdlVKsWBS6GyhVfqjwl1FjZN4TxP6cCw0jP2w7V3Hf5uX7M0AZJ16vey9yE0ny7Sa59ZA==",
      "dependencies": {
        "pako": "^1.0.6"
      }
    },
    "node_modules/@pdf-lib/standard-fonts/node_modules/pako": {
      "version": "1.0.11",
      "resolved": "https://registry.npmjs.org/pako/-/pako-1.0.11.tgz",
      "integrity": "sha512-4hLB8Py4zZce5s4yd9XzopqwVv/yGNhV1Bl8NTmCq1763HeK2+EwVTv+leGeL13Dnh2wfbqowVPXCIO0z4taYw=="
    },
    "node_modules/@pdf-lib/upng": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/@pdf-lib/upng/-/upng-1.0.1.tgz",
      "integrity": "sha512-dQK2FUMQtowVP00mtIksrlZhdFXQZPC+taih1q4CvPZ5vqdxR/LKBaFg0oAfzd1GlHZXXSPdQfzQnt+ViGvEIQ==",
      "dependencies": {
        "pako": "^1.0.10"
      }
    },
    "node_modules/@pdf-lib/upng/node_modules/pako": {
      "version": "1.0.11",
      "resolved": "https://registry.npmjs.org/pako/-/pako-1.0.11.tgz",
      "integrity": "sha512-4hLB8Py4zZce5s4yd9XzopqwVv/yGNhV1Bl8NTmCq1763HeK2+EwVTv+leGeL13Dnh2wfbqowVPXCIO0z4taYw=="
    },
    "node_modules/@redis/bloom": {
      "version": "1.2.0",
      "resolved": "https://registry.npmjs.org/@redis/bloom/-/bloom-1.2.0.tgz",
      "integrity": "sha512-HG2DFjYKbpNmVXsa0keLHp/3leGJz1mjh09f2RLGGLQZzSHpkmZWuwJbAvo3QcRY8p80m5+ZdXZdYOSBLlp7Cg==",
      "peerDependencies": {
        "@redis/client": "^1.0.0"
      }
    },
    "node_modules/@redis/client": {
      "version": "1.5.14",
      "resolved": "https://registry.npmjs.org/@redis/client/-/client-1.5.14.tgz",
      "integrity": "sha512-YGn0GqsRBFUQxklhY7v562VMOP0DcmlrHHs3IV1mFE3cbxe31IITUkqhBcIhVSI/2JqtWAJXg5mjV4aU+zD0HA==",
      "dependencies": {
        "cluster-key-slot": "1.1.2",
        "generic-pool": "3.9.0",
        "yallist": "4.0.0"
      },
      "engines": {
        "node": ">=14"
      }
    },
    "node_modules/@redis/graph": {
      "version": "1.1.1",
      "resolved": "https://registry.npmjs.org/@redis/graph/-/graph-1.1.1.tgz",
      "integrity": "sha512-FEMTcTHZozZciLRl6GiiIB4zGm5z5F3F6a6FZCyrfxdKOhFlGkiAqlexWMBzCi4DcRoyiOsuLfW+cjlGWyExOw==",
      "peerDependencies": {
        "@redis/client": "^1.0.0"
      }
    },
    "node_modules/@redis/json": {
      "version": "1.0.6",
      "resolved": "https://registry.npmjs.org/@redis/json/-/json-1.0.6.tgz",
      "integrity": "sha512-rcZO3bfQbm2zPRpqo82XbW8zg4G/w4W3tI7X8Mqleq9goQjAGLL7q/1n1ZX4dXEAmORVZ4s1+uKLaUOg7LrUhw==",
      "peerDependencies": {
        "@redis/client": "^1.0.0"
      }
    },
    "node_modules/@redis/search": {
      "version": "1.1.6",
      "resolved": "https://registry.npmjs.org/@redis/search/-/search-1.1.6.tgz",
      "integrity": "sha512-mZXCxbTYKBQ3M2lZnEddwEAks0Kc7nauire8q20oA0oA/LoA+E/b5Y5KZn232ztPb1FkIGqo12vh3Lf+Vw5iTw==",
      "peerDependencies": {
        "@redis/client": "^1.0.0"
      }
    },
    "node_modules/@redis/time-series": {
      "version": "1.0.5",
      "resolved": "https://registry.npmjs.org/@redis/time-series/-/time-series-1.0.5.tgz",
      "integrity": "sha512-IFjIgTusQym2B5IZJG3XKr5llka7ey84fw/NOYqESP5WUfQs9zz1ww/9+qoz4ka/S6KcGBodzlCeZ5UImKbscg==",
      "peerDependencies": {
        "@redis/client": "^1.0.0"
      }
    },
    "node_modules/@socket.io/component-emitter": {
      "version": "3.1.0",
      "resolved": "https://registry.npmjs.org/@socket.io/component-emitter/-/component-emitter-3.1.0.tgz",
      "integrity": "sha512-+9jVqKhRSpsc591z5vX+X5Yyw+he/HCB4iQ/RYxw35CEPaY1gnsNE43nf9n9AaYjAQrTiI/mOwKUKdUs9vf7Xg=="
    },
    "node_modules/@socket.io/redis-adapter": {
      "version": "8.2.1",
      "resolved": "https://registry.npmjs.org/@socket.io/redis-adapter/-/redis-adapter-8.2.1.tgz",
      "integrity": "sha512-6Dt7EZgGSBP0qvXeOKGx7NnSr2tPMbVDfDyL97zerZo+v69hMfL99skMCL3RKZlWVqLyRme2T0wcy3udHhtOsg==",
      "dependencies": {
        "debug": "~4.3.1",
        "notepack.io": "~3.0.1",
        "uid2": "1.0.0"
      },
      "engines": {
        "node": ">=10.0.0"
      },
      "peerDependencies": {
        "socket.io-adapter": "^2.4.0"
      }
    },
    "node_modules/@socket.io/redis-adapter/node_modules/debug": {
      "version": "4.3.4",
      "resolved": "https://registry.npmjs.org/debug/-/debug-4.3.4.tgz",
      "integrity": "sha512-PRWFHuSU3eDtQJPvnNY7Jcket1j0t5OuOsFzPPzsekD52Zl8qUfFIPEiswXqIvHWGVHOgX+7G/vCNNhehwxfkQ==",
      "dependencies": {
        "ms": "2.1.2"
      },
      "engines": {
        "node": ">=6.0"
      },
      "peerDependenciesMeta": {
        "supports-color": {
          "optional": true
        }
      }
    },
    "node_modules/@socket.io/redis-adapter/node_modules/ms": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.2.tgz",
      "integrity": "sha512-sGkPx+VjMtmA6MX27oA4FBFELFCZZ4S4XqeGOXCv68tT+jb3vk/RyaKWP0PTKyWtmLSM0b+adUTEvbs1PEaH2w=="
    },
    "node_modules/@types/concat-stream": {
      "version": "1.6.1",
      "resolved": "https://registry.npmjs.org/@types/concat-stream/-/concat-stream-1.6.1.tgz",
      "integrity": "sha512-eHE4cQPoj6ngxBZMvVf6Hw7Mh4jMW4U9lpGmS5GBPB9RYxlFg+CHaVN7ErNY4W9XfLIEn20b4VDYaIrbq0q4uA==",
      "dependencies": {
        "@types/node": "*"
      }
    },
    "node_modules/@types/cookie": {
      "version": "0.4.1",
      "resolved": "https://registry.npmjs.org/@types/cookie/-/cookie-0.4.1.tgz",
      "integrity": "sha512-XW/Aa8APYr6jSVVA1y/DEIZX0/GMKLEVekNG727R8cs56ahETkRAy/3DR7+fJyh7oUgGwNQaRfXCun0+KbWY7Q=="
    },
    "node_modules/@types/cors": {
      "version": "2.8.13",
      "resolved": "https://registry.npmjs.org/@types/cors/-/cors-2.8.13.tgz",
      "integrity": "sha512-RG8AStHlUiV5ysZQKq97copd2UmVYw3/pRMLefISZ3S1hK104Cwm7iLQ3fTKx+lsUH2CE8FlLaYeEA2LSeqYUA==",
      "dependencies": {
        "@types/node": "*"
      }
    },
    "node_modules/@types/debug": {
      "version": "4.1.8",
      "resolved": "https://registry.npmjs.org/@types/debug/-/debug-4.1.8.tgz",
      "integrity": "sha512-/vPO1EPOs306Cvhwv7KfVfYvOJqA/S/AXjaHQiJboCZzcNDb+TIJFN9/2C9DZ//ijSKWioNyUxD792QmDJ+HKQ==",
      "dependencies": {
        "@types/ms": "*"
      }
    },
    "node_modules/@types/form-data": {
      "version": "0.0.33",
      "resolved": "https://registry.npmjs.org/@types/form-data/-/form-data-0.0.33.tgz",
      "integrity": "sha512-8BSvG1kGm83cyJITQMZSulnl6QV8jqAGreJsc5tPu1Jq0vTSOiY/k24Wx82JRpWwZSqrala6sd5rWi6aNXvqcw==",
      "dependencies": {
        "@types/node": "*"
      }
    },
    "node_modules/@types/ms": {
      "version": "0.7.31",
      "resolved": "https://registry.npmjs.org/@types/ms/-/ms-0.7.31.tgz",
      "integrity": "sha512-iiUgKzV9AuaEkZqkOLDIvlQiL6ltuZd9tGcW3gwpnX8JbuiuhFlEGmmFXEXkN50Cvq7Os88IY2v0dkDqXYWVgA=="
    },
    "node_modules/@types/node": {
      "version": "20.4.5",
      "resolved": "https://registry.npmjs.org/@types/node/-/node-20.4.5.tgz",
      "integrity": "sha512-rt40Nk13II9JwQBdeYqmbn2Q6IVTA5uPhvSO+JVqdXw/6/4glI6oR9ezty/A9Hg5u7JH4OmYmuQ+XvjKm0Datg=="
    },
    "node_modules/@types/qs": {
      "version": "6.9.11",
      "resolved": "https://registry.npmjs.org/@types/qs/-/qs-6.9.11.tgz",
      "integrity": "sha512-oGk0gmhnEJK4Yyk+oI7EfXsLayXatCWPHary1MtcmbAifkobT9cM9yutG/hZKIseOU0MqbIwQ/u2nn/Gb+ltuQ=="
    },
    "node_modules/@types/validator": {
      "version": "13.7.17",
      "resolved": "https://registry.npmjs.org/@types/validator/-/validator-13.7.17.tgz",
      "integrity": "sha512-aqayTNmeWrZcvnG2MG9eGYI6b7S5fl+yKgPs6bAjOTwPS316R5SxBGKvtSExfyoJU7pIeHJfsHI0Ji41RVMkvQ=="
    },
    "node_modules/abbrev": {
      "version": "1.1.1",
      "resolved": "https://registry.npmjs.org/abbrev/-/abbrev-1.1.1.tgz",
      "integrity": "sha512-nne9/IiQ/hzIhY6pdDnbBtz7DjPTKrY00P/zvPSm5pOFkl6xuGrGnXn/VtTNNfNtAfZ9/1RtehkszU9qcTii0Q=="
    },
    "node_modules/accepts": {
      "version": "1.3.8",
      "resolved": "https://registry.npmjs.org/accepts/-/accepts-1.3.8.tgz",
      "integrity": "sha512-PYAthTa2m2VKxuvSD3DPC/Gy+U+sOA1LAuT8mkmRuvw+NACSaeXEQ+NHcVF7rONl6qcaxV3Uuemwawk+7+SJLw==",
      "dependencies": {
        "mime-types": "~2.1.34",
        "negotiator": "0.6.3"
      },
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/agent-base": {
      "version": "6.0.2",
      "resolved": "https://registry.npmjs.org/agent-base/-/agent-base-6.0.2.tgz",
      "integrity": "sha512-RZNwNclF7+MS/8bDg70amg32dyeZGZxiDuQmZxKLAlQjr3jGyLx+4Kkk58UO7D2QdgFIQCovuSuZESne6RG6XQ==",
      "dependencies": {
        "debug": "4"
      },
      "engines": {
        "node": ">= 6.0.0"
      }
    },
    "node_modules/agent-base/node_modules/debug": {
      "version": "4.3.4",
      "resolved": "https://registry.npmjs.org/debug/-/debug-4.3.4.tgz",
      "integrity": "sha512-PRWFHuSU3eDtQJPvnNY7Jcket1j0t5OuOsFzPPzsekD52Zl8qUfFIPEiswXqIvHWGVHOgX+7G/vCNNhehwxfkQ==",
      "dependencies": {
        "ms": "2.1.2"
      },
      "engines": {
        "node": ">=6.0"
      },
      "peerDependenciesMeta": {
        "supports-color": {
          "optional": true
        }
      }
    },
    "node_modules/agent-base/node_modules/ms": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.2.tgz",
      "integrity": "sha512-sGkPx+VjMtmA6MX27oA4FBFELFCZZ4S4XqeGOXCv68tT+jb3vk/RyaKWP0PTKyWtmLSM0b+adUTEvbs1PEaH2w=="
    },
    "node_modules/ansi-regex": {
      "version": "5.0.1",
      "resolved": "https://registry.npmjs.org/ansi-regex/-/ansi-regex-5.0.1.tgz",
      "integrity": "sha512-quJQXlTSUGL2LH9SUXo8VwsY4soanhgo6LNSm84E1LBcE8s3O0wpdiRzyR9z/ZZJMlMWv37qOOb9pdJlMUEKFQ==",
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/ansi-styles": {
      "version": "4.3.0",
      "resolved": "https://registry.npmjs.org/ansi-styles/-/ansi-styles-4.3.0.tgz",
      "integrity": "sha512-zbB9rCJAT1rbjiVDb2hqKFHNYLxgtk8NURxZ3IZwD3F6NtxbXZQCnnSi1Lkx+IDohdPlFp222wVALIheZJQSEg==",
      "dev": true,
      "dependencies": {
        "color-convert": "^2.0.1"
      },
      "engines": {
        "node": ">=8"
      },
      "funding": {
        "url": "https://github.com/chalk/ansi-styles?sponsor=1"
      }
    },
    "node_modules/anymatch": {
      "version": "3.1.3",
      "resolved": "https://registry.npmjs.org/anymatch/-/anymatch-3.1.3.tgz",
      "integrity": "sha512-KMReFUr0B4t+D+OBkjR3KYqvocp2XaSzO55UcB6mgQMd3KbcE+mWTyvVV7D/zsdEbNnV6acZUutkiHQXvTr1Rw==",
      "dev": true,
      "dependencies": {
        "normalize-path": "^3.0.0",
        "picomatch": "^2.0.4"
      },
      "engines": {
        "node": ">= 8"
      }
    },
    "node_modules/aproba": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/aproba/-/aproba-2.0.0.tgz",
      "integrity": "sha512-lYe4Gx7QT+MKGbDsA+Z+he/Wtef0BiwDOlK/XkBrdfsh9J/jPPXbX0tE9x9cl27Tmu5gg3QUbUrQYa/y+KOHPQ=="
    },
    "node_modules/are-we-there-yet": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/are-we-there-yet/-/are-we-there-yet-2.0.0.tgz",
      "integrity": "sha512-Ci/qENmwHnsYo9xKIcUJN5LeDKdJ6R1Z1j9V/J5wyq8nh/mYPEpIKJbBZXtZjG04HiK7zV/p6Vs9952MrMeUIw==",
      "dependencies": {
        "delegates": "^1.0.0",
        "readable-stream": "^3.6.0"
      },
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/array-flatten": {
      "version": "1.1.1",
      "resolved": "https://registry.npmjs.org/array-flatten/-/array-flatten-1.1.1.tgz",
      "integrity": "sha512-PCVAQswWemu6UdxsDFFX/+gVeYqKAod3D3UVm91jHwynguOwAvYPhx8nNlM++NqRcK6CxxpUafjmhIdKiHibqg=="
    },
    "node_modules/asap": {
      "version": "2.0.6",
      "resolved": "https://registry.npmjs.org/asap/-/asap-2.0.6.tgz",
      "integrity": "sha512-BSHWgDSAiKs50o2Re8ppvp3seVHXSRM44cdSsT9FfNEUUZLOGWVCsiWaRPWM1Znn+mqZ1OfVZ3z3DWEzSp7hRA=="
    },
    "node_modules/asynckit": {
      "version": "0.4.0",
      "resolved": "https://registry.npmjs.org/asynckit/-/asynckit-0.4.0.tgz",
      "integrity": "sha512-Oei9OH4tRh0YqU3GxhX79dM/mwVgvbZJaSNaRk+bshkj0S5cfHcgYakreBjrHwatXKbz+IoIdYLxrKim2MjW0Q=="
    },
    "node_modules/at-least-node": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/at-least-node/-/at-least-node-1.0.0.tgz",
      "integrity": "sha512-+q/t7Ekv1EDY2l6Gda6LLiX14rU9TV20Wa3ofeQmwPFZbOMo9DXrLbOjFaaclkXKWidIaopwAObQDqwWtGUjqg==",
      "dev": true,
      "engines": {
        "node": ">= 4.0.0"
      }
    },
    "node_modules/available-typed-arrays": {
      "version": "1.0.5",
      "resolved": "https://registry.npmjs.org/available-typed-arrays/-/available-typed-arrays-1.0.5.tgz",
      "integrity": "sha512-DMD0KiN46eipeziST1LPP/STfDU0sufISXmjSgvVsoU2tqxctQeASejWcfNtxYKqETM1UxQ8sp2OrSBWpHY6sw==",
      "engines": {
        "node": ">= 0.4"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/aws-sdk": {
      "version": "2.1446.0",
      "resolved": "https://registry.npmjs.org/aws-sdk/-/aws-sdk-2.1446.0.tgz",
      "integrity": "sha512-QaIyQz9csPSgujM+asHNWHh6uw1FDh+SxpUERLbePDYwqycQha/0BkOxTciGh/Jhp26tKMnHL7rwrYl37H6RYA==",
      "dependencies": {
        "buffer": "4.9.2",
        "events": "1.1.1",
        "ieee754": "1.1.13",
        "jmespath": "0.16.0",
        "querystring": "0.2.0",
        "sax": "1.2.1",
        "url": "0.10.3",
        "util": "^0.12.4",
        "uuid": "8.0.0",
        "xml2js": "0.5.0"
      },
      "engines": {
        "node": ">= 10.0.0"
      }
    },
    "node_modules/aws-sdk/node_modules/uuid": {
      "version": "8.0.0",
      "resolved": "https://registry.npmjs.org/uuid/-/uuid-8.0.0.tgz",
      "integrity": "sha512-jOXGuXZAWdsTH7eZLtyXMqUb9EcWMGZNbL9YcGBJl4MH4nrxHmZJhEHvyLFrkxo+28uLb/NYRcStH48fnD0Vzw==",
      "bin": {
        "uuid": "dist/bin/uuid"
      }
    },
    "node_modules/balanced-match": {
      "version": "1.0.2",
      "resolved": "https://registry.npmjs.org/balanced-match/-/balanced-match-1.0.2.tgz",
      "integrity": "sha512-3oSeUO0TMV67hN1AmbXsK4yaqU7tjiHlbxRDZOpH0KW9+CeX4bRAaX0Anxt0tx2MrpRpWwQaPwIlISEJhYU5Pw=="
    },
    "node_modules/base64-js": {
      "version": "1.5.1",
      "resolved": "https://registry.npmjs.org/base64-js/-/base64-js-1.5.1.tgz",
      "integrity": "sha512-AKpaYlHn8t4SVbOHCy+b5+KKgvR4vrsD8vbvrbiQJps7fKDTkjkDry6ji0rUJjC0kzbNePLwzxq8iypo41qeWA==",
      "funding": [
        {
          "type": "github",
          "url": "https://github.com/sponsors/feross"
        },
        {
          "type": "patreon",
          "url": "https://www.patreon.com/feross"
        },
        {
          "type": "consulting",
          "url": "https://feross.org/support"
        }
      ]
    },
    "node_modules/base64id": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/base64id/-/base64id-2.0.0.tgz",
      "integrity": "sha512-lGe34o6EHj9y3Kts9R4ZYs/Gr+6N7MCaMlIFA3F1R2O5/m7K06AxfSeO5530PEERE6/WyEg3lsuyw4GHlPZHog==",
      "engines": {
        "node": "^4.5.0 || >= 5.9"
      }
    },
    "node_modules/bcrypt": {
      "version": "5.1.0",
      "resolved": "https://registry.npmjs.org/bcrypt/-/bcrypt-5.1.0.tgz",
      "integrity": "sha512-RHBS7HI5N5tEnGTmtR/pppX0mmDSBpQ4aCBsj7CEQfYXDcO74A8sIBYcJMuCsis2E81zDxeENYhv66oZwLiA+Q==",
      "hasInstallScript": true,
      "dependencies": {
        "@mapbox/node-pre-gyp": "^1.0.10",
        "node-addon-api": "^5.0.0"
      },
      "engines": {
        "node": ">= 10.0.0"
      }
    },
    "node_modules/bignumber.js": {
      "version": "9.0.0",
      "resolved": "https://registry.npmjs.org/bignumber.js/-/bignumber.js-9.0.0.tgz",
      "integrity": "sha512-t/OYhhJ2SD+YGBQcjY8GzzDHEk9f3nerxjtfa6tlMXfe7frs/WozhvCNoGvpM0P3bNf3Gq5ZRMlGr5f3r4/N8A==",
      "engines": {
        "node": "*"
      }
    },
    "node_modules/binary-extensions": {
      "version": "2.2.0",
      "resolved": "https://registry.npmjs.org/binary-extensions/-/binary-extensions-2.2.0.tgz",
      "integrity": "sha512-jDctJ/IVQbZoJykoeHbhXpOlNBqGNcwXJKJog42E5HDPUwQTSdjCHdihjj0DlnheQ7blbT6dHOafNAiS8ooQKA==",
      "dev": true,
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/bluebird": {
      "version": "3.7.2",
      "resolved": "https://registry.npmjs.org/bluebird/-/bluebird-3.7.2.tgz",
      "integrity": "sha512-XpNj6GDQzdfW+r2Wnn7xiSAd7TM3jzkxGXBGTtWKuSXv1xUV+azxAm8jdWZN06QTQk+2N2XB9jRDkvbmQmcRtg==",
      "dev": true
    },
    "node_modules/body-parser": {
      "version": "1.20.2",
      "resolved": "https://registry.npmjs.org/body-parser/-/body-parser-1.20.2.tgz",
      "integrity": "sha512-ml9pReCu3M61kGlqoTm2umSXTlRTuGTx0bfYj+uIUKKYycG5NtSbeetV3faSU6R7ajOPw0g/J1PvK4qNy7s5bA==",
      "dependencies": {
        "bytes": "3.1.2",
        "content-type": "~1.0.5",
        "debug": "2.6.9",
        "depd": "2.0.0",
        "destroy": "1.2.0",
        "http-errors": "2.0.0",
        "iconv-lite": "0.4.24",
        "on-finished": "2.4.1",
        "qs": "6.11.0",
        "raw-body": "2.5.2",
        "type-is": "~1.6.18",
        "unpipe": "1.0.0"
      },
      "engines": {
        "node": ">= 0.8",
        "npm": "1.2.8000 || >= 1.4.16"
      }
    },
    "node_modules/brace-expansion": {
      "version": "1.1.11",
      "resolved": "https://registry.npmjs.org/brace-expansion/-/brace-expansion-1.1.11.tgz",
      "integrity": "sha512-iCuPHDFgrHX7H2vEI/5xpz07zSHB00TpugqhmYtVmMO6518mCuRMoOYFldEBl0g187ufozdaHgWKcYFb61qGiA==",
      "dependencies": {
        "balanced-match": "^1.0.0",
        "concat-map": "0.0.1"
      }
    },
    "node_modules/braces": {
      "version": "3.0.2",
      "resolved": "https://registry.npmjs.org/braces/-/braces-3.0.2.tgz",
      "integrity": "sha512-b8um+L1RzM3WDSzvhm6gIz1yfTbBt6YTlcEKAvsmqCZZFw46z626lVj9j1yEPW33H5H+lBQpZMP1k8l+78Ha0A==",
      "dev": true,
      "dependencies": {
        "fill-range": "^7.0.1"
      },
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/buffer": {
      "version": "4.9.2",
      "resolved": "https://registry.npmjs.org/buffer/-/buffer-4.9.2.tgz",
      "integrity": "sha512-xq+q3SRMOxGivLhBNaUdC64hDTQwejJ+H0T/NB1XMtTVEwNTrfFF3gAxiyW0Bu/xWEGhjVKgUcMhCrUy2+uCWg==",
      "dependencies": {
        "base64-js": "^1.0.2",
        "ieee754": "^1.1.4",
        "isarray": "^1.0.0"
      }
    },
    "node_modules/buffer-equal-constant-time": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/buffer-equal-constant-time/-/buffer-equal-constant-time-1.0.1.tgz",
      "integrity": "sha512-zRpUiDwd/xk6ADqPMATG8vc9VPrkck7T07OIx0gnjmJAnHnTVXNQG3vfvWNuiZIkwu9KrKdA1iJKfsfTVxE6NA=="
    },
    "node_modules/buffer-from": {
      "version": "1.1.2",
      "resolved": "https://registry.npmjs.org/buffer-from/-/buffer-from-1.1.2.tgz",
      "integrity": "sha512-E+XQCRwSbaaiChtv6k6Dwgc+bx+Bs6vuKJHHl5kox/BaKbhiXzqQOwK4cO22yElGp2OCmjwVhT3HmxgyPGnJfQ=="
    },
    "node_modules/buffer-writer": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/buffer-writer/-/buffer-writer-2.0.0.tgz",
      "integrity": "sha512-a7ZpuTZU1TRtnwyCNW3I5dc0wWNC3VR9S++Ewyk2HHZdrO3CQJqSpd+95Us590V6AL7JqUAH2IwZ/398PmNFgw==",
      "engines": {
        "node": ">=4"
      }
    },
    "node_modules/busboy": {
      "version": "1.6.0",
      "resolved": "https://registry.npmjs.org/busboy/-/busboy-1.6.0.tgz",
      "integrity": "sha512-8SFQbg/0hQ9xy3UNTB0YEnsNBbWfhf7RtnzpL7TkBiTBRfrQ9Fxcnz7VJsleJpyp6rVLvXiuORqjlHi5q+PYuA==",
      "dependencies": {
        "streamsearch": "^1.1.0"
      },
      "engines": {
        "node": ">=10.16.0"
      }
    },
    "node_modules/bytes": {
      "version": "3.1.2",
      "resolved": "https://registry.npmjs.org/bytes/-/bytes-3.1.2.tgz",
      "integrity": "sha512-/Nf7TyzTx6S3yRJObOAV7956r8cr2+Oj8AC5dt8wSP3BQAoeX58NoHyCU8P8zGkNXStjTSi6fzO6F0pBdcYbEg==",
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/call-bind": {
      "version": "1.0.2",
      "resolved": "https://registry.npmjs.org/call-bind/-/call-bind-1.0.2.tgz",
      "integrity": "sha512-7O+FbCihrB5WGbFYesctwmTKae6rOiIzmz1icreWJ+0aA7LJfuqhEso2T9ncpcFtzMQtzXf2QGGueWJGTYsqrA==",
      "dependencies": {
        "function-bind": "^1.1.1",
        "get-intrinsic": "^1.0.2"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/caseless": {
      "version": "0.12.0",
      "resolved": "https://registry.npmjs.org/caseless/-/caseless-0.12.0.tgz",
      "integrity": "sha512-4tYFyifaFfGacoiObjJegolkwSU4xQNGbVgUiNYVUxbQ2x2lUsFvY4hVgVzGiIe6WLOPqycWXA40l+PWsxthUw=="
    },
    "node_modules/chokidar": {
      "version": "3.5.3",
      "resolved": "https://registry.npmjs.org/chokidar/-/chokidar-3.5.3.tgz",
      "integrity": "sha512-Dr3sfKRP6oTcjf2JmUmFJfeVMvXBdegxB0iVQ5eb2V10uFJUCAS8OByZdVAyVb8xXNz3GjjTgj9kLWsZTqE6kw==",
      "dev": true,
      "funding": [
        {
          "type": "individual",
          "url": "https://paulmillr.com/funding/"
        }
      ],
      "dependencies": {
        "anymatch": "~3.1.2",
        "braces": "~3.0.2",
        "glob-parent": "~5.1.2",
        "is-binary-path": "~2.1.0",
        "is-glob": "~4.0.1",
        "normalize-path": "~3.0.0",
        "readdirp": "~3.6.0"
      },
      "engines": {
        "node": ">= 8.10.0"
      },
      "optionalDependencies": {
        "fsevents": "~2.3.2"
      }
    },
    "node_modules/chownr": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/chownr/-/chownr-2.0.0.tgz",
      "integrity": "sha512-bIomtDF5KGpdogkLd9VspvFzk9KfpyyGlS8YFVZl7TGPBHL5snIOnxeshwVgPteQ9b4Eydl+pVbIyE1DcvCWgQ==",
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/cli-color": {
      "version": "2.0.3",
      "resolved": "https://registry.npmjs.org/cli-color/-/cli-color-2.0.3.tgz",
      "integrity": "sha512-OkoZnxyC4ERN3zLzZaY9Emb7f/MhBOIpePv0Ycok0fJYT+Ouo00UBEIwsVsr0yoow++n5YWlSUgST9GKhNHiRQ==",
      "dev": true,
      "dependencies": {
        "d": "^1.0.1",
        "es5-ext": "^0.10.61",
        "es6-iterator": "^2.0.3",
        "memoizee": "^0.4.15",
        "timers-ext": "^0.1.7"
      },
      "engines": {
        "node": ">=0.10"
      }
    },
    "node_modules/cliui": {
      "version": "7.0.4",
      "resolved": "https://registry.npmjs.org/cliui/-/cliui-7.0.4.tgz",
      "integrity": "sha512-OcRE68cOsVMXp1Yvonl/fzkQOyjLSu/8bhPDfQt0e0/Eb283TKP20Fs2MqoPsr9SwA595rRCA+QMzYc9nBP+JQ==",
      "dev": true,
      "dependencies": {
        "string-width": "^4.2.0",
        "strip-ansi": "^6.0.0",
        "wrap-ansi": "^7.0.0"
      }
    },
    "node_modules/cluster-key-slot": {
      "version": "1.1.2",
      "resolved": "https://registry.npmjs.org/cluster-key-slot/-/cluster-key-slot-1.1.2.tgz",
      "integrity": "sha512-RMr0FhtfXemyinomL4hrWcYJxmX6deFdCxpJzhDttxgO1+bcCnkk+9drydLVDmAMG7NE6aN/fl4F7ucU/90gAA==",
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/color-convert": {
      "version": "2.0.1",
      "resolved": "https://registry.npmjs.org/color-convert/-/color-convert-2.0.1.tgz",
      "integrity": "sha512-RRECPsj7iu/xb5oKYcsFHSppFNnsj/52OVTRKb4zP5onXwVF3zVmmToNcOfGC+CRDpfK/U584fMg38ZHCaElKQ==",
      "dev": true,
      "dependencies": {
        "color-name": "~1.1.4"
      },
      "engines": {
        "node": ">=7.0.0"
      }
    },
    "node_modules/color-name": {
      "version": "1.1.4",
      "resolved": "https://registry.npmjs.org/color-name/-/color-name-1.1.4.tgz",
      "integrity": "sha512-dOy+3AuW3a2wNbZHIuMZpTcgjGuLU/uBL/ubcZF9OXbDo8ff4O8yVp5Bf0efS8uEoYo5q4Fx7dY9OgQGXgAsQA==",
      "dev": true
    },
    "node_modules/color-support": {
      "version": "1.1.3",
      "resolved": "https://registry.npmjs.org/color-support/-/color-support-1.1.3.tgz",
      "integrity": "sha512-qiBjkpbMLO/HL68y+lh4q0/O1MZFj2RX6X/KmMa3+gJD3z+WwI1ZzDHysvqHGS3mP6mznPckpXmw1nI9cJjyRg==",
      "bin": {
        "color-support": "bin.js"
      }
    },
    "node_modules/combined-stream": {
      "version": "1.0.8",
      "resolved": "https://registry.npmjs.org/combined-stream/-/combined-stream-1.0.8.tgz",
      "integrity": "sha512-FQN4MRfuJeHf7cBbBMJFXhKSDq+2kAArBlmRBvcvFE5BB1HZKXtSFASDhdlz9zOYwxh8lDdnvmMOe/+5cdoEdg==",
      "dependencies": {
        "delayed-stream": "~1.0.0"
      },
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/commander": {
      "version": "10.0.1",
      "resolved": "https://registry.npmjs.org/commander/-/commander-10.0.1.tgz",
      "integrity": "sha512-y4Mg2tXshplEbSGzx7amzPwKKOCGuoSRP/CjEdwwk0FOGlUbq6lKuoyDZTNZkmxHdJtp54hdfY/JUrdL7Xfdug==",
      "dev": true,
      "engines": {
        "node": ">=14"
      }
    },
    "node_modules/concat-map": {
      "version": "0.0.1",
      "resolved": "https://registry.npmjs.org/concat-map/-/concat-map-0.0.1.tgz",
      "integrity": "sha512-/Srv4dswyQNBfohGpz9o6Yb3Gz3SrUDqBH5rTuhGR7ahtlbYKnVxw2bCFMRljaA7EXHaXZ8wsHdodFvbkhKmqg=="
    },
    "node_modules/concat-stream": {
      "version": "1.6.2",
      "resolved": "https://registry.npmjs.org/concat-stream/-/concat-stream-1.6.2.tgz",
      "integrity": "sha512-27HBghJxjiZtIk3Ycvn/4kbJk/1uZuJFfuPEns6LaEvpvG1f0hTea8lilrouyo9mVc2GWdcEZ8OLoGmSADlrCw==",
      "engines": [
        "node >= 0.8"
      ],
      "dependencies": {
        "buffer-from": "^1.0.0",
        "inherits": "^2.0.3",
        "readable-stream": "^2.2.2",
        "typedarray": "^0.0.6"
      }
    },
    "node_modules/concat-stream/node_modules/readable-stream": {
      "version": "2.3.8",
      "resolved": "https://registry.npmjs.org/readable-stream/-/readable-stream-2.3.8.tgz",
      "integrity": "sha512-8p0AUk4XODgIewSi0l8Epjs+EVnWiK7NoDIEGU0HhE7+ZyY8D1IMY7odu5lRrFXGg71L15KG8QrPmum45RTtdA==",
      "dependencies": {
        "core-util-is": "~1.0.0",
        "inherits": "~2.0.3",
        "isarray": "~1.0.0",
        "process-nextick-args": "~2.0.0",
        "safe-buffer": "~5.1.1",
        "string_decoder": "~1.1.1",
        "util-deprecate": "~1.0.1"
      }
    },
    "node_modules/concat-stream/node_modules/safe-buffer": {
      "version": "5.1.2",
      "resolved": "https://registry.npmjs.org/safe-buffer/-/safe-buffer-5.1.2.tgz",
      "integrity": "sha512-Gd2UZBJDkXlY7GbJxfsE8/nvKkUEU1G38c1siN6QP6a9PT9MmHB8GnpscSmMJSoF8LOIrt8ud/wPtojys4G6+g=="
    },
    "node_modules/concat-stream/node_modules/string_decoder": {
      "version": "1.1.1",
      "resolved": "https://registry.npmjs.org/string_decoder/-/string_decoder-1.1.1.tgz",
      "integrity": "sha512-n/ShnvDi6FHbbVfviro+WojiFzv+s8MPMHBczVePfUpDJLwoLT0ht1l4YwBCbi8pJAveEEdnkHyPyTP/mzRfwg==",
      "dependencies": {
        "safe-buffer": "~5.1.0"
      }
    },
    "node_modules/config-chain": {
      "version": "1.1.13",
      "resolved": "https://registry.npmjs.org/config-chain/-/config-chain-1.1.13.tgz",
      "integrity": "sha512-qj+f8APARXHrM0hraqXYb2/bOVSV4PvJQlNZ/DVj0QrmNM2q2euizkeuVckQ57J+W0mRH6Hvi+k50M4Jul2VRQ==",
      "dev": true,
      "dependencies": {
        "ini": "^1.3.4",
        "proto-list": "~1.2.1"
      }
    },
    "node_modules/console-control-strings": {
      "version": "1.1.0",
      "resolved": "https://registry.npmjs.org/console-control-strings/-/console-control-strings-1.1.0.tgz",
      "integrity": "sha512-ty/fTekppD2fIwRvnZAVdeOiGd1c7YXEixbgJTNzqcxJWKQnjJ/V1bNEEE6hygpM3WjwHFUVK6HTjWSzV4a8sQ=="
    },
    "node_modules/content-disposition": {
      "version": "0.5.4",
      "resolved": "https://registry.npmjs.org/content-disposition/-/content-disposition-0.5.4.tgz",
      "integrity": "sha512-FveZTNuGw04cxlAiWbzi6zTAL/lhehaWbTtgluJh4/E95DqMwTmha3KZN1aAWA8cFIhHzMZUvLevkw5Rqk+tSQ==",
      "dependencies": {
        "safe-buffer": "5.2.1"
      },
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/content-type": {
      "version": "1.0.5",
      "resolved": "https://registry.npmjs.org/content-type/-/content-type-1.0.5.tgz",
      "integrity": "sha512-nTjqfcBFEipKdXCv4YDQWCfmcLZKm81ldF0pAopTvyrFGVbcR6P/VAAd5G7N+0tTr8QqiU0tFadD6FK4NtJwOA==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/cookie": {
      "version": "0.5.0",
      "resolved": "https://registry.npmjs.org/cookie/-/cookie-0.5.0.tgz",
      "integrity": "sha512-YZ3GUyn/o8gfKJlnlX7g7xq4gyO6OSuhGPKaaGssGB2qgDUS0gPgtTvoyZLTt9Ab6dC4hfc9dV5arkvc/OCmrw==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/cookie-parser": {
      "version": "1.4.6",
      "resolved": "https://registry.npmjs.org/cookie-parser/-/cookie-parser-1.4.6.tgz",
      "integrity": "sha512-z3IzaNjdwUC2olLIB5/ITd0/setiaFMLYiZJle7xg5Fe9KWAceil7xszYfHHBtDFYLSgJduS2Ty0P1uJdPDJeA==",
      "dependencies": {
        "cookie": "0.4.1",
        "cookie-signature": "1.0.6"
      },
      "engines": {
        "node": ">= 0.8.0"
      }
    },
    "node_modules/cookie-parser/node_modules/cookie": {
      "version": "0.4.1",
      "resolved": "https://registry.npmjs.org/cookie/-/cookie-0.4.1.tgz",
      "integrity": "sha512-ZwrFkGJxUR3EIoXtO+yVE69Eb7KlixbaeAWfBQB9vVsNn/o+Yw69gBWSSDK825hQNdN+wF8zELf3dFNl/kxkUA==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/cookie-signature": {
      "version": "1.0.6",
      "resolved": "https://registry.npmjs.org/cookie-signature/-/cookie-signature-1.0.6.tgz",
      "integrity": "sha512-QADzlaHc8icV8I7vbaJXJwod9HWYp8uCqf1xa4OfNu1T7JVxQIrUgOWtHdNDtPiywmFbiS12VjotIXLrKM3orQ=="
    },
    "node_modules/core-util-is": {
      "version": "1.0.3",
      "resolved": "https://registry.npmjs.org/core-util-is/-/core-util-is-1.0.3.tgz",
      "integrity": "sha512-ZQBvi1DcpJ4GDqanjucZ2Hj3wEO5pZDS89BWbkcrvdxksJorwUDDZamX9ldFkp9aw2lmBDLgkObEA4DWNJ9FYQ=="
    },
    "node_modules/cors": {
      "version": "2.8.5",
      "resolved": "https://registry.npmjs.org/cors/-/cors-2.8.5.tgz",
      "integrity": "sha512-KIHbLJqu73RGr/hnbrO9uBeixNGuvSQjul/jdFvS/KFSIH1hWVd1ng7zOHx+YrEfInLG7q4n6GHQ9cDtxv/P6g==",
      "dependencies": {
        "object-assign": "^4",
        "vary": "^1"
      },
      "engines": {
        "node": ">= 0.10"
      }
    },
    "node_modules/cron-parser": {
      "version": "4.9.0",
      "resolved": "https://registry.npmjs.org/cron-parser/-/cron-parser-4.9.0.tgz",
      "integrity": "sha512-p0SaNjrHOnQeR8/VnfGbmg9te2kfyYSQ7Sc/j/6DtPL3JQvKxmjO9TSjNFpujqV3vEYYBvNNvXSxzyksBWAx1Q==",
      "dependencies": {
        "luxon": "^3.2.1"
      },
      "engines": {
        "node": ">=12.0.0"
      }
    },
    "node_modules/crypto-js": {
      "version": "4.1.1",
      "resolved": "https://registry.npmjs.org/crypto-js/-/crypto-js-4.1.1.tgz",
      "integrity": "sha512-o2JlM7ydqd3Qk9CA0L4NL6mTzU2sdx96a+oOfPu8Mkl/PK51vSyoi8/rQ8NknZtk44vq15lmhAj9CIAGwgeWKw=="
    },
    "node_modules/d": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/d/-/d-1.0.1.tgz",
      "integrity": "sha512-m62ShEObQ39CfralilEQRjH6oAMtNCV1xJyEx5LpRYUVN+EviphDgUc/F3hnYbADmkiNs67Y+3ylmlG7Lnu+FA==",
      "dev": true,
      "dependencies": {
        "es5-ext": "^0.10.50",
        "type": "^1.0.1"
      }
    },
    "node_modules/data-uri-to-buffer": {
      "version": "4.0.1",
      "resolved": "https://registry.npmjs.org/data-uri-to-buffer/-/data-uri-to-buffer-4.0.1.tgz",
      "integrity": "sha512-0R9ikRb668HB7QDxT1vkpuUBtqc53YyAwMwGeUFKRojY/NWKvdZ+9UYtRfGmhqNbRkTSVpMbmyhXipFFv2cb/A==",
      "engines": {
        "node": ">= 12"
      }
    },
    "node_modules/dayjs": {
      "version": "1.11.9",
      "resolved": "https://registry.npmjs.org/dayjs/-/dayjs-1.11.9.tgz",
      "integrity": "sha512-QvzAURSbQ0pKdIye2txOzNaHmxtUBXerpY0FJsFXUMKbIZeFm5ht1LS/jFsrncjnmtv8HsG0W2g6c0zUjZWmpA=="
    },
    "node_modules/debug": {
      "version": "2.6.9",
      "resolved": "https://registry.npmjs.org/debug/-/debug-2.6.9.tgz",
      "integrity": "sha512-bC7ElrdJaJnPbAP+1EotYvqZsb3ecl5wi6Bfi6BJTUcNowp6cvspg0jXznRTKDjm/E7AdgFBVeAPVMNcKGsHMA==",
      "dependencies": {
        "ms": "2.0.0"
      }
    },
    "node_modules/delayed-stream": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/delayed-stream/-/delayed-stream-1.0.0.tgz",
      "integrity": "sha512-ZySD7Nf91aLB0RxL4KGrKHBXl7Eds1DAmEdcoVawXnLD7SDhpNgtuII2aAkg7a7QS41jxPSZ17p4VdGnMHk3MQ==",
      "engines": {
        "node": ">=0.4.0"
      }
    },
    "node_modules/delegates": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/delegates/-/delegates-1.0.0.tgz",
      "integrity": "sha512-bd2L678uiWATM6m5Z1VzNCErI3jiGzt6HGY8OVICs40JQq/HALfbyNJmp0UDakEY4pMMaN0Ly5om/B1VI/+xfQ=="
    },
    "node_modules/denque": {
      "version": "2.1.0",
      "resolved": "https://registry.npmjs.org/denque/-/denque-2.1.0.tgz",
      "integrity": "sha512-HVQE3AAb/pxF8fQAoiqpvg9i3evqug3hoiwakOyZAwJm+6vZehbkYXZ0l4JxS+I3QxM97v5aaRNhj8v5oBhekw==",
      "engines": {
        "node": ">=0.10"
      }
    },
    "node_modules/depd": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/depd/-/depd-2.0.0.tgz",
      "integrity": "sha512-g7nH6P6dyDioJogAAGprGpCtVImJhpPk/roCzdb3fIh61/s/nPsfR6onyMwkCAR/OlC3yBC0lESvUoQEAssIrw==",
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/destroy": {
      "version": "1.2.0",
      "resolved": "https://registry.npmjs.org/destroy/-/destroy-1.2.0.tgz",
      "integrity": "sha512-2sJGJTaXIIaR1w4iJSNoN0hnMY7Gpc/n8D4qSCJw8QqFWXf7cuAgnEHxBpweaVcPevC2l3KpjYCx3NypQQgaJg==",
      "engines": {
        "node": ">= 0.8",
        "npm": "1.2.8000 || >= 1.4.16"
      }
    },
    "node_modules/detect-libc": {
      "version": "2.0.2",
      "resolved": "https://registry.npmjs.org/detect-libc/-/detect-libc-2.0.2.tgz",
      "integrity": "sha512-UX6sGumvvqSaXgdKGUsgZWqcUyIXZ/vZTrlRT/iobiKhGL0zL4d3osHj3uqllWJK+i+sixDS/3COVEOFbupFyw==",
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/dotenv": {
      "version": "16.3.1",
      "resolved": "https://registry.npmjs.org/dotenv/-/dotenv-16.3.1.tgz",
      "integrity": "sha512-IPzF4w4/Rd94bA9imS68tZBaYyBWSCE47V1RGuMrB94iyTOIEwRmVL2x/4An+6mETpLrKJ5hQkB8W4kFAadeIQ==",
      "engines": {
        "node": ">=12"
      },
      "funding": {
        "url": "https://github.com/motdotla/dotenv?sponsor=1"
      }
    },
    "node_modules/dottie": {
      "version": "2.0.6",
      "resolved": "https://registry.npmjs.org/dottie/-/dottie-2.0.6.tgz",
      "integrity": "sha512-iGCHkfUc5kFekGiqhe8B/mdaurD+lakO9txNnTvKtA6PISrw86LgqHvRzWYPyoE2Ph5aMIrCw9/uko6XHTKCwA=="
    },
    "node_modules/ecdsa-sig-formatter": {
      "version": "1.0.11",
      "resolved": "https://registry.npmjs.org/ecdsa-sig-formatter/-/ecdsa-sig-formatter-1.0.11.tgz",
      "integrity": "sha512-nagl3RYrbNv6kQkeJIpt6NJZy8twLB/2vtz6yN9Z4vRKHN4/QZJIEbqohALSgwKdnksuY3k5Addp5lg8sVoVcQ==",
      "dependencies": {
        "safe-buffer": "^5.0.1"
      }
    },
    "node_modules/editorconfig": {
      "version": "1.0.4",
      "resolved": "https://registry.npmjs.org/editorconfig/-/editorconfig-1.0.4.tgz",
      "integrity": "sha512-L9Qe08KWTlqYMVvMcTIvMAdl1cDUubzRNYL+WfA4bLDMHe4nemKkpmYzkznE1FwLKu0EEmy6obgQKzMJrg4x9Q==",
      "dev": true,
      "dependencies": {
        "@one-ini/wasm": "0.1.1",
        "commander": "^10.0.0",
        "minimatch": "9.0.1",
        "semver": "^7.5.3"
      },
      "bin": {
        "editorconfig": "bin/editorconfig"
      },
      "engines": {
        "node": ">=14"
      }
    },
    "node_modules/editorconfig/node_modules/brace-expansion": {
      "version": "2.0.1",
      "resolved": "https://registry.npmjs.org/brace-expansion/-/brace-expansion-2.0.1.tgz",
      "integrity": "sha512-XnAIvQ8eM+kC6aULx6wuQiwVsnzsi9d3WxzV3FpWTGA19F621kwdbsAcFKXgKUHZWsy+mY6iL1sHTxWEFCytDA==",
      "dev": true,
      "dependencies": {
        "balanced-match": "^1.0.0"
      }
    },
    "node_modules/editorconfig/node_modules/minimatch": {
      "version": "9.0.1",
      "resolved": "https://registry.npmjs.org/minimatch/-/minimatch-9.0.1.tgz",
      "integrity": "sha512-0jWhJpD/MdhPXwPuiRkCbfYfSKp2qnn2eOc279qI7f+osl/l+prKSrvhg157zSYvx/1nmgn2NqdT6k2Z7zSH9w==",
      "dev": true,
      "dependencies": {
        "brace-expansion": "^2.0.1"
      },
      "engines": {
        "node": ">=16 || 14 >=14.17"
      },
      "funding": {
        "url": "https://github.com/sponsors/isaacs"
      }
    },
    "node_modules/ee-first": {
      "version": "1.1.1",
      "resolved": "https://registry.npmjs.org/ee-first/-/ee-first-1.1.1.tgz",
      "integrity": "sha512-WMwm9LhRUo+WUaRN+vRuETqG89IgZphVSNkdFgeb6sS/E4OrDIN7t48CAewSHXc6C8lefD8KKfr5vY61brQlow=="
    },
    "node_modules/emoji-regex": {
      "version": "8.0.0",
      "resolved": "https://registry.npmjs.org/emoji-regex/-/emoji-regex-8.0.0.tgz",
      "integrity": "sha512-MSjYzcWNOA0ewAHpz0MxpYFvwg6yjy1NG3xteoqz644VCo/RPgnr1/GGt+ic3iJTzQ8Eu3TdM14SawnVUmGE6A=="
    },
    "node_modules/encodeurl": {
      "version": "1.0.2",
      "resolved": "https://registry.npmjs.org/encodeurl/-/encodeurl-1.0.2.tgz",
      "integrity": "sha512-TPJXq8JqFaVYm2CWmPvnP2Iyo4ZSM7/QKcSmuMLDObfpH5fi7RUGmd/rTDf+rut/saiDiQEeVTNgAmJEdAOx0w==",
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/engine.io": {
      "version": "6.5.2",
      "resolved": "https://registry.npmjs.org/engine.io/-/engine.io-6.5.2.tgz",
      "integrity": "sha512-IXsMcGpw/xRfjra46sVZVHiSWo/nJ/3g1337q9KNXtS6YRzbW5yIzTCb9DjhrBe7r3GZQR0I4+nq+4ODk5g/cA==",
      "dependencies": {
        "@types/cookie": "^0.4.1",
        "@types/cors": "^2.8.12",
        "@types/node": ">=10.0.0",
        "accepts": "~1.3.4",
        "base64id": "2.0.0",
        "cookie": "~0.4.1",
        "cors": "~2.8.5",
        "debug": "~4.3.1",
        "engine.io-parser": "~5.2.1",
        "ws": "~8.11.0"
      },
      "engines": {
        "node": ">=10.2.0"
      }
    },
    "node_modules/engine.io-parser": {
      "version": "5.2.1",
      "resolved": "https://registry.npmjs.org/engine.io-parser/-/engine.io-parser-5.2.1.tgz",
      "integrity": "sha512-9JktcM3u18nU9N2Lz3bWeBgxVgOKpw7yhRaoxQA3FUDZzzw+9WlA6p4G4u0RixNkg14fH7EfEc/RhpurtiROTQ==",
      "engines": {
        "node": ">=10.0.0"
      }
    },
    "node_modules/engine.io/node_modules/cookie": {
      "version": "0.4.2",
      "resolved": "https://registry.npmjs.org/cookie/-/cookie-0.4.2.tgz",
      "integrity": "sha512-aSWTXFzaKWkvHO1Ny/s+ePFpvKsPnjc551iI41v3ny/ow6tBG5Vd+FuqGNhh1LxOmVzOlGUriIlOaokOvhaStA==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/engine.io/node_modules/debug": {
      "version": "4.3.4",
      "resolved": "https://registry.npmjs.org/debug/-/debug-4.3.4.tgz",
      "integrity": "sha512-PRWFHuSU3eDtQJPvnNY7Jcket1j0t5OuOsFzPPzsekD52Zl8qUfFIPEiswXqIvHWGVHOgX+7G/vCNNhehwxfkQ==",
      "dependencies": {
        "ms": "2.1.2"
      },
      "engines": {
        "node": ">=6.0"
      },
      "peerDependenciesMeta": {
        "supports-color": {
          "optional": true
        }
      }
    },
    "node_modules/engine.io/node_modules/ms": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.2.tgz",
      "integrity": "sha512-sGkPx+VjMtmA6MX27oA4FBFELFCZZ4S4XqeGOXCv68tT+jb3vk/RyaKWP0PTKyWtmLSM0b+adUTEvbs1PEaH2w=="
    },
    "node_modules/es5-ext": {
      "version": "0.10.62",
      "resolved": "https://registry.npmjs.org/es5-ext/-/es5-ext-0.10.62.tgz",
      "integrity": "sha512-BHLqn0klhEpnOKSrzn/Xsz2UIW8j+cGmo9JLzr8BiUapV8hPL9+FliFqjwr9ngW7jWdnxv6eO+/LqyhJVqgrjA==",
      "dev": true,
      "hasInstallScript": true,
      "dependencies": {
        "es6-iterator": "^2.0.3",
        "es6-symbol": "^3.1.3",
        "next-tick": "^1.1.0"
      },
      "engines": {
        "node": ">=0.10"
      }
    },
    "node_modules/es6-iterator": {
      "version": "2.0.3",
      "resolved": "https://registry.npmjs.org/es6-iterator/-/es6-iterator-2.0.3.tgz",
      "integrity": "sha512-zw4SRzoUkd+cl+ZoE15A9o1oQd920Bb0iOJMQkQhl3jNc03YqVjAhG7scf9C5KWRU/R13Orf588uCC6525o02g==",
      "dev": true,
      "dependencies": {
        "d": "1",
        "es5-ext": "^0.10.35",
        "es6-symbol": "^3.1.1"
      }
    },
    "node_modules/es6-symbol": {
      "version": "3.1.3",
      "resolved": "https://registry.npmjs.org/es6-symbol/-/es6-symbol-3.1.3.tgz",
      "integrity": "sha512-NJ6Yn3FuDinBaBRWl/q5X/s4koRHBrgKAu+yGI6JCBeiu3qrcbJhwT2GeR/EXVfylRk8dpQVJoLEFhK+Mu31NA==",
      "dev": true,
      "dependencies": {
        "d": "^1.0.1",
        "ext": "^1.1.2"
      }
    },
    "node_modules/es6-weak-map": {
      "version": "2.0.3",
      "resolved": "https://registry.npmjs.org/es6-weak-map/-/es6-weak-map-2.0.3.tgz",
      "integrity": "sha512-p5um32HOTO1kP+w7PRnB+5lQ43Z6muuMuIMffvDN8ZB4GcnjLBV6zGStpbASIMk4DCAvEaamhe2zhyCb/QXXsA==",
      "dev": true,
      "dependencies": {
        "d": "1",
        "es5-ext": "^0.10.46",
        "es6-iterator": "^2.0.3",
        "es6-symbol": "^3.1.1"
      }
    },
    "node_modules/escalade": {
      "version": "3.1.1",
      "resolved": "https://registry.npmjs.org/escalade/-/escalade-3.1.1.tgz",
      "integrity": "sha512-k0er2gUkLf8O0zKJiAhmkTnJlTvINGv7ygDNPbeIsX/TJjGJZHuh9B2UxbsaEkmlEo9MfhrSzmhIlhRlI2GXnw==",
      "dev": true,
      "engines": {
        "node": ">=6"
      }
    },
    "node_modules/escape-html": {
      "version": "1.0.3",
      "resolved": "https://registry.npmjs.org/escape-html/-/escape-html-1.0.3.tgz",
      "integrity": "sha512-NiSupZ4OeuGwr68lGIeym/ksIZMJodUGOSCZ/FSnTxcrekbvqrgdUxlJOMpijaKZVjAJrWrGs/6Jy8OMuyj9ow=="
    },
    "node_modules/etag": {
      "version": "1.8.1",
      "resolved": "https://registry.npmjs.org/etag/-/etag-1.8.1.tgz",
      "integrity": "sha512-aIL5Fx7mawVa300al2BnEE4iNvo1qETxLrPI/o05L7z6go7fCw1J6EQmbK4FmJ2AS7kgVF/KEZWufBfdClMcPg==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/event-emitter": {
      "version": "0.3.5",
      "resolved": "https://registry.npmjs.org/event-emitter/-/event-emitter-0.3.5.tgz",
      "integrity": "sha512-D9rRn9y7kLPnJ+hMq7S/nhvoKwwvVJahBi2BPmx3bvbsEdK3W9ii8cBSGjP+72/LnM4n6fo3+dkCX5FeTQruXA==",
      "dev": true,
      "dependencies": {
        "d": "1",
        "es5-ext": "~0.10.14"
      }
    },
    "node_modules/events": {
      "version": "1.1.1",
      "resolved": "https://registry.npmjs.org/events/-/events-1.1.1.tgz",
      "integrity": "sha512-kEcvvCBByWXGnZy6JUlgAp2gBIUjfCAV6P6TgT1/aaQKcmuAEC4OZTV1I4EWQLz2gxZw76atuVyvHhTxvi0Flw==",
      "engines": {
        "node": ">=0.4.x"
      }
    },
    "node_modules/express": {
      "version": "4.18.2",
      "resolved": "https://registry.npmjs.org/express/-/express-4.18.2.tgz",
      "integrity": "sha512-5/PsL6iGPdfQ/lKM1UuielYgv3BUoJfz1aUwU9vHZ+J7gyvwdQXFEBIEIaxeGf0GIcreATNyBExtalisDbuMqQ==",
      "dependencies": {
        "accepts": "~1.3.8",
        "array-flatten": "1.1.1",
        "body-parser": "1.20.1",
        "content-disposition": "0.5.4",
        "content-type": "~1.0.4",
        "cookie": "0.5.0",
        "cookie-signature": "1.0.6",
        "debug": "2.6.9",
        "depd": "2.0.0",
        "encodeurl": "~1.0.2",
        "escape-html": "~1.0.3",
        "etag": "~1.8.1",
        "finalhandler": "1.2.0",
        "fresh": "0.5.2",
        "http-errors": "2.0.0",
        "merge-descriptors": "1.0.1",
        "methods": "~1.1.2",
        "on-finished": "2.4.1",
        "parseurl": "~1.3.3",
        "path-to-regexp": "0.1.7",
        "proxy-addr": "~2.0.7",
        "qs": "6.11.0",
        "range-parser": "~1.2.1",
        "safe-buffer": "5.2.1",
        "send": "0.18.0",
        "serve-static": "1.15.0",
        "setprototypeof": "1.2.0",
        "statuses": "2.0.1",
        "type-is": "~1.6.18",
        "utils-merge": "1.0.1",
        "vary": "~1.1.2"
      },
      "engines": {
        "node": ">= 0.10.0"
      }
    },
    "node_modules/express-fileupload": {
      "version": "1.4.0",
      "resolved": "https://registry.npmjs.org/express-fileupload/-/express-fileupload-1.4.0.tgz",
      "integrity": "sha512-RjzLCHxkv3umDeZKeFeMg8w7qe0V09w3B7oGZprr/oO2H/ISCgNzuqzn7gV3HRWb37GjRk429CCpSLS2KNTqMQ==",
      "dependencies": {
        "busboy": "^1.6.0"
      },
      "engines": {
        "node": ">=12.0.0"
      }
    },
    "node_modules/express/node_modules/body-parser": {
      "version": "1.20.1",
      "resolved": "https://registry.npmjs.org/body-parser/-/body-parser-1.20.1.tgz",
      "integrity": "sha512-jWi7abTbYwajOytWCQc37VulmWiRae5RyTpaCyDcS5/lMdtwSz5lOpDE67srw/HYe35f1z3fDQw+3txg7gNtWw==",
      "dependencies": {
        "bytes": "3.1.2",
        "content-type": "~1.0.4",
        "debug": "2.6.9",
        "depd": "2.0.0",
        "destroy": "1.2.0",
        "http-errors": "2.0.0",
        "iconv-lite": "0.4.24",
        "on-finished": "2.4.1",
        "qs": "6.11.0",
        "raw-body": "2.5.1",
        "type-is": "~1.6.18",
        "unpipe": "1.0.0"
      },
      "engines": {
        "node": ">= 0.8",
        "npm": "1.2.8000 || >= 1.4.16"
      }
    },
    "node_modules/express/node_modules/raw-body": {
      "version": "2.5.1",
      "resolved": "https://registry.npmjs.org/raw-body/-/raw-body-2.5.1.tgz",
      "integrity": "sha512-qqJBtEyVgS0ZmPGdCFPWJ3FreoqvG4MVQln/kCgF7Olq95IbOp0/BWyMwbdtn4VTvkM8Y7khCQ2Xgk/tcrCXig==",
      "dependencies": {
        "bytes": "3.1.2",
        "http-errors": "2.0.0",
        "iconv-lite": "0.4.24",
        "unpipe": "1.0.0"
      },
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/ext": {
      "version": "1.7.0",
      "resolved": "https://registry.npmjs.org/ext/-/ext-1.7.0.tgz",
      "integrity": "sha512-6hxeJYaL110a9b5TEJSj0gojyHQAmA2ch5Os+ySCiA1QGdS697XWY1pzsrSjqA9LDEEgdB/KypIlR59RcLuHYw==",
      "dev": true,
      "dependencies": {
        "type": "^2.7.2"
      }
    },
    "node_modules/ext/node_modules/type": {
      "version": "2.7.2",
      "resolved": "https://registry.npmjs.org/type/-/type-2.7.2.tgz",
      "integrity": "sha512-dzlvlNlt6AXU7EBSfpAscydQ7gXB+pPGsPnfJnZpiNJBDj7IaJzQlBZYGdEi4R9HmPdBv2XmWJ6YUtoTa7lmCw==",
      "dev": true
    },
    "node_modules/extend": {
      "version": "3.0.2",
      "resolved": "https://registry.npmjs.org/extend/-/extend-3.0.2.tgz",
      "integrity": "sha512-fjquC59cD7CyW6urNXK0FBufkZcoiGG80wTuPujX590cB5Ttln20E2UB4S/WARVqhXffZl2LNgS+gQdPIIim/g=="
    },
    "node_modules/fetch-blob": {
      "version": "3.2.0",
      "resolved": "https://registry.npmjs.org/fetch-blob/-/fetch-blob-3.2.0.tgz",
      "integrity": "sha512-7yAQpD2UMJzLi1Dqv7qFYnPbaPx7ZfFK6PiIxQ4PfkGPyNyl2Ugx+a/umUonmKqjhM4DnfbMvdX6otXq83soQQ==",
      "funding": [
        {
          "type": "github",
          "url": "https://github.com/sponsors/jimmywarting"
        },
        {
          "type": "paypal",
          "url": "https://paypal.me/jimmywarting"
        }
      ],
      "dependencies": {
        "node-domexception": "^1.0.0",
        "web-streams-polyfill": "^3.0.3"
      },
      "engines": {
        "node": "^12.20 || >= 14.13"
      }
    },
    "node_modules/fill-range": {
      "version": "7.0.1",
      "resolved": "https://registry.npmjs.org/fill-range/-/fill-range-7.0.1.tgz",
      "integrity": "sha512-qOo9F+dMUmC2Lcb4BbVvnKJxTPjCm+RRpe4gDuGrzkL7mEVl/djYSu2OdQ2Pa302N4oqkSg9ir6jaLWJ2USVpQ==",
      "dev": true,
      "dependencies": {
        "to-regex-range": "^5.0.1"
      },
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/finalhandler": {
      "version": "1.2.0",
      "resolved": "https://registry.npmjs.org/finalhandler/-/finalhandler-1.2.0.tgz",
      "integrity": "sha512-5uXcUVftlQMFnWC9qu/svkWv3GTd2PfUhK/3PLkYNAe7FbqJMt3515HaxE6eRL74GdsriiwujiawdaB1BpEISg==",
      "dependencies": {
        "debug": "2.6.9",
        "encodeurl": "~1.0.2",
        "escape-html": "~1.0.3",
        "on-finished": "2.4.1",
        "parseurl": "~1.3.3",
        "statuses": "2.0.1",
        "unpipe": "~1.0.0"
      },
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/follow-redirects": {
      "version": "1.15.2",
      "resolved": "https://registry.npmjs.org/follow-redirects/-/follow-redirects-1.15.2.tgz",
      "integrity": "sha512-VQLG33o04KaQ8uYi2tVNbdrWp1QWxNNea+nmIB4EVM28v0hmP17z7aG1+wAkNzVq4KeXTq3221ye5qTJP91JwA==",
      "funding": [
        {
          "type": "individual",
          "url": "https://github.com/sponsors/RubenVerborgh"
        }
      ],
      "engines": {
        "node": ">=4.0"
      },
      "peerDependenciesMeta": {
        "debug": {
          "optional": true
        }
      }
    },
    "node_modules/for-each": {
      "version": "0.3.3",
      "resolved": "https://registry.npmjs.org/for-each/-/for-each-0.3.3.tgz",
      "integrity": "sha512-jqYfLp7mo9vIyQf8ykW2v7A+2N4QjeCeI5+Dz9XraiO1ign81wjiH7Fb9vSOWvQfNtmSa4H2RoQTrrXivdUZmw==",
      "dependencies": {
        "is-callable": "^1.1.3"
      }
    },
    "node_modules/form-data": {
      "version": "2.5.1",
      "resolved": "https://registry.npmjs.org/form-data/-/form-data-2.5.1.tgz",
      "integrity": "sha512-m21N3WOmEEURgk6B9GLOE4RuWOFf28Lhh9qGYeNlGq4VDXUlJy2th2slBNU8Gp8EzloYZOibZJ7t5ecIrFSjVA==",
      "dependencies": {
        "asynckit": "^0.4.0",
        "combined-stream": "^1.0.6",
        "mime-types": "^2.1.12"
      },
      "engines": {
        "node": ">= 0.12"
      }
    },
    "node_modules/formdata-polyfill": {
      "version": "4.0.10",
      "resolved": "https://registry.npmjs.org/formdata-polyfill/-/formdata-polyfill-4.0.10.tgz",
      "integrity": "sha512-buewHzMvYL29jdeQTVILecSaZKnt/RJWjoZCF5OW60Z67/GmSLBkOFM7qh1PI3zFNtJbaZL5eQu1vLfazOwj4g==",
      "dependencies": {
        "fetch-blob": "^3.1.2"
      },
      "engines": {
        "node": ">=12.20.0"
      }
    },
    "node_modules/forwarded": {
      "version": "0.2.0",
      "resolved": "https://registry.npmjs.org/forwarded/-/forwarded-0.2.0.tgz",
      "integrity": "sha512-buRG0fpBtRHSTCOASe6hD258tEubFoRLb4ZNA6NxMVHNw2gOcwHo9wyablzMzOA5z9xA9L1KNjk/Nt6MT9aYow==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/fresh": {
      "version": "0.5.2",
      "resolved": "https://registry.npmjs.org/fresh/-/fresh-0.5.2.tgz",
      "integrity": "sha512-zJ2mQYM18rEFOudeV4GShTGIQ7RbzA7ozbU9I/XBpm7kqgMywgmylMwXHxZJmkVoYkna9d2pVXVXPdYTP9ej8Q==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/fs-extra": {
      "version": "9.1.0",
      "resolved": "https://registry.npmjs.org/fs-extra/-/fs-extra-9.1.0.tgz",
      "integrity": "sha512-hcg3ZmepS30/7BSFqRvoo3DOMQu7IjqxO5nCDt+zM9XWjb33Wg7ziNT+Qvqbuc3+gWpzO02JubVyk2G4Zvo1OQ==",
      "dev": true,
      "dependencies": {
        "at-least-node": "^1.0.0",
        "graceful-fs": "^4.2.0",
        "jsonfile": "^6.0.1",
        "universalify": "^2.0.0"
      },
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/fs-minipass": {
      "version": "2.1.0",
      "resolved": "https://registry.npmjs.org/fs-minipass/-/fs-minipass-2.1.0.tgz",
      "integrity": "sha512-V/JgOLFCS+R6Vcq0slCuaeWEdNC3ouDlJMNIsacH2VtALiu9mV4LPrHc5cDl8k5aw6J8jwgWWpiTo5RYhmIzvg==",
      "dependencies": {
        "minipass": "^3.0.0"
      },
      "engines": {
        "node": ">= 8"
      }
    },
    "node_modules/fs-minipass/node_modules/minipass": {
      "version": "3.3.6",
      "resolved": "https://registry.npmjs.org/minipass/-/minipass-3.3.6.tgz",
      "integrity": "sha512-DxiNidxSEK+tHG6zOIklvNOwm3hvCrbUrdtzY74U6HKTJxvIDfOUL5W5P2Ghd3DTkhhKPYGqeNUIh5qcM4YBfw==",
      "dependencies": {
        "yallist": "^4.0.0"
      },
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/fs.realpath": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/fs.realpath/-/fs.realpath-1.0.0.tgz",
      "integrity": "sha512-OO0pH2lK6a0hZnAdau5ItzHPI6pUlvI7jMVnxUQRtw4owF2wk8lOSabtGDCTP4Ggrg2MbGnWO9X8K1t4+fGMDw=="
    },
    "node_modules/fsevents": {
      "version": "2.3.2",
      "resolved": "https://registry.npmjs.org/fsevents/-/fsevents-2.3.2.tgz",
      "integrity": "sha512-xiqMQR4xAeHTuB9uWm+fFRcIOgKBMiOBP+eXiyT7jsgVCq1bkVygt00oASowB7EdtpOHaaPgKt812P9ab+DDKA==",
      "dev": true,
      "hasInstallScript": true,
      "optional": true,
      "os": [
        "darwin"
      ],
      "engines": {
        "node": "^8.16.0 || ^10.6.0 || >=11.0.0"
      }
    },
    "node_modules/function-bind": {
      "version": "1.1.1",
      "resolved": "https://registry.npmjs.org/function-bind/-/function-bind-1.1.1.tgz",
      "integrity": "sha512-yIovAzMX49sF8Yl58fSCWJ5svSLuaibPxXQJFLmBObTuCr0Mf1KiPopGM9NiFjiYBCbfaa2Fh6breQ6ANVTI0A=="
    },
    "node_modules/gauge": {
      "version": "3.0.2",
      "resolved": "https://registry.npmjs.org/gauge/-/gauge-3.0.2.tgz",
      "integrity": "sha512-+5J6MS/5XksCuXq++uFRsnUd7Ovu1XenbeuIuNRJxYWjgQbPuFhT14lAvsWfqfAmnwluf1OwMjz39HjfLPci0Q==",
      "dependencies": {
        "aproba": "^1.0.3 || ^2.0.0",
        "color-support": "^1.1.2",
        "console-control-strings": "^1.0.0",
        "has-unicode": "^2.0.1",
        "object-assign": "^4.1.1",
        "signal-exit": "^3.0.0",
        "string-width": "^4.2.3",
        "strip-ansi": "^6.0.1",
        "wide-align": "^1.1.2"
      },
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/gaxios": {
      "version": "6.1.1",
      "resolved": "https://registry.npmjs.org/gaxios/-/gaxios-6.1.1.tgz",
      "integrity": "sha512-bw8smrX+XlAoo9o1JAksBwX+hi/RG15J+NTSxmNPIclKC3ZVK6C2afwY8OSdRvOK0+ZLecUJYtj2MmjOt3Dm0w==",
      "dependencies": {
        "extend": "^3.0.2",
        "https-proxy-agent": "^7.0.1",
        "is-stream": "^2.0.0",
        "node-fetch": "^2.6.9"
      },
      "engines": {
        "node": ">=14"
      }
    },
    "node_modules/gaxios/node_modules/agent-base": {
      "version": "7.1.0",
      "resolved": "https://registry.npmjs.org/agent-base/-/agent-base-7.1.0.tgz",
      "integrity": "sha512-o/zjMZRhJxny7OyEF+Op8X+efiELC7k7yOjMzgfzVqOzXqkBkWI79YoTdOtsuWd5BWhAGAuOY/Xa6xpiaWXiNg==",
      "dependencies": {
        "debug": "^4.3.4"
      },
      "engines": {
        "node": ">= 14"
      }
    },
    "node_modules/gaxios/node_modules/debug": {
      "version": "4.3.4",
      "resolved": "https://registry.npmjs.org/debug/-/debug-4.3.4.tgz",
      "integrity": "sha512-PRWFHuSU3eDtQJPvnNY7Jcket1j0t5OuOsFzPPzsekD52Zl8qUfFIPEiswXqIvHWGVHOgX+7G/vCNNhehwxfkQ==",
      "dependencies": {
        "ms": "2.1.2"
      },
      "engines": {
        "node": ">=6.0"
      },
      "peerDependenciesMeta": {
        "supports-color": {
          "optional": true
        }
      }
    },
    "node_modules/gaxios/node_modules/https-proxy-agent": {
      "version": "7.0.2",
      "resolved": "https://registry.npmjs.org/https-proxy-agent/-/https-proxy-agent-7.0.2.tgz",
      "integrity": "sha512-NmLNjm6ucYwtcUmL7JQC1ZQ57LmHP4lT15FQ8D61nak1rO6DH+fz5qNK2Ap5UN4ZapYICE3/0KodcLYSPsPbaA==",
      "dependencies": {
        "agent-base": "^7.0.2",
        "debug": "4"
      },
      "engines": {
        "node": ">= 14"
      }
    },
    "node_modules/gaxios/node_modules/ms": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.2.tgz",
      "integrity": "sha512-sGkPx+VjMtmA6MX27oA4FBFELFCZZ4S4XqeGOXCv68tT+jb3vk/RyaKWP0PTKyWtmLSM0b+adUTEvbs1PEaH2w=="
    },
    "node_modules/gcp-metadata": {
      "version": "6.0.0",
      "resolved": "https://registry.npmjs.org/gcp-metadata/-/gcp-metadata-6.0.0.tgz",
      "integrity": "sha512-Ozxyi23/1Ar51wjUT2RDklK+3HxqDr8TLBNK8rBBFQ7T85iIGnXnVusauj06QyqCXRFZig8LZC+TUddWbndlpQ==",
      "dependencies": {
        "gaxios": "^6.0.0",
        "json-bigint": "^1.0.0"
      },
      "engines": {
        "node": ">=14"
      }
    },
    "node_modules/generate-function": {
      "version": "2.3.1",
      "resolved": "https://registry.npmjs.org/generate-function/-/generate-function-2.3.1.tgz",
      "integrity": "sha512-eeB5GfMNeevm/GRYq20ShmsaGcmI81kIX2K9XQx5miC8KdHaC6Jm0qQ8ZNeGOi7wYB8OsdxKs+Y2oVuTFuVwKQ==",
      "dependencies": {
        "is-property": "^1.0.2"
      }
    },
    "node_modules/generic-pool": {
      "version": "3.9.0",
      "resolved": "https://registry.npmjs.org/generic-pool/-/generic-pool-3.9.0.tgz",
      "integrity": "sha512-hymDOu5B53XvN4QT9dBmZxPX4CWhBPPLguTZ9MMFeFa/Kg0xWVfylOVNlJji/E7yTZWFd/q9GO5TxDLq156D7g==",
      "engines": {
        "node": ">= 4"
      }
    },
    "node_modules/get-caller-file": {
      "version": "2.0.5",
      "resolved": "https://registry.npmjs.org/get-caller-file/-/get-caller-file-2.0.5.tgz",
      "integrity": "sha512-DyFP3BM/3YHTQOCUL/w0OZHR0lpKeGrxotcHWcqNEdnltqFwXVfhEBQ94eIo34AfQpo0rGki4cyIiftY06h2Fg==",
      "dev": true,
      "engines": {
        "node": "6.* || 8.* || >= 10.*"
      }
    },
    "node_modules/get-intrinsic": {
      "version": "1.2.1",
      "resolved": "https://registry.npmjs.org/get-intrinsic/-/get-intrinsic-1.2.1.tgz",
      "integrity": "sha512-2DcsyfABl+gVHEfCOaTrWgyt+tb6MSEGmKq+kI5HwLbIYgjgmMcV8KQ41uaKz1xxUcn9tJtgFbQUEVcEbd0FYw==",
      "dependencies": {
        "function-bind": "^1.1.1",
        "has": "^1.0.3",
        "has-proto": "^1.0.1",
        "has-symbols": "^1.0.3"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/get-port": {
      "version": "3.2.0",
      "resolved": "https://registry.npmjs.org/get-port/-/get-port-3.2.0.tgz",
      "integrity": "sha512-x5UJKlgeUiNT8nyo/AcnwLnZuZNcSjSw0kogRB+Whd1fjjFq4B1hySFxSFWWSn4mIBzg3sRNUDFYc4g5gjPoLg==",
      "engines": {
        "node": ">=4"
      }
    },
    "node_modules/glob": {
      "version": "7.2.3",
      "resolved": "https://registry.npmjs.org/glob/-/glob-7.2.3.tgz",
      "integrity": "sha512-nFR0zLpU2YCaRxwoCJvL6UvCH2JFyFVIvwTLsIf21AuHlMskA1hhTdk+LlYJtOlYt9v6dvszD2BGRqBL+iQK9Q==",
      "dependencies": {
        "fs.realpath": "^1.0.0",
        "inflight": "^1.0.4",
        "inherits": "2",
        "minimatch": "^3.1.1",
        "once": "^1.3.0",
        "path-is-absolute": "^1.0.0"
      },
      "engines": {
        "node": "*"
      },
      "funding": {
        "url": "https://github.com/sponsors/isaacs"
      }
    },
    "node_modules/glob-parent": {
      "version": "5.1.2",
      "resolved": "https://registry.npmjs.org/glob-parent/-/glob-parent-5.1.2.tgz",
      "integrity": "sha512-AOIgSQCepiJYwP3ARnGx+5VnTu2HBYdzbGP45eLw1vr3zB3vZLeyed1sC9hnbcOc9/SrMyM5RPQrkGz4aS9Zow==",
      "dev": true,
      "dependencies": {
        "is-glob": "^4.0.1"
      },
      "engines": {
        "node": ">= 6"
      }
    },
    "node_modules/google-auth-library": {
      "version": "9.0.0",
      "resolved": "https://registry.npmjs.org/google-auth-library/-/google-auth-library-9.0.0.tgz",
      "integrity": "sha512-IQGjgQoVUAfOk6khqTVMLvWx26R+yPw9uLyb1MNyMQpdKiKt0Fd9sp4NWoINjyGHR8S3iw12hMTYK7O8J07c6Q==",
      "dependencies": {
        "base64-js": "^1.3.0",
        "ecdsa-sig-formatter": "^1.0.11",
        "gaxios": "^6.0.0",
        "gcp-metadata": "^6.0.0",
        "gtoken": "^7.0.0",
        "jws": "^4.0.0",
        "lru-cache": "^6.0.0"
      },
      "engines": {
        "node": ">=14"
      }
    },
    "node_modules/google-auth-library/node_modules/jwa": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/jwa/-/jwa-2.0.0.tgz",
      "integrity": "sha512-jrZ2Qx916EA+fq9cEAeCROWPTfCwi1IVHqT2tapuqLEVVDKFDENFw1oL+MwrTvH6msKxsd1YTDVw6uKEcsrLEA==",
      "dependencies": {
        "buffer-equal-constant-time": "1.0.1",
        "ecdsa-sig-formatter": "1.0.11",
        "safe-buffer": "^5.0.1"
      }
    },
    "node_modules/google-auth-library/node_modules/jws": {
      "version": "4.0.0",
      "resolved": "https://registry.npmjs.org/jws/-/jws-4.0.0.tgz",
      "integrity": "sha512-KDncfTmOZoOMTFG4mBlG0qUIOlc03fmzH+ru6RgYVZhPkyiy/92Owlt/8UEN+a4TXR1FQetfIpJE8ApdvdVxTg==",
      "dependencies": {
        "jwa": "^2.0.0",
        "safe-buffer": "^5.0.1"
      }
    },
    "node_modules/googleapis": {
      "version": "126.0.1",
      "resolved": "https://registry.npmjs.org/googleapis/-/googleapis-126.0.1.tgz",
      "integrity": "sha512-4N8LLi+hj6ytK3PhE52KcM8iSGhJjtXnCDYB4fp6l+GdLbYz4FoDmx074WqMbl7iYMDN87vqD/8drJkhxW92mQ==",
      "dependencies": {
        "google-auth-library": "^9.0.0",
        "googleapis-common": "^7.0.0"
      },
      "engines": {
        "node": ">=14.0.0"
      }
    },
    "node_modules/googleapis-common": {
      "version": "7.0.0",
      "resolved": "https://registry.npmjs.org/googleapis-common/-/googleapis-common-7.0.0.tgz",
      "integrity": "sha512-58iSybJPQZ8XZNMpjrklICefuOuyJ0lMxfKmBqmaC0/xGT4SiOs4BE60LAOOGtBURy1n8fHa2X2YUNFEWWbXyQ==",
      "dependencies": {
        "extend": "^3.0.2",
        "gaxios": "^6.0.3",
        "google-auth-library": "^9.0.0",
        "qs": "^6.7.0",
        "url-template": "^2.0.8",
        "uuid": "^9.0.0"
      },
      "engines": {
        "node": ">=14.0.0"
      }
    },
    "node_modules/gopd": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/gopd/-/gopd-1.0.1.tgz",
      "integrity": "sha512-d65bNlIadxvpb/A2abVdlqKqV563juRnZ1Wtk6s1sIR8uNsXR70xqIzVqxVf1eTqDunwT2MkczEeaezCKTZhwA==",
      "dependencies": {
        "get-intrinsic": "^1.1.3"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/graceful-fs": {
      "version": "4.2.11",
      "resolved": "https://registry.npmjs.org/graceful-fs/-/graceful-fs-4.2.11.tgz",
      "integrity": "sha512-RbJ5/jmFcNNCcDV5o9eTnBLJ/HszWV0P73bc+Ff4nS/rJj+YaS6IGyiOL0VoBYX+l1Wrl3k63h/KrH+nhJ0XvQ==",
      "dev": true
    },
    "node_modules/gtoken": {
      "version": "7.0.1",
      "resolved": "https://registry.npmjs.org/gtoken/-/gtoken-7.0.1.tgz",
      "integrity": "sha512-KcFVtoP1CVFtQu0aSk3AyAt2og66PFhZAlkUOuWKwzMLoulHXG5W5wE5xAnHb+yl3/wEFoqGW7/cDGMU8igDZQ==",
      "dependencies": {
        "gaxios": "^6.0.0",
        "jws": "^4.0.0"
      },
      "engines": {
        "node": ">=14.0.0"
      }
    },
    "node_modules/gtoken/node_modules/jwa": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/jwa/-/jwa-2.0.0.tgz",
      "integrity": "sha512-jrZ2Qx916EA+fq9cEAeCROWPTfCwi1IVHqT2tapuqLEVVDKFDENFw1oL+MwrTvH6msKxsd1YTDVw6uKEcsrLEA==",
      "dependencies": {
        "buffer-equal-constant-time": "1.0.1",
        "ecdsa-sig-formatter": "1.0.11",
        "safe-buffer": "^5.0.1"
      }
    },
    "node_modules/gtoken/node_modules/jws": {
      "version": "4.0.0",
      "resolved": "https://registry.npmjs.org/jws/-/jws-4.0.0.tgz",
      "integrity": "sha512-KDncfTmOZoOMTFG4mBlG0qUIOlc03fmzH+ru6RgYVZhPkyiy/92Owlt/8UEN+a4TXR1FQetfIpJE8ApdvdVxTg==",
      "dependencies": {
        "jwa": "^2.0.0",
        "safe-buffer": "^5.0.1"
      }
    },
    "node_modules/h264-profile-level-id": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/h264-profile-level-id/-/h264-profile-level-id-1.0.1.tgz",
      "integrity": "sha512-D3Rln/jKNjKDW5ZTJTK3niSoOGE+pFqPvRHHVgQN3G7umcn/zWGPUo8Q8VpDj16x3hKz++zVviRNRmXu5cpN+Q==",
      "dependencies": {
        "debug": "^4.1.1"
      },
      "engines": {
        "node": ">=8.0.0"
      }
    },
    "node_modules/h264-profile-level-id/node_modules/debug": {
      "version": "4.3.4",
      "resolved": "https://registry.npmjs.org/debug/-/debug-4.3.4.tgz",
      "integrity": "sha512-PRWFHuSU3eDtQJPvnNY7Jcket1j0t5OuOsFzPPzsekD52Zl8qUfFIPEiswXqIvHWGVHOgX+7G/vCNNhehwxfkQ==",
      "dependencies": {
        "ms": "2.1.2"
      },
      "engines": {
        "node": ">=6.0"
      },
      "peerDependenciesMeta": {
        "supports-color": {
          "optional": true
        }
      }
    },
    "node_modules/h264-profile-level-id/node_modules/ms": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.2.tgz",
      "integrity": "sha512-sGkPx+VjMtmA6MX27oA4FBFELFCZZ4S4XqeGOXCv68tT+jb3vk/RyaKWP0PTKyWtmLSM0b+adUTEvbs1PEaH2w=="
    },
    "node_modules/has": {
      "version": "1.0.3",
      "resolved": "https://registry.npmjs.org/has/-/has-1.0.3.tgz",
      "integrity": "sha512-f2dvO0VU6Oej7RkWJGrehjbzMAjFp5/VKPp5tTpWIV4JHHZK1/BxbFRtf/siA2SWTe09caDmVtYYzWEIbBS4zw==",
      "dependencies": {
        "function-bind": "^1.1.1"
      },
      "engines": {
        "node": ">= 0.4.0"
      }
    },
    "node_modules/has-flag": {
      "version": "3.0.0",
      "resolved": "https://registry.npmjs.org/has-flag/-/has-flag-3.0.0.tgz",
      "integrity": "sha512-sKJf1+ceQBr4SMkvQnBDNDtf4TXpVhVGateu0t918bl30FnbE2m4vNLX+VWe/dpjlb+HugGYzW7uQXH98HPEYw==",
      "dev": true,
      "engines": {
        "node": ">=4"
      }
    },
    "node_modules/has-proto": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/has-proto/-/has-proto-1.0.1.tgz",
      "integrity": "sha512-7qE+iP+O+bgF9clE5+UoBFzE65mlBiVj3tKCrlNQ0Ogwm0BjpT/gK4SlLYDMybDh5I3TCTKnPPa0oMG7JDYrhg==",
      "engines": {
        "node": ">= 0.4"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/has-symbols": {
      "version": "1.0.3",
      "resolved": "https://registry.npmjs.org/has-symbols/-/has-symbols-1.0.3.tgz",
      "integrity": "sha512-l3LCuF6MgDNwTDKkdYGEihYjt5pRPbEg46rtlmnSPlUbgmB8LOIrKJbYYFBSbnPaJexMKtiPO8hmeRjRz2Td+A==",
      "engines": {
        "node": ">= 0.4"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/has-tostringtag": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/has-tostringtag/-/has-tostringtag-1.0.0.tgz",
      "integrity": "sha512-kFjcSNhnlGV1kyoGk7OXKSawH5JOb/LzUc5w9B02hOTO0dfFRjbHQKvg1d6cf3HbeUmtU9VbbV3qzZ2Teh97WQ==",
      "dependencies": {
        "has-symbols": "^1.0.2"
      },
      "engines": {
        "node": ">= 0.4"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/has-unicode": {
      "version": "2.0.1",
      "resolved": "https://registry.npmjs.org/has-unicode/-/has-unicode-2.0.1.tgz",
      "integrity": "sha512-8Rf9Y83NBReMnx0gFzA8JImQACstCYWUplepDa9xprwwtmgEZUF0h/i5xSA625zB/I37EtrswSST6OXxwaaIJQ=="
    },
    "node_modules/http-basic": {
      "version": "8.1.3",
      "resolved": "https://registry.npmjs.org/http-basic/-/http-basic-8.1.3.tgz",
      "integrity": "sha512-/EcDMwJZh3mABI2NhGfHOGOeOZITqfkEO4p/xK+l3NpyncIHUQBoMvCSF/b5GqvKtySC2srL/GGG3+EtlqlmCw==",
      "dependencies": {
        "caseless": "^0.12.0",
        "concat-stream": "^1.6.2",
        "http-response-object": "^3.0.1",
        "parse-cache-control": "^1.0.1"
      },
      "engines": {
        "node": ">=6.0.0"
      }
    },
    "node_modules/http-errors": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/http-errors/-/http-errors-2.0.0.tgz",
      "integrity": "sha512-FtwrG/euBzaEjYeRqOgly7G0qviiXoJWnvEH2Z1plBdXgbyjv34pHTSb9zoeHMyDy33+DWy5Wt9Wo+TURtOYSQ==",
      "dependencies": {
        "depd": "2.0.0",
        "inherits": "2.0.4",
        "setprototypeof": "1.2.0",
        "statuses": "2.0.1",
        "toidentifier": "1.0.1"
      },
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/http-response-object": {
      "version": "3.0.2",
      "resolved": "https://registry.npmjs.org/http-response-object/-/http-response-object-3.0.2.tgz",
      "integrity": "sha512-bqX0XTF6fnXSQcEJ2Iuyr75yVakyjIDCqroJQ/aHfSdlM743Cwqoi2nDYMzLGWUcuTWGWy8AAvOKXTfiv6q9RA==",
      "dependencies": {
        "@types/node": "^10.0.3"
      }
    },
    "node_modules/http-response-object/node_modules/@types/node": {
      "version": "10.17.60",
      "resolved": "https://registry.npmjs.org/@types/node/-/node-10.17.60.tgz",
      "integrity": "sha512-F0KIgDJfy2nA3zMLmWGKxcH2ZVEtCZXHHdOQs2gSaQ27+lNeEfGxzkIw90aXswATX7AZ33tahPbzy6KAfUreVw=="
    },
    "node_modules/https-proxy-agent": {
      "version": "5.0.1",
      "resolved": "https://registry.npmjs.org/https-proxy-agent/-/https-proxy-agent-5.0.1.tgz",
      "integrity": "sha512-dFcAjpTQFgoLMzC2VwU+C/CbS7uRL0lWmxDITmqm7C+7F0Odmj6s9l6alZc6AELXhrnggM2CeWSXHGOdX2YtwA==",
      "dependencies": {
        "agent-base": "6",
        "debug": "4"
      },
      "engines": {
        "node": ">= 6"
      }
    },
    "node_modules/https-proxy-agent/node_modules/debug": {
      "version": "4.3.4",
      "resolved": "https://registry.npmjs.org/debug/-/debug-4.3.4.tgz",
      "integrity": "sha512-PRWFHuSU3eDtQJPvnNY7Jcket1j0t5OuOsFzPPzsekD52Zl8qUfFIPEiswXqIvHWGVHOgX+7G/vCNNhehwxfkQ==",
      "dependencies": {
        "ms": "2.1.2"
      },
      "engines": {
        "node": ">=6.0"
      },
      "peerDependenciesMeta": {
        "supports-color": {
          "optional": true
        }
      }
    },
    "node_modules/https-proxy-agent/node_modules/ms": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.2.tgz",
      "integrity": "sha512-sGkPx+VjMtmA6MX27oA4FBFELFCZZ4S4XqeGOXCv68tT+jb3vk/RyaKWP0PTKyWtmLSM0b+adUTEvbs1PEaH2w=="
    },
    "node_modules/iconv-lite": {
      "version": "0.4.24",
      "resolved": "https://registry.npmjs.org/iconv-lite/-/iconv-lite-0.4.24.tgz",
      "integrity": "sha512-v3MXnZAcvnywkTUEZomIActle7RXXeedOR31wwl7VlyoXO4Qi9arvSenNQWne1TcRwhCL1HwLI21bEqdpj8/rA==",
      "dependencies": {
        "safer-buffer": ">= 2.1.2 < 3"
      },
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/ieee754": {
      "version": "1.1.13",
      "resolved": "https://registry.npmjs.org/ieee754/-/ieee754-1.1.13.tgz",
      "integrity": "sha512-4vf7I2LYV/HaWerSo3XmlMkp5eZ83i+/CDluXi/IGTs/O1sejBNhTtnxzmRZfvOUqj7lZjqHkeTvpgSFDlWZTg=="
    },
    "node_modules/ignore-by-default": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/ignore-by-default/-/ignore-by-default-1.0.1.tgz",
      "integrity": "sha512-Ius2VYcGNk7T90CppJqcIkS5ooHUZyIQK+ClZfMfMNFEF9VSE73Fq+906u/CWu92x4gzZMWOwfFYckPObzdEbA==",
      "dev": true
    },
    "node_modules/inflection": {
      "version": "1.13.4",
      "resolved": "https://registry.npmjs.org/inflection/-/inflection-1.13.4.tgz",
      "integrity": "sha512-6I/HUDeYFfuNCVS3td055BaXBwKYuzw7K3ExVMStBowKo9oOAMJIXIHvdyR3iboTCp1b+1i5DSkIZTcwIktuDw==",
      "engines": [
        "node >= 0.4.0"
      ]
    },
    "node_modules/inflight": {
      "version": "1.0.6",
      "resolved": "https://registry.npmjs.org/inflight/-/inflight-1.0.6.tgz",
      "integrity": "sha512-k92I/b08q4wvFscXCLvqfsHCrjrF7yiXsQuIVvVE7N82W3+aqpzuUdBbfhWcy/FZR3/4IgflMgKLOsvPDrGCJA==",
      "dependencies": {
        "once": "^1.3.0",
        "wrappy": "1"
      }
    },
    "node_modules/inherits": {
      "version": "2.0.4",
      "resolved": "https://registry.npmjs.org/inherits/-/inherits-2.0.4.tgz",
      "integrity": "sha512-k/vGaX4/Yla3WzyMCvTQOXYeIHvqOKtnqBduzTHpzpQZzAskKMhZ2K+EnBiSM9zGSoIFeMpXKxa4dYeZIQqewQ=="
    },
    "node_modules/ini": {
      "version": "1.3.8",
      "resolved": "https://registry.npmjs.org/ini/-/ini-1.3.8.tgz",
      "integrity": "sha512-JV/yugV2uzW5iMRSiZAyDtQd+nxtUnjeLt0acNdw98kKLrvuRVyB80tsREOE7yvGVgalhZ6RNXCmEHkUKBKxew==",
      "dev": true
    },
    "node_modules/ipaddr.js": {
      "version": "1.9.1",
      "resolved": "https://registry.npmjs.org/ipaddr.js/-/ipaddr.js-1.9.1.tgz",
      "integrity": "sha512-0KI/607xoxSToH7GjN1FfSbLoU0+btTicjsQSWQlh/hZykN8KpmMf7uYwPW3R+akZ6R/w18ZlXSHBYXiYUPO3g==",
      "engines": {
        "node": ">= 0.10"
      }
    },
    "node_modules/is-arguments": {
      "version": "1.1.1",
      "resolved": "https://registry.npmjs.org/is-arguments/-/is-arguments-1.1.1.tgz",
      "integrity": "sha512-8Q7EARjzEnKpt/PCD7e1cgUS0a6X8u5tdSiMqXhojOdoV9TsMsiO+9VLC5vAmO8N7/GmXn7yjR8qnA6bVAEzfA==",
      "dependencies": {
        "call-bind": "^1.0.2",
        "has-tostringtag": "^1.0.0"
      },
      "engines": {
        "node": ">= 0.4"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/is-binary-path": {
      "version": "2.1.0",
      "resolved": "https://registry.npmjs.org/is-binary-path/-/is-binary-path-2.1.0.tgz",
      "integrity": "sha512-ZMERYes6pDydyuGidse7OsHxtbI7WVeUEozgR/g7rd0xUimYNlvZRE/K2MgZTjWy725IfelLeVcEM97mmtRGXw==",
      "dev": true,
      "dependencies": {
        "binary-extensions": "^2.0.0"
      },
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/is-callable": {
      "version": "1.2.7",
      "resolved": "https://registry.npmjs.org/is-callable/-/is-callable-1.2.7.tgz",
      "integrity": "sha512-1BC0BVFhS/p0qtw6enp8e+8OD0UrK0oFLztSjNzhcKA3WDuJxxAPXzPuPtKkjEY9UUoEWlX/8fgKeu2S8i9JTA==",
      "engines": {
        "node": ">= 0.4"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/is-core-module": {
      "version": "2.13.0",
      "resolved": "https://registry.npmjs.org/is-core-module/-/is-core-module-2.13.0.tgz",
      "integrity": "sha512-Z7dk6Qo8pOCp3l4tsX2C5ZVas4V+UxwQodwZhLopL91TX8UyyHEXafPcyoeeWuLrwzHcr3igO78wNLwHJHsMCQ==",
      "dev": true,
      "dependencies": {
        "has": "^1.0.3"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/is-extglob": {
      "version": "2.1.1",
      "resolved": "https://registry.npmjs.org/is-extglob/-/is-extglob-2.1.1.tgz",
      "integrity": "sha512-SbKbANkN603Vi4jEZv49LeVJMn4yGwsbzZworEoyEiutsN3nJYdbO36zfhGJ6QEDpOZIFkDtnq5JRxmvl3jsoQ==",
      "dev": true,
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/is-fullwidth-code-point": {
      "version": "3.0.0",
      "resolved": "https://registry.npmjs.org/is-fullwidth-code-point/-/is-fullwidth-code-point-3.0.0.tgz",
      "integrity": "sha512-zymm5+u+sCsSWyD9qNaejV3DFvhCKclKdizYaJUuHA83RLjb7nSuGnddCHGv0hk+KY7BMAlsWeK4Ueg6EV6XQg==",
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/is-generator-function": {
      "version": "1.0.10",
      "resolved": "https://registry.npmjs.org/is-generator-function/-/is-generator-function-1.0.10.tgz",
      "integrity": "sha512-jsEjy9l3yiXEQ+PsXdmBwEPcOxaXWLspKdplFUVI9vq1iZgIekeC0L167qeu86czQaxed3q/Uzuw0swL0irL8A==",
      "dependencies": {
        "has-tostringtag": "^1.0.0"
      },
      "engines": {
        "node": ">= 0.4"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/is-glob": {
      "version": "4.0.3",
      "resolved": "https://registry.npmjs.org/is-glob/-/is-glob-4.0.3.tgz",
      "integrity": "sha512-xelSayHH36ZgE7ZWhli7pW34hNbNl8Ojv5KVmkJD4hBdD3th8Tfk9vYasLM+mXWOZhFkgZfxhLSnrwRr4elSSg==",
      "dev": true,
      "dependencies": {
        "is-extglob": "^2.1.1"
      },
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/is-number": {
      "version": "7.0.0",
      "resolved": "https://registry.npmjs.org/is-number/-/is-number-7.0.0.tgz",
      "integrity": "sha512-41Cifkg6e8TylSpdtTpeLVMqvSBEVzTttHvERD741+pnZ8ANv0004MRL43QKPDlK9cGvNp6NZWZUBlbGXYxxng==",
      "dev": true,
      "engines": {
        "node": ">=0.12.0"
      }
    },
    "node_modules/is-promise": {
      "version": "2.2.2",
      "resolved": "https://registry.npmjs.org/is-promise/-/is-promise-2.2.2.tgz",
      "integrity": "sha512-+lP4/6lKUBfQjZ2pdxThZvLUAafmZb8OAxFb8XXtiQmS35INgr85hdOGoEs124ez1FCnZJt6jau/T+alh58QFQ==",
      "dev": true
    },
    "node_modules/is-property": {
      "version": "1.0.2",
      "resolved": "https://registry.npmjs.org/is-property/-/is-property-1.0.2.tgz",
      "integrity": "sha512-Ks/IoX00TtClbGQr4TWXemAnktAQvYB7HzcCxDGqEZU6oCmb2INHuOoKxbtR+HFkmYWBKv/dOZtGRiAjDhj92g=="
    },
    "node_modules/is-stream": {
      "version": "2.0.1",
      "resolved": "https://registry.npmjs.org/is-stream/-/is-stream-2.0.1.tgz",
      "integrity": "sha512-hFoiJiTl63nn+kstHGBtewWSKnQLpyb155KHheA1l39uvtO9nWIop1p3udqPcUd/xbF1VLMO4n7OI6p7RbngDg==",
      "engines": {
        "node": ">=8"
      },
      "funding": {
        "url": "https://github.com/sponsors/sindresorhus"
      }
    },
    "node_modules/is-typed-array": {
      "version": "1.1.12",
      "resolved": "https://registry.npmjs.org/is-typed-array/-/is-typed-array-1.1.12.tgz",
      "integrity": "sha512-Z14TF2JNG8Lss5/HMqt0//T9JeHXttXy5pH/DBU4vi98ozO2btxzq9MwYDZYnKwU8nRsz/+GVFVRDq3DkVuSPg==",
      "dependencies": {
        "which-typed-array": "^1.1.11"
      },
      "engines": {
        "node": ">= 0.4"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/isarray": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/isarray/-/isarray-1.0.0.tgz",
      "integrity": "sha512-VLghIWNM6ELQzo7zwmcg0NmTVyWKYjvIeM83yjp0wRDTmUnrM678fQbcKBo6n2CJEF0szoG//ytg+TKla89ALQ=="
    },
    "node_modules/jmespath": {
      "version": "0.16.0",
      "resolved": "https://registry.npmjs.org/jmespath/-/jmespath-0.16.0.tgz",
      "integrity": "sha512-9FzQjJ7MATs1tSpnco1K6ayiYE3figslrXA72G2HQ/n76RzvYlofyi5QM+iX4YRs/pu3yzxlVQSST23+dMDknw==",
      "engines": {
        "node": ">= 0.6.0"
      }
    },
    "node_modules/js-beautify": {
      "version": "1.14.9",
      "resolved": "https://registry.npmjs.org/js-beautify/-/js-beautify-1.14.9.tgz",
      "integrity": "sha512-coM7xq1syLcMyuVGyToxcj2AlzhkDjmfklL8r0JgJ7A76wyGMpJ1oA35mr4APdYNO/o/4YY8H54NQIJzhMbhBg==",
      "dev": true,
      "dependencies": {
        "config-chain": "^1.1.13",
        "editorconfig": "^1.0.3",
        "glob": "^8.1.0",
        "nopt": "^6.0.0"
      },
      "bin": {
        "css-beautify": "js/bin/css-beautify.js",
        "html-beautify": "js/bin/html-beautify.js",
        "js-beautify": "js/bin/js-beautify.js"
      },
      "engines": {
        "node": ">=12"
      }
    },
    "node_modules/js-beautify/node_modules/brace-expansion": {
      "version": "2.0.1",
      "resolved": "https://registry.npmjs.org/brace-expansion/-/brace-expansion-2.0.1.tgz",
      "integrity": "sha512-XnAIvQ8eM+kC6aULx6wuQiwVsnzsi9d3WxzV3FpWTGA19F621kwdbsAcFKXgKUHZWsy+mY6iL1sHTxWEFCytDA==",
      "dev": true,
      "dependencies": {
        "balanced-match": "^1.0.0"
      }
    },
    "node_modules/js-beautify/node_modules/glob": {
      "version": "8.1.0",
      "resolved": "https://registry.npmjs.org/glob/-/glob-8.1.0.tgz",
      "integrity": "sha512-r8hpEjiQEYlF2QU0df3dS+nxxSIreXQS1qRhMJM0Q5NDdR386C7jb7Hwwod8Fgiuex+k0GFjgft18yvxm5XoCQ==",
      "dev": true,
      "dependencies": {
        "fs.realpath": "^1.0.0",
        "inflight": "^1.0.4",
        "inherits": "2",
        "minimatch": "^5.0.1",
        "once": "^1.3.0"
      },
      "engines": {
        "node": ">=12"
      },
      "funding": {
        "url": "https://github.com/sponsors/isaacs"
      }
    },
    "node_modules/js-beautify/node_modules/minimatch": {
      "version": "5.1.6",
      "resolved": "https://registry.npmjs.org/minimatch/-/minimatch-5.1.6.tgz",
      "integrity": "sha512-lKwV/1brpG6mBUFHtb7NUmtABCb2WZZmm2wNiOA5hAb8VdCS4B3dtMWyvcoViccwAW/COERjXLt0zP1zXUN26g==",
      "dev": true,
      "dependencies": {
        "brace-expansion": "^2.0.1"
      },
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/js-beautify/node_modules/nopt": {
      "version": "6.0.0",
      "resolved": "https://registry.npmjs.org/nopt/-/nopt-6.0.0.tgz",
      "integrity": "sha512-ZwLpbTgdhuZUnZzjd7nb1ZV+4DoiC6/sfiVKok72ym/4Tlf+DFdlHYmT2JPmcNNWV6Pi3SDf1kT+A4r9RTuT9g==",
      "dev": true,
      "dependencies": {
        "abbrev": "^1.0.0"
      },
      "bin": {
        "nopt": "bin/nopt.js"
      },
      "engines": {
        "node": "^12.13.0 || ^14.15.0 || >=16.0.0"
      }
    },
    "node_modules/json-bigint": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/json-bigint/-/json-bigint-1.0.0.tgz",
      "integrity": "sha512-SiPv/8VpZuWbvLSMtTDU8hEfrZWg/mH/nV/b4o0CYbSxu1UIQPLdwKOCIyLQX+VIPO5vrLX3i8qtqFyhdPSUSQ==",
      "dependencies": {
        "bignumber.js": "^9.0.0"
      }
    },
    "node_modules/jsonfile": {
      "version": "6.1.0",
      "resolved": "https://registry.npmjs.org/jsonfile/-/jsonfile-6.1.0.tgz",
      "integrity": "sha512-5dgndWOriYSm5cnYaJNhalLNDKOqFwyDB/rr1E9ZsGciGvKPs8R2xYGCacuf3z6K1YKDz182fd+fY3cn3pMqXQ==",
      "dev": true,
      "dependencies": {
        "universalify": "^2.0.0"
      },
      "optionalDependencies": {
        "graceful-fs": "^4.1.6"
      }
    },
    "node_modules/jsonwebtoken": {
      "version": "9.0.2",
      "resolved": "https://registry.npmjs.org/jsonwebtoken/-/jsonwebtoken-9.0.2.tgz",
      "integrity": "sha512-PRp66vJ865SSqOlgqS8hujT5U4AOgMfhrwYIuIhfKaoSCZcirrmASQr8CX7cUg+RMih+hgznrjp99o+W4pJLHQ==",
      "dependencies": {
        "jws": "^3.2.2",
        "lodash.includes": "^4.3.0",
        "lodash.isboolean": "^3.0.3",
        "lodash.isinteger": "^4.0.4",
        "lodash.isnumber": "^3.0.3",
        "lodash.isplainobject": "^4.0.6",
        "lodash.isstring": "^4.0.1",
        "lodash.once": "^4.0.0",
        "ms": "^2.1.1",
        "semver": "^7.5.4"
      },
      "engines": {
        "node": ">=12",
        "npm": ">=6"
      }
    },
    "node_modules/jsonwebtoken/node_modules/ms": {
      "version": "2.1.3",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.3.tgz",
      "integrity": "sha512-6FlzubTLZG3J2a/NVCAleEhjzq5oxgHyaCU9yYXvcLsvoVaHJq/s5xXI6/XXP6tz7R9xAOtHnSO/tXtF3WRTlA=="
    },
    "node_modules/jwa": {
      "version": "1.4.1",
      "resolved": "https://registry.npmjs.org/jwa/-/jwa-1.4.1.tgz",
      "integrity": "sha512-qiLX/xhEEFKUAJ6FiBMbes3w9ATzyk5W7Hvzpa/SLYdxNtng+gcurvrI7TbACjIXlsJyr05/S1oUhZrc63evQA==",
      "dependencies": {
        "buffer-equal-constant-time": "1.0.1",
        "ecdsa-sig-formatter": "1.0.11",
        "safe-buffer": "^5.0.1"
      }
    },
    "node_modules/jws": {
      "version": "3.2.2",
      "resolved": "https://registry.npmjs.org/jws/-/jws-3.2.2.tgz",
      "integrity": "sha512-YHlZCB6lMTllWDtSPHz/ZXTsi8S00usEV6v1tjq8tOUZzw7DpSDWVXjXDre6ed1w/pd495ODpHZYSdkRTsa0HA==",
      "dependencies": {
        "jwa": "^1.4.1",
        "safe-buffer": "^5.0.1"
      }
    },
    "node_modules/lodash": {
      "version": "4.17.21",
      "resolved": "https://registry.npmjs.org/lodash/-/lodash-4.17.21.tgz",
      "integrity": "sha512-v2kDEe57lecTulaDIuNTPy3Ry4gLGJ6Z1O3vE1krgXZNrsQ+LFTGHVxVjcXPs17LhbZVGedAJv8XZ1tvj5FvSg=="
    },
    "node_modules/lodash.includes": {
      "version": "4.3.0",
      "resolved": "https://registry.npmjs.org/lodash.includes/-/lodash.includes-4.3.0.tgz",
      "integrity": "sha512-W3Bx6mdkRTGtlJISOvVD/lbqjTlPPUDTMnlXZFnVwi9NKJ6tiAk6LVdlhZMm17VZisqhKcgzpO5Wz91PCt5b0w=="
    },
    "node_modules/lodash.isboolean": {
      "version": "3.0.3",
      "resolved": "https://registry.npmjs.org/lodash.isboolean/-/lodash.isboolean-3.0.3.tgz",
      "integrity": "sha512-Bz5mupy2SVbPHURB98VAcw+aHh4vRV5IPNhILUCsOzRmsTmSQ17jIuqopAentWoehktxGd9e/hbIXq980/1QJg=="
    },
    "node_modules/lodash.isinteger": {
      "version": "4.0.4",
      "resolved": "https://registry.npmjs.org/lodash.isinteger/-/lodash.isinteger-4.0.4.tgz",
      "integrity": "sha512-DBwtEWN2caHQ9/imiNeEA5ys1JoRtRfY3d7V9wkqtbycnAmTvRRmbHKDV4a0EYc678/dia0jrte4tjYwVBaZUA=="
    },
    "node_modules/lodash.isnumber": {
      "version": "3.0.3",
      "resolved": "https://registry.npmjs.org/lodash.isnumber/-/lodash.isnumber-3.0.3.tgz",
      "integrity": "sha512-QYqzpfwO3/CWf3XP+Z+tkQsfaLL/EnUlXWVkIk5FUPc4sBdTehEqZONuyRt2P67PXAk+NXmTBcc97zw9t1FQrw=="
    },
    "node_modules/lodash.isplainobject": {
      "version": "4.0.6",
      "resolved": "https://registry.npmjs.org/lodash.isplainobject/-/lodash.isplainobject-4.0.6.tgz",
      "integrity": "sha512-oSXzaWypCMHkPC3NvBEaPHf0KsA5mvPrOPgQWDsbg8n7orZ290M0BmC/jgRZ4vcJ6DTAhjrsSYgdsW/F+MFOBA=="
    },
    "node_modules/lodash.isstring": {
      "version": "4.0.1",
      "resolved": "https://registry.npmjs.org/lodash.isstring/-/lodash.isstring-4.0.1.tgz",
      "integrity": "sha512-0wJxfxH1wgO3GrbuP+dTTk7op+6L41QCXbGINEmD+ny/G/eCqGzxyCsh7159S+mgDDcoarnBw6PC1PS5+wUGgw=="
    },
    "node_modules/lodash.once": {
      "version": "4.1.1",
      "resolved": "https://registry.npmjs.org/lodash.once/-/lodash.once-4.1.1.tgz",
      "integrity": "sha512-Sb487aTOCr9drQVL8pIxOzVhafOjZN9UU54hiN8PU3uAiSV7lx1yYNpbNmex2PK6dSJoNTSJUUswT651yww3Mg=="
    },
    "node_modules/long": {
      "version": "5.2.3",
      "resolved": "https://registry.npmjs.org/long/-/long-5.2.3.tgz",
      "integrity": "sha512-lcHwpNoggQTObv5apGNCTdJrO69eHOZMi4BNC+rTLER8iHAqGrUVeLh/irVIM7zTw2bOXA8T6uNPeujwOLg/2Q=="
    },
    "node_modules/long-timeout": {
      "version": "0.1.1",
      "resolved": "https://registry.npmjs.org/long-timeout/-/long-timeout-0.1.1.tgz",
      "integrity": "sha512-BFRuQUqc7x2NWxfJBCyUrN8iYUYznzL9JROmRz1gZ6KlOIgmoD+njPVbb+VNn2nGMKggMsK79iUNErillsrx7w=="
    },
    "node_modules/lru-cache": {
      "version": "6.0.0",
      "resolved": "https://registry.npmjs.org/lru-cache/-/lru-cache-6.0.0.tgz",
      "integrity": "sha512-Jo6dJ04CmSjuznwJSS3pUeWmd/H0ffTlkXXgwZi+eq1UCmqQwCh+eLsYOYCwY991i2Fah4h1BEMCx4qThGbsiA==",
      "dependencies": {
        "yallist": "^4.0.0"
      },
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/lru-queue": {
      "version": "0.1.0",
      "resolved": "https://registry.npmjs.org/lru-queue/-/lru-queue-0.1.0.tgz",
      "integrity": "sha512-BpdYkt9EvGl8OfWHDQPISVpcl5xZthb+XPsbELj5AQXxIC8IriDZIQYjBJPEm5rS420sjZ0TLEzRcq5KdBhYrQ==",
      "dev": true,
      "dependencies": {
        "es5-ext": "~0.10.2"
      }
    },
    "node_modules/luxon": {
      "version": "3.4.0",
      "resolved": "https://registry.npmjs.org/luxon/-/luxon-3.4.0.tgz",
      "integrity": "sha512-7eDo4Pt7aGhoCheGFIuq4Xa2fJm4ZpmldpGhjTYBNUYNCN6TIEP6v7chwwwt3KRp7YR+rghbfvjyo3V5y9hgBw==",
      "engines": {
        "node": ">=12"
      }
    },
    "node_modules/make-dir": {
      "version": "3.1.0",
      "resolved": "https://registry.npmjs.org/make-dir/-/make-dir-3.1.0.tgz",
      "integrity": "sha512-g3FeP20LNwhALb/6Cz6Dd4F2ngze0jz7tbzrD2wAV+o9FeNHe4rL+yK2md0J/fiSf1sa1ADhXqi5+oVwOM/eGw==",
      "dependencies": {
        "semver": "^6.0.0"
      },
      "engines": {
        "node": ">=8"
      },
      "funding": {
        "url": "https://github.com/sponsors/sindresorhus"
      }
    },
    "node_modules/make-dir/node_modules/semver": {
      "version": "6.3.1",
      "resolved": "https://registry.npmjs.org/semver/-/semver-6.3.1.tgz",
      "integrity": "sha512-BR7VvDCVHO+q2xBEWskxS6DJE1qRnb7DxzUrogb71CWoSficBxYsiAGd+Kl0mmq/MprG9yArRkyrQxTO6XjMzA==",
      "bin": {
        "semver": "bin/semver.js"
      }
    },
    "node_modules/media-typer": {
      "version": "0.3.0",
      "resolved": "https://registry.npmjs.org/media-typer/-/media-typer-0.3.0.tgz",
      "integrity": "sha512-dq+qelQ9akHpcOl/gUVRTxVIOkAJ1wR3QAvb4RsVjS8oVoFjDGTc679wJYmUmknUF5HwMLOgb5O+a3KxfWapPQ==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/mediasoup": {
      "version": "3.12.8",
      "resolved": "https://registry.npmjs.org/mediasoup/-/mediasoup-3.12.8.tgz",
      "integrity": "sha512-8K3UlFq9ZTnRbIWjrU/4WDWptKIBZfyqWrXMsaORc2uPmVfwSbMl0aMPc55FA64hg957l8QzKPnFkGBZgg7/4g==",
      "hasInstallScript": true,
      "dependencies": {
        "debug": "^4.3.4",
        "h264-profile-level-id": "^1.0.1",
        "node-fetch": "^3.3.1",
        "supports-color": "^9.4.0",
        "tar": "^6.1.15",
        "uuid": "^9.0.0"
      },
      "engines": {
        "node": ">=16"
      },
      "funding": {
        "type": "opencollective",
        "url": "https://opencollective.com/mediasoup"
      }
    },
    "node_modules/mediasoup/node_modules/debug": {
      "version": "4.3.4",
      "resolved": "https://registry.npmjs.org/debug/-/debug-4.3.4.tgz",
      "integrity": "sha512-PRWFHuSU3eDtQJPvnNY7Jcket1j0t5OuOsFzPPzsekD52Zl8qUfFIPEiswXqIvHWGVHOgX+7G/vCNNhehwxfkQ==",
      "dependencies": {
        "ms": "2.1.2"
      },
      "engines": {
        "node": ">=6.0"
      },
      "peerDependenciesMeta": {
        "supports-color": {
          "optional": true
        }
      }
    },
    "node_modules/mediasoup/node_modules/ms": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.2.tgz",
      "integrity": "sha512-sGkPx+VjMtmA6MX27oA4FBFELFCZZ4S4XqeGOXCv68tT+jb3vk/RyaKWP0PTKyWtmLSM0b+adUTEvbs1PEaH2w=="
    },
    "node_modules/mediasoup/node_modules/node-fetch": {
      "version": "3.3.2",
      "resolved": "https://registry.npmjs.org/node-fetch/-/node-fetch-3.3.2.tgz",
      "integrity": "sha512-dRB78srN/l6gqWulah9SrxeYnxeddIG30+GOqK/9OlLVyLg3HPnr6SqOWTWOXKRwC2eGYCkZ59NNuSgvSrpgOA==",
      "dependencies": {
        "data-uri-to-buffer": "^4.0.0",
        "fetch-blob": "^3.1.4",
        "formdata-polyfill": "^4.0.10"
      },
      "engines": {
        "node": "^12.20.0 || ^14.13.1 || >=16.0.0"
      },
      "funding": {
        "type": "opencollective",
        "url": "https://opencollective.com/node-fetch"
      }
    },
    "node_modules/mediasoup/node_modules/supports-color": {
      "version": "9.4.0",
      "resolved": "https://registry.npmjs.org/supports-color/-/supports-color-9.4.0.tgz",
      "integrity": "sha512-VL+lNrEoIXww1coLPOmiEmK/0sGigko5COxI09KzHc2VJXJsQ37UaQ+8quuxjDeA7+KnLGTWRyOXSLLR2Wb4jw==",
      "engines": {
        "node": ">=12"
      },
      "funding": {
        "url": "https://github.com/chalk/supports-color?sponsor=1"
      }
    },
    "node_modules/memoizee": {
      "version": "0.4.15",
      "resolved": "https://registry.npmjs.org/memoizee/-/memoizee-0.4.15.tgz",
      "integrity": "sha512-UBWmJpLZd5STPm7PMUlOw/TSy972M+z8gcyQ5veOnSDRREz/0bmpyTfKt3/51DhEBqCZQn1udM/5flcSPYhkdQ==",
      "dev": true,
      "dependencies": {
        "d": "^1.0.1",
        "es5-ext": "^0.10.53",
        "es6-weak-map": "^2.0.3",
        "event-emitter": "^0.3.5",
        "is-promise": "^2.2.2",
        "lru-queue": "^0.1.0",
        "next-tick": "^1.1.0",
        "timers-ext": "^0.1.7"
      }
    },
    "node_modules/merge-descriptors": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/merge-descriptors/-/merge-descriptors-1.0.1.tgz",
      "integrity": "sha512-cCi6g3/Zr1iqQi6ySbseM1Xvooa98N0w31jzUYrXPX2xqObmFGHJ0tQ5u74H3mVh7wLouTseZyYIq39g8cNp1w=="
    },
    "node_modules/methods": {
      "version": "1.1.2",
      "resolved": "https://registry.npmjs.org/methods/-/methods-1.1.2.tgz",
      "integrity": "sha512-iclAHeNqNm68zFtnZ0e+1L2yUIdvzNoauKU4WBA3VvH/vPFieF7qfRlwUZU+DA9P9bPXIS90ulxoUoCH23sV2w==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/mime": {
      "version": "1.6.0",
      "resolved": "https://registry.npmjs.org/mime/-/mime-1.6.0.tgz",
      "integrity": "sha512-x0Vn8spI+wuJ1O6S7gnbaQg8Pxh4NNHb7KSINmEWKiPE4RKOplvijn+NkmYmmRgP68mc70j2EbeTFRsrswaQeg==",
      "bin": {
        "mime": "cli.js"
      },
      "engines": {
        "node": ">=4"
      }
    },
    "node_modules/mime-db": {
      "version": "1.52.0",
      "resolved": "https://registry.npmjs.org/mime-db/-/mime-db-1.52.0.tgz",
      "integrity": "sha512-sPU4uV7dYlvtWJxwwxHD0PuihVNiE7TyAbQ5SWxDCB9mUYvOgroQOwYQQOKPJ8CIbE+1ETVlOoK1UC2nU3gYvg==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/mime-types": {
      "version": "2.1.35",
      "resolved": "https://registry.npmjs.org/mime-types/-/mime-types-2.1.35.tgz",
      "integrity": "sha512-ZDY+bPm5zTTF+YpCrAU9nK0UgICYPT0QtT1NZWFv4s++TNkcgVaT0g6+4R2uI4MjQjzysHB1zxuWL50hzaeXiw==",
      "dependencies": {
        "mime-db": "1.52.0"
      },
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/minimatch": {
      "version": "3.1.2",
      "resolved": "https://registry.npmjs.org/minimatch/-/minimatch-3.1.2.tgz",
      "integrity": "sha512-J7p63hRiAjw1NDEww1W7i37+ByIrOWO5XQQAzZ3VOcL0PNybwpfmV/N05zFAzwQ9USyEcX6t3UO+K5aqBQOIHw==",
      "dependencies": {
        "brace-expansion": "^1.1.7"
      },
      "engines": {
        "node": "*"
      }
    },
    "node_modules/minipass": {
      "version": "5.0.0",
      "resolved": "https://registry.npmjs.org/minipass/-/minipass-5.0.0.tgz",
      "integrity": "sha512-3FnjYuehv9k6ovOEbyOswadCDPX1piCfhV8ncmYtHOjuPwylVWsghTLo7rabjC3Rx5xD4HDx8Wm1xnMF7S5qFQ==",
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/minizlib": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/minizlib/-/minizlib-2.1.2.tgz",
      "integrity": "sha512-bAxsR8BVfj60DWXHE3u30oHzfl4G7khkSuPW+qvpd7jFRHm7dLxOjUk1EHACJ/hxLY8phGJ0YhYHZo7jil7Qdg==",
      "dependencies": {
        "minipass": "^3.0.0",
        "yallist": "^4.0.0"
      },
      "engines": {
        "node": ">= 8"
      }
    },
    "node_modules/minizlib/node_modules/minipass": {
      "version": "3.3.6",
      "resolved": "https://registry.npmjs.org/minipass/-/minipass-3.3.6.tgz",
      "integrity": "sha512-DxiNidxSEK+tHG6zOIklvNOwm3hvCrbUrdtzY74U6HKTJxvIDfOUL5W5P2Ghd3DTkhhKPYGqeNUIh5qcM4YBfw==",
      "dependencies": {
        "yallist": "^4.0.0"
      },
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/mkdirp": {
      "version": "1.0.4",
      "resolved": "https://registry.npmjs.org/mkdirp/-/mkdirp-1.0.4.tgz",
      "integrity": "sha512-vVqVZQyf3WLx2Shd0qJ9xuvqgAyKPLAiqITEtqW0oIUjzo3PePDd6fW9iFz30ef7Ysp/oiWqbhszeGWW2T6Gzw==",
      "bin": {
        "mkdirp": "bin/cmd.js"
      },
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/moment": {
      "version": "2.29.4",
      "resolved": "https://registry.npmjs.org/moment/-/moment-2.29.4.tgz",
      "integrity": "sha512-5LC9SOxjSc2HF6vO2CyuTDNivEdoz2IvyJJGj6X8DJ0eFyfszE0QiEd+iXmBvUP3WHxSjFH/vIsA0EN00cgr8w==",
      "engines": {
        "node": "*"
      }
    },
    "node_modules/moment-timezone": {
      "version": "0.5.43",
      "resolved": "https://registry.npmjs.org/moment-timezone/-/moment-timezone-0.5.43.tgz",
      "integrity": "sha512-72j3aNyuIsDxdF1i7CEgV2FfxM1r6aaqJyLB2vwb33mXYyoyLly+F1zbWqhA3/bVIoJ4szlUoMbUnVdid32NUQ==",
      "dependencies": {
        "moment": "^2.29.4"
      },
      "engines": {
        "node": "*"
      }
    },
    "node_modules/ms": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.0.0.tgz",
      "integrity": "sha512-Tpp60P6IUJDTuOq/5Z8cdskzJujfwqfOTkrwIwj7IRISpnkJnT6SyJ4PCPnGMoFjC9ddhal5KVIYtAt97ix05A=="
    },
    "node_modules/mysql": {
      "version": "2.18.1",
      "resolved": "https://registry.npmjs.org/mysql/-/mysql-2.18.1.tgz",
      "integrity": "sha512-Bca+gk2YWmqp2Uf6k5NFEurwY/0td0cpebAucFpY/3jhrwrVGuxU2uQFCHjU19SJfje0yQvi+rVWdq78hR5lig==",
      "dependencies": {
        "bignumber.js": "9.0.0",
        "readable-stream": "2.3.7",
        "safe-buffer": "5.1.2",
        "sqlstring": "2.3.1"
      },
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/mysql/node_modules/readable-stream": {
      "version": "2.3.7",
      "resolved": "https://registry.npmjs.org/readable-stream/-/readable-stream-2.3.7.tgz",
      "integrity": "sha512-Ebho8K4jIbHAxnuxi7o42OrZgF/ZTNcsZj6nRKyUmkhLFq8CHItp/fy6hQZuZmP/n3yZ9VBUbp4zz/mX8hmYPw==",
      "dependencies": {
        "core-util-is": "~1.0.0",
        "inherits": "~2.0.3",
        "isarray": "~1.0.0",
        "process-nextick-args": "~2.0.0",
        "safe-buffer": "~5.1.1",
        "string_decoder": "~1.1.1",
        "util-deprecate": "~1.0.1"
      }
    },
    "node_modules/mysql/node_modules/safe-buffer": {
      "version": "5.1.2",
      "resolved": "https://registry.npmjs.org/safe-buffer/-/safe-buffer-5.1.2.tgz",
      "integrity": "sha512-Gd2UZBJDkXlY7GbJxfsE8/nvKkUEU1G38c1siN6QP6a9PT9MmHB8GnpscSmMJSoF8LOIrt8ud/wPtojys4G6+g=="
    },
    "node_modules/mysql/node_modules/sqlstring": {
      "version": "2.3.1",
      "resolved": "https://registry.npmjs.org/sqlstring/-/sqlstring-2.3.1.tgz",
      "integrity": "sha512-ooAzh/7dxIG5+uDik1z/Rd1vli0+38izZhGzSa34FwR7IbelPWCCKSNIl8jlL/F7ERvy8CB2jNeM1E9i9mXMAQ==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/mysql/node_modules/string_decoder": {
      "version": "1.1.1",
      "resolved": "https://registry.npmjs.org/string_decoder/-/string_decoder-1.1.1.tgz",
      "integrity": "sha512-n/ShnvDi6FHbbVfviro+WojiFzv+s8MPMHBczVePfUpDJLwoLT0ht1l4YwBCbi8pJAveEEdnkHyPyTP/mzRfwg==",
      "dependencies": {
        "safe-buffer": "~5.1.0"
      }
    },
    "node_modules/mysql2": {
      "version": "3.6.0",
      "resolved": "https://registry.npmjs.org/mysql2/-/mysql2-3.6.0.tgz",
      "integrity": "sha512-EWUGAhv6SphezurlfI2Fpt0uJEWLmirrtQR7SkbTHFC+4/mJBrPiSzHESHKAWKG7ALVD6xaG/NBjjd1DGJGQQQ==",
      "dependencies": {
        "denque": "^2.1.0",
        "generate-function": "^2.3.1",
        "iconv-lite": "^0.6.3",
        "long": "^5.2.1",
        "lru-cache": "^8.0.0",
        "named-placeholders": "^1.1.3",
        "seq-queue": "^0.0.5",
        "sqlstring": "^2.3.2"
      },
      "engines": {
        "node": ">= 8.0"
      }
    },
    "node_modules/mysql2/node_modules/iconv-lite": {
      "version": "0.6.3",
      "resolved": "https://registry.npmjs.org/iconv-lite/-/iconv-lite-0.6.3.tgz",
      "integrity": "sha512-4fCk79wshMdzMp2rH06qWrJE4iolqLhCUH+OiuIgU++RB0+94NlDL81atO7GX55uUKueo0txHNtvEyI6D7WdMw==",
      "dependencies": {
        "safer-buffer": ">= 2.1.2 < 3.0.0"
      },
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/mysql2/node_modules/lru-cache": {
      "version": "8.0.5",
      "resolved": "https://registry.npmjs.org/lru-cache/-/lru-cache-8.0.5.tgz",
      "integrity": "sha512-MhWWlVnuab1RG5/zMRRcVGXZLCXrZTgfwMikgzCegsPnG62yDQo5JnqKkrK4jO5iKqDAZGItAqN5CtKBCBWRUA==",
      "engines": {
        "node": ">=16.14"
      }
    },
    "node_modules/named-placeholders": {
      "version": "1.1.3",
      "resolved": "https://registry.npmjs.org/named-placeholders/-/named-placeholders-1.1.3.tgz",
      "integrity": "sha512-eLoBxg6wE/rZkJPhU/xRX1WTpkFEwDJEN96oxFrTsqBdbT5ec295Q+CoHrL9IT0DipqKhmGcaZmwOt8OON5x1w==",
      "dependencies": {
        "lru-cache": "^7.14.1"
      },
      "engines": {
        "node": ">=12.0.0"
      }
    },
    "node_modules/named-placeholders/node_modules/lru-cache": {
      "version": "7.18.3",
      "resolved": "https://registry.npmjs.org/lru-cache/-/lru-cache-7.18.3.tgz",
      "integrity": "sha512-jumlc0BIUrS3qJGgIkWZsyfAM7NCWiBcCDhnd+3NNM5KbBmLTgHVfWBcg6W+rLUsIpzpERPsvwUP7CckAQSOoA==",
      "engines": {
        "node": ">=12"
      }
    },
    "node_modules/negotiator": {
      "version": "0.6.3",
      "resolved": "https://registry.npmjs.org/negotiator/-/negotiator-0.6.3.tgz",
      "integrity": "sha512-+EUsqGPLsM+j/zdChZjsnX51g4XrHFOIXwfnCVPGlQk/k5giakcKsuxCObBRu6DSm9opw/O6slWbJdghQM4bBg==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/next-tick": {
      "version": "1.1.0",
      "resolved": "https://registry.npmjs.org/next-tick/-/next-tick-1.1.0.tgz",
      "integrity": "sha512-CXdUiJembsNjuToQvxayPZF9Vqht7hewsvy2sOWafLvi2awflj9mOC6bHIg50orX8IJvWKY9wYQ/zB2kogPslQ==",
      "dev": true
    },
    "node_modules/node-addon-api": {
      "version": "5.1.0",
      "resolved": "https://registry.npmjs.org/node-addon-api/-/node-addon-api-5.1.0.tgz",
      "integrity": "sha512-eh0GgfEkpnoWDq+VY8OyvYhFEzBk6jIYbRKdIlyTiAXIVJ8PyBaKb0rp7oDtoddbdoHWhq8wwr+XZ81F1rpNdA=="
    },
    "node_modules/node-domexception": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/node-domexception/-/node-domexception-1.0.0.tgz",
      "integrity": "sha512-/jKZoMpw0F8GRwl4/eLROPA3cfcXtLApP0QzLmUT/HuPCZWyB7IY9ZrMeKw2O/nFIqPQB3PVM9aYm0F312AXDQ==",
      "funding": [
        {
          "type": "github",
          "url": "https://github.com/sponsors/jimmywarting"
        },
        {
          "type": "github",
          "url": "https://paypal.me/jimmywarting"
        }
      ],
      "engines": {
        "node": ">=10.5.0"
      }
    },
    "node_modules/node-fetch": {
      "version": "2.6.12",
      "resolved": "https://registry.npmjs.org/node-fetch/-/node-fetch-2.6.12.tgz",
      "integrity": "sha512-C/fGU2E8ToujUivIO0H+tpQ6HWo4eEmchoPIoXtxCrVghxdKq+QOHqEZW7tuP3KlV3bC8FRMO5nMCC7Zm1VP6g==",
      "dependencies": {
        "whatwg-url": "^5.0.0"
      },
      "engines": {
        "node": "4.x || >=6.0.0"
      },
      "peerDependencies": {
        "encoding": "^0.1.0"
      },
      "peerDependenciesMeta": {
        "encoding": {
          "optional": true
        }
      }
    },
    "node_modules/node-schedule": {
      "version": "2.1.1",
      "resolved": "https://registry.npmjs.org/node-schedule/-/node-schedule-2.1.1.tgz",
      "integrity": "sha512-OXdegQq03OmXEjt2hZP33W2YPs/E5BcFQks46+G2gAxs4gHOIVD1u7EqlYLYSKsaIpyKCK9Gbk0ta1/gjRSMRQ==",
      "dependencies": {
        "cron-parser": "^4.2.0",
        "long-timeout": "0.1.1",
        "sorted-array-functions": "^1.3.0"
      },
      "engines": {
        "node": ">=6"
      }
    },
    "node_modules/nodemailer": {
      "version": "6.9.4",
      "resolved": "https://registry.npmjs.org/nodemailer/-/nodemailer-6.9.4.tgz",
      "integrity": "sha512-CXjQvrQZV4+6X5wP6ZIgdehJamI63MFoYFGGPtHudWym9qaEHDNdPzaj5bfMCvxG1vhAileSWW90q7nL0N36mA==",
      "engines": {
        "node": ">=6.0.0"
      }
    },
    "node_modules/nodemon": {
      "version": "3.1.0",
      "resolved": "https://registry.npmjs.org/nodemon/-/nodemon-3.1.0.tgz",
      "integrity": "sha512-xqlktYlDMCepBJd43ZQhjWwMw2obW/JRvkrLxq5RCNcuDDX1DbcPT+qT1IlIIdf+DhnWs90JpTMe+Y5KxOchvA==",
      "dev": true,
      "dependencies": {
        "chokidar": "^3.5.2",
        "debug": "^4",
        "ignore-by-default": "^1.0.1",
        "minimatch": "^3.1.2",
        "pstree.remy": "^1.1.8",
        "semver": "^7.5.3",
        "simple-update-notifier": "^2.0.0",
        "supports-color": "^5.5.0",
        "touch": "^3.1.0",
        "undefsafe": "^2.0.5"
      },
      "bin": {
        "nodemon": "bin/nodemon.js"
      },
      "engines": {
        "node": ">=10"
      },
      "funding": {
        "type": "opencollective",
        "url": "https://opencollective.com/nodemon"
      }
    },
    "node_modules/nodemon/node_modules/debug": {
      "version": "4.3.4",
      "resolved": "https://registry.npmjs.org/debug/-/debug-4.3.4.tgz",
      "integrity": "sha512-PRWFHuSU3eDtQJPvnNY7Jcket1j0t5OuOsFzPPzsekD52Zl8qUfFIPEiswXqIvHWGVHOgX+7G/vCNNhehwxfkQ==",
      "dev": true,
      "dependencies": {
        "ms": "2.1.2"
      },
      "engines": {
        "node": ">=6.0"
      },
      "peerDependenciesMeta": {
        "supports-color": {
          "optional": true
        }
      }
    },
    "node_modules/nodemon/node_modules/ms": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.2.tgz",
      "integrity": "sha512-sGkPx+VjMtmA6MX27oA4FBFELFCZZ4S4XqeGOXCv68tT+jb3vk/RyaKWP0PTKyWtmLSM0b+adUTEvbs1PEaH2w==",
      "dev": true
    },
    "node_modules/nopt": {
      "version": "5.0.0",
      "resolved": "https://registry.npmjs.org/nopt/-/nopt-5.0.0.tgz",
      "integrity": "sha512-Tbj67rffqceeLpcRXrT7vKAN8CwfPeIBgM7E6iBkmKLV7bEMwpGgYLGv0jACUsECaa/vuxP0IjEont6umdMgtQ==",
      "dependencies": {
        "abbrev": "1"
      },
      "bin": {
        "nopt": "bin/nopt.js"
      },
      "engines": {
        "node": ">=6"
      }
    },
    "node_modules/normalize-path": {
      "version": "3.0.0",
      "resolved": "https://registry.npmjs.org/normalize-path/-/normalize-path-3.0.0.tgz",
      "integrity": "sha512-6eZs5Ls3WtCisHWp9S2GUy8dqkpGi4BVSz3GaqiE6ezub0512ESztXUwUB6C6IKbQkY2Pnb/mD4WYojCRwcwLA==",
      "dev": true,
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/notepack.io": {
      "version": "3.0.1",
      "resolved": "https://registry.npmjs.org/notepack.io/-/notepack.io-3.0.1.tgz",
      "integrity": "sha512-TKC/8zH5pXIAMVQio2TvVDTtPRX+DJPHDqjRbxogtFiByHyzKmy96RA0JtCQJ+WouyyL4A10xomQzgbUT+1jCg=="
    },
    "node_modules/npmlog": {
      "version": "5.0.1",
      "resolved": "https://registry.npmjs.org/npmlog/-/npmlog-5.0.1.tgz",
      "integrity": "sha512-AqZtDUWOMKs1G/8lwylVjrdYgqA4d9nu8hc+0gzRxlDb1I10+FHBGMXs6aiQHFdCUUlqH99MUMuLfzWDNDtfxw==",
      "dependencies": {
        "are-we-there-yet": "^2.0.0",
        "console-control-strings": "^1.1.0",
        "gauge": "^3.0.0",
        "set-blocking": "^2.0.0"
      }
    },
    "node_modules/object-assign": {
      "version": "4.1.1",
      "resolved": "https://registry.npmjs.org/object-assign/-/object-assign-4.1.1.tgz",
      "integrity": "sha512-rJgTQnkUnH1sFw8yT6VSU3zD3sWmu6sZhIseY8VX+GRu3P6F7Fu+JNDoXfklElbLJSnc3FUQHVe4cU5hj+BcUg==",
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/object-inspect": {
      "version": "1.12.3",
      "resolved": "https://registry.npmjs.org/object-inspect/-/object-inspect-1.12.3.tgz",
      "integrity": "sha512-geUvdk7c+eizMNUDkRpW1wJwgfOiOeHbxBR/hLXK1aT6zmVSO0jsQcs7fj6MGw89jC/cjGfLcNOrtMYtGqm81g==",
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/on-finished": {
      "version": "2.4.1",
      "resolved": "https://registry.npmjs.org/on-finished/-/on-finished-2.4.1.tgz",
      "integrity": "sha512-oVlzkg3ENAhCk2zdv7IJwd/QUD4z2RxRwpkcGY8psCVcCYZNq4wYnVWALHM+brtuJjePWiYF/ClmuDr8Ch5+kg==",
      "dependencies": {
        "ee-first": "1.1.1"
      },
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/once": {
      "version": "1.4.0",
      "resolved": "https://registry.npmjs.org/once/-/once-1.4.0.tgz",
      "integrity": "sha512-lNaJgI+2Q5URQBkccEKHTQOPaXdUxnZZElQTZY0MFUAuaEqe1E+Nyvgdz/aIyNi6Z9MzO5dv1H8n58/GELp3+w==",
      "dependencies": {
        "wrappy": "1"
      }
    },
    "node_modules/packet-reader": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/packet-reader/-/packet-reader-1.0.0.tgz",
      "integrity": "sha512-HAKu/fG3HpHFO0AA8WE8q2g+gBJaZ9MG7fcKk+IJPLTGAD6Psw4443l+9DGRbOIh3/aXr7Phy0TjilYivJo5XQ=="
    },
    "node_modules/parse-cache-control": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/parse-cache-control/-/parse-cache-control-1.0.1.tgz",
      "integrity": "sha512-60zvsJReQPX5/QP0Kzfd/VrpjScIQ7SHBW6bFCYfEP+fp0Eppr1SHhIO5nd1PjZtvclzSzES9D/p5nFJurwfWg=="
    },
    "node_modules/parseurl": {
      "version": "1.3.3",
      "resolved": "https://registry.npmjs.org/parseurl/-/parseurl-1.3.3.tgz",
      "integrity": "sha512-CiyeOxFT/JZyN5m0z9PfXw4SCBJ6Sygz1Dpl0wqjlhDEGGBP1GnsUVEL0p63hoG1fcj3fHynXi9NYO4nWOL+qQ==",
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/path-is-absolute": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/path-is-absolute/-/path-is-absolute-1.0.1.tgz",
      "integrity": "sha512-AVbw3UJ2e9bq64vSaS9Am0fje1Pa8pbGqTTsmXfaIiMpnr5DlDhfJOuLj9Sf95ZPVDAUerDfEk88MPmPe7UCQg==",
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/path-parse": {
      "version": "1.0.7",
      "resolved": "https://registry.npmjs.org/path-parse/-/path-parse-1.0.7.tgz",
      "integrity": "sha512-LDJzPVEEEPR+y48z93A0Ed0yXb8pAByGWo/k5YYdYgpY2/2EsOsksJrq7lOHxryrVOn1ejG6oAp8ahvOIQD8sw==",
      "dev": true
    },
    "node_modules/path-to-regexp": {
      "version": "0.1.7",
      "resolved": "https://registry.npmjs.org/path-to-regexp/-/path-to-regexp-0.1.7.tgz",
      "integrity": "sha512-5DFkuoqlv1uYQKxy8omFBeJPQcdoE07Kv2sferDCrAq1ohOU+MSDswDIbnx3YAM60qIOnYa53wBhXW0EbMonrQ=="
    },
    "node_modules/pdf-lib": {
      "version": "1.17.1",
      "resolved": "https://registry.npmjs.org/pdf-lib/-/pdf-lib-1.17.1.tgz",
      "integrity": "sha512-V/mpyJAoTsN4cnP31vc0wfNA1+p20evqqnap0KLoRUN0Yk/p3wN52DOEsL4oBFcLdb76hlpKPtzJIgo67j/XLw==",
      "dependencies": {
        "@pdf-lib/standard-fonts": "^1.0.0",
        "@pdf-lib/upng": "^1.0.1",
        "pako": "^1.0.11",
        "tslib": "^1.11.1"
      }
    },
    "node_modules/pdf-lib/node_modules/pako": {
      "version": "1.0.11",
      "resolved": "https://registry.npmjs.org/pako/-/pako-1.0.11.tgz",
      "integrity": "sha512-4hLB8Py4zZce5s4yd9XzopqwVv/yGNhV1Bl8NTmCq1763HeK2+EwVTv+leGeL13Dnh2wfbqowVPXCIO0z4taYw=="
    },
    "node_modules/pg": {
      "version": "8.11.1",
      "resolved": "https://registry.npmjs.org/pg/-/pg-8.11.1.tgz",
      "integrity": "sha512-utdq2obft07MxaDg0zBJI+l/M3mBRfIpEN3iSemsz0G5F2/VXx+XzqF4oxrbIZXQxt2AZzIUzyVg/YM6xOP/WQ==",
      "dependencies": {
        "buffer-writer": "2.0.0",
        "packet-reader": "1.0.0",
        "pg-connection-string": "^2.6.1",
        "pg-pool": "^3.6.1",
        "pg-protocol": "^1.6.0",
        "pg-types": "^2.1.0",
        "pgpass": "1.x"
      },
      "engines": {
        "node": ">= 8.0.0"
      },
      "optionalDependencies": {
        "pg-cloudflare": "^1.1.1"
      },
      "peerDependencies": {
        "pg-native": ">=3.0.1"
      },
      "peerDependenciesMeta": {
        "pg-native": {
          "optional": true
        }
      }
    },
    "node_modules/pg-cloudflare": {
      "version": "1.1.1",
      "resolved": "https://registry.npmjs.org/pg-cloudflare/-/pg-cloudflare-1.1.1.tgz",
      "integrity": "sha512-xWPagP/4B6BgFO+EKz3JONXv3YDgvkbVrGw2mTo3D6tVDQRh1e7cqVGvyR3BE+eQgAvx1XhW/iEASj4/jCWl3Q==",
      "optional": true
    },
    "node_modules/pg-connection-string": {
      "version": "2.6.1",
      "resolved": "https://registry.npmjs.org/pg-connection-string/-/pg-connection-string-2.6.1.tgz",
      "integrity": "sha512-w6ZzNu6oMmIzEAYVw+RLK0+nqHPt8K3ZnknKi+g48Ak2pr3dtljJW3o+D/n2zzCG07Zoe9VOX3aiKpj+BN0pjg=="
    },
    "node_modules/pg-hstore": {
      "version": "2.3.4",
      "resolved": "https://registry.npmjs.org/pg-hstore/-/pg-hstore-2.3.4.tgz",
      "integrity": "sha512-N3SGs/Rf+xA1M2/n0JBiXFDVMzdekwLZLAO0g7mpDY9ouX+fDI7jS6kTq3JujmYbtNSJ53TJ0q4G98KVZSM4EA==",
      "dependencies": {
        "underscore": "^1.13.1"
      },
      "engines": {
        "node": ">= 0.8.x"
      }
    },
    "node_modules/pg-int8": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/pg-int8/-/pg-int8-1.0.1.tgz",
      "integrity": "sha512-WCtabS6t3c8SkpDBUlb1kjOs7l66xsGdKpIPZsg4wR+B3+u9UAum2odSsF9tnvxg80h4ZxLWMy4pRjOsFIqQpw==",
      "engines": {
        "node": ">=4.0.0"
      }
    },
    "node_modules/pg-pool": {
      "version": "3.6.1",
      "resolved": "https://registry.npmjs.org/pg-pool/-/pg-pool-3.6.1.tgz",
      "integrity": "sha512-jizsIzhkIitxCGfPRzJn1ZdcosIt3pz9Sh3V01fm1vZnbnCMgmGl5wvGGdNN2EL9Rmb0EcFoCkixH4Pu+sP9Og==",
      "peerDependencies": {
        "pg": ">=8.0"
      }
    },
    "node_modules/pg-protocol": {
      "version": "1.6.0",
      "resolved": "https://registry.npmjs.org/pg-protocol/-/pg-protocol-1.6.0.tgz",
      "integrity": "sha512-M+PDm637OY5WM307051+bsDia5Xej6d9IR4GwJse1qA1DIhiKlksvrneZOYQq42OM+spubpcNYEo2FcKQrDk+Q=="
    },
    "node_modules/pg-types": {
      "version": "2.2.0",
      "resolved": "https://registry.npmjs.org/pg-types/-/pg-types-2.2.0.tgz",
      "integrity": "sha512-qTAAlrEsl8s4OiEQY69wDvcMIdQN6wdz5ojQiOy6YRMuynxenON0O5oCpJI6lshc6scgAY8qvJ2On/p+CXY0GA==",
      "dependencies": {
        "pg-int8": "1.0.1",
        "postgres-array": "~2.0.0",
        "postgres-bytea": "~1.0.0",
        "postgres-date": "~1.0.4",
        "postgres-interval": "^1.1.0"
      },
      "engines": {
        "node": ">=4"
      }
    },
    "node_modules/pgpass": {
      "version": "1.0.5",
      "resolved": "https://registry.npmjs.org/pgpass/-/pgpass-1.0.5.tgz",
      "integrity": "sha512-FdW9r/jQZhSeohs1Z3sI1yxFQNFvMcnmfuj4WBMUTxOrAyLMaTcE1aAMBiTlbMNaXvBCQuVi0R7hd8udDSP7ug==",
      "dependencies": {
        "split2": "^4.1.0"
      }
    },
    "node_modules/picomatch": {
      "version": "2.3.1",
      "resolved": "https://registry.npmjs.org/picomatch/-/picomatch-2.3.1.tgz",
      "integrity": "sha512-JU3teHTNjmE2VCGFzuY8EXzCDVwEqB2a8fsIvwaStHhAWJEeVd1o1QD80CU6+ZdEXXSLbSsuLwJjkCBWqRQUVA==",
      "dev": true,
      "engines": {
        "node": ">=8.6"
      },
      "funding": {
        "url": "https://github.com/sponsors/jonschlinkert"
      }
    },
    "node_modules/postgres-array": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/postgres-array/-/postgres-array-2.0.0.tgz",
      "integrity": "sha512-VpZrUqU5A69eQyW2c5CA1jtLecCsN2U/bD6VilrFDWq5+5UIEVO7nazS3TEcHf1zuPYO/sqGvUvW62g86RXZuA==",
      "engines": {
        "node": ">=4"
      }
    },
    "node_modules/postgres-bytea": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/postgres-bytea/-/postgres-bytea-1.0.0.tgz",
      "integrity": "sha512-xy3pmLuQqRBZBXDULy7KbaitYqLcmxigw14Q5sj8QBVLqEwXfeybIKVWiqAXTlcvdvb0+xkOtDbfQMOf4lST1w==",
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/postgres-date": {
      "version": "1.0.7",
      "resolved": "https://registry.npmjs.org/postgres-date/-/postgres-date-1.0.7.tgz",
      "integrity": "sha512-suDmjLVQg78nMK2UZ454hAG+OAW+HQPZ6n++TNDUX+L0+uUlLywnoxJKDou51Zm+zTCjrCl0Nq6J9C5hP9vK/Q==",
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/postgres-interval": {
      "version": "1.2.0",
      "resolved": "https://registry.npmjs.org/postgres-interval/-/postgres-interval-1.2.0.tgz",
      "integrity": "sha512-9ZhXKM/rw350N1ovuWHbGxnGh/SNJ4cnxHiM0rxE4VN41wsg8P8zWn9hv/buK00RP4WvlOyr/RBDiptyxVbkZQ==",
      "dependencies": {
        "xtend": "^4.0.0"
      },
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/process-nextick-args": {
      "version": "2.0.1",
      "resolved": "https://registry.npmjs.org/process-nextick-args/-/process-nextick-args-2.0.1.tgz",
      "integrity": "sha512-3ouUOpQhtgrbOa17J7+uxOTpITYWaGP7/AhoR3+A+/1e9skrzelGi/dXzEYyvbxubEF6Wn2ypscTKiKJFFn1ag=="
    },
    "node_modules/promise": {
      "version": "8.3.0",
      "resolved": "https://registry.npmjs.org/promise/-/promise-8.3.0.tgz",
      "integrity": "sha512-rZPNPKTOYVNEEKFaq1HqTgOwZD+4/YHS5ukLzQCypkj+OkYx7iv0mA91lJlpPPZ8vMau3IIGj5Qlwrx+8iiSmg==",
      "dependencies": {
        "asap": "~2.0.6"
      }
    },
    "node_modules/proto-list": {
      "version": "1.2.4",
      "resolved": "https://registry.npmjs.org/proto-list/-/proto-list-1.2.4.tgz",
      "integrity": "sha512-vtK/94akxsTMhe0/cbfpR+syPuszcuwhqVjJq26CuNDgFGj682oRBXOP5MJpv2r7JtE8MsiepGIqvvOTBwn2vA==",
      "dev": true
    },
    "node_modules/proxy-addr": {
      "version": "2.0.7",
      "resolved": "https://registry.npmjs.org/proxy-addr/-/proxy-addr-2.0.7.tgz",
      "integrity": "sha512-llQsMLSUDUPT44jdrU/O37qlnifitDP+ZwrmmZcoSKyLKvtZxpyV0n2/bD/N4tBAAZ/gJEdZU7KMraoK1+XYAg==",
      "dependencies": {
        "forwarded": "0.2.0",
        "ipaddr.js": "1.9.1"
      },
      "engines": {
        "node": ">= 0.10"
      }
    },
    "node_modules/pstree.remy": {
      "version": "1.1.8",
      "resolved": "https://registry.npmjs.org/pstree.remy/-/pstree.remy-1.1.8.tgz",
      "integrity": "sha512-77DZwxQmxKnu3aR542U+X8FypNzbfJ+C5XQDk3uWjWxn6151aIMGthWYRXTqT1E5oJvg+ljaa2OJi+VfvCOQ8w==",
      "dev": true
    },
    "node_modules/punycode": {
      "version": "1.3.2",
      "resolved": "https://registry.npmjs.org/punycode/-/punycode-1.3.2.tgz",
      "integrity": "sha512-RofWgt/7fL5wP1Y7fxE7/EmTLzQVnB0ycyibJ0OOHIlJqTNzglYFxVwETOcIoJqJmpDXJ9xImDv+Fq34F/d4Dw=="
    },
    "node_modules/qs": {
      "version": "6.11.0",
      "resolved": "https://registry.npmjs.org/qs/-/qs-6.11.0.tgz",
      "integrity": "sha512-MvjoMCJwEarSbUYk5O+nmoSzSutSsTwF85zcHPQ9OrlFoZOYIjaqBAJIqIXjptyD5vThxGq52Xu/MaJzRkIk4Q==",
      "dependencies": {
        "side-channel": "^1.0.4"
      },
      "engines": {
        "node": ">=0.6"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/querystring": {
      "version": "0.2.0",
      "resolved": "https://registry.npmjs.org/querystring/-/querystring-0.2.0.tgz",
      "integrity": "sha512-X/xY82scca2tau62i9mDyU9K+I+djTMUsvwf7xnUX5GLvVzgJybOJf4Y6o9Zx3oJK/LSXg5tTZBjwzqVPaPO2g==",
      "deprecated": "The querystring API is considered Legacy. new code should use the URLSearchParams API instead.",
      "engines": {
        "node": ">=0.4.x"
      }
    },
    "node_modules/querystringify": {
      "version": "2.2.0",
      "resolved": "https://registry.npmjs.org/querystringify/-/querystringify-2.2.0.tgz",
      "integrity": "sha512-FIqgj2EUvTa7R50u0rGsyTftzjYmv/a3hO345bZNrqabNqjtgiDMgmo4mkUjd+nzU5oF3dClKqFIPUKybUyqoQ=="
    },
    "node_modules/range-parser": {
      "version": "1.2.1",
      "resolved": "https://registry.npmjs.org/range-parser/-/range-parser-1.2.1.tgz",
      "integrity": "sha512-Hrgsx+orqoygnmhFbKaHE6c296J+HTAQXoxEF6gNupROmmGJRoyzfG3ccAveqCBrwr/2yxQ5BVd/GTl5agOwSg==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/raw-body": {
      "version": "2.5.2",
      "resolved": "https://registry.npmjs.org/raw-body/-/raw-body-2.5.2.tgz",
      "integrity": "sha512-8zGqypfENjCIqGhgXToC8aB2r7YrBX+AQAfIPs/Mlk+BtPTztOvTS01NRW/3Eh60J+a48lt8qsCzirQ6loCVfA==",
      "dependencies": {
        "bytes": "3.1.2",
        "http-errors": "2.0.0",
        "iconv-lite": "0.4.24",
        "unpipe": "1.0.0"
      },
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/readable-stream": {
      "version": "3.6.2",
      "resolved": "https://registry.npmjs.org/readable-stream/-/readable-stream-3.6.2.tgz",
      "integrity": "sha512-9u/sniCrY3D5WdsERHzHE4G2YCXqoG5FTHUiCC4SIbr6XcLZBY05ya9EKjYek9O5xOAwjGq+1JdGBAS7Q9ScoA==",
      "dependencies": {
        "inherits": "^2.0.3",
        "string_decoder": "^1.1.1",
        "util-deprecate": "^1.0.1"
      },
      "engines": {
        "node": ">= 6"
      }
    },
    "node_modules/readdirp": {
      "version": "3.6.0",
      "resolved": "https://registry.npmjs.org/readdirp/-/readdirp-3.6.0.tgz",
      "integrity": "sha512-hOS089on8RduqdbhvQ5Z37A0ESjsqz6qnRcffsMU3495FuTdqSm+7bhJ29JvIOsBDEEnan5DPu9t3To9VRlMzA==",
      "dev": true,
      "dependencies": {
        "picomatch": "^2.2.1"
      },
      "engines": {
        "node": ">=8.10.0"
      }
    },
    "node_modules/redis": {
      "version": "4.6.13",
      "resolved": "https://registry.npmjs.org/redis/-/redis-4.6.13.tgz",
      "integrity": "sha512-MHgkS4B+sPjCXpf+HfdetBwbRz6vCtsceTmw1pHNYJAsYxrfpOP6dz+piJWGos8wqG7qb3vj/Rrc5qOlmInUuA==",
      "dependencies": {
        "@redis/bloom": "1.2.0",
        "@redis/client": "1.5.14",
        "@redis/graph": "1.1.1",
        "@redis/json": "1.0.6",
        "@redis/search": "1.1.6",
        "@redis/time-series": "1.0.5"
      }
    },
    "node_modules/request-ip": {
      "version": "3.3.0",
      "resolved": "https://registry.npmjs.org/request-ip/-/request-ip-3.3.0.tgz",
      "integrity": "sha512-cA6Xh6e0fDBBBwH77SLJaJPBmD3nWVAcF9/XAcsrIHdjhFzFiB5aNQFytdjCGPezU3ROwrR11IddKAM08vohxA=="
    },
    "node_modules/require-directory": {
      "version": "2.1.1",
      "resolved": "https://registry.npmjs.org/require-directory/-/require-directory-2.1.1.tgz",
      "integrity": "sha512-fGxEI7+wsG9xrvdjsrlmL22OMTTiHRwAMroiEeMgq8gzoLC/PQr7RsRDSTLUg/bZAZtF+TVIkHc6/4RIKrui+Q==",
      "dev": true,
      "engines": {
        "node": ">=0.10.0"
      }
    },
    "node_modules/requires-port": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/requires-port/-/requires-port-1.0.0.tgz",
      "integrity": "sha512-KigOCHcocU3XODJxsu8i/j8T9tzT4adHiecwORRQ0ZZFcp7ahwXuRU1m+yuO90C5ZUyGeGfocHDI14M3L3yDAQ=="
    },
    "node_modules/resolve": {
      "version": "1.22.4",
      "resolved": "https://registry.npmjs.org/resolve/-/resolve-1.22.4.tgz",
      "integrity": "sha512-PXNdCiPqDqeUou+w1C2eTQbNfxKSuMxqTCuvlmmMsk1NWHL5fRrhY6Pl0qEYYc6+QqGClco1Qj8XnjPego4wfg==",
      "dev": true,
      "dependencies": {
        "is-core-module": "^2.13.0",
        "path-parse": "^1.0.7",
        "supports-preserve-symlinks-flag": "^1.0.0"
      },
      "bin": {
        "resolve": "bin/resolve"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/retry-as-promised": {
      "version": "7.0.4",
      "resolved": "https://registry.npmjs.org/retry-as-promised/-/retry-as-promised-7.0.4.tgz",
      "integrity": "sha512-XgmCoxKWkDofwH8WddD0w85ZfqYz+ZHlr5yo+3YUCfycWawU56T5ckWXsScsj5B8tqUcIG67DxXByo3VUgiAdA=="
    },
    "node_modules/rimraf": {
      "version": "3.0.2",
      "resolved": "https://registry.npmjs.org/rimraf/-/rimraf-3.0.2.tgz",
      "integrity": "sha512-JZkJMZkAGFFPP2YqXZXPbMlMBgsxzE8ILs4lMIX/2o0L9UBw9O/Y3o6wFw/i9YLapcUJWwqbi3kdxIPdC62TIA==",
      "dependencies": {
        "glob": "^7.1.3"
      },
      "bin": {
        "rimraf": "bin.js"
      },
      "funding": {
        "url": "https://github.com/sponsors/isaacs"
      }
    },
    "node_modules/safe-buffer": {
      "version": "5.2.1",
      "resolved": "https://registry.npmjs.org/safe-buffer/-/safe-buffer-5.2.1.tgz",
      "integrity": "sha512-rp3So07KcdmmKbGvgaNxQSJr7bGVSVk5S9Eq1F+ppbRo70+YeaDxkw5Dd8NPN+GD6bjnYm2VuPuCXmpuYvmCXQ==",
      "funding": [
        {
          "type": "github",
          "url": "https://github.com/sponsors/feross"
        },
        {
          "type": "patreon",
          "url": "https://www.patreon.com/feross"
        },
        {
          "type": "consulting",
          "url": "https://feross.org/support"
        }
      ]
    },
    "node_modules/safer-buffer": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/safer-buffer/-/safer-buffer-2.1.2.tgz",
      "integrity": "sha512-YZo3K82SD7Riyi0E1EQPojLz7kpepnSQI9IyPbHHg1XXXevb5dJI7tpyN2ADxGcQbHG7vcyRHk0cbwqcQriUtg=="
    },
    "node_modules/sax": {
      "version": "1.2.1",
      "resolved": "https://registry.npmjs.org/sax/-/sax-1.2.1.tgz",
      "integrity": "sha512-8I2a3LovHTOpm7NV5yOyO8IHqgVsfK4+UuySrXU8YXkSRX7k6hCV9b3HrkKCr3nMpgj+0bmocaJJWpvp1oc7ZA=="
    },
    "node_modules/scmp": {
      "version": "2.1.0",
      "resolved": "https://registry.npmjs.org/scmp/-/scmp-2.1.0.tgz",
      "integrity": "sha512-o/mRQGk9Rcer/jEEw/yw4mwo3EU/NvYvp577/Btqrym9Qy5/MdWGBqipbALgd2lrdWTJ5/gqDusxfnQBxOxT2Q=="
    },
    "node_modules/semver": {
      "version": "7.5.4",
      "resolved": "https://registry.npmjs.org/semver/-/semver-7.5.4.tgz",
      "integrity": "sha512-1bCSESV6Pv+i21Hvpxp3Dx+pSD8lIPt8uVjRrxAUt/nbswYc+tK6Y2btiULjd4+fnq15PX+nqQDC7Oft7WkwcA==",
      "dependencies": {
        "lru-cache": "^6.0.0"
      },
      "bin": {
        "semver": "bin/semver.js"
      },
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/send": {
      "version": "0.18.0",
      "resolved": "https://registry.npmjs.org/send/-/send-0.18.0.tgz",
      "integrity": "sha512-qqWzuOjSFOuqPjFe4NOsMLafToQQwBSOEpS+FwEt3A2V3vKubTquT3vmLTQpFgMXp8AlFWFuP1qKaJZOtPpVXg==",
      "dependencies": {
        "debug": "2.6.9",
        "depd": "2.0.0",
        "destroy": "1.2.0",
        "encodeurl": "~1.0.2",
        "escape-html": "~1.0.3",
        "etag": "~1.8.1",
        "fresh": "0.5.2",
        "http-errors": "2.0.0",
        "mime": "1.6.0",
        "ms": "2.1.3",
        "on-finished": "2.4.1",
        "range-parser": "~1.2.1",
        "statuses": "2.0.1"
      },
      "engines": {
        "node": ">= 0.8.0"
      }
    },
    "node_modules/send/node_modules/ms": {
      "version": "2.1.3",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.3.tgz",
      "integrity": "sha512-6FlzubTLZG3J2a/NVCAleEhjzq5oxgHyaCU9yYXvcLsvoVaHJq/s5xXI6/XXP6tz7R9xAOtHnSO/tXtF3WRTlA=="
    },
    "node_modules/seq-queue": {
      "version": "0.0.5",
      "resolved": "https://registry.npmjs.org/seq-queue/-/seq-queue-0.0.5.tgz",
      "integrity": "sha512-hr3Wtp/GZIc/6DAGPDcV4/9WoZhjrkXsi5B/07QgX8tsdc6ilr7BFM6PM6rbdAX1kFSDYeZGLipIZZKyQP0O5Q=="
    },
    "node_modules/sequelize": {
      "version": "6.32.1",
      "resolved": "https://registry.npmjs.org/sequelize/-/sequelize-6.32.1.tgz",
      "integrity": "sha512-3Iv0jruv57Y0YvcxQW7BE56O7DC1BojcfIrqh6my+IQwde+9u/YnuYHzK+8kmZLhLvaziRT1eWu38nh9yVwn/g==",
      "funding": [
        {
          "type": "opencollective",
          "url": "https://opencollective.com/sequelize"
        }
      ],
      "dependencies": {
        "@types/debug": "^4.1.8",
        "@types/validator": "^13.7.17",
        "debug": "^4.3.4",
        "dottie": "^2.0.4",
        "inflection": "^1.13.4",
        "lodash": "^4.17.21",
        "moment": "^2.29.4",
        "moment-timezone": "^0.5.43",
        "pg-connection-string": "^2.6.0",
        "retry-as-promised": "^7.0.4",
        "semver": "^7.5.1",
        "sequelize-pool": "^7.1.0",
        "toposort-class": "^1.0.1",
        "uuid": "^8.3.2",
        "validator": "^13.9.0",
        "wkx": "^0.5.0"
      },
      "engines": {
        "node": ">=10.0.0"
      },
      "peerDependenciesMeta": {
        "ibm_db": {
          "optional": true
        },
        "mariadb": {
          "optional": true
        },
        "mysql2": {
          "optional": true
        },
        "oracledb": {
          "optional": true
        },
        "pg": {
          "optional": true
        },
        "pg-hstore": {
          "optional": true
        },
        "snowflake-sdk": {
          "optional": true
        },
        "sqlite3": {
          "optional": true
        },
        "tedious": {
          "optional": true
        }
      }
    },
    "node_modules/sequelize-cli": {
      "version": "6.6.1",
      "resolved": "https://registry.npmjs.org/sequelize-cli/-/sequelize-cli-6.6.1.tgz",
      "integrity": "sha512-C3qRpy1twBsFa855qOQFSYWer8ngiaZP05/OAsT1QCUwtc6UxVNNiQ0CGUt98T9T1gi5D3TGWL6le8HWUKELyw==",
      "dev": true,
      "dependencies": {
        "cli-color": "^2.0.3",
        "fs-extra": "^9.1.0",
        "js-beautify": "^1.14.5",
        "lodash": "^4.17.21",
        "resolve": "^1.22.1",
        "umzug": "^2.3.0",
        "yargs": "^16.2.0"
      },
      "bin": {
        "sequelize": "lib/sequelize",
        "sequelize-cli": "lib/sequelize"
      },
      "engines": {
        "node": ">=10.0.0"
      }
    },
    "node_modules/sequelize-pool": {
      "version": "7.1.0",
      "resolved": "https://registry.npmjs.org/sequelize-pool/-/sequelize-pool-7.1.0.tgz",
      "integrity": "sha512-G9c0qlIWQSK29pR/5U2JF5dDQeqqHRragoyahj/Nx4KOOQ3CPPfzxnfqFPCSB7x5UgjOgnZ61nSxz+fjDpRlJg==",
      "engines": {
        "node": ">= 10.0.0"
      }
    },
    "node_modules/sequelize/node_modules/debug": {
      "version": "4.3.4",
      "resolved": "https://registry.npmjs.org/debug/-/debug-4.3.4.tgz",
      "integrity": "sha512-PRWFHuSU3eDtQJPvnNY7Jcket1j0t5OuOsFzPPzsekD52Zl8qUfFIPEiswXqIvHWGVHOgX+7G/vCNNhehwxfkQ==",
      "dependencies": {
        "ms": "2.1.2"
      },
      "engines": {
        "node": ">=6.0"
      },
      "peerDependenciesMeta": {
        "supports-color": {
          "optional": true
        }
      }
    },
    "node_modules/sequelize/node_modules/ms": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.2.tgz",
      "integrity": "sha512-sGkPx+VjMtmA6MX27oA4FBFELFCZZ4S4XqeGOXCv68tT+jb3vk/RyaKWP0PTKyWtmLSM0b+adUTEvbs1PEaH2w=="
    },
    "node_modules/sequelize/node_modules/uuid": {
      "version": "8.3.2",
      "resolved": "https://registry.npmjs.org/uuid/-/uuid-8.3.2.tgz",
      "integrity": "sha512-+NYs2QeMWy+GWFOEm9xnn6HCDp0l7QBD7ml8zLUmJ+93Q5NF0NocErnwkTkXVFNiX3/fpC6afS8Dhb/gz7R7eg==",
      "bin": {
        "uuid": "dist/bin/uuid"
      }
    },
    "node_modules/serve-static": {
      "version": "1.15.0",
      "resolved": "https://registry.npmjs.org/serve-static/-/serve-static-1.15.0.tgz",
      "integrity": "sha512-XGuRDNjXUijsUL0vl6nSD7cwURuzEgglbOaFuZM9g3kwDXOWVTck0jLzjPzGD+TazWbboZYu52/9/XPdUgne9g==",
      "dependencies": {
        "encodeurl": "~1.0.2",
        "escape-html": "~1.0.3",
        "parseurl": "~1.3.3",
        "send": "0.18.0"
      },
      "engines": {
        "node": ">= 0.8.0"
      }
    },
    "node_modules/set-blocking": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/set-blocking/-/set-blocking-2.0.0.tgz",
      "integrity": "sha512-KiKBS8AnWGEyLzofFfmvKwpdPzqiy16LvQfK3yv/fVH7Bj13/wl3JSR1J+rfgRE9q7xUJK4qvgS8raSOeLUehw=="
    },
    "node_modules/setprototypeof": {
      "version": "1.2.0",
      "resolved": "https://registry.npmjs.org/setprototypeof/-/setprototypeof-1.2.0.tgz",
      "integrity": "sha512-E5LDX7Wrp85Kil5bhZv46j8jOeboKq5JMmYM3gVGdGH8xFpPWXUMsNrlODCrkoxMEeNi/XZIwuRvY4XNwYMJpw=="
    },
    "node_modules/side-channel": {
      "version": "1.0.4",
      "resolved": "https://registry.npmjs.org/side-channel/-/side-channel-1.0.4.tgz",
      "integrity": "sha512-q5XPytqFEIKHkGdiMIrY10mvLRvnQh42/+GoBlFW3b2LXLE2xxJpZFdm94we0BaoV3RwJyGqg5wS7epxTv0Zvw==",
      "dependencies": {
        "call-bind": "^1.0.0",
        "get-intrinsic": "^1.0.2",
        "object-inspect": "^1.9.0"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/signal-exit": {
      "version": "3.0.7",
      "resolved": "https://registry.npmjs.org/signal-exit/-/signal-exit-3.0.7.tgz",
      "integrity": "sha512-wnD2ZE+l+SPC/uoS0vXeE9L1+0wuaMqKlfz9AMUo38JsyLSBWSFcHR1Rri62LZc12vLr1gb3jl7iwQhgwpAbGQ=="
    },
    "node_modules/simple-update-notifier": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/simple-update-notifier/-/simple-update-notifier-2.0.0.tgz",
      "integrity": "sha512-a2B9Y0KlNXl9u/vsW6sTIu9vGEpfKu2wRV6l1H3XEas/0gUIzGzBoP/IouTcUQbm9JWZLH3COxyn03TYlFax6w==",
      "dev": true,
      "dependencies": {
        "semver": "^7.5.3"
      },
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/socket.io": {
      "version": "4.7.2",
      "resolved": "https://registry.npmjs.org/socket.io/-/socket.io-4.7.2.tgz",
      "integrity": "sha512-bvKVS29/I5fl2FGLNHuXlQaUH/BlzX1IN6S+NKLNZpBsPZIDH+90eQmCs2Railn4YUiww4SzUedJ6+uzwFnKLw==",
      "dependencies": {
        "accepts": "~1.3.4",
        "base64id": "~2.0.0",
        "cors": "~2.8.5",
        "debug": "~4.3.2",
        "engine.io": "~6.5.2",
        "socket.io-adapter": "~2.5.2",
        "socket.io-parser": "~4.2.4"
      },
      "engines": {
        "node": ">=10.2.0"
      }
    },
    "node_modules/socket.io-adapter": {
      "version": "2.5.2",
      "resolved": "https://registry.npmjs.org/socket.io-adapter/-/socket.io-adapter-2.5.2.tgz",
      "integrity": "sha512-87C3LO/NOMc+eMcpcxUBebGjkpMDkNBS9tf7KJqcDsmL936EChtVva71Dw2q4tQcuVC+hAUy4an2NO/sYXmwRA==",
      "dependencies": {
        "ws": "~8.11.0"
      }
    },
    "node_modules/socket.io-parser": {
      "version": "4.2.4",
      "resolved": "https://registry.npmjs.org/socket.io-parser/-/socket.io-parser-4.2.4.tgz",
      "integrity": "sha512-/GbIKmo8ioc+NIWIhwdecY0ge+qVBSMdgxGygevmdHj24bsfgtCmcUUcQ5ZzcylGFHsN3k4HB4Cgkl96KVnuew==",
      "dependencies": {
        "@socket.io/component-emitter": "~3.1.0",
        "debug": "~4.3.1"
      },
      "engines": {
        "node": ">=10.0.0"
      }
    },
    "node_modules/socket.io-parser/node_modules/debug": {
      "version": "4.3.4",
      "resolved": "https://registry.npmjs.org/debug/-/debug-4.3.4.tgz",
      "integrity": "sha512-PRWFHuSU3eDtQJPvnNY7Jcket1j0t5OuOsFzPPzsekD52Zl8qUfFIPEiswXqIvHWGVHOgX+7G/vCNNhehwxfkQ==",
      "dependencies": {
        "ms": "2.1.2"
      },
      "engines": {
        "node": ">=6.0"
      },
      "peerDependenciesMeta": {
        "supports-color": {
          "optional": true
        }
      }
    },
    "node_modules/socket.io-parser/node_modules/ms": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.2.tgz",
      "integrity": "sha512-sGkPx+VjMtmA6MX27oA4FBFELFCZZ4S4XqeGOXCv68tT+jb3vk/RyaKWP0PTKyWtmLSM0b+adUTEvbs1PEaH2w=="
    },
    "node_modules/socket.io/node_modules/debug": {
      "version": "4.3.4",
      "resolved": "https://registry.npmjs.org/debug/-/debug-4.3.4.tgz",
      "integrity": "sha512-PRWFHuSU3eDtQJPvnNY7Jcket1j0t5OuOsFzPPzsekD52Zl8qUfFIPEiswXqIvHWGVHOgX+7G/vCNNhehwxfkQ==",
      "dependencies": {
        "ms": "2.1.2"
      },
      "engines": {
        "node": ">=6.0"
      },
      "peerDependenciesMeta": {
        "supports-color": {
          "optional": true
        }
      }
    },
    "node_modules/socket.io/node_modules/ms": {
      "version": "2.1.2",
      "resolved": "https://registry.npmjs.org/ms/-/ms-2.1.2.tgz",
      "integrity": "sha512-sGkPx+VjMtmA6MX27oA4FBFELFCZZ4S4XqeGOXCv68tT+jb3vk/RyaKWP0PTKyWtmLSM0b+adUTEvbs1PEaH2w=="
    },
    "node_modules/sorted-array-functions": {
      "version": "1.3.0",
      "resolved": "https://registry.npmjs.org/sorted-array-functions/-/sorted-array-functions-1.3.0.tgz",
      "integrity": "sha512-2sqgzeFlid6N4Z2fUQ1cvFmTOLRi/sEDzSQ0OKYchqgoPmQBVyM3959qYx3fpS6Esef80KjmpgPeEr028dP3OA=="
    },
    "node_modules/split2": {
      "version": "4.2.0",
      "resolved": "https://registry.npmjs.org/split2/-/split2-4.2.0.tgz",
      "integrity": "sha512-UcjcJOWknrNkF6PLX83qcHM6KHgVKNkV62Y8a5uYDVv9ydGQVwAHMKqHdJje1VTWpljG0WYpCDhrCdAOYH4TWg==",
      "engines": {
        "node": ">= 10.x"
      }
    },
    "node_modules/sqlstring": {
      "version": "2.3.3",
      "resolved": "https://registry.npmjs.org/sqlstring/-/sqlstring-2.3.3.tgz",
      "integrity": "sha512-qC9iz2FlN7DQl3+wjwn3802RTyjCx7sDvfQEXchwa6CWOx07/WVfh91gBmQ9fahw8snwGEWU3xGzOt4tFyHLxg==",
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/statuses": {
      "version": "2.0.1",
      "resolved": "https://registry.npmjs.org/statuses/-/statuses-2.0.1.tgz",
      "integrity": "sha512-RwNA9Z/7PrK06rYLIzFMlaF+l73iwpzsqRIFgbMLbTcLD6cOao82TaWefPXQvB2fOC4AjuYSEndS7N/mTCbkdQ==",
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/streamsearch": {
      "version": "1.1.0",
      "resolved": "https://registry.npmjs.org/streamsearch/-/streamsearch-1.1.0.tgz",
      "integrity": "sha512-Mcc5wHehp9aXz1ax6bZUyY5afg9u2rv5cqQI3mRrYkGC8rW2hM02jWuwjtL++LS5qinSyhj2QfLyNsuc+VsExg==",
      "engines": {
        "node": ">=10.0.0"
      }
    },
    "node_modules/string_decoder": {
      "version": "1.3.0",
      "resolved": "https://registry.npmjs.org/string_decoder/-/string_decoder-1.3.0.tgz",
      "integrity": "sha512-hkRX8U1WjJFd8LsDJ2yQ/wWWxaopEsABU1XfkM8A+j0+85JAGppt16cr1Whg6KIbb4okU6Mql6BOj+uup/wKeA==",
      "dependencies": {
        "safe-buffer": "~5.2.0"
      }
    },
    "node_modules/string-width": {
      "version": "4.2.3",
      "resolved": "https://registry.npmjs.org/string-width/-/string-width-4.2.3.tgz",
      "integrity": "sha512-wKyQRQpjJ0sIp62ErSZdGsjMJWsap5oRNihHhu6G7JVO/9jIB6UyevL+tXuOqrng8j/cxKTWyWUwvSTriiZz/g==",
      "dependencies": {
        "emoji-regex": "^8.0.0",
        "is-fullwidth-code-point": "^3.0.0",
        "strip-ansi": "^6.0.1"
      },
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/strip-ansi": {
      "version": "6.0.1",
      "resolved": "https://registry.npmjs.org/strip-ansi/-/strip-ansi-6.0.1.tgz",
      "integrity": "sha512-Y38VPSHcqkFrCpFnQ9vuSXmquuv5oXOKpGeT6aGrr3o3Gc9AlVa6JBfUSOCnbxGGZF+/0ooI7KrPuUSztUdU5A==",
      "dependencies": {
        "ansi-regex": "^5.0.1"
      },
      "engines": {
        "node": ">=8"
      }
    },
    "node_modules/supports-color": {
      "version": "5.5.0",
      "resolved": "https://registry.npmjs.org/supports-color/-/supports-color-5.5.0.tgz",
      "integrity": "sha512-QjVjwdXIt408MIiAqCX4oUKsgU2EqAGzs2Ppkm4aQYbjm+ZEWEcW4SfFNTr4uMNZma0ey4f5lgLrkB0aX0QMow==",
      "dev": true,
      "dependencies": {
        "has-flag": "^3.0.0"
      },
      "engines": {
        "node": ">=4"
      }
    },
    "node_modules/supports-preserve-symlinks-flag": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/supports-preserve-symlinks-flag/-/supports-preserve-symlinks-flag-1.0.0.tgz",
      "integrity": "sha512-ot0WnXS9fgdkgIcePe6RHNk1WA8+muPa6cSjeR3V8K27q9BB1rTE3R1p7Hv0z1ZyAc8s6Vvv8DIyWf681MAt0w==",
      "dev": true,
      "engines": {
        "node": ">= 0.4"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/sync-request": {
      "version": "6.1.0",
      "resolved": "https://registry.npmjs.org/sync-request/-/sync-request-6.1.0.tgz",
      "integrity": "sha512-8fjNkrNlNCrVc/av+Jn+xxqfCjYaBoHqCsDz6mt030UMxJGr+GSfCV1dQt2gRtlL63+VPidwDVLr7V2OcTSdRw==",
      "dependencies": {
        "http-response-object": "^3.0.1",
        "sync-rpc": "^1.2.1",
        "then-request": "^6.0.0"
      },
      "engines": {
        "node": ">=8.0.0"
      }
    },
    "node_modules/sync-rpc": {
      "version": "1.3.6",
      "resolved": "https://registry.npmjs.org/sync-rpc/-/sync-rpc-1.3.6.tgz",
      "integrity": "sha512-J8jTXuZzRlvU7HemDgHi3pGnh/rkoqR/OZSjhTyyZrEkkYQbk7Z33AXp37mkPfPpfdOuj7Ex3H/TJM1z48uPQw==",
      "dependencies": {
        "get-port": "^3.1.0"
      }
    },
    "node_modules/tar": {
      "version": "6.1.15",
      "resolved": "https://registry.npmjs.org/tar/-/tar-6.1.15.tgz",
      "integrity": "sha512-/zKt9UyngnxIT/EAGYuxaMYgOIJiP81ab9ZfkILq4oNLPFX50qyYmu7jRj9qeXoxmJHjGlbH0+cm2uy1WCs10A==",
      "dependencies": {
        "chownr": "^2.0.0",
        "fs-minipass": "^2.0.0",
        "minipass": "^5.0.0",
        "minizlib": "^2.1.1",
        "mkdirp": "^1.0.3",
        "yallist": "^4.0.0"
      },
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/then-request": {
      "version": "6.0.2",
      "resolved": "https://registry.npmjs.org/then-request/-/then-request-6.0.2.tgz",
      "integrity": "sha512-3ZBiG7JvP3wbDzA9iNY5zJQcHL4jn/0BWtXIkagfz7QgOL/LqjCEOBQuJNZfu0XYnv5JhKh+cDxCPM4ILrqruA==",
      "dependencies": {
        "@types/concat-stream": "^1.6.0",
        "@types/form-data": "0.0.33",
        "@types/node": "^8.0.0",
        "@types/qs": "^6.2.31",
        "caseless": "~0.12.0",
        "concat-stream": "^1.6.0",
        "form-data": "^2.2.0",
        "http-basic": "^8.1.1",
        "http-response-object": "^3.0.1",
        "promise": "^8.0.0",
        "qs": "^6.4.0"
      },
      "engines": {
        "node": ">=6.0.0"
      }
    },
    "node_modules/then-request/node_modules/@types/node": {
      "version": "8.10.66",
      "resolved": "https://registry.npmjs.org/@types/node/-/node-8.10.66.tgz",
      "integrity": "sha512-tktOkFUA4kXx2hhhrB8bIFb5TbwzS4uOhKEmwiD+NoiL0qtP2OQ9mFldbgD4dV1djrlBYP6eBuQZiWjuHUpqFw=="
    },
    "node_modules/timers-ext": {
      "version": "0.1.7",
      "resolved": "https://registry.npmjs.org/timers-ext/-/timers-ext-0.1.7.tgz",
      "integrity": "sha512-b85NUNzTSdodShTIbky6ZF02e8STtVVfD+fu4aXXShEELpozH+bCpJLYMPZbsABN2wDH7fJpqIoXxJpzbf0NqQ==",
      "dev": true,
      "dependencies": {
        "es5-ext": "~0.10.46",
        "next-tick": "1"
      }
    },
    "node_modules/to-regex-range": {
      "version": "5.0.1",
      "resolved": "https://registry.npmjs.org/to-regex-range/-/to-regex-range-5.0.1.tgz",
      "integrity": "sha512-65P7iz6X5yEr1cwcgvQxbbIw7Uk3gOy5dIdtZ4rDveLqhrdJP+Li/Hx6tyK0NEb+2GCyneCMJiGqrADCSNk8sQ==",
      "dev": true,
      "dependencies": {
        "is-number": "^7.0.0"
      },
      "engines": {
        "node": ">=8.0"
      }
    },
    "node_modules/toidentifier": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/toidentifier/-/toidentifier-1.0.1.tgz",
      "integrity": "sha512-o5sSPKEkg/DIQNmH43V0/uerLrpzVedkUh8tGNvaeXpfpuwjKenlSox/2O/BTlZUtEe+JG7s5YhEz608PlAHRA==",
      "engines": {
        "node": ">=0.6"
      }
    },
    "node_modules/toposort-class": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/toposort-class/-/toposort-class-1.0.1.tgz",
      "integrity": "sha512-OsLcGGbYF3rMjPUf8oKktyvCiUxSbqMMS39m33MAjLTC1DVIH6x3WSt63/M77ihI09+Sdfk1AXvfhCEeUmC7mg=="
    },
    "node_modules/touch": {
      "version": "3.1.0",
      "resolved": "https://registry.npmjs.org/touch/-/touch-3.1.0.tgz",
      "integrity": "sha512-WBx8Uy5TLtOSRtIq+M03/sKDrXCLHxwDcquSP2c43Le03/9serjQBIztjRz6FkJez9D/hleyAXTBGLwwZUw9lA==",
      "dev": true,
      "dependencies": {
        "nopt": "~1.0.10"
      },
      "bin": {
        "nodetouch": "bin/nodetouch.js"
      }
    },
    "node_modules/touch/node_modules/nopt": {
      "version": "1.0.10",
      "resolved": "https://registry.npmjs.org/nopt/-/nopt-1.0.10.tgz",
      "integrity": "sha512-NWmpvLSqUrgrAC9HCuxEvb+PSloHpqVu+FqcO4eeF2h5qYRhA7ev6KvelyQAKtegUbC6RypJnlEOhd8vloNKYg==",
      "dev": true,
      "dependencies": {
        "abbrev": "1"
      },
      "bin": {
        "nopt": "bin/nopt.js"
      },
      "engines": {
        "node": "*"
      }
    },
    "node_modules/tr46": {
      "version": "0.0.3",
      "resolved": "https://registry.npmjs.org/tr46/-/tr46-0.0.3.tgz",
      "integrity": "sha512-N3WMsuqV66lT30CrXNbEjx4GEwlow3v6rr4mCcv6prnfwhS01rkgyFdjPNBYd9br7LpXV1+Emh01fHnq2Gdgrw=="
    },
    "node_modules/tree-kill": {
      "version": "1.2.2",
      "resolved": "https://registry.npmjs.org/tree-kill/-/tree-kill-1.2.2.tgz",
      "integrity": "sha512-L0Orpi8qGpRG//Nd+H90vFB+3iHnue1zSSGmNOOCh1GLJ7rUKVwV2HvijphGQS2UmhUZewS9VgvxYIdgr+fG1A==",
      "bin": {
        "tree-kill": "cli.js"
      }
    },
    "node_modules/tslib": {
      "version": "1.14.1",
      "resolved": "https://registry.npmjs.org/tslib/-/tslib-1.14.1.tgz",
      "integrity": "sha512-Xni35NKzjgMrwevysHTCArtLDpPvye8zV/0E4EyYn43P7/7qvQwPh9BGkHewbMulVntbigmcT7rdX3BNo9wRJg=="
    },
    "node_modules/twilio": {
      "version": "4.15.0",
      "resolved": "https://registry.npmjs.org/twilio/-/twilio-4.15.0.tgz",
      "integrity": "sha512-wkHfIpAr2oOMfJ6A/Z6WD0jzLZZwMdbzys3XbslxNkmu4gGgRCre7o3IgaXRR5HIELEilbRhZjsmYHpG3fL8HA==",
      "dependencies": {
        "axios": "^0.26.1",
        "dayjs": "^1.11.9",
        "https-proxy-agent": "^5.0.0",
        "jsonwebtoken": "^9.0.0",
        "qs": "^6.9.4",
        "scmp": "^2.1.0",
        "url-parse": "^1.5.9",
        "xmlbuilder": "^13.0.2"
      },
      "engines": {
        "node": ">=14.0"
      }
    },
    "node_modules/twilio/node_modules/axios": {
      "version": "0.26.1",
      "resolved": "https://registry.npmjs.org/axios/-/axios-0.26.1.tgz",
      "integrity": "sha512-fPwcX4EvnSHuInCMItEhAGnaSEXRBjtzh9fOtsE6E1G6p7vl7edEeZe11QHf18+6+9gR5PbKV/sGKNaD8YaMeA==",
      "dependencies": {
        "follow-redirects": "^1.14.8"
      }
    },
    "node_modules/twilio/node_modules/xmlbuilder": {
      "version": "13.0.2",
      "resolved": "https://registry.npmjs.org/xmlbuilder/-/xmlbuilder-13.0.2.tgz",
      "integrity": "sha512-Eux0i2QdDYKbdbA6AM6xE4m6ZTZr4G4xF9kahI2ukSEMCzwce2eX9WlTI5J3s+NU7hpasFsr8hWIONae7LluAQ==",
      "engines": {
        "node": ">=6.0"
      }
    },
    "node_modules/type": {
      "version": "1.2.0",
      "resolved": "https://registry.npmjs.org/type/-/type-1.2.0.tgz",
      "integrity": "sha512-+5nt5AAniqsCnu2cEQQdpzCAh33kVx8n0VoFidKpB1dVVLAN/F+bgVOqOJqOnEnrhp222clB5p3vUlD+1QAnfg==",
      "dev": true
    },
    "node_modules/type-is": {
      "version": "1.6.18",
      "resolved": "https://registry.npmjs.org/type-is/-/type-is-1.6.18.tgz",
      "integrity": "sha512-TkRKr9sUTxEH8MdfuCSP7VizJyzRNMjj2J2do2Jr3Kym598JVdEksuzPQCnlFPW4ky9Q+iA+ma9BGm06XQBy8g==",
      "dependencies": {
        "media-typer": "0.3.0",
        "mime-types": "~2.1.24"
      },
      "engines": {
        "node": ">= 0.6"
      }
    },
    "node_modules/typedarray": {
      "version": "0.0.6",
      "resolved": "https://registry.npmjs.org/typedarray/-/typedarray-0.0.6.tgz",
      "integrity": "sha512-/aCDEGatGvZ2BIk+HmLf4ifCJFwvKFNb9/JeZPMulfgFracn9QFcAf5GO8B/mweUjSoblS5In0cWhqpfs/5PQA=="
    },
    "node_modules/uid2": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/uid2/-/uid2-1.0.0.tgz",
      "integrity": "sha512-+I6aJUv63YAcY9n4mQreLUt0d4lvwkkopDNmpomkAUz0fAkEMV9pRWxN0EjhW1YfRhcuyHg2v3mwddCDW1+LFQ==",
      "engines": {
        "node": ">= 4.0.0"
      }
    },
    "node_modules/umzug": {
      "version": "2.3.0",
      "resolved": "https://registry.npmjs.org/umzug/-/umzug-2.3.0.tgz",
      "integrity": "sha512-Z274K+e8goZK8QJxmbRPhl89HPO1K+ORFtm6rySPhFKfKc5GHhqdzD0SGhSWHkzoXasqJuItdhorSvY7/Cgflw==",
      "dev": true,
      "dependencies": {
        "bluebird": "^3.7.2"
      },
      "engines": {
        "node": ">=6.0.0"
      }
    },
    "node_modules/undefsafe": {
      "version": "2.0.5",
      "resolved": "https://registry.npmjs.org/undefsafe/-/undefsafe-2.0.5.tgz",
      "integrity": "sha512-WxONCrssBM8TSPRqN5EmsjVrsv4A8X12J4ArBiiayv3DyyG3ZlIg6yysuuSYdZsVz3TKcTg2fd//Ujd4CHV1iA==",
      "dev": true
    },
    "node_modules/underscore": {
      "version": "1.13.6",
      "resolved": "https://registry.npmjs.org/underscore/-/underscore-1.13.6.tgz",
      "integrity": "sha512-+A5Sja4HP1M08MaXya7p5LvjuM7K6q/2EaC0+iovj/wOcMsTzMvDFbasi/oSapiwOlt252IqsKqPjCl7huKS0A=="
    },
    "node_modules/universalify": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/universalify/-/universalify-2.0.0.tgz",
      "integrity": "sha512-hAZsKq7Yy11Zu1DE0OzWjw7nnLZmJZYTDZZyEFHZdUhV8FkH5MCfoU1XMaxXovpyW5nq5scPqq0ZDP9Zyl04oQ==",
      "dev": true,
      "engines": {
        "node": ">= 10.0.0"
      }
    },
    "node_modules/unpipe": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/unpipe/-/unpipe-1.0.0.tgz",
      "integrity": "sha512-pjy2bYhSsufwWlKwPc+l3cN7+wuJlK6uz0YdJEOlQDbl6jo/YlPi4mb8agUkVC8BF7V8NuzeyPNqRksA3hztKQ==",
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/url": {
      "version": "0.10.3",
      "resolved": "https://registry.npmjs.org/url/-/url-0.10.3.tgz",
      "integrity": "sha512-hzSUW2q06EqL1gKM/a+obYHLIO6ct2hwPuviqTTOcfFVc61UbfJ2Q32+uGL/HCPxKqrdGB5QUwIe7UqlDgwsOQ==",
      "dependencies": {
        "punycode": "1.3.2",
        "querystring": "0.2.0"
      }
    },
    "node_modules/url-parse": {
      "version": "1.5.10",
      "resolved": "https://registry.npmjs.org/url-parse/-/url-parse-1.5.10.tgz",
      "integrity": "sha512-WypcfiRhfeUP9vvF0j6rw0J3hrWrw6iZv3+22h6iRMJ/8z1Tj6XfLP4DsUix5MhMPnXpiHDoKyoZ/bdCkwBCiQ==",
      "dependencies": {
        "querystringify": "^2.1.1",
        "requires-port": "^1.0.0"
      }
    },
    "node_modules/url-template": {
      "version": "2.0.8",
      "resolved": "https://registry.npmjs.org/url-template/-/url-template-2.0.8.tgz",
      "integrity": "sha512-XdVKMF4SJ0nP/O7XIPB0JwAEuT9lDIYnNsK8yGVe43y0AWoKeJNdv3ZNWh7ksJ6KqQFjOO6ox/VEitLnaVNufw=="
    },
    "node_modules/util": {
      "version": "0.12.5",
      "resolved": "https://registry.npmjs.org/util/-/util-0.12.5.tgz",
      "integrity": "sha512-kZf/K6hEIrWHI6XqOFUiiMa+79wE/D8Q+NCNAWclkyg3b4d2k7s0QGepNjiABc+aR3N1PAyHL7p6UcLY6LmrnA==",
      "dependencies": {
        "inherits": "^2.0.3",
        "is-arguments": "^1.0.4",
        "is-generator-function": "^1.0.7",
        "is-typed-array": "^1.1.3",
        "which-typed-array": "^1.1.2"
      }
    },
    "node_modules/util-deprecate": {
      "version": "1.0.2",
      "resolved": "https://registry.npmjs.org/util-deprecate/-/util-deprecate-1.0.2.tgz",
      "integrity": "sha512-EPD5q1uXyFxJpCrLnCc1nHnq3gOa6DZBocAIiI2TaSCA7VCJ1UJDMagCzIkXNsUYfD1daK//LTEQ8xiIbrHtcw=="
    },
    "node_modules/utils-merge": {
      "version": "1.0.1",
      "resolved": "https://registry.npmjs.org/utils-merge/-/utils-merge-1.0.1.tgz",
      "integrity": "sha512-pMZTvIkT1d+TFGvDOqodOclx0QWkkgi6Tdoa8gC8ffGAAqz9pzPTZWAybbsHHoED/ztMtkv/VoYTYyShUn81hA==",
      "engines": {
        "node": ">= 0.4.0"
      }
    },
    "node_modules/uuid": {
      "version": "9.0.0",
      "resolved": "https://registry.npmjs.org/uuid/-/uuid-9.0.0.tgz",
      "integrity": "sha512-MXcSTerfPa4uqyzStbRoTgt5XIe3x5+42+q1sDuy3R5MDk66URdLMOZe5aPX/SQd+kuYAh0FdP/pO28IkQyTeg==",
      "bin": {
        "uuid": "dist/bin/uuid"
      }
    },
    "node_modules/validator": {
      "version": "13.9.0",
      "resolved": "https://registry.npmjs.org/validator/-/validator-13.9.0.tgz",
      "integrity": "sha512-B+dGG8U3fdtM0/aNK4/X8CXq/EcxU2WPrPEkJGslb47qyHsxmbggTWK0yEA4qnYVNF+nxNlN88o14hIcPmSIEA==",
      "engines": {
        "node": ">= 0.10"
      }
    },
    "node_modules/vary": {
      "version": "1.1.2",
      "resolved": "https://registry.npmjs.org/vary/-/vary-1.1.2.tgz",
      "integrity": "sha512-BNGbWLfd0eUPabhkXUVm0j8uuvREyTh5ovRa/dyow/BqAbZJyC+5fU+IzQOzmAKzYqYRAISoRhdQr3eIZ/PXqg==",
      "engines": {
        "node": ">= 0.8"
      }
    },
    "node_modules/web-streams-polyfill": {
      "version": "3.2.1",
      "resolved": "https://registry.npmjs.org/web-streams-polyfill/-/web-streams-polyfill-3.2.1.tgz",
      "integrity": "sha512-e0MO3wdXWKrLbL0DgGnUV7WHVuw9OUvL4hjgnPkIeEvESk74gAITi5G606JtZPp39cd8HA9VQzCIvA49LpPN5Q==",
      "engines": {
        "node": ">= 8"
      }
    },
    "node_modules/webidl-conversions": {
      "version": "3.0.1",
      "resolved": "https://registry.npmjs.org/webidl-conversions/-/webidl-conversions-3.0.1.tgz",
      "integrity": "sha512-2JAn3z8AR6rjK8Sm8orRC0h/bcl/DqL7tRPdGZ4I1CjdF+EaMLmYxBHyXuKL849eucPFhvBoxMsflfOb8kxaeQ=="
    },
    "node_modules/whatwg-url": {
      "version": "5.0.0",
      "resolved": "https://registry.npmjs.org/whatwg-url/-/whatwg-url-5.0.0.tgz",
      "integrity": "sha512-saE57nupxk6v3HY35+jzBwYa0rKSy0XR8JSxZPwgLr7ys0IBzhGviA1/TUGJLmSVqs8pb9AnvICXEuOHLprYTw==",
      "dependencies": {
        "tr46": "~0.0.3",
        "webidl-conversions": "^3.0.0"
      }
    },
    "node_modules/which-typed-array": {
      "version": "1.1.11",
      "resolved": "https://registry.npmjs.org/which-typed-array/-/which-typed-array-1.1.11.tgz",
      "integrity": "sha512-qe9UWWpkeG5yzZ0tNYxDmd7vo58HDBc39mZ0xWWpolAGADdFOzkfamWLDxkOWcvHQKVmdTyQdLD4NOfjLWTKew==",
      "dependencies": {
        "available-typed-arrays": "^1.0.5",
        "call-bind": "^1.0.2",
        "for-each": "^0.3.3",
        "gopd": "^1.0.1",
        "has-tostringtag": "^1.0.0"
      },
      "engines": {
        "node": ">= 0.4"
      },
      "funding": {
        "url": "https://github.com/sponsors/ljharb"
      }
    },
    "node_modules/wide-align": {
      "version": "1.1.5",
      "resolved": "https://registry.npmjs.org/wide-align/-/wide-align-1.1.5.tgz",
      "integrity": "sha512-eDMORYaPNZ4sQIuuYPDHdQvf4gyCF9rEEV/yPxGfwPkRodwEgiMUUXTx/dex+Me0wxx53S+NgUHaP7y3MGlDmg==",
      "dependencies": {
        "string-width": "^1.0.2 || 2 || 3 || 4"
      }
    },
    "node_modules/wkx": {
      "version": "0.5.0",
      "resolved": "https://registry.npmjs.org/wkx/-/wkx-0.5.0.tgz",
      "integrity": "sha512-Xng/d4Ichh8uN4l0FToV/258EjMGU9MGcA0HV2d9B/ZpZB3lqQm7nkOdZdm5GhKtLLhAE7PiVQwN4eN+2YJJUg==",
      "dependencies": {
        "@types/node": "*"
      }
    },
    "node_modules/wrap-ansi": {
      "version": "7.0.0",
      "resolved": "https://registry.npmjs.org/wrap-ansi/-/wrap-ansi-7.0.0.tgz",
      "integrity": "sha512-YVGIj2kamLSTxw6NsZjoBxfSwsn0ycdesmc4p+Q21c5zPuZ1pl+NfxVdxPtdHvmNVOQ6XSYG4AUtyt/Fi7D16Q==",
      "dev": true,
      "dependencies": {
        "ansi-styles": "^4.0.0",
        "string-width": "^4.1.0",
        "strip-ansi": "^6.0.0"
      },
      "engines": {
        "node": ">=10"
      },
      "funding": {
        "url": "https://github.com/chalk/wrap-ansi?sponsor=1"
      }
    },
    "node_modules/wrappy": {
      "version": "1.0.2",
      "resolved": "https://registry.npmjs.org/wrappy/-/wrappy-1.0.2.tgz",
      "integrity": "sha512-l4Sp/DRseor9wL6EvV2+TuQn63dMkPjZ/sp9XkghTEbV9KlPS1xUsZ3u7/IQO4wxtcFB4bgpQPRcR3QCvezPcQ=="
    },
    "node_modules/ws": {
      "version": "8.11.0",
      "resolved": "https://registry.npmjs.org/ws/-/ws-8.11.0.tgz",
      "integrity": "sha512-HPG3wQd9sNQoT9xHyNCXoDUa+Xw/VevmY9FoHyQ+g+rrMn4j6FB4np7Z0OhdTgjx6MgQLK7jwSy1YecU1+4Asg==",
      "engines": {
        "node": ">=10.0.0"
      },
      "peerDependencies": {
        "bufferutil": "^4.0.1",
        "utf-8-validate": "^5.0.2"
      },
      "peerDependenciesMeta": {
        "bufferutil": {
          "optional": true
        },
        "utf-8-validate": {
          "optional": true
        }
      }
    },
    "node_modules/xml2js": {
      "version": "0.5.0",
      "resolved": "https://registry.npmjs.org/xml2js/-/xml2js-0.5.0.tgz",
      "integrity": "sha512-drPFnkQJik/O+uPKpqSgr22mpuFHqKdbS835iAQrUC73L2F5WkboIRd63ai/2Yg6I1jzifPFKH2NTK+cfglkIA==",
      "dependencies": {
        "sax": ">=0.6.0",
        "xmlbuilder": "~11.0.0"
      },
      "engines": {
        "node": ">=4.0.0"
      }
    },
    "node_modules/xmlbuilder": {
      "version": "11.0.1",
      "resolved": "https://registry.npmjs.org/xmlbuilder/-/xmlbuilder-11.0.1.tgz",
      "integrity": "sha512-fDlsI/kFEx7gLvbecc0/ohLG50fugQp8ryHzMTuW9vSa1GJ0XYWKnhsUx7oie3G98+r56aTQIUB4kht42R3JvA==",
      "engines": {
        "node": ">=4.0"
      }
    },
    "node_modules/xtend": {
      "version": "4.0.2",
      "resolved": "https://registry.npmjs.org/xtend/-/xtend-4.0.2.tgz",
      "integrity": "sha512-LKYU1iAXJXUgAXn9URjiu+MWhyUXHsvfp7mcuYm9dSUKK0/CjtrUwFAxD82/mCWbtLsGjFIad0wIsod4zrTAEQ==",
      "engines": {
        "node": ">=0.4"
      }
    },
    "node_modules/y18n": {
      "version": "5.0.8",
      "resolved": "https://registry.npmjs.org/y18n/-/y18n-5.0.8.tgz",
      "integrity": "sha512-0pfFzegeDWJHJIAmTLRP2DwHjdF5s7jo9tuztdQxAhINCdvS+3nGINqPd00AphqJR/0LhANUS6/+7SCb98YOfA==",
      "dev": true,
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/yallist": {
      "version": "4.0.0",
      "resolved": "https://registry.npmjs.org/yallist/-/yallist-4.0.0.tgz",
      "integrity": "sha512-3wdGidZyq5PB084XLES5TpOSRA3wjXAlIWMhum2kRcv/41Sn2emQ0dycQW4uZXLejwKvg6EsvbdlVL+FYEct7A=="
    },
    "node_modules/yargs": {
      "version": "16.2.0",
      "resolved": "https://registry.npmjs.org/yargs/-/yargs-16.2.0.tgz",
      "integrity": "sha512-D1mvvtDG0L5ft/jGWkLpG1+m0eQxOfaBvTNELraWj22wSVUMWxZUvYgJYcKh6jGGIkJFhH4IZPQhR4TKpc8mBw==",
      "dev": true,
      "dependencies": {
        "cliui": "^7.0.2",
        "escalade": "^3.1.1",
        "get-caller-file": "^2.0.5",
        "require-directory": "^2.1.1",
        "string-width": "^4.2.0",
        "y18n": "^5.0.5",
        "yargs-parser": "^20.2.2"
      },
      "engines": {
        "node": ">=10"
      }
    },
    "node_modules/yargs-parser": {
      "version": "20.2.9",
      "resolved": "https://registry.npmjs.org/yargs-parser/-/yargs-parser-20.2.9.tgz",
      "integrity": "sha512-y11nGElTIV+CT3Zv9t7VKl+Q3hTQoT9a1Qzezhhl6Rp21gJ/IVTW7Z3y9EWXhuUBC2Shnf+DX0antecpAwSP8w==",
      "dev": true,
      "engines": {
        "node": ">=10"
      }
    }
  }
}

```

