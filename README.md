# android-geo-example
Example app for an Android beginners course of Hamburg Coding School. 

In this course, we go through the following points step by step. 
This readme is a summary of this course and lists all the resources you need for your setup.

Do you want to take part in our courses? Register on our website:  
https://hamburgcodingschool.com/courses/

## What do you need to get started?

* Your laptop.
* An internet connection.
* Android Studio.

## Setting up Android Studio.

Download Android Studio here:  
https://developer.android.com/studio/index.html

Install Android Studio. If it asks you to install Java, install it, too.

Open Android Studio. Follow the instructions and install at least one SDK.

## Create your first App

Open Android Studio and select "Start a new Android Studio project".

Give it a name (e.g. "Geo App").

As company domain, use the following pattern:  
```
firstnamelastname.com
```
So if your name would be Lisa Simpson, this would be:
```
lisasimpson.com
```

Press next, choose "Phone and Tablet", press next.

Select "Google Maps Activity", press next and finish.

## Create a Google Maps API Key

For this, you need a Google account. You might already have one (i.e. your Gmail address). If not, create one at:  
https://accounts.google.com/SignUp

In Android Studio, go to the file `google_maps_api.xml`. At line 7, you see a long link. 
Copy this link and paste it into your browser. It will send you to the Google Developer Console.

"Create Project" is selected, press Continue. Then click "Create API Key". 

Copy the key that it shows you and paste it in `google_maps_api.xml` at line 23 to replace `YOUR_KEY_HERE`.

If you are having problems, read about creating the Google Maps API key in the Developer Console here:  
https://developers.google.com/maps/documentation/android/start#get-key 

## Start Your App

Click "AVD Manager" and  "Create virtual device...".  
Select “Phone” and choose one (e.g. Pixel). Click next.

For the system image, choose the first one, click "Download", and once it's downloaded, press Finish.
Your device will now be listed in the list of emulators.  
Click the "run" button and wait until it started.

In Android Studio, press the green "play" icon. It starts up your app in the emulator.

## Set a Marker

Open file `MapsActivity.java`.
Go to line 42 and change longitude and latitude (the blue numbers) in this statement:
```
LatLng sydney = new LatLng(-34, 151);
```

For finding out longitude and latitude of places, go to http://www.latlong.net/.

Go to line 43 and change the snippet text (the green text). 
This will be the text that is displayed when you touch the marker in your app.
```
mMap.addMarker(new MarkerOptions().position(sydney).title("Marker in Sydney"));
```

Try changing the text that is displayed if you click the marker. Can you find it?

## Change the Marker Color

The standard marker is red. How can we change its color?

In `MapsActivity.java`, at line 43, change the code to:
```java
mMap.addMarker(new MarkerOptions()
                .position(sydney)
                .icon(BitmapDescriptorFactory.defaultMarker(BitmapDescriptorFactory.HUE_ORANGE))
                .title("Marker in Sydney"));
```

Now, if you place your cursor after `HUE_` and press `control+space`, it will show you what colors are available, 
and you can select them with a click.

## Change the Marker Icon

In your Android Studio project, navigate to the `res` folder.  
Create all the `drawable-...` folders there that you see at https://github.com/hamburgcodingschool/android-geo-example/tree/master/app/src/main/res.

From each of these folders, download the `marker_hcs.png` file and put them at the same folder in your project.

In `MapsActivity.java` at line 43, change the code into this:
```
mMap.addMarker(new MarkerOptions()
        .position(sydney)
        .icon(BitmapDescriptorFactory.fromResource(R.drawable.marker_hcs))
        .title("Marker in Sydney"));
```

Now the icon is on top of the location, with the point on the map being on the bottom center of the icon. To change it to the center, change the code to this:
```
mMap.addMarker(new MarkerOptions()
        .position(sydney)
        .icon(BitmapDescriptorFactory.fromResource(R.drawable.marker_hcs))
        .anchor(0.5f, 0.5f)
        .title("Marker in Sydney"));
```

If you want to look up how else you can modify your markers, go to:  
https://developers.google.com/maps/documentation/android-api/marker

If you want to create your own icons, you can use the Android Asset Studio:  
https://romannurik.github.io/AndroidAssetStudio/index.html 

## Share A Location

For listening to clicks on the info window (the text shown when a marker is clicked), we need to create a listener like this:
```
mMap.setOnInfoWindowClickListener(new GoogleMap.OnInfoWindowClickListener() {
     @Override
     public void onInfoWindowClick(Marker marker) {

     }
});
```

To share the latitude and longitude of that marker, add the following code:
```
mMap.setOnInfoWindowClickListener(new GoogleMap.OnInfoWindowClickListener() {
    @Override
    public void onInfoWindowClick(Marker marker) {
        LatLng position = marker.getPosition();
        String location = "https://www.google.com/maps?ll=" + position.latitude + "," + position.longitude
                        + "&q=" + position.latitude + "," + position.longitude;
        Intent sendIntent = new Intent(Intent.ACTION_SEND);
        sendIntent.putExtra(Intent.EXTRA_TEXT, location);
        sendIntent.setType("text/plain");
        startActivity(Intent.createChooser(sendIntent, "Send location"));
    }
});
```

We explain all you need to know in this Android beginners course.

Read more about sharing content in Android here:  
https://developer.android.com/training/sharing/send.html

## Show the phone's location

At the end of the method `onMapReady()`, switch on the maps blue icon for the location:

```java
mMap.setMyLocationEnabled(true);
```

You see it marked as an error, so we need to fix this. It is about location permissions. This permission needs to be granted.

Add the following code for checking the location permission:

```java
if (ContextCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION)
        == PackageManager.PERMISSION_GRANTED) {
    mMap.setMyLocationEnabled(true);
} else {
    //Request Location Permission
    ActivityCompat.requestPermissions(this,
            new String[]{Manifest.permission.ACCESS_FINE_LOCATION},
            MY_PERMISSIONS_REQUEST_LOCATION);
}
```

The variable `MY_PERMISSIONS_REQUEST_LOCATION` is marked red, because it's not there yet. We need to add it to the beginning of the class:

```java
public static final int MY_PERMISSIONS_REQUEST_LOCATION = 99;
```

This is showing a dialog to the user that asks whether or not the app is allowed to use the phone's location.

We also need to react to the decision of the user: it's called a "Callback".

Directly at the beginning of the class, after `implements OnMapReadyCallback`, make a comma and add this:  
`ActivityCompat.OnRequestPermissionsResultCallback`.

At the end of the class, just before the last `}`, put the following code:

```java
@SuppressWarnings("MissingPermission")
@Override
public void onRequestPermissionsResult(int requestCode, 
                                       @NonNull String[] permissions, 
                                       @NonNull int[] grantResults) {
    if (requestCode == MY_PERMISSIONS_REQUEST_LOCATION) {
        if (permissions.length == 1 &&
                Manifest.permission.ACCESS_FINE_LOCATION.equals(permissions[0]) &&
                grantResults[0] == PackageManager.PERMISSION_GRANTED) {
            mMap.setMyLocationEnabled(true);
        }
    }
}
```

Now, if you start your app, you get a dialog asking you to grant permission to use the location.  

You now see a little crosshairs icon on the top right corner. If you click it, it zooms to your location.

## Create a Button

Open the file `activity_maps.xml`. We need to change the layout so that it shows a button.

Now we need to put a `LinearLayout` around the `Fragment`.

Put the following at the beginning of the file:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:map="http://schemas.android.com/apk/res-auto"
              xmlns:tools="http://schemas.android.com/tools"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:orientation="vertical"
              tools:context="com.teresaholfeld.geoapp.MapsActivity">
```

And this on the end:

```xml
</LinearLayout>
```

From the `<fragment>` remove the following:

```xml
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:map="http://schemas.android.com/apk/res-auto"
xmlns:tools="http://schemas.android.com/tools"
tools:context="com.teresaholfeld.geoapp.MapsActivity"
```

Then, in the `<fragment>`, change the layout height to 0:

```xml
android:layout_height="0dp"
```

And add a layout weight of 1:

```xml
android:layout_weight="1"
```

Below the `<fragment>` add a `<Button>` like this:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button"/>
```

Try changing the label text to `Share My Location`.

Run your app and see how the layout changed.  
The button is not doing anything yet when it's clicked.

## Get the device's location

In the file `app/build.gradle` at line 29 add the following line:

```groovy
compile 'com.google.android.gms:play-services-location:11.0.4'
```

Press `Sync now`.

In `MapsActivity` at line 31, add the following line:

```java
private FusedLocationProviderClient mFusedLocationClient;
```

At the end of method `onCreate()`, just one line before the `}`, add:

```java
mFusedLocationClient = LocationServices.getFusedLocationProviderClient(this);
```

Now we prepare the function that is showing the location on button click. 
Add this to the end of the class, just before the last `}` :

```java
private void shareLocation() {
    if (ActivityCompat.checkSelfPermission(this,
            Manifest.permission.ACCESS_FINE_LOCATION)
            != PackageManager.PERMISSION_GRANTED) {
        return;
    }
    mFusedLocationClient.getLastLocation()
            .addOnSuccessListener(this, new OnSuccessListener<Location>() {
                @Override
                public void onSuccess(Location location) {
                    if (location != null) {
                        Toast.makeText(MapsActivity.this, "Location: " + location, Toast.LENGTH_LONG).show();
                    }
                }
            });
}
```

## Show Location on Button Click

Now we want to show what the current location is when we click the button.  
For that, we need an `OnClickListener` that is listening to Button clicks.
Find method `onMapReady()` and put this just before the end:

```java
Button button = (Button) findViewById(R.id.button);
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        shareLocation();
    }
});
```

Now run your app on the emulator and try the new button.

## Share Your Location

Now, instead of showing the current location in a toast, we want to share them.

Find the line that says `Toast.makeText(MapsActivity.this, "Location: " + location, Toast.LENGTH_LONG).show();`.

Delete this line, and at the exact same place put this:

```java
String locationUrl = "https://www.google.com/maps?ll="
        + location.getLatitude() + "," + location.getLongitude()
        + "&q=" + location.getLatitude() + "," + location.getLongitude();
Intent sendIntent = new Intent(Intent.ACTION_SEND);
sendIntent.putExtra(Intent.EXTRA_TEXT, locationUrl);
sendIntent.setType("text/plain");
startActivity(Intent.createChooser(sendIntent, "Send location"));
```

Run your app and try it out!

## Fake Your Location

Ever wondered if you can fake your location? You can trick your phone into believing that it is 
someplace else. Follow these steps to do this:

In your phone, open the Play Store. Search for `Fake GPS Location`. Install the app that looks like this:

![screen shot 2017-10-09 at 20 28 00 1](https://user-images.githubusercontent.com/11841927/31353126-9af99fcc-ad31-11e7-9434-e5d33002a5f8.png)

In the phone, go to Settings. Go to `System` > `Developer options`. It should be switched on.
In there, look for the option `Select mock location app`. The installed Fake GPS app will be listed there. Click it.

Now open the Fake GPS app. Put the marker somewhere you like, and click play.  
The app will go to the background. If you open your own Maps app, it will show your location at that place.

Try sharing your fake location with your friends.

To stop the fake location, open the notification from the top and click it. Press the pause button. 
This stops the fake location and you will have your original location back.
