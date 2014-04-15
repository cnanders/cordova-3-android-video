# cordova-3-android-video

A Cordova 3.x compatible version of https://github.com/macdonst/VideoPlayer.  This makes it possible to play HTML5 videos in a Cordova/PhoneGap Android application.  

### Installation
1. Download the repo
2. Create your cordova project if you haven't already

        $ cordova create helloworld
  
3. Navigate to helloworld/plugins
4. Side note: when you use the CLI to install apache-branded plugins, you usually do something like:

		$ cordova plugin add org.apache.cordova.device
		
however, I have not pushed this plugin to the apache community, so we need to install it "manually" (which is actually super simple, as you'll see below)

5. Create a directory named "com.github.cnanders.cordova-3-android-video"
6. Copy the contents of the repo to helloworld/plugins/com.github.cnanders.cordova-3-android-video.
7. At this point, the plugin is now "available" but it still won't be added to any platforms.  
8. If you have already added the Android platform, remove it 

        $ cordova platform remove android
        
9. Add the Android platform.  This will loop through all "available" plugins in helloworld/plugins and install them to the platform codebase

        $ cordova platform add android
        
10. Terminal will say "Installing com.github.cnanders.cordova-3-android-video (android)"
11.  Now if you go to helloworld/platforms/android/src/ you will see the cordova-3-android-video folder with VideoPlugin.java in it.  In addition, if you navigate to helloworld/platforms/android/assets/www you will see that videos.js is available within your js folder.  But wait... how did cordova know to do this?  If you've never actually looked at how Cordova installs plugins to each platform, it is worth a look since it is simple.  Navigate to helloworld/plugins/com.github.cnanders.cordova-3-android-video/plugin.xml.  This XML provides all of the instruction to cordova for how to copy the plugin assets (in this case the .java and .js files bundled with the plugin) into the platform codebase.  The XML should be self-explanitory, just thought I'd point you here since I found it illuminating. 
