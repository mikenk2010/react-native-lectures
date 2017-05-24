
# Assignment 4 - Chat app - iMess v2

* Login screen
* TabBar (List conversations and users)
* Chat screen (using react-native-gifted-chat)
    * Send image and location
* Redux
* What is [Socket.IO](https://socket.io)? How to connect to Socket.IO?
* What is Push Notifications? Local and remote push notifications. When to use Local or Remote?

The goal is to help you to know how to make a chat app, how socketio works and practice using Redux

![Image](http://i.imgur.com/3g6QuE0.png)

### Milestone 1: Prepare project with Socket IO, Gifted Chat and setup Redux
* Create a new react native project
* Setup Redux, don't forget to use Redux to manage state.
* Install Socket Client 
  * `npm install --save react-native-socket.io-client`
* [Install Gifted Chat](https://github.com/FaridSafi/react-native-gifted-chat)

### Milestone 2: Prepare layout
* Init App navigation
* Login screen
* TabBar screen with Conversations and Users screen
* Chat screen
* Connect to socketio and login with Username field, after login successfully, go to TabBar screen
    
### Milestone 3: Connect to Socket IO server    
  
* Connecting to Socket IO server
   
  ```
      window.navigator.userAgent = "react-native";
      let io = require('react-native-socket.io-client/socket.io');
      ...
      componentWillMount(){
          console.log("===CONNECTING SOCKET====");
          socket = io('http://127.0.0.1:3000', {jsonp: false});
        }
  ```
  
### Milestone 4: Broadcast and listener from Server - User
* Users:
    * Login
    * Load list users
    * Tap on User will move to Conversation screen
    ** Hint ** Server will return all users, do not load user was logined in your device

### Milestone 5: Broadcast and listener from Server - Messages    
   
* Messages:
    * Load list conversations
    * Tap on conversation will move to Conversation screen

* Conversation screen:
    * Make a chat screen with ListView
    * Add toolbar for actions: send images, send location

        * ![Image](http://i.imgur.com/jOyannh.png)

    * A message will contain text, image or maps
        * Tap on Image will open Image in fullscreen
        * Tap on Map will open Man in fullscreen
    * Send private message to User
   
## Explain socket listenser and broadcast
* `io.sockets.on("connection", function (socket)` Connect client to Server
* `socket.on("joinserver", function (name, device) {` Join server, by sending `username` and `device type` such as `android` or `ios`
* `io.sockets.emit("update-people", {people: people, count: sizePeople});` listener to send list online user
* `socket.on("countryUpdate", function(data) {` send current country to server
* `socket.on("typing", function(data) {` listener to show the current users typing
* `socket.on("send", function(msTime, msg) {` send message to server, `msTime`: `new Date().getTime()` and `msg`: message content
* `socket.on("whisper", function(msTime, person, msg) {` send private msg to `person` (obj), `msTime`: `new Date().getTime()` and `msg`: message
* `socket.on("disconnect", function() {` user logout
* `socket.on("createRoom", function(name) {` create room, `name` : room name (string)
* `io.sockets.emit("roomList", {rooms: rooms, count: sizeRooms});` broadcast list rooms 
* `socket.on("removeRoom", function(id) {` remove room, `id`: room id
* `socket.on("joinRoom", function(id) {` join room, `id`: room id
* `socket.on("leaveRoom", function(id) {` leave room, `id`: room id

### Chat App Server - NodeJS
[NodeJS Chat Server](https://github.com/mikenk2010/chat-app-socket-io-demo) Feel free to update, clone this chat app

### Demo

![Image](https://gifyu.com/images/demo_chatapp.gif)

## Resouces
- Push Notification, A push notification is a message that pops up on a mobile device. App publishers can send them at any time; users don't have to be in the app or using their devices to receive them. They can do a lot of things; for example, they can show the latest sports scores, get a user to take an action, such as downloading a coupon, or let a user know about an event, such as a flash sale.

![Image](https://www.simicart.com/blog/wp-content/uploads/2015/12/simicart-the-magic-of-pushing-notification-from-mobile-app-2.png)

  - Badge Notification
  
  ![Image](http://www.takecontrolbooks.com/resources/0182/site/images/Notification-Center-badges.png)
  
- [Implement React Native Push Notification](http://learning.coderschool.vn/courses/react_native_fast_track/unit/4#!exercises)
  


