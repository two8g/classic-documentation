---
template: page-sidebar
title: "Optimizely Classic iOS SDK Guide"
---
# Getting started with Optimizely's Classic iOS SDK

<div class="attention attention--warning push--bottom">
Optimizely Classic will sunset on September 30, 2018. Please [transition to Optimizely X Full Stack](https://help.optimizely.com/Set_Up_Optimizely/Make_the_transition_to_Optimizely_X_Full_Stack). Have questions? Our support team is here to help.
</div>

The following SDK Install Steps will allow you to install the SDK and run experiments with Optimizely's Visual Editor.

## SDK Download

You can download the iOS SDK through Cocoapods, download it manually from GitHub, or install it through Fabric:

[![SDK Version](http://img.shields.io/cocoapods/v/Optimizely-iOS-SDK.svg?style=flat)](/ios/getting-started/index.html#using-cocoapods)

<a href="https://fabric.io/kits/ios/optimizely"><img src="/assets/img/mobile/fabric_button.png" style="width: 150px; height: 58px;"/></a>

[ZIP](http://github.com/optimizely/Optimizely-iOS-SDK/zipball/master) | [TAR](http://github.com/optimizely/Optimizely-iOS-SDK/tarball/master) | [GitHub](http://github.com/optimizely/Optimizely-iOS-SDK)

##### *iOS SDK Install Video*
<iframe src="//fast.wistia.net/embed/iframe/5apoabzheh" allowtransparency="true" frameborder="0" scrolling="no" class="wistia_embed" name="wistia_embed" allowfullscreen mozallowfullscreen webkitallowfullscreen oallowfullscreen msallowfullscreen width="720" height="450"></iframe>
<script src="//fast.wistia.net/assets/external/E-v1.js" async></script>

<h2 id="create-an-ios-project">1. Create an iOS Project</h2>

To create an iOS project, select "Create New Project" in the [Optimizely Dashboard](https://www.optimizely.com/dashboard):

   <img src="/assets/img/ios/create-project.png" alt="Drawing" style="width: 80%;"/>

<a name="project-code"></a>Once you've created a project, please take a look at the `Settings` tab to find your project ID and API key which you will use during installation:

<img src="/assets/img/ios/project-code.png" alt="Project Code Dialog">

<h2 id="sdk-integration">2. SDK Integration</h2>

To use Optimizely's iOS SDK you must first integrate the SDK into your app. You can either install the Optimizely SDK using [CocoaPods](#using-cocoapods) (recommended) or via [Manual Installation](#manual-installation).  Our SDK supports iOS 7.0 and above.

### Using CocoaPods

0. Your Xcode project must be set up for CocoaPods. Refer to [CocoaPods Getting Started](http://cocoapods.org/#getstarted) if you haven't yet configured your project to work with CocoaPods.

1. Our SDK only supports iOS 7.0 and above, so please make sure your `Podfile` specifies a [deployment target](http://guides.cocoapods.org/syntax/podfile.html#platform) of iOS 7.0 (or above). Then, add Optimizely to your `Podfile`:

	```ruby
	platform :ios, '7.0'
	# Other Pods
	pod 'Optimizely-iOS-SDK'
	```


2. Run `pod install` from the command line.  This will add and install the Optimizely iOS SDK in your generated CocoaPods workspace.

 *Note: By default CocoaPods installs to the first build target in the project.*

### Manual Installation

For new installations, please follow all steps. For upgrades, please follow steps 1 and 2.

0. Clone the Optimizely SDK using `git clone https://github.com/optimizely/Optimizely-iOS-SDK`

1. Drag `Optimizely.framework` from the SDK repository into your project. Check "Copy items into destination group's folder" and make sure the appropriate targets are checked.

2. Open the "Build Phases" tab for the app's target. Under "Link Binary with Libraries," add the required frameworks if they're not already included:
   * AudioToolbox.framework
   * CFNetwork.framework
   * Foundation.framework
   * libicucore.tbd (libicucore.dylib for XCode 7.0 and below)
   * libsqlite3.dylib
   * MobileCoreServices.framework
   * Security.framework
   * SystemConfiguration.framework
   * UIKit.framework
<br  />
3. <a name="objc"></a>Switch to the "Build Settings" tab. Add `-ObjC` to the "Other Linker Flags" build setting.

<h2 id="add-your-api-token">3. Add Your API token</h2>

### Using Objective-C

1. Now, you're ready to write some code!  Include this file at the top of your `AppDelegate` class implementation. This is usually found in a file called `AppDelegate.m` in the Project Navigator.

	```obj-c
	#import <Optimizely/Optimizely.h>
	```

2. Add the following to the beginning of `application:didFinishLaunchingWithOptions:` in your
app delegate. The code can be copied from your `Project Code`, which you can find by selecting the appropriate iOS Project in your [Optimizely Dashboard](https://www.optimizely.com/dashboard).  For more details, you can refer back to [Step 1: Create an iOS project](#accountcreation).

    ```obj-c
    // You can find the following code snippet in your project code.

    [Optimizely startOptimizelyWithAPIToken:YOUR_API_TOKEN launchOptions:launchOptions];

    // The rest of your initialization code...
  	```
*Note: We recommend putting this code at the beginning of your `application:didFinishLaunchingWithOptions:` function.*

3. <a name="urlscheme"></a> In order to enter Edit Mode (which will allow your app to be detected by Optimizely's Editor), you'll have to define a URL scheme for Optimizely.

   0. Add `[Optimizely handleOpenURL:]` to `application:openURL` in your app delegate.  This will notify Optimizely when the application has been loaded from a URL:

         ```obj-c
         - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation{
            if ([Optimizely handleOpenURL:url]) {
               return YES;
            }
            return NO;
         }
         ```

   1. <a name="urlschemeInfo"></a> In the project editor, click on "Targets" -> Your app name -> "Info" tab.
   2. Locate the section called "URL Types" and click the plus icon (+) to expand the section.
   3. Paste the following into the field called "Identifer":

         ```obj-c
         com.optimizely
         ```

   4. Add `optly{PROJECT_ID}` to "URL Schemes."  Your Project ID is available at the bottom of the <a href="#project-code">Project Code</a> dialog box. For instance, if your Project ID is `123456`, your URL Scheme would be `optly123456`.
      Once completed, your `URL Scheme` should look like this:
      <img src="/assets/img/ios/project-plist.png" alt="Drawing" style="width: 100%; padding-bottom:10px;"/>

   5. <a name="urllinkgenerate"></a>Once you run your app in DEBUG mode with the SDK installed, you should see the image below in your [Optimizely Dashboard](https://www.optimizely.com/dashboard).  Once the SDK is detected, the Create Experiment button will appear, and you can continue to Step 4 to create your experiment.

        <img src="/assets/img/ios/sdk-detected.png" alt="Drawing" style="width: 80%;"/>

### Using Swift

1. Now, you're ready to write some code!  Include this file at the top of your `AppDelegate` class implementation. This is usually found in a file called `AppDelegate.swift` in the Project Navigator.

	```swift
	import Optimizely
	```

2. Add the following to the beginning of `application(_:didFinishLaunchingWithOptions:)` in your
app delegate. The code can be copied from your `Project Code`, which you can find by selecting the appropriate iOS Project in your [Optimizely Dashboard](https://www.optimizely.com/dashboard).  For more details, you can refer back to [Step 1: Create an iOS project](#accountcreation).

	```swift
	// You can find the following code snippet in your project code.

	Optimizely.startOptimizelyWithAPIToken(YOUR_API_TOKEN, launchOptions:launchOptions);

	// The rest of your initialization code...
	```		
*Note: We recommend putting this code at the beginning of your `application(_:didFinishLaunchingWithOptions:)` function.*

3. <a name="urlscheme"></a> In order to enter Edit Mode (which will allow your app to be detected by Optimizely's Editor), you'll have to define a URL scheme for Optimizely.

   0. Add `Optimizely.handleOpenURL(_:)` to `application(_:openURL:)` in your app delegate.  This will notify Optimizely when the application has been loaded from a URL:

         ```swift
		func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject) → Bool {
			if Optimizely.handleOpenURL(url) {
				return true
			}
			return false
		}
         ```

   1. <a name="urlschemeInfo"></a> In the project editor, click on "Targets" -> Your app name -> "Info" tab.
   2. Locate the section called "URL Types" and click the plus icon (+) to expand the section.
   3. Paste the following into the field called "Identifer":

         ```swift
         com.optimizely
         ```

   4. Add `optly{PROJECT_ID}` to "URL Schemes."  Your Project ID is available at the bottom of the <a href="#project-code">Project Code</a> dialog box. For instance, if your Project ID is `123456`, your URL Scheme would be `optly123456`.
      Once completed, your `URL Scheme` should look like this:
      <img src="/assets/img/ios/project-plist.png" alt="Drawing" style="width: 100%; padding-bottom:10px;"/>

   5. <a name="urllinkgenerate"></a>Once you run your app in DEBUG mode with the SDK installed, you should see the image below in your [Optimizely Dashboard](https://www.optimizely.com/dashboard).  Once the SDK is detected, the Create Experiment button will appear, and you can continue to Step 4 to create your experiment.

        <img src="/assets/img/ios/sdk-detected.png" alt="Drawing" style="width: 80%;"/>


<h2 id="create-an-experiment">4. Create an Experiment</h2>
After creating an iOS project and installing the SDK, reference [this guide in our Knowledge Base](https://help.optimizely.com/hc/en-us/articles/202296994), which will walk you through how to set up an experiment.

<h2 id="qa">5. QA</h2>

### Preview Mode
Preview mode allows you to view your app in a different variations for a given experiment in order to check that your app and the experiment are both running smoothly. To enter preview mode, connect your device to the editor, open the `Preview` menu, and click `Launch Preview`

<img src="/assets/img/mobile/launch-preview.png" style="width: 60%;" alt="Enter Preview Mode" />

Your app will restart and you will see the Optimizely preview menu icon displayed over your app content.
 The icon may be repositioned by dragging it. Tapping the icon will reveal the Preview Menu which allows you to switch variations, view the goals that have been triggered so far, and see the code blocks and live variables that are included in the experiment.

<img src="/assets/img/ios/preview-menu.gif" style="width: 40%;" alt="Preview Mode Demo" />

Now that you've created an experiment and successfully installed the Optimizely iOS SDK, below is a checklist to go through prior to releasing your app to the app store with the SDK:

1. In order to set up your app such that you can QA experiments (beyond using Preview), we recommend either having a separate [Project](#accountcreation) for development and production or inserting [Custom Tags](#customtags), that are only set for certain QA devices.  If you decide to go with setting up 2 separate projects, we recommend setting up an `#ifdef` to ensure that only one project code snippet is defined at any given time.

2. Were you able to connect to Optimizely's Visual Editor?  If you ran into issues, you can try out this [troubleshooting tip](../faqs/index.html#visualeditorchange).

3. [Configure your app](#urlscheme) so that non-technical members of your team can set up and run experiments using the visual editor on a physical device without the need of XCode.  You can try downloading the development build on your device and try opening the [link](#urllinkgenerate) to make sure that this is set up properly.

4. (Optional) If you have a separate project for development and production, you can run your experiments in your development environment to check that results are updating and that you are seeing the different variations.
    - A useful debugging tool is to enable logging (be sure to disable this feature when your app is live in the app store) `[Optimizely sharedInstance].verboseLogging = YES`  For each event that is triggered, you will see a log statement.  Be sure to check that verboseLogging is *not* enabled in production.
    - You will want to make sure that each experiment does not make changes to the same element (otherwise only one of the experiments will run).
    - Optimizely tracks unique visitors, so that we make sure that the same user sees the same experience.  If you would like to check that you are getting a random experience, you will need to delete the app to be counted as a new visitor.
    - By default, Optimizely sends network calls every 2 minutes or upon backgrounding. (You can find more details about modifying the SDK network settings [here](#networksettings)). In order to check that your event data is being updated in Optimizely's dashboard as expected, you can either:
       1. Trigger events in the app and keep the app foregrounded for 2 minutes
       2. Background the app so that events are sent to our servers.
<br  />
5. Once you've checked all these steps, you're ready to release to the app store!  To learn more about how to use Optimizely's editor and get additional testing ideas, you can check out our articles in [Optiverse](https://help.optimizely.com/hc/en-us/sections/200666084-Mobile-Optimization).

### Programmatically Enable Preview Mode

While preview mode can be enabled from the dashboard it can also be enabled from code.  This allows you to preview variations across all of your experiments without needing to connect to the editor.  Preview mode has UI that allows you easily switch variations and view event logs.

```obj-c
[Optimizely enablePreview];
[Optimizely startOptimizelyWithAPIToken:YOUR_API_TOKEN launchOptions:launchOptions];
```


## Advanced Setup

Once you have run your first few visual editor experiments or tried out Optimizely's SDK, you may find you would like to include programmatic experiments, additional tracking calls, or analytics integrations.  For advanced setup, below are a subset of advanced features we recommend utilizing prior to releasing to the App Store:

1. [Live Variables](../reference/index.html#register-live-variables)
2. [Code Blocks](../reference/index.html#code-blocks)
3. [Custom Tags](../reference/index.html#custom-tags)
4. [Track Event](../reference/index.html#track-event) (for key metrics you would like to track in your app)
5. [Revenue Tracking](../reference/index.html#revenue-tracking)
6. [Analytics Integration](../reference/index.html#analytics-integrations)

For a comprehensive list of all additional methods available in the SDK you can refer to the [Reference](../reference/index.html) section or the [Apple Docs](/ios/help/html/index.html).
