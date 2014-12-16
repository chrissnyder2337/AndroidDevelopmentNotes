

## Permissions
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

### Defining Fragments

#### Staticly Defined
Declare the fragments in the Activity’s layout file:

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

and create a reference to in you activity:
```java
public class MainActivity extends Activity {
    protected void onCreate(Bundle savedInstanceState) {
        mFragmentOne = (FragmentOne) getFragmentManager().findFragmentById(R.id.frag_one);
    }
}
```
#### Progra
* Add it programmatically using the FragmentManager
