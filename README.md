# login_demo

In this Flutter project I learned how to login to Firebase with a username and password.

## Resources
I followed along with [Andrea Bizzotto's](https://www.youtube.com/redirect?redir_token=fDMUYq6T-67GDzX0LWIUcaBo-pN8MTU0ODcxODk5N0AxNTQ4NjMyNTk3&event=video_description&v=aaKef60iuy8&q=https%3A%2F%2Fcodingwithflutter.com%2F) YouTube videos:
- [Flutter & Firebase Auth 01-03](https://www.youtube.com/watch?v=u_Lyx8KJWpg&list=PLNnAcB93JKV9iZ2cwk9MEx3_JG8BRikMP)

I also got a bit out of the Medium article ["_Flutter : How to do user login with Firebase_"](https://medium.com/flutterpub/flutter-how-to-do-user-login-with-firebase-a6af760b14d5) by David Cheah

David's example is so strikingly similar to Andrea's that I am led to believe he used Andrea's work as a starting point.  There were a few enhancements.  Like adding the icon, using a ListView instead of a Column (which is the way we need to do it to work on Android), and walking through setting up the Firebase <-> Flutter connection for Android.
## Challenges with Firebase Dart Package
The most current [Firebase Dart Package](https://pub.dartlang.org/packages/firebase_auth) as I write this is   
```
firebase_auth: ^0.8.0+1
```
After putting this in pubspec.yaml, I was getting errors building for an Android emulator:  
```
Launching lib/main.dart on Android SDK built for x86 in debug mode...
registerResGeneratingTask is deprecated, use registerGeneratedResFolders(FileCollection)
registerResGeneratingTask is deprecated, use registerGeneratedResFolders(FileCollection)
registerResGeneratingTask is deprecated, use registerGeneratedResFolders(FileCollection)
registerResGeneratingTask is deprecated, use registerGeneratedResFolders(FileCollection)
registerResGeneratingTask is deprecated, use registerGeneratedResFolders(FileCollection)
/Users/margaret 1/Flutter/flutter/.pub-cache/hosted/pub.dartlang.org/firebase_auth-0.8.0+1/android/src/main/java/io/flutter/plugins/firebaseauth/FirebaseAuthPlugin.java:9: error: cannot find symbol
import androidx.annotation.NonNull;
                          ^
  symbol:   class NonNull
  location: package androidx.annotation
```        
The "deprecated" warnings I ignored.  Although I'm bummed how ugly it is... this other goop around ```error: cannot find symbol``` Showed a conflict using the latest Firebase Dart package.  I reported this as [an issue on the Dart GitHub](https://github.com/flutter/flutter/issues/27156) repository.  dev-vinicius's comment got stuff working:
>Here i put this in file: android/gradle.properties:

>android.useAndroidX=true

>android.enableJetifier=true

>and changed targetSdkVersion to 28 and it worked. 
## The Biggest...oh...
The biggest challenge I had was bumbling through the steps to register the app with the Firebase console.  Based on what I learned:
- a Firebase project is best served by 1 Flutter app.  This becomes 2 apps in Firebase - 1 for iOS and one for Android.
### iOS
- I found it initially confusing to register either an iOS or Android app.  For iOS, [this section of Andrea's video](https://youtu.be/BNOUtPSN-kA?list=PLNnAcB93JKV9iZ2cwk9MEx3_JG8BRikMP&t=885) (i.e.: 14:45 in on Flutter & Firebase Auth 02) provided an excellent walk through.
### Android
Andrea's videos did not cover Android.  If it did, he would have needed to change a few things.  
#### ListView
In the code the main thing is going from a Column widget to a ListView widget.  When I used the Column widget, the keyboard caused Flutter to let me know it did not have enough room for the UI.  I.e.: in login_page.dart use ListView:
```
child: ListView(
    children: <Widget>[
         _showLogo(),
```                
#### Adding Firebase Dart Package
Refer to the challenges I noted earlier.
#### Letting Gradle Know
There are two gradle files that need to be modified:
- android/app/build.gradle  
  - add `implementation 'com.google.firebase:firebase-core:16.0.1'` to the dependencies section.
  - put `apply plugin: 'com.google.gms.google-services'` as the last line of the file.  
- android/build.gradle  
  - add `        classpath 'com.google.gms:google-services:4.0.1'` to the dependencies section.
## Validators
I was impressed with the TextFormField widget's ability to use validators.  For example, the one I have for zip code check:  
```
validator: (value) => _validateUSZip(value),
```
It was extremely easy to add UI widgets that looked nice and responded as expected.  


