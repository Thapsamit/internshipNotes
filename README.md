### PROBLEM STATEMENT 
## We are using auto scaling of EC2 that scales up new instances when laod increases but issues is let suppose 100 people are joining the class using server1 and next 100 joining using server 2 within same class so the streams from mentor to student and crosss communication can't happen it only works within server

# Solutions 
- Firstly we need signalling mechanism with a redis adapter redis adapter allows to create socket.io that can intercommunicate within server
- Now for a same room even if the user are divided in two different server then within their servere they will be having same room Id
- Expected thing here is let say someone from server 1 sends message then due to redis adapter the socket can deliver those signals to server 2 as well so both server will remain in sync they will get the signal or events
- Now for mediasoup the pipeToRouter are able to communicate within the same server and can deliver the streams
- But now how to pipe down the streams within different server
- Let say we have scenario a mentor connected with server 1 and starts producing his stream so all server 1 people will be able to get the streams but server 2 people won't get so we can use here a pipeToTransport feature that can pipe transports running in different server to transport the streams within those server so then all peoples within same room can get the producer stream even if they are connected to differnt servers
- Some links to implement this
- https://mediasoup.discourse.group/t/pipetransport/5131/3
- https://mediasoup.discourse.group/t/pipetransport-clarification/4686/4
- https://mediasoup.org/documentation/v3/scalability/

- PipeTransport may be needed to pipe everything between server through routers
