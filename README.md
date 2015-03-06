#Taplytics-Android SDK

_Taplytics is a native mobile A/B testing platform that helps you optimize your Android app!_


**Current Version**: [1.3.13](https://github.com/taplytics/Taplytics-Android-SDK/releases/tag/1.3.13)


##Project Setup


_How do I, as a developer, start using Taplytics?_ 

1. _Sign up for a free account at Taplytics.com._
2. _Install the SDK._
3. _Create an experiment and push it live to your users!_

####The Border / Shake menu. 

When connected to an experiment on a **debug** build, a border will show around your app window. This shows which experiment and variation you are currently viewing.

You can long-press on the top of the border to switch experiments, or shake your device and pick from the menu, or select an experiment from the Taplytics website.

**The border and shake menu will _NOT_ appear on release builds.**


### Android Studio Installation

1. _In your module’s build.gradle, add the url to the sdk._

	  ```
  repositories {                                                                                              
    maven { url "https://github.com/taplytics/Taplytics-Android-SDK/raw/master/AndroidStudio/" }
  }      
  ```
  
2. _In your module’s build.gradle dependencies, compile Taplytics and its dependencies._

  	```
  dependencies {                                                                   
    //Taplytics                                                                        
    compile("com.taplytics.sdk:taplytics:+@aar")  
    
    //Dependencies for taplytics
    compile("com.mcxiaoke.volley:library:+")
    compile("com.squareup.okhttp:okhttp-urlconnection:+")
    compile("com.squareup.okhttp:okhttp:+")
   
    //Excluding org.json due to compiler warnings
    //socket.io connections only made on debug devices OR if making live changes to a release build.
    //No socket.io connection will be made on your release devices unless explicitly told to do so. 
    compile("com.github.nkzawa:socket.io-client:+") {
        exclude group: 'org.json'
    }
    compile("com.github.nkzawa:engine.io-client:+") {
        exclude group: 'org.json'
    }
    
    //Only include this if you wish to enable push notifications:
    compile("com.google.android.gms:play-services-base:6.5.87")
  }    
  ```
  
  
3. _Override your application’s onCreate() method (not your main activity) and call Taplytics.startTaplytics(). It should look like this:_

	```java	  	  
  public class ExampleApplication extends Application {
    @Override
    public void onCreate() {
      super.onCreate();
      Taplytics.startTaplytics(this, "YOUR TAPLYTICS API KEY");
    }
  }
  ```
4. _Now, add the proper permissions, and the Application class to your app’s AndroidManifest.xml in the Application tag._

 	 ```xml
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <application
    android:name=".ExampleApplication"
    ...
  ```

5. _Finally, add the following intent-filter tag to the end of your *MAIN* activity:_
	
	First, [get your Taplytics URL Scheme from your Project's Settings](https://taplytics.com/dashboard):

	![image](https://taplytics.com/assets/docs/install-sdk/url-scheme.png)
	
	Then, add it to your manifest:

	```xml
			...
	           <intent-filter>
                <action android:name="android.intent.action.VIEW"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <category android:name="android.intent.category.BROWSABLE"/>
                <data android:scheme="YOUR URL SCHEME"/>
            </intent-filter>
        </activity>
    ```


6. _Add the following to your Proguard rules:_

  
  
  (Only if using Support Fragments)

	```
  -keep class android.support.v4.app.Fragment { *; }
  -keep class android.support.v4.view.ViewPager
  -keepclassmembers class android.support.v4.view.ViewPager$LayoutParams {*;}
  ```
  
  (Only if using Mixpanel)
  
  	```
  	-keep class com.mixpanel.android.mpmetrics.MixpanelAPI { *;}
  	```
  
  (Only if using Flurry)
  
  	```
	-keep class com.flurry.android.FlurryAgent { *; }
  	```
  	
  (Only if you see gradle compiler errors with com.okio)
  ```
  	-dontwarn okio.**
	-dontwarn java.nio.file.*
	-dontwarn org.codehaus.mojo.animal_sniffer.IgnoreJRERequirement
```
  
7. _That's it! Now build and run your app, you can start creating experiments with Taplytics!_


### Eclipse Installation

1. _Download the taplytics.jar [here](https://github.com/taplytics/Taplytics-Android-SDK/raw/master/taplytics.jar)_
2. _Copy the jar into your 'libs' directory in your project._
3. _Right click the jar in Eclipse, click Build Path > add to build path_
4. **NEW:** _Add Google Play Services to your project by following the steps listed [here.](http://developer.android.com/google/play-services/setup.html) Be sure to change the dropdown to "Eclipse with ADT"
5. _Override your application’s onCreate() method (not your main activity) and call Taplytics.startTaplytics(). It should look like this:_

	```java
  public class ExampleApplication extends Application {
    @Override
    public void onCreate() {
      super.onCreate();
      Taplytics.startTaplytics(this, "YOUR TAPLYTICS API KEY");
    }
  }
  ```
6. _Add the proper permissions, and the Application class to your app’s AndroidManifest.xml in the Application tag._

  	```xml
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <application
    android:name=".ExampleApplication"
    ...
  ```
  
7. _Finally, add the following intent-filter tag to the end of your *MAIN* activity:_
	
	First, [get your Taplytics URL Scheme from your Project's Settings](https://taplytics.com/dashboard):

	![image](https://taplytics.com/assets/docs/install-sdk/url-scheme.png)
	
	Then, add it to your manifest:

	```xml
			...
	           <intent-filter>
                <action android:name="android.intent.action.VIEW"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <category android:name="android.intent.category.BROWSABLE"/>
                <data android:scheme="YOUR URL SCHEME"/>
            </intent-filter>
        </activity>
    ```

8. _Add the following to your Proguard rules:_
  
  (Only if using Support Fragments)

	```
  -keep class android.support.v4.app.Fragment { *; }
  -keep class android.support.v4.view.ViewPager
  -keepclassmembers class android.support.v4.view.ViewPager$LayoutParams {*;}
  ```
  
  (Only if using Mixpanel)
  
  	```
  	-keep class com.mixpanel.android.mpmetrics.MixpanelAPI { *;}
  	```
  
  (Only if using Flurry)
  
  	```
	-keep class com.flurry.android.FlurryAgent { *; }
  	```
  
  (Only if you see gradle compiler errors with com.okio or com.nio)
  ```
  	-dontwarn okio.**
	-dontwarn java.nio.file.*
	-dontwarn org.codehaus.mojo.animal_sniffer.IgnoreJRERequirement
```
  
  
9. _That's it! Now build and run your app, you can start creating experiments with Taplytics!_

##Usage

To use Taplytics and set up experiments, refer to [the Taplytics online docs.](https://taplytics.com/docs/)

To switch between experiments or variations in debug mode, select them on the website, or **shake the device ** and select which variation or experiment you wish to view.

###Starting Options


Taplytics can be started with a few options to help you use it during development.

First, the base method:

```java
Taplytics.startTaplytics(this, "Your Api Key");
```

Or, add a map of options.

```java
Hashmap<String, Object> options = new Hashmap<>();
options.put("optionName",optionValue);
Taplytics.startTaplytics(this, "Your Api Key");
```

Possible options are:

| Option Name  | Values | Default | Explanation
|---|---|---|---|
|liveUpdate   | boolean: true/false  | true  | Disable live update to remove the border, and activity refreshing in your debug builds to test the functionality of your applications as if they were in release mode. Note that this functionality is always disabled in release builds.  |   
| shakeMenu | boolean: true/false   | true  | In your debug builds, disable the quick menu that appears when you shake your device. This menu is never present in release builds.|   

###Code Experiments

####Setup

To set up a code experiment in Taplytics, please refer to the [Taplycs code-based experiment docs](https://taplytics.com/docs/code-experiments).

####Usage

Taplytics automatically generates the base needed for your code experiment. Paste it into the relevant section of your app, and apply the variables as necessary. 

It is suggested that you place the code calling the experiment in its own function (as opposed to an oncreate).

For example:

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(some_layout);
	
	// Run this code experiment. This triggers the experiment.
	runAnExperiment();
}
```
	
Then, in that function, add your experiment code generated by Taplytics, and modify it as you need.
	
```java
private void runAnExperiment(){
	Taplytics.runCodeExperiment("experiment name", new TaplyticsCodeExperimentListener() {
	
		@Override
		public void baselineVariation(Map<String, Object> variables) {
        
        		// Insert baseline variation code here.
        		Object myVar0 = variables.get("foo"); // can be null if no experiment is found
        	}
        	
		@Override
		public void experimentVariation(String variationName, Map<String, Object> variables) {
    	 		Object myVar0 = variables.get("foo"); // can be null if no experiment is found
    	 	
    	 		if (variationName.equals("Variation 1")) {
    	 			// Insert Variation 1 variation code here.
    	 		} else if (variationName.equals("Variation 2")) {
				// Insert Variation 2 variation code here.
			}
		}
	
		@Override
		public void experimentUpdated() {
			// Use this method to re-run your code experiments when testing your ex	periment variations.
		}
	});
}
```

This separate function is suggested, because if you would like to update experiments instantly for debug testing or another reason, you can simply place the `runAnExperiment()` function into the `experimentUpdated()` block.

###Events

####Automatic Events

Some events are automatically tracked by Taplytics and will appear on your dashboard. These events are:

* App Start
* Activity and/or Fragment load
* Activity and/or Fragment destroy
* Activity pause
* App background
* Viewpager changes

App terminate is also tracked, but this is only true when your MAIN activity is at the bottom of your activity stack, and the user exits the app from that activity.

No changes are needed in your code for this event tracking to occur. 

####Custom Events

To log your own events, simply call: 		

```java
Taplytics.logEvent("Your Event Name");
```

You can also log events with numerical values:

```java
Number num = 0;
Taplytics.logEvent("Your Event Name", num);
```
	
And with custom object data:

```java
Number num = 0;
JSONObject customInfo = new JSONObject();
customInfo.put("some title",someValue)
Taplytics.logEvent("Your Event Name", num, customInfo);
```

####Revenue Logging

Its also possible to log revenue.

Revenue logging is the exact same as event logging, only call `logRevenue`:

```java
Number someRevenue = 10000000;	
Taplytics.logRevenue("Revenue Name", someRevenue);
```
	
And similarly, with custom object data:

```java	
Number someRevenue = 10000000;
JSONObject customInfo = new JSONObject();
customInfo.put("some rag",someValue)
	
Taplytics.logRevenue("Revenue Name", someRevenue, customInfo);
```

###User Attributes

Its possible to send custom user attributes to Taplytics using a JSONObject of user info. 

The possible fields are:

|Parameter  |Type         |
|---	    |---          |
|email	    |	String    |
|user_id    |	String    |
|firstname  |	String    |
|lastname   |	String    |
|name	    |	String    |
|age	    |	Number    |
|gender	    |	String    |
|customData |	JsonObject|

Where the customData key lets you pass in custom user data as a JSONObject.

For example:

```java
JSONObject attributes = new JSONObject();
attributes.put("email", "johnDoe@taplytics.com");
attributes.put("name", "John Doe");
attributes.put("age", 25);
attributes.put("gender", "male");
attributes.put("avatarUrl", "https://someurl.com/someavatar.png");

JSONObject customData = new JSONObject();
customData.put("paidSubscriber", true);
customData.put("subscriptionPlan", "yearly");
attributes.put("customData", customData);

Taplytics.setUserAttributes(attributes);
```

###Visual Editing

All visual editing is done on the Taplytics dashboard. See the docs on visual editing [here](https://taplytics.com/docs/visual-experiments).

###Advanced Device Pairing

Link a device (even in release mode) to Taplytics.

**NOTE: This is used only for deeplink pairing, and is unnecessary if your main activity does NOT have a singleTask flag.**

Retrieve deeplink through Taplytics deeplink intercepted via either email or SMS device pairing. It contains your Taplytics URL scheme and device token. If you wish to intercept the deeplink and then pair the device yourself in your application's code, call this method, like so:

```java
private void handleDeepLink(Intent intent) {
  String tlDeeplink = intent.getDataString(); //example deep link: 'tl-506f596f://e10651f9ef6b'
  if (tlDeeplink == null) {
      // No deeplink found
      return;
  }
  Taplytics.deviceLink(tlDeeplink);
}
```

###Push Notifications
If you wish to use Push Notifications on Taplytics, you must add the following permissions (replace the example name with your app name) to your Android Manifest:

```xml
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
<permission android:name="com.taplytics.testapp.permission.C2D_MESSAGE"/>
<uses-permission android:name="com.taplytics.testapp.permission.C2D_MESSAGE" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
```
And you must add the following receiver and service under your application tag:

```xml
<receiver
    android:name="com.taplytics.sdk.TLGcmBroadcastReceiver"
    android:permission="com.google.android.c2dm.permission.SEND" >
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    </intent-filter>
</receiver>
<service android:name="com.taplytics.sdk.TLGcmIntentService" />
```

Now, just follow the docs on [Taplytics](https://taplytics.com/docs/push-notifications/google-push-certificates) to get started with Push Notifications in your application!
