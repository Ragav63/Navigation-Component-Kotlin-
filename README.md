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


plugins {
    alias(libs.plugins.androidApplication)
    alias(libs.plugins.jetbrainsKotlinAndroid)
    id("androidx.navigation.safeargs.kotlin")
}

android {
    namespace = "com.example.navigationcomponentkotlin"
    compileSdk = 34

    defaultConfig {
        applicationId = "com.example.navigationcomponentkotlin"
        minSdk = 24
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"

        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            isMinifyEnabled = false
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = "1.8"
    }
    buildFeatures{
        viewBinding = true
    }
}

dependencies {

    implementation(libs.androidx.core.ktx)
    implementation(libs.androidx.appcompat)
    implementation(libs.material)
    implementation(libs.androidx.activity)
    implementation(libs.androidx.constraintlayout)
    testImplementation(libs.junit)
    androidTestImplementation(libs.androidx.junit)
    androidTestImplementation(libs.androidx.espresso.core)
    val navVersion = "2.7.3"
    implementation("androidx.navigation:navigation-fragment-ktx:$navVersion")
    implementation("androidx.navigation:navigation-ui-ktx:$navVersion")
}


// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    repositories {
        google()
    }
    dependencies {
        val navVersion = "2.7.3"
        classpath("androidx.navigation:navigation-safe-args-gradle-plugin:$navVersion")
    }
}

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
2. Create a navigation resource file “nav_graph.xml”, the name should follow the resource file name rules.


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

<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/welcomeFragment">

    <fragment
        android:id="@+id/welcomeFragment"
        android:name="com.example.navigationcomponentkotlin.WelcomeFragment"
        android:label="fragment_welcome"
        tools:layout="@layout/fragment_welcome" >
        <action
            android:id="@+id/action_welcomeFragment_to_loginFragment"
            app:destination="@id/loginFragment" />
        <action
            android:id="@+id/action_welcomeFragment_to_signUpFragment"
            app:destination="@id/signUpFragment" />
    </fragment>
    <fragment
        android:id="@+id/loginFragment"
        android:name="com.example.navigationcomponentkotlin.LoginFragment"
        android:label="fragment_login"
        tools:layout="@layout/fragment_login" />
    <fragment
        android:id="@+id/signUpFragment"
        android:name="com.example.navigationcomponentkotlin.SignUpFragment"
        android:label="fragment_sign_up"
        tools:layout="@layout/fragment_sign_up" />
</navigation>
There 3 parameters used

android:id: unique id for the fragment, just like we are assigning id to any other widgets in xml layout
android:name: It is the fully qualified name of your fragment class in kotlin/java
android:label: A string to identify the fragment
tools:layout: An id of layout resource file from res/layout
To define the start view we use startDestination tag in the root tag of navigation and specify the fragment id
package com.example.navigationcomponentkotlin

import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.navigation.findNavController
import com.example.navigationcomponentkotlin.databinding.FragmentWelcomeBinding


class WelcomeFragment : Fragment() {

    private lateinit var binding: FragmentWelcomeBinding

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        binding = FragmentWelcomeBinding.inflate(inflater, container, false)

        binding.btnLogin.setOnClickListener {
            it.findNavController().navigate(R.id.action_welcomeFragment_to_loginFragment)
        }

        binding.btnSignup.setOnClickListener {
            it.findNavController().navigate(R.id.action_welcomeFragment_to_signUpFragment)
        }


        return binding.root
    }


}
4. DeepLink failed

To work deep link we have to add navigation graph dependancy at Manifest.xml file under the Activity tag

