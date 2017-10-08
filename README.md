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

# Share Your Location

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
