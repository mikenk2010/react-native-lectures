## Week 1 Lab Exercises - Tumblr

### Topics in Focus

While there are many topics covered each week, the topics in focus are the ones that require deeper experimentation. This week the topics are:

### Challenge

This week we will be playing with how to layout views and style user interfaces. We will be doing this challenge in pairs during the lab. There will be about 1 1/2 hours allotted for development.

![HomeScreen](http://i.imgur.com/kHx0sJj.png)

### Getting Started
The checkpoints below should be implemented as pairs. In pair programming, there are two roles: supervisor and driver.

The supervisor makes the decision on what step to do next. Their job is to describe the step using high level language ("Let's print out something when the user is scrolling"). They also have a browser open in case they need to do any research. The driver is typing and their role is to translate the high level task into code ("Set the scroll view delegate, implement the didScroll method").

*Note: We will work on React native > 0.42 and simulator is iOS, we mainly work on iOS first then back to Android later cause iOS was fully supported more than Android at the moment.

### Milestone 1: Setup
     
 * Prepare Post Component

![Imgur](http://i.imgur.com/c7a2ADf.png)

 * Prepare [ListView](http://facebook.github.io/react-native/releases/0.42/docs/listview.html#listview)

     ```javascript
       class MyComponent extends Component {
        constructor(props) {
          super(props);
          const ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
          this.state = {
            dataSource: ds.cloneWithRows(['row 1', 'row 2']),
          };
        }

        render() {
          return (
            <ListView
              dataSource={this.state.dataSource}
              renderRow={(rowData) => <Text>{rowData}</Text>}
            />
          );
        }
      }
     ```

### Milestone 2: Fetch Actual Data

 * Turn off [App Transport Security for iOS only](https://guides.codepath.com/ios/App-Transport-Security):
     - Skip this step if we are working on HTTPS. We need to turn off `App Transport Security`. App Transport Security was introduced in iOS9 as a way to enforce best practices in secure connections between an app and its back end.
 * Fetch data and render to (`ListView`)
 1. API: https://api.tumblr.com/v2/blog/xkcn.info/posts/photo?api_key=Q6vHoaVm5L1u2ZAW1fqv3Jw48gFzYVg9P0vH0VHl3GVy6quoGV
 2. [Tumblr API Document](https://www.tumblr.com/docs/en/api/v2)

 ![Image](http://i.imgur.com/Hgw0d4Q.png)
  
 * [Timestamp to relative, using Moment Module](https://momentjs.com), convert timstamp to datetime ago same as Facebook
     * Usage
     
     ```
     import moment from 'moment';
     var now = moment().format();
     ```
     
### Milestone 3: Implement GridView

* ![Image](http://i.imgur.com/YwtKBEi.jpg)
* **Hint**
     * Option 1: [Source to render grid view](http://stackoverflow.com/questions/29394297/listview-grid-in-react-native)    
          * Using `Dimensions` component to calculator `width` and `height`
     * Option 2: [Reference code for render items as group](https://github.com/lucholaf/react-native-grid-view/blob/master/index.js)

### Milestone 4: Tap to image to like or dislike.

* **Hint**

```
  <Image
    style=\{\{width: 50, height: 50\}\}
    source=\{\{uri: 'https://facebook.github.io/react/img/logo_og.png'\}\}>
    <View>
      <Text>Liked</Text>
    </View>

  </Image>
```

### Milestone 5: Install your application on your real device
* iOS, you have to register an Apple Account and sign your app with new account you have just created

     ![Image](http://i.imgur.com/s2qXB7m.png)

* Android, plug your phone to laptop and build android app again `react-native run-android`

### Bonus 1: Infinite Scrolling

```javascript
  <ListView
     enableEmptySections={true}
     dataSource={this.state.dataSource}
     renderRow={this.renderMovieCell}
     renderFooter={this.renderFooter}     
     onEndReached={this._onEndReached}
   />
 ....
 
  _onEndReached = () => {
    alert("Ahah, onEndReached fired !!!")
  }

```

### Bonus 2: [RefreshControl](http://facebook.github.io/react-native/releases/0.41/docs/refreshcontrol.html#refreshcontrol)

![Image](https://media.giphy.com/media/3o7buaVzXqxpRxRh96/giphy.gif)

### Concept Review

* Most common screens in mobile apps? (Login, Feed, Detail, Create Form)
* What is a ListView?
* Describe the method by which items are populated into list, what steps are needed?

These are in addition to the mini-projects and are not required but are supplemental content.
