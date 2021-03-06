---
template: page-sidebar
title: "Optimizely Android SDK FAQs"
---

# Optimizely Classic Android SDK FAQs

Below are some frequently asked questions about the Android SDK. You may also want to check out [general product FAQs](https://help.optimizely.com/hc/en-us/articles/201893400).

Can't find an answer to your question? [File a support ticket](http://optimizely.com/support).

<a href="#androidversion">*Q:* What version of Android is supported by the Android SDK?</a><br>
<a href="#androidlibs">*Q:* What third party libraries are used in the Android SDK?</a><br>
<a href="#androidsize">*Q:* What is the size of the Android SDK?</a><br>
<a href="#howandroidworks">*Q:* How does the SDK work?</a><br>
<a href="#negativesize">*Q:* Why are there negative values (-1, -2) for width and height in the Visual Editor?</a><br>
<a href="#matchwrap">*Q:* How do I use `MATCH_PARENT` or `WRAP_CONTENT` for width and height in the Visual Editor?</a><br>
<a href="#blocking">*Q:* Is `Optimizely#startOptimizelyWithAPIToken()` a blocking method?</a><br>
<a href="#startoptimizely">*Q:* Where and when can I call `Optimizely#startOptimizelyWithAPIToken()`?</a><br>
<a href="#codeblockmemory">*Q:* Should we unregister or nullify OptimizelyCodeBlock with anonymous handlers that have a reference to an Activity after finishing the `OptimizelyCodeBlock#execute()` method call?</a><br>
<a href="#threadsafety">*Q:* Are LiveVariable evaluations thread-safe?</a><br>
<a href="#proguard">*Q:* Do I need to include any ProGuard configuration rules to use the Optimizely SDK?</a><br>
<a href="#cantseeappineditor">*Q:* My device is running the app but I can't see it in the editor.</a><br>
<a href="#3rdparty">*Q:* Does Optimizely work with my other 3rd party SDKs?</a><br>
<a href="#resultspage">*Q:* I am not seeing conversions or visitors on the results page.</a><br>
<a href="#22andbelow">*Q:* I haven't gotten a chance yet to upgrade my app to API Level 23 and am using API Level 22 or below.  My app is not compiling after including Optimizely.</a><br>
<a href="#locked">*Q:* Why are my variables or code tests locked?</a><br>


<a name="androidversion"></a>
##### *Q: What version of Android is supported by the Android SDK?*
*A:* Optimizely currently supports apps that are built for Android AP 14 (Ice Cream Sandwich) and above.  For versions of Android AP 8-13, the app will still run but the SDK will be disabled.

<a name="androidlibs"></a>
##### *Q: What third party libraries are used in the Android SDK?*
*A: Libraries:*
- Google GSON v2.3.1
- Square OkHTTP v2.3.0
- Square OkIO v1.4.0
- Android Support Libraries v22.1.1
- Android Platform v22
- Murmur Hash v1
- Autobahn v0.5.3

<a name="androidsize"></a>
##### *Q: What is the size of the Android SDK?*
*A:* Approximate SDK size below:
<img src="/assets/img/android/Android_SDK_Size.png" alt="Drawing" style="width: 80%;"/>


<a name="howandroidworks"></a>
##### *Q: How does the SDK work?*
*A:* Optimizely is implemented through an SDK and the Optimizely datafile.  To get started with Optimizely and run your first experiments, you simply need to install the SDK and add one line of code to your app.  The SDK will download the datafile which is comprised of JSON and contains all of the experiment and goal information necessary to deploy and control experiments and return data to our reporting.  The data file is hosted on our CDN and follows the attached schemas for iOS and Android.  You may view your datafile at cdn.optimizely.com/json/android/1.0/&lt;project_id&gt;.json. Replace "1.0" in the example with the version of the SDK that you are using. For more details, you can refer to the following [article](https://help.optimizely.com/hc/en-us/articles/205014107-How-Optimizely-s-SDKs-Work-SDK-Order-of-execution-experiment-activation-and-goals).  The platform works as shown below:

*SDK Contents:*
- Data File Contents
- Compiled into the app
- Downloads Config from CDN
- Executes experiments and tracks goals locally
- Offline caching/network logic
- Reports events back
- Integrated with dependency management systems for easy updates

*Datafile contents:*
- Active Experiments
- Draft Experiments
- Project goals

The datafile follows this [schema](/android/schema).

<a name="negativesize"></a>
##### *Q: Why are there negative values (-1, -2) for width and height in the Visual Editor?*
*A:* Android uses `-1` for the constant `MATCH_PARENT` and `-2` for `WRAP_CONTENT`. To set the size of the view to an explicit pixel value, enter a positive number of pixels
in the width or height fields in the visual editor. We are working on an update to the editor that will better understand these special values.

<a name="matchwrap"></a>
##### *Q: How do I use `MATCH_PARENT` or `WRAP_CONTENT` for width and height in the Visual Editor?*
*A:* Android uses `-1` for the constant `MATCH_PARENT` and `-2` for `WRAP_CONTENT`. To set the width or height of the view to `WRAP_CONTENT`, enter `-2` in the corresponding field. Similarly, to set the width or height to `MATCH_PARENT`, ENTER `-1` in the corresponding field in the visual editor. We are working on an update to the editor that will better understand these special values.

<a name="blocking"></a>
##### *Q: Is `Optimizely#startOptimizelyWithAPIToken()` a blocking method?*
*A:* Yes, `Optimizely#startOptimizelyWithAPIToken()` is blocking. It will exit and return `true` if the configuration file was downloaded and parsed, or `false` if it hits the network timeout (or another error occurred). The default timeout is 2.5 seconds and is configurable through the `setNetworkTimeout()` API. Calls to `startOptimizely()` are not synchronized, and are not thread safe.

<a name="startoptimizely"></a>
##### *Q: Where and when can I call `Optimizely#startOptimizelyWithAPIToken()`?*
*A:* `Optimizely#startOptimizely()` must be called from the UI thread, but may be called as many times as you like. Multiple calls after the first activation call are no-ops. We recommend using `Optimizely#startOptimizelyWithAPIToken()` in the main launcher activity `onCreate()`. We do not currently support initializing in `Application#onCreate()`.

<a name="codeblockmemory"></a>
##### *Q: Should we unregister or nullify OptimizelyCodeBlock with anonymous handlers that have a reference to an Activity after finishing the `OptimizelyCodeBlock#execute()` method call?*
*A:* OptimizelyCodeBlocks are long-lived, so any references to Activities/Views/Applications should be weak references. This is a known issue that is being addressed.

<a name="threadsafety"></a>
##### *Q: Are LiveVariable evaluations thread-safe?*
*A:* In production mode, all Live Variable values are set when Optimizely starts up. Any further access to the LiveVariable is a read-only action and is thus thread safe.

<a name="proguard"></a>
##### *Q: Do I need to include any ProGuard configuration rules to use the Optimizely SDK?*
*A:* No. Proguard rules are bundled with our SDK as of version 1.2.2.

<a name="permissions"></a>
##### *Q: Does the Optimizely SDK require any permissions?*
*A:* The Optimizely SDK only requires the [INTERNET](http://developer.android.com/reference/android/Manifest.permission.html#INTERNET) permission.

<a name="cantseeappineditor"></a>
##### *Q: My device is running the app but I can't see it in the editor.*
*A:* First, confirm your device is connected to the internet and make sure that the API token in your call to `startOptimizely` matches what you see in the Project Code box within Optimizely. For more information, you can set `Optimizely.setVerboseLogging(true);` and look for error messages in Logcat.

<a name="3rdparty"></a>
##### *Q: Does Optimizely work with my other 3rd party SDKs?*
*A:* Optimizely works with many 3rd party SDKs. If we encounter specific 3rd party SDKs that cause conflicts with Optimizely we will list them here.

<a name="resultspage"></a>
##### *Q: I am not seeing conversions or visitors on the results page.*
*A:* There are two things to check if you not being counted as a visitor or seeing conversions on the results page as you're doing QA: that you meet targeting conditions and goals are being triggered properly.

To be counted as a visitor and show up on the results page, you actually have to see the change that you made to the app (i.e. the Live Variable code has to actually execute or you have to see the change you made via the visual editor).  Goals are only sent to the server every 2 minutes (the app must be open for at least 2 minutes), or you need to background the app in order for events store locally to be sent to the server.

To check that goals are triggering properly, you can use our developer APIs including: [OptimizelyEventListener](/android/help/reference/com/optimizely/integration/OptimizelyEventListener.html) and [getVisitedExperiments](/android/help/reference/com/optimizely/Optimizely.html#getVisitedExperiments(%29) to check that:

- Your experiment is running
- Your goals are triggered properly

<a name="22andbelow"></a>
##### *I haven't gotten a chance yet to upgrade my app to API Level 23 and am using API Level 22 or below.  My app is not compiling after including Optimizely.*

We have created a separate build that supports API level 8 - 22.  To use this version of Optimizely, you can add the following to your gradle file:

```groovy
compile ('com.optimizely:optimizely-legacy:+@aar') {
        transitive = true
}
```

<a name="locked"></a>
##### *Q: Why are my variables or code tests locked?*
*A:* When a Live Variable or Code Block is used, it becomes locked to make sure your users have a consistent experience. If activating an experiment fails because its Live Variables or Code Blocks are locked, you can unlock them with `Optimizely.refreshExperiments();` before activating the experiment.