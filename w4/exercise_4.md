
# Lab 4 - Push Notification with Firebase


The goal of this lab is to learn how to use MapView, get Location, pick photos from Library, Camera.
![Image|300](https://firebase.google.com/docs/notifications/images/notifications-overview.png)


### Getting Started

The checkpoints below should be implemented as pairs. In pair programming, there are two roles: supervisor and driver.
The supervisor makes the decision on what step to do next. Their job is to describe the step using high level language ("Let's print out something when the user is scrolling"). They also have a browser open in case they need to do any research. 
The driver is typing and their role is to translate the high level task into code ("Set the scroll view delegate, implement the didScroll method").
After you finish each checkpoint, switch the supervisor and driver roles. The person on the right will be the first supervisor.

### Milestone 1: Setup
* Create a new react native project 
* Install fcm component 
  * `npm install react-native-fcm --save`
  * `react-native link react-native-fcm`
  * Integrate Push notification for Android and iOS (https://github.com/evollu/react-native-fcm)
  
### Milestone 2: Usage

* Get device id

```
  FCM.requestPermissions(); // for iOS
  FCM.getFCMToken().then(token => {
      console.log("Device ID", token); // Device ID
  });
```

* Local push notification

```
FCM.scheduleLocalNotification({
  fire_date: new Date().getTime(), // Time to fire push event
  id: "UNIQ_ID_STRING", 
  body: "from future past" + new Date().getTime(),
  icon: "cs_icon",
  repeat_interval: "hour" //day, hour
})
```

* Listener event when receive notification

```
this.notificationListener = FCM.on(FCMEvent.Notification, async (notif) => {
  console.log("Notification", notif);
});
```

* Stop listener event

```
componentWillUnmount() {
  // stop listening for events
  this.notificationListener.remove();
  this.refreshTokenListener.remove();
}
```
  
### Milestone 3: Send push notification 
* http://pushtry.com/
* Send by via REST API (using Postman tool)
 * **Hint** Get server Key go to (https://console.firebase.google.com) > Setting > Cloud Messing

```
URL: https://fcm.googleapis.com/fcm/send
Method: POST
Header:
{
  "Authorization":"key=<Server Key>"
}
Body:
{
  "to": "<Device ID>",
  "notification": {
     "body" : "great match!",
      "title" : "Portugal vs. Denmark",
      "icon" : "ic_launcher",
      "color" : "red"
  },
  "data" : {
  	"action" : "gotoMessages"
  },
  "priority": 10
}
```
