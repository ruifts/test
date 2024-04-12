![alt text][logo]

[logo]: images/MiniclipLogo.png "Miniclip Logo"
# MUAds

## Frequently Asked Questions - FAQ

&nbsp;

#### Where can I get my App Keys? Do they change with the platform?
A: App Keys are typically required for mediation initialization and they are dependent on both your game and the platform. This means that you will have one App Key for Android and one for iOS for any given game.  
Your Advertising representative will be able to provide you with the App Keys once he has configured your game in the mediation dashboard. We recommend that you use separate values for development and production builds.  

&nbsp;

#### What's the minimum iOS required? And mindSDK for Android?
A: The minimum iOS version is iOS 10 and the minSDK version for Android is 19.  

&nbsp;

#### I need to add a new entry to the SKAdNetworkItems in my Info.plist, how can I do it ?
A: Check out Apple's documentation on how to add SKAdNetworkIdentifiers [here](https://developer.apple.com/documentation/storekit/skadnetwork/configuring_the_participating_apps).

&nbsp;

#### What's the recommended way of updating MUAds?
A: We recommend that you remove all files that are from a previous version of MUAds in order to prevent conflicts. This is most important for Android libraries because it's usually a source of conflicts during compilation.

&nbsp;

#### A new permission was added to my game when submitting, what can I do about this? Is it mandatory?

A: There are only two permissions that are mandatory for MUAds, these are:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

If you see that there are permissions that were added after your MUAds integration, that do not correspond to the ones you have declared on your `AndroidManifest.xml`, feel free to remove them by adding these lines on your manifest (this is just an example permission):

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"
tools:node="remove" />
```

&nbsp;

#### My app keeps crashing on initialization after integrating MUAds, what happened?

A: As stated in the [Getting Started](getting_started.md) (point 4. for Android, point 1. for iOS) you need to add your AdMob appKey value to the respective configuration file for that platform: `AndroidManifest.xml` for Android and `Info.plist` for iOS.  
Typically an AdMob appKey looks like this:

```
ca-app-pub-3940256099942544~3347511713 
// This is just a sample, do not use for live games
```

If your key looks different (common error is using an adUnit instead of an appKey) then you will likely also get crashes at initialization.

&nbsp;

#### The initialization callback for the mediation `onIronSourceInitialized()` sometimes doesn't arrive on development. Is there a problem with the integration?

A: The ironSource initialization callback (`onIronSourceInitialized()`) will only arrive once the ironSource mediation has a Rewarded Video loaded.

In order to fix this issue for development, please provide the `IDFA` (iOS) or `Advertising ID` (Android) of your test device to the Advertising representative, so that it can be added to the list of "Test Devices" for ironSource. This should guarantee fill - and that the initialization callback arrives.  

For production this has never been a problem. The ironSource mediation always manages to get fill and finish the initialization.

*Note: For `MUAds` versions greater than `6.0.0` this is no longer the case. From that version forward, `MUAds` makes use of the IronSource given callback and forwards it. An effect of this, is that `MUAds` no longer guarantees an ad will be preloaded when the initialization callback arrives.*

&nbsp;

#### On the second video, my `preload()` call immediatly returned false and the Rewarded Video didn't finish loading in time. Is there any way around this?

A: The ironSource mediation works a bit differently from other mediations, in the sense that it does not give us the control of ad loading.  
For this mediation the `preload()` call will always return instantly because it's a query to a binary state: either the ad is loaded and ready to show, or it's not loaded yet - there is no `Loading` state.  
So a common approach to address this concern (and specifically for Rewarded Video placements that happen really close to each other, like Progressive Rewards) is to make the `preload()` call multiple times during the opportunity window (the time you have to try to load an ad) until it returns `true`. For example once every second, or once every two seconds.

&nbsp;

#### I have more than one Rewarded Video ad placement (e.g. Progressive Rewards + Chest speedup) how many `Mu.Ads.Placement` should I create?

A: If you are more concerned about managing the complexity of the code, we recommend only using one `Mu.Ads.Placement` for more than one Rewarded Video ad Placements. The upsides of this approach is that you only need to manage the lifecycle of one `Mu.Ads.Placement` - the downside is that an ad might not be loaded in time if the user watches different Rewarded Video ad placements in a short time frame (specially if using AdMob Mediation). If you are taking this approach, you should re-name the `Mu.Ads.Placement` according to the placement that you are about to show - using the `EventReportingName` API - this way you guarantee that it will appear correctly for data analysis.  

If you want to minimize loading times (this is more important if you are using AdMob Mediation) - you will want to take the approach of having multiple `Mu.Ads.Placements`. One for each Rewarded Video ad placement (or even more in the case of Progressive Rewards). The upside of this approach is that you can do load in parallel for these placements, so you can effectively pre-cache more than one ad. The downside, besides the added complexity, is that you likely will always be pre-caching more than one video and you don't know for sure if the user will interact with it.  

For Progressive Rewards, if you feel like the loading times aren't reasonable, you can consider having two `Mu.Ads.Placement` just for that Rewarded Video ad Placement (that consists of several ad views). If you rotate between then, you should always have one ready to serve an ad after the previous has finished.

&nbsp;

#### How do Rewards work? What's the difference between Client Side and Server Side Rewards?

A: A reward is the prize that the user gets from sucessfully watching a Rewarded Video. If a user closes the app or the video before it is finished, he won't have access to the Reward.  

The Reward attribution usually happens after the user has watched the video. This means that the Client will receive a callback from MUAds stating that the user has completed watching the video. This callback includes information about the amount and currency that is configured in the Reward. Usually this is enough to have a working solution and it's what most games opt to do - but you can add an extra step of security (or for more complex reward mechanisms) by using Server Side Rewards.  

It is possible to configure a Server URL for Ad Networks to communicate with at Reward Time. If this URL is configured, then the Ad Network will send callbacks to both the Client and the Server simultaneously. It is possible to configure what sort of information the Ad Network sends to the server (like userId or token) and this allows the server to do verifications about the Reward that it's about to give. Usually, when taking the Server Side Rewards route, the game should discard the Client Side Reward Callback and wait from confirmation from the Server in order to provide the Reward. While this operation takes a bit more time, it's more secure and allows for more complex reward attribution (rather than just currency).

Note: Each mediation has it's own protocol for communicating with the Server and validating Rewards, so if you do chose to engage with Server Side Rewards you will need to implement said protocol.

&nbsp;

#### My ad is taking a long time to load. What can I do to improve this?

A: On AdMob Mediation, the ad will only start loading once you make the `preload()` call. In order to maximize the time the ad has to load, we want to make this call as early as possible. This is called pre-caching.  
We recommend that the initialization of AdMob Mediation is done at app start (or as early as possible) and that once this is complete that you pre-cache your first ads (one for each ad format).  
For subsequent ads, we recommend that you preload them as soon as the previous one is dismissed (for example on the `OnAdDismissed()` callback).  
If you would like more information, we have a study about the benefits of pre-caching [here](extra/precaching.pdf).

&nbsp;

#### Can I change Network or Adapter versions?

A: You shouldn't unless if you have a good reason to do it: like crashes/ANRs or any incompatibility with the underlying system (like Unity version, XCode version, Android minSDK) - you can do it, but carefully.  
The versions of Networks and Adapters that you are currently integrated have been thoroughly tested by us and are likely already live in other games so we have a high degree of trust on them. Changing to a new version comes with a degree of uncertainty, but sometimes it's necessary.
The first thing you should do is notify us that you are having a specific issue with a Network SDK or Adapter so that we can find a solution.  
Changing versions is an easy task. On most cases it can be done simply by just changing the Editor script that controls that network. These Editor scripts are present on the path:  
`Assets/MUAds/Mediation/<Network Name>/SDK/Editor/<Network Name Dependencies>.xml`.

&nbsp;

#### What can I remove from the package?

A: The package contains three main areas. The C# Scripting folders, the Android folders and the iOS folders.  
The biggest contributors to game size are the network SDKs themselves - adapters and C# scripts have residual sizes in comparison. 
Before removing any network you should get in touch with your Advertising representative in order to make sure that it is the right call.
To remove networks go to `Miniclip/MUAds/Ad Networks Selector`. This will open a window where you will be able to enable and disable networks and mediations. Disabled networks and mediations will not be compiled.
Note that the tool will not allow you to remove networks that are mediating and will automatically enable networks that were selected to mediate. The network SDK is necessary to mediate other networks.
Finally, run the app in the editor to quickly check if nothing is broken. If you run into any issues please report to any advertising tech team member.

&nbsp;

#### When should I Force UnityFramework.framework Static Linkage?

A: Starting from Unity version 2019.3, on iOS, Unity tries to load the UnityFramework.framwork dynamically in runtime. While usually this is ok, we have seen Watchdog Crashes (8badfood) (similar to ANRs in Android) that are related to the loading of this framework in runtime. This typically happens when both the AdMob network and Firebase are present. If you find yourself in a situation like this, activating this feature will force the framework to be statically linked, avoiding the crash.







