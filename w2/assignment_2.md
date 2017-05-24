## Week 2 Project - Yelp

### **Due: Monday, March 27th at 10:00pm**

Build a Yelp search app.

<a href="http://imgur.com/hp264Yn"><img src="http://imgur.com/hp264Yn.gif?1" title="source: imgur.com" /></a>
<img src="https://github.com/thanhcs94/ass2_search_yelp_redux/blob/master/fuck_react_native.gif?raw=true" title="source: imgur.com" />

### Submission instructions

Follow the guide for [[Submitting Assignments]]. Remember to include an animated gif and # of hours spent in the README. You can get started by copying this [README template](https://raw.githubusercontent.com/mikenk2010/react-native-examples/master/yelp_readme.md) and checking things off as you complete them.

### User Stories

The following user stories **must** be completed:

* Login Screen, just a page before go to Homepage

* Search results page:
    * Add filter button and search text input.
    * Display items, rows should be dynamic height according to the content height.
    * Infinite scroll for restaurant results.

<a href="http://imgur.com/kRnGghh"><img src="http://i.imgur.com/kRnGghh.png?1" title="source: imgur.com" /></a>

* Filter page:
    * Categories should show a subset of the full list, and when click "Show all" row to expand all subset.
        * `Hint` A formatted list of categories available in the Public API can be found here (https://www.yelp.com/developers/documentation/v3/all_category_list/categories.json).
        * `Hint` [Switch Component](https://facebook.github.io/react-native/docs/switch.html).
    * Clicking on the "Search" button should dismiss the filters page and trigger the search with the new filter settings.
    * Using Redux to storage filter data
      * `Hint`, [Redux example project](https://github.com/mikenk2010/react-native-demo-redux)
    
* Show loading page when waiting to fetch data from Yelp
    
#### OPTIONAL

* Implement the restaurant detail page with map view (show restaurant's position). [react-native-maps](https://github.com/airbnb/react-native-maps))
* Implement a custom switch to look like Yelp app.

![Image](https://media.giphy.com/media/xUPGcFu0WDyYrdCdDq/giphy.gif)

* Implement login page with [Facebook SDK](https://github.com/facebook/react-native-fbsdk)

* Implements TabbarIOS
   * ListView, the layout as description above
   * MapView, list all place on Map as Marker, and the layout as below. [Example](https://github.com/airbnb/react-native-maps#using-the-mapview-with-the-animated-api).
   
   ![Image](http://i.giphy.com/3o6UBdGQdM1GmVoIdq.gif)
    
### Yelp API

- You can peruse the Yelp API and sign up for a free developer key at the [Yelp Developers Portal](https://www.yelp.com/developers)
- [Introduction](https://www.yelp.com/developers/documentation/v3) to the Yelp API
- Yelp [Search API](https://www.yelp.com/developers/documentation/v3/business_search) for Yelp
- Yelp [API Console](https://www.yelp.com/developers/api_console)

![](http://i.imgur.com/VENlMB0.gif)

### Walkthroughs
#### Yelp API Class
- Function Get Yelp Token

```javascript
fetchToken() {
 const params = {
   client_id: 'tQyHUDn9ocxSt62kfaLS1w', // use your own
   client_secret: '863nET7GMULkWMRaqCsnwV5xLwsmMv6TsQEsMxT3uoUMvV6mg6sCGXEO3XyccPUr', // use your own
   grant_type: 'client_credentials'
 }
 /*
 // Another API in case the first one does not work
   client_id: 'qDPlyf_EBtljgqKxPALx6Q',  
   client_secret: 'RlFVBx8XonMjZcNnal3e827ooycXR7Pc4JngdpbM6UmdbW61GEfiss22OMRK0p4M',
 */

 const request = new Request('https://api.yelp.com/oauth2/token', {
  method: 'POST',
  headers: new Headers({
    'content-type': 'application/x-www-form-urlencoded;charset=utf-8',
  }),
  body: `client_id=${params.client_id}&client_secret=${params.client_secret}&grant_type=${params.grant_type}`
});

return fetch(request)
  .then(response => {
    return response.json()
  })
  .then(json => {
    console.log(json);
    return json; // Token
  })
}

/*
Example result
{
  "access_token": "ARolQDdpABpYZYLfO1NkbZ2hDzMwg_5pFz9KUoVITe2oBtXRhxSzMBamA5TTHA5vttN23P1EfONq_Iedo1Yvqm0IjFHOjtMdmDUb6lmQ22qIK_Ll7LajjpBi0OjQWHYx",
  "token_type": "Bearer",
  "expires_in": 15533847
}
*/
```

- Send request to get data from Token on step 1
   - Request example: https://api.yelp.com/v3/businesses/search?term=delis&location=San%20Francisco
   - Header : `{Authorization: Bearer [Token From fetchToken() function]}`
   
   ![Image](http://i.imgur.com/E2ijCxq.png)

#### Redux Architecture
- Install Redux and React Redux

```bash
npm install --save redux react-redux
```

- Import and Wrap your component inside `Provider`

```
import {Provider} from 'react-redux';
import {createStore} from 'redux';
import reducers from './reducers'; // Will be defined in next step
const store = createStore(reducers);
...
render() {
    return (
      <Provider store={store}>
        <Navigator
          initialRoute={defaultRoute}
          renderScene={(route, navigator) => this.renderScene(route, navigator)}/>
      </Provider>
    )
  };
```

- Create reducer.js
   - `Hint` you can use this file as a Reducer template

```javascript
import {combineReducers} from 'redux';

// Defined types, can be based on your scenes that you want they get data each the other.
const types = {
  dataScene1: 'textScene1',
  dataScene3: 'textScene3'
}

// Defined actions
export const actionCreators = {
  storeDataScene1 (params){
    return {
      type: types.dataScene1,
      payload: params,
    }
  },

  storeDataScene3 (params){
    return {
      type: types.dataScene3,
      payload: params,
    }
  }
}

const initialState = {};

// Store your data to Redux based on your action
const searchReducer = (state = initialState, action) => {
  const {type, payload} = action;
  switch(type) {
    case types.dataScene1: {
      return {
        ...state,
        params: payload,
      }
    }
    case types.dataScene3: {
      return {
        ...state,
        params: payload,
      }
    }
    default:
      return state;
  }
}

export default combineReducers({
  searchReducer,
})

```

- Store value to redux, this case I stay in Scene1 Component

```javascript
import {actionCreators} from './reducer.js';
import {connect} from 'react-redux';
...
render(){
const {navigator, dispatch} = this.props;
return(
<TouchableOpacity
    onPress={() => {
      dispatch(actionCreators.dataScene1({textKeyScreen1: this.state.dataRedux})); // Store value from state to dataScene1 in Redux, you can debug in reducer.js to undestand how does it work
      navigator.push({
        title: 'Scene2',
        component: Scene2,
        passProps: {dataProps: this.state.dataRedux}
      });
    }}
    >
    <Text>Go to Scene 2</Text>
</TouchableOpacity>

// End of your component
// Connect this component to Redux
export default connect()(Scene1);
```
- Get data from Redux

```javascript
import {connect} from 'react-redux';
...
class Scene2 extends Component {
   render(){
      return(
         <View>
            <Text>
               {this.props.scene1Params.textKeyScene1}
            </Text>
         </View>
      );
   }
}

// End of your component
const mapStateToProps = (state) => {
  return {
    scene1Params: state.searchReducer.params
  }
}
// Maping storage of Redux to props of your component
export default connect(mapStateToProps)(Scene2);
```

- [Example Redux](https://github.com/mikenk2010/react-native-demo-redux)

#### [Redux DevTools Extension](https://github.com/zalmoxisus/redux-devtools-extension)
- Redux DevTools for debugging application's state changes.

![Image](https://cloud.githubusercontent.com/assets/7957859/18002950/aacb82fc-6b93-11e6-9ae9-609862c18302.png)

### Guides and References
- [React Redux Concepts](http://www.reactnativeexpress.com/react_redux)
- [ES6 and coding definition](http://www.reactnativeexpress.com/es6)
- [JSX](http://www.reactnativeexpress.com/jsx)
