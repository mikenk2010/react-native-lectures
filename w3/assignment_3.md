# Week 3 Project - Twitter

**Due:** **{{PROJECT(3) | longdate}} at 6:00pm**

### Overview

Build a simple Twitter client.

![Image:250](https://media.giphy.com/media/3og0ILtXSKCsiKl3oI/giphy.gif)

### Submission instructions

Follow the guide for [Submitting Assignments](http://learning.coderschool.vn/courses/react_native_fast_track/pages/submitting_assignments). Remember to include an animated gif and # of hours spent in the README. You can get started by copying this [README template](https://raw.githubusercontent.com/mikenk2010/coderschool-readmefiles/master/assignment_3.md) and checking things off as you complete them.

### Project Requirements

The following user stories **must** be completed:

* Login, click button to sign in Twitter using OAuth login flow
    * Add Login button
    * Click button to sign in Twitter using OAuth login flow
        * Create an app on [https://apps.twitter.com](https://apps.twitter.com/) to get API keys.
        * Twitter OAuth login
            * Install module react-native-simple-auth (https://github.com/adamjmcgrath/react-native-simple-auth).
            * Set up deep linking for your Android and iOS application using the instructions on the [react-native website](https://facebook.github.io/react-native/docs/linking.html) (set the `launchMode` of `MainActivity` to `singleTask` in `AndroidManifest.xml`, create the deep link schemes in [Providers Setup](https://github.com/adamjmcgrath/react-native-simple-auth#twitter))
                * Android: Add the deep link scheme for the callback (Your App Name, eg `com.reactnativesimpleauthexample`) to your `AndroidManifest.xml` eg https://github.com/adamjmcgrath/ReactNativeSimpleAuthExample/blob/master/android/app/src/main/AndroidManifest.xml#L28-L33.
                * iOS: Add the deep link scheme for the callback to your iOS app, eg https://dev.twitter.com/cards/mobile/url-schemes.
            * After get Twitter profile successfully, go to Main screen

            ```
            **import **{ twitter, } **from **'react-native-simple-auth';
            async _loginWithTwitter() {
                try {
                  const info = await twitter({
                    appId: 'Your Consumer Key (API Key)',
                    appSecret: 'Your Consumer Secret (API Secret)',
                    callback: 'rncs://authorize', //rncs is deep link scheme
                  });

                console.log('info', info);
            }
            ```

* Hamburger menu
   * Dragging from left to right in the view should reveal the menu.
      * You can refer react-native-drawer (https://github.com/root-two/react-native-drawer)
      * Show your Twitter profile.
      * The menu should include links to [your timeline](https://dev.twitter.com/rest/reference/get/statuses/user_timeline), the [home timeline](https://dev.twitter.com/rest/reference/get/statuses/home_timeline).
      * Example code to get home timeline:
      
      ```
        async _getHomeTimeline(info, params = {}) {
            const { credentials: { oauth_token, oauth_token_secret } } = info;
            const httpMethod = 'GET';
            const url = 'https://api.twitter.com/1.1/statuses/home_timeline.json';
            const headers = getHeaders(url, params, {}, config.consumerKey, config.consumerSecret, httpMethod, oauth_token, oauth_token_secret);

            const response = await fetch(url, {
              method: httpMethod,
              headers,
            });
            const json = await response.json();
            console.log('home_timeline', json);
         }
        ```

![Image](http://i.imgur.com/OUSCQyYl.png)

* Profile page (feel free to design you own profile page)

* Home Timeline

![Image](http://i.imgur.com/sfCRakp.png)

* Like, tape heart icon to like and unlike

* In case, you have any problems, you can [download this project](https://github.com/ngothanhtai/twitter-react-native) to get started. (works for iOS & Android)


### References

* [Twitter API](https://dev.twitter.com/rest/reference)
* [react-native-drawer](https://github.com/root-two/react-native-drawer)

#### Examples API
* [Posting a tweet](https://dev.twitter.com/rest/reference/post/statuses/update) by tapping on a compose button.
*  User can tap on a tweet to view it, with controls to [retweet](https://dev.twitter.com/rest/reference/get/statuses/retweets/id), [favorite](https://dev.twitter.com/rest/reference/post/favorites/create), and [reply](https://dev.twitter.com/rest/reference/post/statuses/update).
*  User can [retweet](https://dev.twitter.com/rest/reference/get/statuses/retweets/id), [favorite](https://dev.twitter.com/rest/reference/post/favorites/create), and [reply](https://dev.twitter.com/rest/reference/post/statuses/update) to the tweet directly from the timeline feed.




