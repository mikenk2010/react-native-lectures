## Week 1 Project: Flicks

**Due:** **{{PROJECT(1) | longdate}} at 6:00pm**

### Overview

![Flicks|250](https://media.giphy.com/media/xUPGcGRUIba42hPSVy/giphy.gif)

### Submission instructions

Follow the guide for [Submitting Assignments](http://i.imgur.com/LUrLupi.gifv). Remember to include an animated gif and # of hours spent in the README. You can get started by copying this [README template](https://gist.githubusercontent.com/mikenk2010/84faa1572c8e4d14409ba99650bc4cf2/raw/ea03236081d45b55205ffab2d0ab1282bc75c92f/README.md) and checking things off as you complete them.

### User Stories

The following user stories **must** be completed:

- User can view a list of movies currently playing in theaters from The Movie Database. Poster images must be loaded [asynchronously](http://facebook.github.io/react-native/releases/0.42/docs/network.html#using-fetch).
  - [The Movie Database API](https://www.themoviedb.org/documentation/api)
  -  Ensure you can hit the "Now Playing" endpoint in a browser. This shows the data we will be using from The Movies Database.
   * **Note:** It's helpful to install the [JSONView Chrome Extension](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en) to view the returned JSON more easily.
   * Sample Request: https://api.themoviedb.org/3/movie/now_playing?api_key=a07e22bc18f5cb106bfe4cc1f83ad8ed
- User can view movie details by tapping on a cell. 
- User can [pull to refresh](http://facebook.github.io/react-native/releases/0.42/docs/refreshcontrol.html#refreshcontrol) the movie list or you can reference W1 Lab [Bonus 2: Infinite Scrolling]


The following advanced user stories are **optional**:
(_high_, _med_, and _low_ refer to the effort to implement the feature, with _high_ being the most work and _low_ being the least)
- ListView (_high_)
  - **Hint:** [ListView](http://facebook.github.io/react-native/releases/0.42/docs/listviewdatasource.html#listviewdatasource)
- Add a tab bar for **Now Playing** or **Top Rated** movies. (_high_)
   - **Hint:** React Native does not design Tabbar for android, you we have to use an external component [tab bar component](https://github.com/react-native-community/react-native-tab-view).
- Implement a Nagivator to switch between a list view and a grid view. (_high_)
   - **Hint:** Follow the Prework instruction [Navigator](https://facebook.github.io/react-native/docs/navigator.html)
- Add a search bar. (_med_)
   - **Hint:** Will be updated
- All images fade in as they are loading. (_low_)
   - **Hint:** The image should only fade in if it's coming from network, not cache, we use external component [react-native-image-progress](https://github.com/oblador/react-native-image-progress)
- For the large poster, load the low-res image first and switch to high-res when complete. (_low_)

### Additional Requirements

* Mainly working on iOS and using iPhone emulator
* React native version >= 0.42 and npm 4 or yarn 0.21

```bash
> react-native -v
react-native-cli: 2.0.1
react-native: 0.42.0
```
```bash
> npm -v
4.1.2
```
```bash
> yarn --version
0.21.3
```

### <a name="movie-db-api"></a>The Movie Database API

Check out The Movie Database [documentation](http://docs.themoviedb.apiary.io/#).  In particular:

- the ["Now Playing" endpoint](http://docs.themoviedb.apiary.io/#reference/movies/movienowplaying).
- the ["Top Rated" endpoint](http://docs.themoviedb.apiary.io/#reference/movies/movietoprated)
- The movie poster is available by appending the returned `poster_path` to `https://image.tmdb.org/t/p/w342`.
- The low resolution movie poster is available by appending the returned `poster_path` to `https://image.tmdb.org/t/p/w45`
- The high resolution movie poster is available by appending the returned `poster_path` to `https://image.tmdb.org/t/p/original`


### Walkthrough
#### Navigator pass data to next page

* [Follow example to understand how it works](https://github.com/mikenk2010/react-native-examples)

#### Tabbar
- `constructor` to set default tab selected
  
```javascript
  constructor() {
    super();  
    this.state = {
      selectedTab: 'homeTab',
    };
  }
```

- [TabBarIOS](http://facebook.github.io/react-native/releases/0.42/docs/tabbarios.html#tabbarios)
  
```
  return (
   <TabBarIOS
     unselectedTintColor="#CCC"
     tintColor="yellow"
     unselectedItemTintColor="#CCC"
     barTintColor="darkslateblue">
     <TabBarIOS.Item
       title="Home"
       icon={{uri: base64Icon, scale: 25}}
       selected={this.state.selectedTab === 'homeTab'}
       onPress={() => {
         this.setState({
           selectedTab: 'homeTab',
         });
       }}>
       {this._renderContent('#414A8C', 'Home tab')}
     </TabBarIOS.Item>
     <TabBarIOS.Item
       systemIcon="featured"
       badge=10
       badgeColor="pink"
       selected={this.state.selectedTab === 'newMobieTab'}
       onPress={() => {
         this.setState({
           selectedTab: 'newMobieTab',              
         });
       }}>
       {this._renderContent('#783E33', 'Featured Tab')}
     </TabBarIOS.Item>

   </TabBarIOS>
 );  
```

- _renderContent()

```javascript
    _renderContent(color: string, pageText: string){
      return (
        <View style={[styles.tabContent, {backgroundColor: color}]}>
          <Text style={styles.tabText}>{pageText}</Text>
          <Text style={styles.tabText}>{num} re-renders of the {pageText}</Text>
        </View>
      );
    }  
```

![Image|250](http://i.imgur.com/qUkcUHB.png)

#### Search Bar
  1. Add textinput and trigger when input change
  2. Filter function, using javascript search method, [example](https://www.w3schools.com/jsref/jsref_search.asp)

```javascript
  var rows = [];
  var title = rowData.title.toLowerCase();
  var desc = rowData.overview.toLowerCase();
  if(title.search(this.state.searchText.toLowerCase()) !== -1 || desc.search(this.state.searchText.toLowerCase()) !== -1){
    return(
    ...
    )
  }
```

#### Animation
- [Follow example](https://github.com/mikenk2010/react-native-examples/tree/master/apps/Animations)
- Using ScrollView and LayoutAnimation

![Image](https://github.com/mikenk2010/react-native-examples/raw/master/apps/Animations/animation.gif)

##### Alternative filter
- [Examples](https://briandouglas.me/posts/2016/02/08/searching-data-in-react-native)
- The concept is you filter when fetch data and update to datasource

```
 fetch(‘notes’, {
   context: this,
   asArray: true,
   then(data){
     let filteredData = this.filterNotes(searchText, data); // Filter here
     this.setState({
       dataSource: this.ds.cloneWithRows(filteredData),
     });
   }
 });
}
// Process filter data
filterNotes(searchText) {
 
}
```

![Image|250](http://i.imgur.com/y1NLxZH.png)

* Loading images

```bash
npm install react-native-image-progress --save
npm install react-native-progress --save
```

movies.js
```
import Image from 'react-native-image-progress';
import Progress from 'react-native-progress';
...

<Image
indicator={Progress}
 style={{height: 150}}
 source={{uri: 'https://image.tmdb.org/t/p/w342/' + rowData.backdrop_path}}
/>

```

#### Animation (Simple)
1. 

### References

* [React Native example](https://github.com/facebook/react-native)
* [React Native LifeCycle](https://facebook.github.io/react/docs/react-component.html)

```javascript
  constructor(); // 1. first
  componentWillMount(); // 2 second
  render(); // 3 third
  componentDidMount(); // 4 last
```

### Troubleshooting Tips:
1. **[For iOS] Did you run into the following error when sending a network request?**  [App Transport Security](https://guides.codepath.com/ios/App-Transport-Security):
     - Skip this step if we are working on HTTPS. We need to turn off `App Transport Security`. App Transport Security was introduced in iOS9 as a way to enforce best practices in secure connections between an app and its back end.
   * `NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9802)`
2. **Wait to load data from API ?** cause JS using async method, so we have to wait util the datasouce has data:

```javascript
  render(){        
    if(this.state.dataSource.getRowCount() === 0 ){
      var rows = <View><Text>Loading.....</Text></View>
    }else {
      var rows = <ListView
                    dataSource={this.state.dataSource}
                    renderRow={this._renderRow.bind(this)}
                    renderSeparator={this._renderSeparator}
                  />
    }
```

# Other Sample Submissions

![](https://github.com/thanhcs94/Reactnative-ListviewSearch/blob/master/listmoviesearch.gif?raw=true)
