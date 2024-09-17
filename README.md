Android Jetpack is a set of components, tools, libraries and guidance to help developers to create a brilliant quality-oriented application. In this blog we are going to discuss the latest addition to the Jetpack, the “Navigation Component”. Assuming that you have basic knowledge in android and you are eager to master in android.


In addition, your app can run on the various versions because android Jetpack components run independently and providing backward compatibility.

The “Navigation Component” simplifies implementing navigation, while also helping you visualize your app’s navigation flow. The library provides a number of benefits, including:

Automatic handling of fragment transactions
Correctly handling up to and back by default.
Default behaviours for animations and transitions
Deep linking as a first-class operation.
Implementing navigation UI patterns (like navigation drawers and bottom nav) with little additional work.
Type safety when passing information while navigating.
Android Studio tooling for visualizing and editing the navigation flow of an app
Overview of Navigation
If the creator cannot define it well, then who can define it well?. I loved the way Google defined “Navigation Component”

From the developer site :

Navigation is a framework for navigating between ‘destinations’ within an Android application that provides a consistent API whether destinations are implemented as Fragments, Activities, or other components.

The Navigation Component consits of three key parts:


1. Navigation Graph (New XML resource) — This is a resource that contains all navigation-related information in one centralized location. This includes all the places in your app, known as destinations, and possible paths a user could take through your app.

2. NavHostFragment (Layout XML view) — This is a special widget you add to your layout. It displays different destinations from your Navigation Graph.

3. NavController (Kotlin/Java object) — This is an object that keeps track of the current position within the navigation graph. It orchestrates swapping destination content in the NavHostFragment as you move through a navigation graph.

Getting Started
Be ready with Android Studio 3.2 or higher and enable Navigation

The navigation editor is a standard part of Android Studio 3.3 and higher. If you’re using Android Studio 3.2, navigation is an experimental feature and you’ll need to enable it:

Click File > Settings (Android Studio > Preferences on Mac)
Select the Experimental category in the left pane
Check Enable Navigation Editor
Restart Android Studio

Enable Navigation
Gradle Plugins
Add the dependencies for the artifacts you need in the build.gradle file for your app or module:


dependencies {
 def nav_version = “2.1.0-beta01”
 def nav_version_ktx = “2.1.0-beta01”
 // For Java
 implementation “androidx.navigation:navigation-fragment:$nav_version”
 implementation “androidx.navigation:navigation-ui:$nav_version”
 // For Kotlin
 implementation “androidx.navigation:navigation-fragment-ktx:$nav_version_ktx”
 implementation “androidx.navigation:navigation-ui-ktx:$nav_version_ktx”
}
Fore SafeArgs (we are discussing this later in this post), we need to add the following classpath in your top level build.gradle file:

buildscript {
    ext.kotlin_version = '1.3.40'
    repositories {
        google()
        jcenter()
        
    }
    dependencies {
        def nav_version = "2.1.0-beta01"

        classpath 'com.android.tools.build:gradle:3.4.1'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
    }
}
And you have to apply the plugin in the build.gradle of your app module

apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'
apply plugin: "androidx.navigation.safeargs.kotlin"
You must have android.useAndroidX=true in your gradle.properties file as per Migrating to AndroidX.


#....Other Settings
android.useAndroidX=true
#......

You Are Ready!! Let’s Start “Navigation”
After the plugins are configured, we can start navigation!. After understanding the power of “Navigation Component” I am so excited to create all of my application with a single Activity and couple of Fragments.

Following are major components:

Your MainActivity class
A couple of fragments
Navigation Graph
Action
Destinations
Pop Up To
Arguments
Deep Linking
Navigation Host Fragment
Navigation Controller
The first two components you will be used in your past/current applications and I am not going to describe those. I would like to suggest to have fragments ready before you creating Navigation Graph, which will help you to relate things with what you know and will avoid any confusions.

Navigation Graph
So again I am quoting developer site definition

A navigation graph is a resource file that contains all of your destinations and actions. The graph represents all of your app’s navigation paths.

The following figure shows a visual representation of a navigation graph for a sample app containing six destinations connected by five actions. Each destination is represented by a preview thumbnail, and connecting actions are represented by arrows that show how users can navigate from one destination to another.


Destinations are the different content areas in your app.
Actions are logical connections between your destinations that represent paths that users can take.
How to create a navigation graph?

Create a new resource directory named “navigation”

Create a navigation graph
2. Create a navigation resource file “app_navigation.xml”, the name should follow the resource file name rules.


Since I am more interested in design through xml coding I will explain through codes. So after creating the “app_navigation.xml” your code will look like this


<?xml version=”1.0" encoding=”utf-8"?>
<navigation xmlns:android=”http://schemas.android.com/apk/res/android"
 xmlns:app=”http://schemas.android.com/apk/res-auto"
 xmlns:tools=”http://schemas.android.com/tools"
 android:id=”@+id/app_navigation”
 >
</navigation>
The next steps should be defining the first navigation view, that is from where the navigation should start and to where the navigation should navigate. Such details are commanding through tag called “destination”

I have used the following 3 fragments to explain the concepts of destination, arguments, action.

MyHomeFragment
MySecondFragment
MyThirdFragment
Add all the fragments as child elements into the navigation parent, make sure that you are assigned some unique id to your fragments.

<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:app="http://schemas.android.com/apk/res-auto"
xmlns:tools="http://schemas.android.com/tools"
android:id="@+id/app_navigation">
    <fragment
         android:id="@+id/myHomeFragment"
      android:name="com.navigation.component.sample.ui.fragments.MyHomeFragment"
              android:label="fragment_my_home"
              tools:layout="@layout/fragment_my_home">

    </fragment>

    <fragment android:id="@+id/mySecondFragment"
              android:name="com.navigation.component.sample.ui.fragments.MySecondFragment"
              android:label="fragment_my_second"
              tools:layout="@layout/fragment_my_second">


    </fragment>

    <fragment android:id="@+id/myThirdFragment"
              android:name="com.navigation.component.sample.ui.fragments.MyThirdFragment"
              android:label="fragment_my_third"
              tools:layout="@layout/fragment_my_third">
      
    </fragment>
</navigation>
There 4 parameters used

android:id: unique id for the fragment, just like we are assigning id to any other widgets in xml layout
android:name: It is the fully qualified name of your fragment class in kotlin/java
android:label: A string to identify the fragment
tools:layout: An id of layout resource file from res/layout
To define the start view we use startDestination tag in the root tag of navigation and specify the fragment id

<?xml version="1.0" encoding="utf-8"?>
<navigation
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:id="@+id/app_navigation"
        app:startDestination="@id/myHomeFragment">
    <fragment android:id="@+id/myHomeFragment"
              android:name="com.navigation.component.sample.ui.fragments.MyHomeFragment"
              android:label="fragment_my_home"
              tools:layout="@layout/fragment_my_home">

    </fragment>

    <fragment android:id="@+id/mySecondFragment"
              android:name="com.navigation.component.sample.ui.fragments.MySecondFragment"
              android:label="fragment_my_second"
              tools:layout="@layout/fragment_my_second">


    </fragment>

    <fragment android:id="@+id/myThirdFragment"
              android:name="com.navigation.component.sample.ui.fragments.MyThirdFragment"
              android:label="fragment_my_third"
              tools:layout="@layout/fragment_my_third">

    </fragment>
</navigation>
Action

Actions
The navigation system also allows you to navigate via actions. As shown in the picture, the lines shown in the navigation graph are visual representations of actions. The end of each arrow is called destinations.

To add action in a fragment we can uses <action/> tag inside the <fragment/> tag. We can define more than one actions with a different id.

<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools" android:id="@+id/app_navigation"
            app:startDestination="@id/myHomeFragment">
    <fragment android:id="@+id/myHomeFragment"
              android:name="com.navigation.component.sample.ui.fragments.MyHomeFragment"
              android:label="fragment_my_home"
              tools:layout="@layout/fragment_my_home">
        <action android:id="@+id/action_myHomeFragment_to_mySecondFragment"
                app:destination="@id/mySecondFragment"
                app:enterAnim="@anim/slide_in_right"
                app:exitAnim="@anim/slide_out_left"
                app:popEnterAnim="@anim/slide_in_left"
                app:popExitAnim="@anim/slide_out_right"/>
    </fragment>
    <fragment android:id="@+id/mySecondFragment"
              android:name="com.navigation.component.sample.ui.fragments.MySecondFragment"
              android:label="fragment_my_second"
              tools:layout="@layout/fragment_my_second">
       
        <action android:id="@+id/action_mySecondFragment_to_myThirdFragment"
                app:destination="@id/myThirdFragment"
                app:enterAnim="@anim/slide_in_right"
                app:exitAnim="@anim/slide_out_left"
                app:popEnterAnim="@anim/slide_in_left"
                app:popExitAnim="@anim/slide_out_right"/>

    </fragment>
    <fragment android:id="@+id/myThirdFragment"
              android:name="com.navigation.component.sample.ui.fragments.MyThirdFragment"
              android:label="fragment_my_third"
              tools:layout="@layout/fragment_my_third">
        <action android:id="@+id/action_myThirdFragment_to_myHomeFragment3"
                app:popUpTo="@id/myHomeFragment"

        />
        <action android:id="@+id/action_myThirdFragment_to_mySecondFragment"
                app:popUpTo="@id/mySecondFragment"/>
    </fragment>
</navigation>
Let’s discuss the parameters used inside <action/>

android:id: unique id for the action, just like we are assigned id to the fragments
app:destination: The unique id of the destination fragment, means this action will move the current view to the destination fragment
app:popUpTo: This is for backward navigation if the app has navigated from a fragment A to fragment B, then fragment B to fragment C. If you want to go fragment A to fragment C you can use this parameter value as the id fragment A. The back stack management and the lifecycle will mange by the Navigation Component.
Also, we can define the animations for the fragment transaction as specified, you must have to create the xml animation file inside res/anim folder.
Now you are ready with navigation design and now go ahead with navigation calls.

But …? How…?

After a few minutes read I can’t find the relation in the source file!!.

Don’t be frustrated, as your mind says we have to do something with our source file, there arises the need of NavHostFragment&NavigationController

NavHostFragment

NavigationHostFragment
To get this all to work, you need to modify your activity layouts to contain a special widget called a NavHostFragment. A NavHostFragment swaps different fragment destinations in and out as you navigate through the navigation graph.

<LinearLayout
    .../>
    <androidx.appcompat.widget.Toolbar
        .../>
    <fragment
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:id="@+id/my_nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        app:navGraph="@navigation/app_navigation"
        app:defaultNavHost="true"
        />
    <com.google.android.material.bottomnavigation.BottomNavigationView
        .../>
</LinearLayout>
This is a layout for an activity. It contains global navigation, including a bottom nav and a toolbar
android:name="androidx.navigation.fragment.NavHostFragment" andapp:defaultNavHost="true" connect the system back button to the NavHostFragment
app:navGraph="@navigation/app_navigation" associates the NavHostFragment with a navigation graph. This navigation graph specifies all the destinations the user can navigate to, in this NavHostFragment
NavigationController
NavController is powerful because when you call methods like navigate() or popBackStack(), it translates these commands into the appropriate framework operations based on the type of destination you are navigating to or from. For example, when you call navigate() with an activity destination, the NavController calls startActivity() on your behalf.

Few ways to get NavigationController

Fragment.findNavController()
View.findNavController()
Activity.findNavController(viewId: Int)
Navigate to a Destination with NavController
It’s your turn to navigate using NavController. You'll hook up the Navigate To Destination button to navigate to the mySecondFragment destination (which is a destination that is a MySecondFragmen

Now open MyHomeFragment.kt or your java fragment file and implement the onClick listener or any other user action and navigate as follows

view?.findViewById<Button>(R.id.button).setOnClickListener(View.OnClickListener {
    findNavController().navigate(R.id.action_myHomeFragment_to_mySecondFragment)
})
How to pass arguments?

A valid doubt at this point..!. There are two ways

The usual bundle, click here if you have more doubt.
SafeArgs
SafeArgs
The navigation component has a Gradle plugin, called safe args, that generates simple object and builder classes for type-safe access to arguments specified for destinations and actions.

<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools" android:id="@+id/app_navigation"
            app:startDestination="@id/myHomeFragment">
    <fragment android:id="@+id/myHomeFragment"
              ..../>
    </fragment>
    <fragment android:id="@+id/mySecondFragment"
              android:name="com.navigation.component.sample.ui.fragments.MySecondFragment"
              android:label="fragment_my_second"
              tools:layout="@layout/fragment_my_second">
        <argument
                android:name="arg2"
                app:argType="integer"
                android:defaultValue="2"/>
        <argument android:name="arg3"
                  app:argType="com.navigation.component.sample.data.MyParcelableDataArgs"
                  app:nullable="true"/>
        <argument
                android:name="arg1"
                app:argType="string"
                android:defaultValue="default string"/>
        <action android:id="@+id/action_mySecondFragment_to_myThirdFragment"
                .../>

    </fragment>
    <fragment android:id="@+id/myThirdFragment"
  .../>
    </fragment>
</navigation>
Since we used the <argument> tag for MySecondFragment, safeargs generates a class called MySecondFragmentArgs


Note that we have used parcelable class for arguments. So from the above code snippet, we have used most of the possible ways to pass arguments, click here to see the complete list of safeArgs data types support by android

Errors I met In my project
You can download or clone the sample app from my GitHub

https://github.com/mriyas/NavigationComponentSample?source=post_page — — — — — — — — — — — — — -

I have met a few errors in creating the sample app, sharing those too

AndroidX dependency.


You have to android.useAndroidX=true in your gradle.properties file

2. Warning on AppBarConfiguration()


Added the following compiling options in my build.gradle file of app module

android {
    compileSdkVersion 28
    defaultConfig {
        applicationId "com.navigation.component.sample"
        minSdkVersion 21
        targetSdkVersion 28
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
  
    compileOptions {
        sourceCompatibility = 1.8
        targetCompatibility = 1.8
    }

    kotlinOptions {
        jvmTarget = "1.8"
    }
}
3. SafeArgs Plugin Failed for Parcelable data class

Cause: androidx.navigation.safeargs plugin failed.
 Following errors found: 
D:\Riyas Projects\WiLabs\NewsApp\Sample Source Codes\NavigationComponentSample\app\src\main\res\navigation\app_navigation.xml:25:9 (app_navigation.xml:25): 
error: android:defaultValue is @null, but ‘arg2’ is not nullable. Add app:nullable=”true” to the argument to make it nullable.
So added app:nullable=”true” at <argument> tag as follows

<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools" android:id="@+id/app_navigation"
            app:startDestination="@id/myHomeFragment">
    <fragment ...>
    <fragment android:id="@+id/mySecondFragment"
              android:name="com.navigation.component.sample.ui.fragments.MySecondFragment"
              android:label="fragment_my_second"
              tools:layout="@layout/fragment_my_second">
        <argument.../>
        <argument android:name="arg3"
                  app:argType="com.navigation.component.sample.data.MyParcelableDataArgs"
                  app:nullable="true"/>
        <argument.../>
        <action .../>

    </fragment>
    <fragment ...>
</navigation>
4. DeepLink failed

To work deep link we have to add navigation graph dependancy at Manifest.xml file under the Activity tag

