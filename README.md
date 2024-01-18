


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


