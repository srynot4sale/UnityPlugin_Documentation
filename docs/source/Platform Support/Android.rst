Android Support
===============

Building for Android
--------------------

Android is similar to desktop systems in that .hvr files are read from disk when being used to play a HvrActor. Previously, the Android HVR playback system expected that all static data was stored in a directory named “8i” in the device public storage. *This is no longer the case*. Files are now expected to be stored within the app's internal storage. The Unity Plugin provides support to export .hvr data using an OBB file, and then unpack this data into the correct location within the app's internal storage. 

**Split Application Binary**

The Google Play Store imposes a size limit of 100mb for APK files uploaded for distribution. To allow larger data to be shipped with an APK, Android supports OBB files, also known as Application Expansion Files (`Read More`__.). Unity also supports these files, via a player setting called "Split Application Binary". This option will export many of the project resources to an OBB, rather than packaging them with the APK. To export your .hvr files with your project, this option must be turned on. 

*** Packing ***
Each time you add or remove an HvrActor or new .hvr files, you should run the "8i/Android/Prepare Assets for Build" menu item. This will place all the .hvr data in the correct location to be exported with your build, and build a manifest file which describes which data should be unpacked. 

*** Unpacking ***
Your assets cannot be used for playback while packed in the OBB. They need to be unpacked and written to your app's internal storage. The Unity Plugin provides a helper scene which does this for you. This scene is called "HvrUnpackingScene", and is found in "8i/core/platform support/android/scenes". If you make this scene the first scene in your build settings (i.e. it will be launched when the app is launched), it will automatically unpack the assets, and then load the next scene. If you look at the "LoadingSceneManager" class used in this scene, you will see how we use the "AndroidAssetUnpacker" class to unpack assets. This is a very simple process, and if you are unhappy with the way our loading scene works, you should be easily able to implement your own asset loader using this class. 

*** Building ***
Once your assets are prepared, you should build via the standard Unity build process. There is no need to run the "Prepare Assets for Build" option unless you've added or removed any .hvr data to your project.

.. note::
	Some devices do not seem to correctly allow the OBB file to be copied to the device when using the "Build and Run" option in Unity, and in some cases will silently fail to update the OBB when you build. If this occurs, you will need to manually copy the OBB to your device. So far only the Samsung Galaxy Note 5 has been observed with this issue. 


Performance on Android
----------------------

Android performs much slower than desktop systems. It is recommended that hvr frames with point counts of 600k or less are used, with the recommended point count being around 300k.

It is recommended to use the ‘Direct’ HvrRender render method on Android as it is the best performing renderer.

The actor render mode should be set to "Point Sprite" in most cases. "Point Blend" does not work on some older devices, and is around twice as expensive to render. 

.. __: https://developer.android.com/google/play/expansion-files.html
