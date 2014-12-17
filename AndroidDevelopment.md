Andriod Development Notes
=========================

Android Software Stack
-------------
1. Linux Kernal
    * Security
    * Memory management
    * Drivers:
        * Example: Display driver
    * Android Specific Linu:
        * Power management
        * Binder
        * Low Memory killer
2. Libraries & Android runtime
    * System C Library, SLL, Webkit
    * Core Java Libraries
        * basic java classes (java.*)
        * android java classes (android.*)
        * Junit classes
    * Dalvaik Virtual Machine
        * designed to run in resource constrined enviroments
3. Appplication Framework
    * Package Manager
        * DB that keeps track of all applications installed.
    * View System
    * Activity Manager
    * Resource Manager
        * Manages non-compliles resources like strings and images
4. Applications
    * phone
    * browse

-------------
Application Components
-------------
There are 4 main components:

* Activity
* Serivce
* Broardcastreceiver
* Contentprovider


### Activity Class

Used to present GUI to user and capture their interaction


#### Activity Lifecycle
*Dont make fun of my ascii art*

```
ACTIVITY LAUNCED
    ||
    \/
onCreate()<=====================\\
    ||                          ||
    \/                          ||
onStart()<=========onRestart()  ||
    ||                   /\     ||
    \/                   ||     || App. process
onResume()<===========   ||     ||    killed
    ||               ||  ||     ||
    \/               ||  ||     ||
ACTIVITY RUNNING     ||  ||     ||
    ||               ||  ||     ||
    \/               || /==\    ||
onPause()===========/_==/||\====++
    ||                   ||     ||
    \/                   ||     ||
onStop()========================//
    ||
    \/
onDestroy()
    ||
    \/
ACTIVITY SHUTDOWN
```
##### onCreate()

Things typicly done during this function call:

* Restore saved state
    * call `super.onCreate()`
* set content view
* initialize UI
* Link UI to actions

##### onRestart()
Called only when the activity has been stopped and is about to be started again.

##### onStart()
When the activity is about to become visible.

##### onResume()
When applicaiton is about to become in forground

##### onPause()

When applicaiton leaves the forground. Last chance to do something before applicaiton closes. It is guarentted to be run. 

##### onStop()

When application is about to become invisible.
NOTE: May not be called if android kills your application.


##### onDestroy()

NOTE: May not be called if android kills your application.

### Service Class

Runs long running operations. Runs in the background.

### BroadcastReciver Class

Listens for and responds to events. They "subscribe" to events.

### ContentProvider Class

Facilitates the storage and access of data. 
Example: bookmarks are stored in a content provider.


### Resource: Strings

Strings strings are stored in *res/values/strings.xml*
Itallian strings are stored in *res/values-it/strings.xml*
Stored in xml tags: `<string name="hello">Hello World</string>`


##### Accessing strings

By other resource files: `@string/string_name`
By java files: `R.string.string_name`

### Resource: Layout fikes

Stored in *res/layout/*.xml*.
Landscape version of the layout file stored in *res/layout-land/*.xml*


### AndroidManifest.xml

* Application Name
* Components
* Permissions
* Minnimanl API Level `<uses-sdk android:minSdkVersion="16" >

### Task Backstack

When activities are started they are added to the task backstack. When a the back button is pressed the current task is killed and is poped off the backstack.

--------
Intents
-------

New activities can be started with intents. Intents can be passed to either:

Intents are passed to the following methods:
* startActivity()
* startActivityForResult()

-------------
Permissions
-------------
### Using Permissions
To state that your application uses a permission add a `<uses-permission>` tag to the AndroidManifest.xml.

```xml
<manifest>
    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
</manifest >
```

### Defining Permissions
To define the existence of the permission add the `<permission>` tag to the to the AndroidManifest.xml.

```xml
<manifest>
    <permission 
    android:name="com.examle.appmane.NEW_PERM"
    android:description="@string/new_perm_description"
    android:label="@string/new_perm_label_string">
    </permission>
</manifest>
```

To define that an application will enforce the permission add the  `android:permission` attribute to the `<application>` tag in the AndroidManifest.xml.

```xml
<manifest>
    <application
    android:icon="..."
    android:label="..."
    android:permission="com.examle.appmane.NEW_PERM">
    
    </application>
</manifest>
```

Fragment Class
-------------

Each fragment represents a portion of the UI and behavior. An Activity can have multiple fragments.

### Fragment Lifecycle States

* **Resumed** - Fragment is visible, and has focus.
* **Paused** - Hosting activity is visible, but fragment does not have focus.
* **Stopped** - Fragment is not visible.

### Fragment Lifecycle 
##### Hosting Activity Created
<dl>
<dt>onAttach()</dt><dd>Fragment is attached to Activity</dd>
<dt>onCreate()</dt><dd>Initialize the Fragment</dd>
<dt>onCreateView()</dt><dd>Fragment sets up the UI and returns it.</dd>
<dt>onActivityCreated()</dt><dd>Fragment has been all set up and ready to go.</dd>
</dl>

##### Hosting Activity Started
<dl>
<dt>onStart()</dt><dd>Hosting Activity about to become visible.</dd>
</dl>

##### Hosting Activity Resumed
<dl><dt>onResume()</dt><dd>Hosting Activity about to become visible and ready for user interaction.</dd></dl>

##### Hosting Activity Paused
<dl><dt>onPaused()</dt><dd>Hosting Activity is paused</dd></dl>

##### Hosting Activity Stopped
<dl><dt>onStop()</dt><dd>Hosting Activity is stopped</dd></dl>

##### Hosting Activity Destroyed
<dl><dt>onDestroyView()</dt><dd>Called when Hosting Activity detaches the view created by onCreateView()</dd>
<dt>onDestroy()</dt><dd>Called when Hosting Activity destroy's the fragment. You should null out references to the hosting activity here.</dd>
<dt>onDetach()</dt><dd>Called when Hosting Activity detaches the Fragment</dd></dl>

#### Staticly Defined
Declare the Fragments in the Activity’s layout file:

```xml
<LinearLayout>
    <fragment
        android:id="@+id/frag_one"
        android:layout_width="..."
        android:layout_height="..."
        android:layout_weight="..."
        class="com.example.ApplicationName.FragmentOne" />
    <fragment
        android:id="@+id/frag_two"
        android:layout_width="..."
        android:layout_height="..."
        android:layout_weight="..."
        class="com.example.ApplicationName.FragmentTwo"
    />
</LinearLayout>
```

Create a reference to in you Activity:
```java
public class MainActivity extends Activity {
    protected void onCreate(Bundle savedInstanceState) {
        mFragmentOne = (FragmentOne) getFragmentManager().findFragmentById(R.id.frag_one);
    }
}
```

Inflate the the fragments's layout file in onCreateView()
```java
public class FragmentOne extends Fragment {
    protected void onCreateView(LayoutInflater inflater, ViewGroup containter, Bundle savedInstanceState) {
        return inflater.inflate(R.layout.frag_one_layout, container, false);
    }
}
```
#### Dynamicly Defined

Define space for the fragments in the Activity's layout.xml by using FrameLayouts:
```xml
<LinearLayout>
    <FrameLayout
        android:id="@+id/first_frame"
        android:layout_width="..."
        android:layout_height="..."
        android:layout_weight="..." />
</LinearLayout>
```

Add the fragment to the frame following the following steps:
1. Get a reference to the FragmentManager
2. Begin a FragmentTransaction
3. Add the Fragment 
4. Commit the FragmentTransaction


```java
public class MainActivity extends Activity {
    protected void onCreate(Bundle savedInstanceState) {
       // 1. Get a reference to the FragmentManager
       FragmentManager fragmentManager = getFragmentManager();
       
       // 2. Begin a FragmentTransaction
       FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
       
       // 3. Add the Fragment(s)
       fragmentTransaction.add(R.id.first_frame, new FragmentOne());
       
       // 4. Commit the FragmentTransaction
       fragmentTransaction.commit();
    }
}
```

##### Notable Fragment methods

* **FragmentTransaction.addToBackStack(null)** - Adds the fragment transaction to the backstack so when the user hit the back button the fragment transaction will "undo"
* **FragmentManager.executePendingTransactions()** - Forces the fragment changes to be applied immedialty. 
* **Fragment.setRetainInstance(true)** - Andorid won't destroy the fragment on configuration changes.

------------
User Interface
--------------
TODO

--------------
User Notifications
---------------

### Toast Messaes

Basic usage:
```java
Toast.makeText(getApplicationContext(), "Message", Toast.LENGTH_SHORT).show()
```

Toast messages with custom view:

```java
Toast toast = new Toast(getApplicationContext());
toast.setGravity(Gravity.CENTER_VERTICAL, 0, 0);
toast.setDuration(Toast.LENGTH_LONG);
toast.setView(getLayoutInflater().inflate(R.layout.custom_layout, null));
toast.show();
```

### Notificaiton Area Notifications

* Notification (core notification
    * Title
    * Detail 
    * Small icon
* Notification Area
    * Ticker text (displayed when notification first arrives in the notification bar)
    * Small icon
* Notification Drawer
    * View
    * Action (action that occurs when the user clicks on the drawer view)

```java
Notificaiton.Builder notificationBuilder = new Notificaiton.Builder(getApplicationContext())
    .setTicker(tickerText)
    .setSmallIcon(android.R.drawable.stat_sys_warning)
    .setAutoCancel(true)
    .setContentIntent(mContentIntent)
    .setSound(soundURI)
    .setVibrate(mVibratePattern)
    .setContent(mContentView);
    
NotificationManager mNotificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
mNotificationManager.notify(MY_NOTIFICATION_ID, notificationBuilder.build());
```

----------------------
BroadcastReciver Class
----------------------

### Register a BroadcastReciver
Staticly Register the BroadcastReciver in the AndroidManifest.xml. Staticly registered broadcastRecivers are registered by Android at boot.

```xml
<receiver
    android:enabled=["true" | "false"]
    android:exported=["true" | "false"]
    android:icon="drawable resource"
    android:label="string resource"
    android:name="string"
    android:permission="string"
    android:process="string" >
    <intent-filter> </intent-filter>
</receiver>
```

### Send a Broadcast
```java
sendBroadcast(new Intent("com.example.SomeClass.SomeIntent"), android.Manifest.permission.VIBRATE);
```


### Recive a Broadcast
```java
public class Revicer extends BroadcastRevicer {
    onReceive(Context context, Intent intent) {
        // Do something with passed intent.
    }
}
```


### Dynamic Registration of BroadcastsRecivers
Dymanic registration can be helpful if you only want to revice broadcasts when your applciation is in the forground. If that is the case you would create it in onCreate() and remvoe it in onPause().

Can use the following methods to register the BroadcastReciver at runtime: 
* LocalBroadcastManager.registerReceiver() 
* Context.registerReceiver()

### Event Broadcasts

###### Normal vs. Ordered
* Normal: processing order undefined
* Ordered: sequential processing in priority order 

###### Sticky vs. Non-Sticky 
* Sticky: Store Intent after initial broadcast 
* Non-Sticky: Discard Intent after initial broadcast 



