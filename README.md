# android-geo-example
Beispiel-App für den Android-Einsteiger-Kurs von Hamburg Coding School. 

In diesem Kurs werden wir Schritt für Schritt eine Android App bauen, mit der wir eine Google Map darstellen.
Dies ist eine Zusammenfassung des Kurses und listet alle Links und Code-Schnipsel auf, die du für das Erstellen der App brauchst.

Möchtest du gern an unseren Kursen teilnehmen? Registrier dich auf unserer Webseite:  
https://hamburgcodingschool.com/courses/

## Was du für den Kurs brauchst:

* Deinen Laptop.
* Eine Internetverbindung.
* Android Studio.

## Android Studio Installieren

Du kannst dir Android Studio hier herunterladen:  
https://developer.android.com/studio/index.html

Wenn du es heruntergeladen hast, installiere es. Wenn es sagt, dass du Java installieren sollst, folge dem Link und 
installiere es.

Wenn es installiert ist, öffne Android Studio und folge den Instruktionen. Installiere mindestens eine SDK 
(wir erklären euch, was das ist, und wie man das macht).

## Erstelle Deine Erste App

Öffne Android Studio und wähle "Start a new Android Studio project" aus.

Gib ihm einen Namen, wie z.B. "Geo App".

Bei "company domain", benutze deinen namen in diesem Schema:
```
vornamenachname.de
```
Wenn du z.B. Lisa Simpson heißt, würdest du das hinschreiben:
```
lisasimpson.de
```

Clicke "next", wähle "Phone and Tablet" aus, und clicke wieder "next".

Wähle "Google Maps Activity" aus, clicke "next" und dann "finish".

## Erstelle einen Google Maps API Key

Um Google Maps in deiner App zu benutzen, brauchst du einen Google Maps API Key.

Hierzu brauchst du einen Google Account. Vielleicht hast du schon einen (wenn du eine Gmail-Adresse hast).
Wenn nicht, erstelle dir hier einen:
https://accounts.google.com/SignUp

Gehe zurück zu Android Studio, und öffne die Datei `google_maps_api.xml`.

> *Tip!*:  
> In Android Studio kannst du nach Dateien suchen in dem du die Shift-Taste zweimal schnell hintereinander drückst.

In Zeile 7 siehst du einen sehr langen Link. Kopiere den Link und Paste ihn in deinen Browser.
Er bringt dich zur Google Developer Console.

Dort ist "Create Project" ("Projekt erstellen") bereits ausgewählt. Clicke "Continue"/"Weiter", und danach 
"Create API Key"/"API Key erstellen".
 
Kopiere den Key, den es dir erstellt hat, und füge ihn in `google_maps_api.xml` in Zeile 23 ein, 
dort wo `YOUR_KEY_HERE` steht.

## Starte Deine App

Wenn du kein Android-Handy hast, kannst du einen Emulator starten.
Clicke dazu auf "AVD Manager" und dann auf "Create virtual device...".  
Bei "Phone", wähle ein Gerät aus, das der Emulator darstellen soll (z.B. das Pixel). Clicke auf "Next".

Beim "System image", wähle das erste aus, und clicke auf "Download". Wenn es fertig geladen ist, clicke "Finish".

Dein erstellter Emulator erscheint jetzt in der Liste der Emulatoren.  
Clicke auf "run" und warte bis es gestartet ist.

Alternativ, wenn du ein Android-Gerät hat, schließe es am Laptop an.

In Android Studio gibt es das grüne "Play"-Icon. Wenn du darauf clicks, started deine App auf dem Handy oder im Emulator.

## Einen Marker Setzen

Öffne die Datei `MapsActivity.java`.
Gehe zu Zeile 42 und ändere Longitude und Latitude (die blauen Zahlen) in dieser Zeile:
```java
LatLng sydney = new LatLng(-34, 151);
```

Wenn du herausfinden willst, welche Städte welche Koordinaten haben, kannst du das hier nachschauen: 
http://www.latlong.net/

Ändere den Text, der dargestellt wird wenn man den Marker clickt, in dem du den günen Text in Zeile 43 änderst.
```
mMap.addMarker(new MarkerOptions().position(sydney).title("Marker in Sydney"));
```

## Die Marker-Farbe Ändern

Der Standard-Marker ist rot. Das kann man aber ändern, indem man einen "Hue" setzt:

Ändere Zeile 43 zu:
```java
mMap.addMarker(new MarkerOptions()
                .position(sydney)
                .icon(BitmapDescriptorFactory.defaultMarker(BitmapDescriptorFactory.HUE_ORANGE))
                .title("Marker in Sydney"));
```

Setze den Cursor hinter den Unterstrich von `HUE_` und drücke `control+space`. Das zeigt eine Auswahl an Farben an. 
Sie wird ausgewählt, wenn du sie anklickst.

## Das Marker-Icon Ändern

Man kann das Icon, das als Marker auf der Karte angezeigt wird, auch ändern.

Gehe hierzu in deinem Android Studio Projekt zum Ordner `res`.  
Erstelle dort alle Unterordner, die mit `drawable-...` anfangen, so wie du es hier siehst:
https://github.com/hamburgcodingschool/android-geo-example/tree/master/app/src/main/res

Lade für jeden der Ordner hinter dem Link das `marker_hcs.png` herunter, und füge es in dem entsprechenden Ordner in 
deinem eigenen Projekt ein.

In `MapsActivity.java`, ändere Zeile 43 (und die folgenden bis zum Semikolon) zu:

```java
mMap.addMarker(new MarkerOptions()
        .position(sydney)
        .icon(BitmapDescriptorFactory.fromResource(R.drawable.marker_hcs))
        .title("Marker in Sydney"));
```

Wenn du die App startest, siehst du dass der Icon jetzt oben über dem Punkt ist. Bei einem Marker macht das Sinn, weil 
es wie eine Nadel in der Karte stecken soll, aber bei unserer Grafik nicht. Um es auf die Mitte des Punktes zu legen,
ändere den Code zu:
```java
mMap.addMarker(new MarkerOptions()
        .position(sydney)
        .icon(BitmapDescriptorFactory.fromResource(R.drawable.marker_hcs))
        .anchor(0.5f, 0.5f)
        .title("Marker in Sydney"));
```

Wenn du nachschauen möchtest, was man noch mit Markern machen kann, gehe zu:  
https://developers.google.com/maps/documentation/android-api/marker

Wenn du deine eigenen Icons für deinen Marker machen möchtest, kannst du das Android Asset Studio benutzen:  
https://romannurik.github.io/AndroidAssetStudio/index.html 

## Eine Location Sharen

In diesem Beispiel wollen wir die Location einen Markers sharen, wenn man auf den Text des Markers klickt.  
Hierzu müssen wir einen `Listener` erstellen. Füge diesen Code unter dem ein, den wir gerade geändert haben:
```java
mMap.setOnInfoWindowClickListener(new GoogleMap.OnInfoWindowClickListener() {
     @Override
     public void onInfoWindowClick(Marker marker) {

     }
});
```

Jetzt haben wir einen Listener, der aber noch nichts macht. Fülle die leere Zeile, so dass am Ende folgender Code dasteht:
```java
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

Du kannst mehr über das Sharen von Inhalten in Android nachlesen unter:  
https://developer.android.com/training/sharing/send.html

## Deine Geo-Position Anzeigen

Hier wollen wir einstellen, dass die Google Maps Karte unseren Standort als blauen Punkt anzeigt.
Füge am Ende der Methode `onMapReady()` folgenden Code ein:

```java
mMap.setMyLocationEnabled(true);
```

Du wirst sehen, dass der Code mit einer roten Linie unterstrichen ist. Er ist als Fehler markiert.
Der Fehler stammt daher, dass wir noch die Permission vom Benutzer brauchen.  
Bei bestimmten Dingen ist es notwendig, dass der Benutzer in einem Dialog bestätigt, ob die App diese Information 
benutzen darf oder nicht. Das muss man als Programmierer tun.

Damit dem Benutzer ein Dialog zum Zustimmen oder Ablehnen angezeigt wird, füge den folgenden Code am Ende der 
 `onMapReady()` Methode ein um die Zeile, die wir gerade geschrieben haben, zu ersetzen:
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

Die Variable `MY_PERMISSIONS_REQUEST_LOCATION` wird als Fehler markiert, weil sie noch nicht angelegt wurde. 
Das machen wir, indem wir die folgende Zeile am Anfang der Klasse einfügen:
```java
public static final int MY_PERMISSIONS_REQUEST_LOCATION = 99;
```

Wenn du jetzt die App startest, siehst du, dass dem Benutzer ein Dialog angezeigt wird, der danach fragt, ob die App 
den Standort benutzen darf.

Jetzt müssen wir noch auf die Entscheidung des Nutzers reagieren. Das nennt man "Callback".

Am Anfang der Klasse, direkt nach `implements OnMapReadyCallback` und noch vor der öffnenden Klammer `{`, füge ein 
Komma und den folgenden Code ein:  
`ActivityCompat.OnRequestPermissionsResultCallback`.

Am Ende der Klasse, eine Zeile vor der letzten schließenden Klammer `}`, füge folgenden Code ein:  
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

Wenn du nun die App startest, bekommst du einen Dialog um der App zu erlauben, den Standort zu nutzen.

Jetzt ist oben in der rechten Ecke ist jetzt ein Fadenkreuz. Wenn du darauf klickst, zoomt die Map zu deinem aktuellen 
Standort.

## Einen Button Anlegen

Öffne die Datei `activity_maps.xml`. Das ist das Layout der App, das dir angezeigt wird. Momentan ist nur ein `Fragment`
mit einer Map darauf zu sehen. Wir wollen es ändern, so dass unter der Map ein Button angezeigt wird.

Zuerst müssen wir ein `LinearLayout` um das Fragment herum machen.

Füge hierzu diesen Code ganz am Anfang der Datei ein:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:map="http://schemas.android.com/apk/res-auto"
              xmlns:tools="http://schemas.android.com/tools"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:orientation="vertical"
              tools:context="com.teresaholfeld.geoapp.MapsActivity">
```

Und diesen ganz ans Ende:

```xml
</LinearLayout>
```

Von dem schon bestehenden `<fragment>` müssen diese Teile gelöscht werden:

```xml
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:map="http://schemas.android.com/apk/res-auto"
xmlns:tools="http://schemas.android.com/tools"
tools:context="com.teresaholfeld.geoapp.MapsActivity"
```

Ändere die Höhe des Layouts vom `<fragment>` auf 0:

```xml
android:layout_height="0dp"
```

Dann braucht das Fragment eine Gewichtung von 1. Füge diese Zeile ins `<fragment>` ein:

```xml
android:layout_weight="1"
```

Jetzt fügen wir unter dem `<fragment>` einen `<Button>` ein. Füge diesen Code unter das `<fragment>` ein:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button"/>
```

Kannst du den Text des Buttons ändern? Versuch mal ihn in `Share My Location` (meinen Standort teilen) zu ändern.

Starte deine App. Du kannst sehen wie sich dein Layout geändert hat, und nun ein Button angezeigt wird.  
Der Button macht allerdings noch nichts, wenn man ihn klickt.

## Ermittlung des GPS Standorts

In diesem Schritt wollen wir die GPS Location des Smartphones ermitteln, damit wir sie später sharen können.

Öffne das File `app/build.gradle` und füge bei Zeile 29 den folgenden Code ein:

```groovy
compile 'com.google.android.gms:play-services-location:11.0.4'
```

Clicke auf `Sync now`.

In `MapsActivity` füge folgenden Code an Zeile 31 hinzu:

```java
private FusedLocationProviderClient mFusedLocationClient;
```

Am Ende der `onCreate()` Methode, eine Zeile über der schließenden Klammer `}`, füge diesen Code ein:

```java
mFusedLocationClient = LocationServices.getFusedLocationProviderClient(this);
```

Jetzt legen wir die Methode an, die dafür zuständig ist, etwas mit der Location zu machen, sobald das Smartphone 
 seine Location weiß.

Füge diesen Code ans Ende der Klasse, ein oder zwei Zeilen vor dem schließenden `}` :

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

## Button Click: Zeige die Location an

Jetzt wollen wir die aktuelle GPS Position anzeigen, wenn der Button geklickt wird.  
Dafür müssen wir einen `OnClickListener` schreiben, der auf die Button-Clicks lauscht.  
Finde die Methode `onMapReady()` und füge dort am Ende den folgenden Code ein:

```java
Button button = (Button) findViewById(R.id.button);
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        shareLocation();
    }
});
```

Starte die App und schau, was der Button macht, wenn man ihn clickt.

## Den GPS Standort Sharen

Jetzt wollen wir nicht nur einen kurzen Hinweis mit der aktuellen GPS Location anzeigen, wenn der Button geclickt wird,
sondern wir wollen die Location sharen.

Finde die Zeile mit `Toast.makeText(MapsActivity.this, "Location: " + location, Toast.LENGTH_LONG).show();`.

Lösche diese Zeile, und füge stattdessen diesen Code an der Stelle ein:

```java
String locationUrl = "https://www.google.com/maps?ll="
        + location.getLatitude() + "," + location.getLongitude()
        + "&q=" + location.getLatitude() + "," + location.getLongitude();
Intent sendIntent = new Intent(Intent.ACTION_SEND);
sendIntent.putExtra(Intent.EXTRA_TEXT, locationUrl);
sendIntent.setType("text/plain");
startActivity(Intent.createChooser(sendIntent, "Send location"));
```

Starte die App und probier es aus!

## Eine Location Vortäuschen

Es gibt eine Möglichkeit, in Android einen fake Standort vorzutäuschen. Man kann sein Smartphone dazu kriegen, zu 
denken, es hätte einen völlig anderen Standort. Das nennt man auch Mock Location.  
Folge den folgenden Schritten um es auszuprobieren:

Öffne den Play Store auf dem Android Phone. Such nach `Fake GPS Location`. Installiere die App, die so aussieht:

![screen shot 2017-10-09 at 20 28 00 1](https://user-images.githubusercontent.com/11841927/31353126-9af99fcc-ad31-11e7-9434-e5d33002a5f8.png)

Gehe zu den Settings im Smartphone. Gehe zu `System` > `Developer options`. Die sollten an sein.  
Schau nach der Option `Select mock location app`. Dort ist die Fake GPS App, die wir gerade installiert haben. Wähle sie aus.

Öffne die Fake GPS App und verschiebe die Karte, so dass der Marker an einem Ort ist, den du vortäuschen willst. 
Click den Play Button.  
Die App geht dann in den Hintergrund. Wenn du deine Maps App jetzt öggnest, siehst du, dass deine eigene Location 
an diesem anderen Ort ist.

Probiere es aus und share diese Fake Location mit deinen Freunden!

Du kannst die falsche Location jederzeit wieder ausschalten. Öffne die Notification, die ganz oben angezeigt wird, 
und clicke sie. Clicke den Pause Button. Das beendet das Vortäuschen der Fake Location, und du hast deine ursprüngliche 
Location wieder.

## Über diesen Kurs

Dies ist der Leitfaden zu unserem Einsteiger-Kurs zum Thema Android-Apps mit Google Maps und Geolocation bauen.
Wir erklären alles was du wissen musst in diesem Kurs.

Erfahre mehr über die Kurse, die wir mit Hamburg Coding School anbieten unter:  
https://hamburgcodingschool.com/courses/

Wir hoffen, dass es dir Spaß gemacht hat und du bald deine eigene Android-App schreiben wirst. :) 

