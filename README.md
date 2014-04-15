# cordova-3-android-video

A Cordova 3.x compatible version of https://github.com/macdonst/VideoPlayer.  This makes it possible to play HTML5 videos in a Cordova/PhoneGap 3.x Android application.  It fixes the .api namespace issue with the Java includes and has Cordova 3.x style plugin installation instructions. (In 2.x Cordova, plugin installation was different, it is actually a lot easier in 3.x)  

### Installation
1. Download the repo
2. Create your cordova project if you haven't already

        $ cordova create helloworld
  
3. Navigate to helloworld/plugins
4. Side note: when you use the CLI to install apache-branded plugins, you usually do something like the code below, which creates a directory named "org.apache.cordova.device" inside of helloworld/plugins, however, I have not pushed this plugin to the apache community, so we need to install it "manually" as shown in steps 5+.
	
        $ cordova plugin add org.apache.cordova.device

5. Copy this entire repo into helloworld/plugins.  It will look like this: helloworld/plugins/cordova-3-android-video-master.
6. Rename this directory to:  helloworld/plugins/com.github.cnanders.cordova3video   (alternatively, you could just create a directory named "com.github.cnanders.cordova3video" inside of helloworld/plugins and copy the contents of this repo to it).  This is to create the reverse-domain namespasce traditionally used for plugins.  The name is important here.  If you use any other name, it won't work.
7. At this point, the plugin is now "available" but it still won't be added to any platforms.  
8. If you have already added the Android platform, remove it 

        $ cordova platform remove android
        
9. Add the Android platform.  This will loop through all "available" plugins in helloworld/plugins and install them to the platform codebase

        $ cordova platform add android
        
10. Terminal will say "Installing com.github.cnanders.cordova3video (android)"

### Description of the code the plugin creates in the platform codebase

Lots of cool stuff just happened. If you go to helloworld/platforms/android/src/ you will see the com/github/cnanders/cordova3video folder tree with VideoPlugin.java at the end.  In addition, if you navigate to helloworld/platforms/android/assets/www you will see that videos.js is available within your js folder. In addition, if you navigate to helloworld/platforms/android/res/xml/config.xml you will see that a "feature" node named "VideoPlayer" corresponding to this package has been automagically pushed to config.xml.   

Cordova does this when it installs the plugin to the platform.  But how did cordova know to do this?  If you've never actually looked at how Cordova installs plugins to each platform, it is worth a look since it is simple.  Navigate to helloworld/plugins/com.github.cnanders.cordova3video/plugin.xml.  This XML provides all of the instruction to cordova for how to copy the plugin assets (in this case the .java and .js files bundled with the plugin) into the platform codebase.  The XML should be self-explanitory, just thought I'd point you here since I found it illuminating. 

### Using the plugin

There is a great example here: http://stackoverflow.com/questions/18521807/playing-video-in-phonegap-cordova-app-for-ios-and-android which I have expanded to include an example of playing a local file.

		<body onload="javascript:init()">
		<div class="app">
			<p><a href="#" onclick="playVideo('https://www.youtube.com/watch?v=gYOLV66XukY')">Play File</a><p/>
		</div>
		<script type="text/javascript" src="cordova-2.3.0.js"></script>
		<script type="text/javascript" charset="utf-8" src="video.js"></script>
		<script type="text/javascript" src="js/index.js"></script>
		<script type="text/javascript">
			app.initialize();

			function init()
			{
				document.addEventListener("deviceready", console.log('ready'), true);
			}

			function playVideo(vidUrl) 
			{
				
				window.plugins.videoPlayer.play(vidUrl);
				
				// REMOTE (WEB) FILES
				//
				// Note that I passed in a YouTube video here, which will always work.  
				//
				// LOCAL FILES
				//
				// Lets say you have a video at helloworld/www/video/test.mp4.  When you
				// build your Android app, this video will be copied to 	
				// helloworld/platform/android/assets/www/video/.  In order to play it, 
				// you need to use the path to the file on the devide: 
				
				path = 'file:///android_asset/www/video/test.mp4';	
				window.plugins.videoPlayer.play(path);
				
				// I know it seems weird that you use "android_asset" instead of "asset"
				// but that is just the way it works as of Cordova 3.4
				
				// ONE MORE THING: Video encoding is important, and easy to get wrong.
				// To be on the safe side when you are initially testing, use a video 
				// that you take with the actual device you are testing on (email it to
				// yourself) that way you know this is not the reason the video isn't 
				// playing.				
				
			}
		</script>