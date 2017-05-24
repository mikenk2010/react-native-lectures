# Lab 3 - PhotoMap

The goal of this lab is to learn how to use MapView, get Location, pick photos from Library, Camera.

![Image](https://media.giphy.com/media/xUA7aVYV31TnD8WhvG/giphy.gif)


### Getting Started

The checkpoints below should be implemented as pairs. In pair programming, there are two roles: supervisor and driver.
The supervisor makes the decision on what step to do next. Their job is to describe the step using high level language ("Let's print out something when the user is scrolling"). They also have a browser open in case they need to do any research. 
The driver is typing and their role is to translate the high level task into code ("Set the scroll view delegate, implement the didScroll method").
After you finish each checkpoint, switch the supervisor and driver roles. The person on the right will be the first supervisor.

### Milestone 1: Setup

* Create a new react native project 
* Get current position using [Geolocation](https://facebook.github.io/react-native/docs/geolocation.html)

```
navigator.geolocation.getCurrentPosition((position) => {
  /*{ 
        coords: 
        { 
            speed: -1,
            longitude: -122.406417,
            latitude: 37.785834,
            accuracy: 5,
            heading: -1,
            altitude: 0,
            altitudeAccuracy: -1 
        },
        timestamp: 1490794016532.687 
  }*/
  console.log('position', position);
}, (error) => {
  console.log('error', error);
});
```

### Milestone 2: Add Mapview and Display current postion

* Create MapView
    * You can use [react-native-maps](https://github.com/airbnb/react-native-maps)
        * `npm install --save react-native-maps`

* Display [MapView](https://github.com/airbnb/react-native-maps/blob/master/docs/mapview.md) with a marker at the current position we got above

* ![Image](http://i.imgur.com/2HleS1Y.png)

```
import MapView from 'react-native-maps';
const region = {
  latitude: 10.8231,
  longitude: 106.6297,
  latitudeDelta: 0.0922,
  longitudeDelta: 0.0421,
}
<MapView
  region={region}
  style={{
    flex: 1,
  }}
>
  <MapView.Marker
    coordinate={{ latitude: region.latitude, longitude: region.longitude, }}
    title={'Current position'}
  />
</MapView>
```

** `react-native link`
** Re-build your app again.


***Hint*** If you don't see the map, please add style `flex: 1 for <MapView/>`


### Milestone 3: Add marker and callout

* Get position on map by Press/ Long press on [MapView](https://github.com/airbnb/react-native-maps/blob/master/docs/mapview.md)

```
<MapView
    onLongPress={(e) => {
       const { coordinate } = e.nativeEvent;
       console.log('coordinate', coordinate);
    }
/>
```

* Add a [MapView.Marker](https://github.com/airbnb/react-native-maps/blob/master/docs/marker.md) for that position
* Add [MapView.Callout](https://github.com/airbnb/react-native-maps/blob/master/docs/callout.md)  for above marker

### Milestone 4: Get Image From Device And Custom Callout

* Add text 'Marker n' and an image from Camera or Gallery to [MapView.Callout](https://github.com/airbnb/react-native-maps/blob/master/docs/callout.md) like this:

    * ![Image](http://i.imgur.com/F5JuISz.png)
    
    * You can use [react-native-image-picker]((https://github.com/marcshilling/react-native-image-picker)) to pick an image or take a picture from device 
    * `npm install react-native-image-picker --save`
    * `react-native link`

```
<MapView.Marker
    coordinate={coordinate}
>
    <MapView.Callout
      style={{
        alignItems: 'center', justifyContent: 'center',
      }}
    >
        <Image source={source}
               style={{
                 width: 60, height: 60,
                 margin: 4,
               }}
        />
        <Text
            style={{
            fontSize: 12,
            }}
        >
            Marker {this.state.markers.length}
        </Text>
    </MapView.Callout>
</MapView.Marker>
```

* Use react-native-image-picker

```
import ImagePicker from 'react-native-image-picker';
ImagePicker.showImagePicker({
  storageOptions: {
    skipBackup: true,    path: 'images'  }
}, (response) => {

  if (response.didCancel) {
    console.log('User cancelled image picker');  }
  else if (response.error) {
    console.log('ImagePicker Error: ', response.error);  }
  else if (response.customButton) {
    console.log('User tapped custom button: ', response.customButton);  }
  else {
    let source = { uri: response.uri };    // You can also display the image using data:    // let source = { uri: 'data:image/jpeg;base64,' + response.data };    this.setState({
      image: source
    });  
  }
```

** [iOS] If your app crashes when picking photos or taking a picture. Please make sure you add these description to Info.plist. Open Info.plist as Source code for easier to add:

```
<key>NSCameraUsageDescription</key>
<string>Please allow the app to access your camera.</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string></string>
<key>NSMicrophoneUsageDescription</key>
<string>Please allow the app to access your microphone.</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>Please allow the app to access your photos.</string>
```

![Image](http://i.imgur.com/c9oi7Tu.png)

### Milestone 5: Add multiple markers and display fullscreen Image

* You can add multiple markers on screen.
* Click image in MapView.Callout will display image in Full screen
    * You can use react-native-lightbox (https://github.com/oblador/react-native-lightbox)

```
<Lightbox
    renderContent={() => {
      return (<Image source={source}
             style={{
               flex: 1,
             }}
      />);
    }}
>
    <Image source={source}
           style={{
             width: 60, height: 60,
           }}
    />
</Lightbox>
```

### Bonus 1:

* Custom marker image

![Image](http://i.imgur.com/LD8gBMk.png)

### Bonus 2:

* Add search box on top of the map
* Search from Yelp API with your current position
* Display marker based on the result from Yelp and show detail when clicking on marker

* ![Image](http://i.imgur.com/qNGt37T.png)


