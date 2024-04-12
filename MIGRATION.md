<img style="float: right;" width="120px" src="./Documentation~/images/MiniclipLogo.jpg"/>

# MUAds Migration guide
 
Whenever there's an interface breaking change (a change in the project's major version),
required migration instructions will be detailed in this file.

The format is based on Make a Migration guide and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## From [7.0.1 to 7.1.0]
- The minimum supported version of XCode is now v13.2.1. There are certain compilation issues with 3rd party SDKs on older versions which necessitate this restriction.

## From [6.x to 7.x]
- `AppLovin` is now a network supported by MUAds. To ensure proper setting up of the network, our recommendation is to "reset" (at least the `AppLovin` value) of the `MUAds AdNetworkSelector`.
  - To do so, you should change the status of the `AppLovin` network in the popup and close the popup to apply the changes at least once. After that you should change it back to the state you'd want it to be.
  - If you get a `Cocoapods` problem while building for iOS due to a missing pod (`GoogleMobileAdsMediationAppLovin`) consider updating your `cocoapods` installation to the latest version. 
    - This can be done by running the command: `sudo gem install cocoapods` in the terminal.

## From [5.x to 6.x]

- Important: Now the minimum iOS version is 11 - ensure this is set on iOS builds.
- The callback `OnIronSourceInitializedForInterstitials` has been removed.
- The call to `Configurator.SetupAdMob` has changed. It no longer requires the MoPub Ad Unit to be passed - just the Data Privacy consent.

## From [4.x to 5.x]

- The callback `OnInterstitialImpressionRegistered` has been added to the interface `IInterstitialsListener`.
- The callback `OnRewardedVideoImpressionRegistered` has been added to the interface `IRewardedVideosListener`.

## From [4.0.2] to [4.1.x]

- The minimum supported version of XCode is now 13 (Facebook Audience Network is introducing this restriction).
- New integration steps were added for games that are using AdMob Mediation on iOS - Frameworks need to be linked statically. Please see [Getting Started](Documentation~/getting_started.md) for more info.

## From [3.x.x] to [4.0.2]

- The GDPR API deprecated in [3.x.x] has been removed.
```c#
Configurator.GetUserConsentGDPR()
Configurator.SetUserConsentGDPR(<true/false>)
```

- The `Configurator` API to initialize AdMob mediation has been changed, and no longer requires the `appkey` to be provided.

- MUAds is now depending on `play-services-ads:20.0.0` (AdMob Android). This is important because you may get some conflicts when building your game due to duplicate classes. This happens because of `firebase-core`. In order to avoid this conflict, you should update `firebase-core` to at least `18.0.1`.

- For iOS, the recommended version for `Firebase` is `8.0.0` due to conflicts with `AdMob`.

- It's recommended to set the `External Dependencies Manager` iOS Settings to `Link frameworks statically`. Not having this setting is likely to make `Facebook Audience Network` fail to compile or crash in runtime (depending on XCode version).

 
## From [2.11.X] to [3.x.x]

### Delete

The recommended way to update MUAds is to completely remove the previous integration. This prevents conflicts during compilation and other editor problems. Cleaning up MUAds means deleting all the folders and files added with its integration. The files that need to be deleted are the following:

```
Assets/GoogleMobileAds*
Assets/IronSource*
Assets/MoPub*
Assets/MUAds*
Assets/Tapjoy*

Assets/Plugins/Android/GoogleMobileAds*
Assets/Plugins/Android/IronSource*
Assets/Plugins/Android/googlemobileads*
Assets/Plugins/Android/vungle*
Assets/Plugins/Android/mopub*
Assets/Plugins/Android/Tapjoy*
Assets/Plugins/Android/tapjoy*
Assets/Plugins/Android/externs.cpp*

Assets/Plugins/iOS/AdColony*
Assets/Plugins/iOS/AdMob*
Assets/Plugins/iOS/GADU*
Assets/Plugins/iOS/IronSource*
Assets/Plugins/iOS/MoPub*
Assets/Plugins/iOS/Vungle*
Assets/Plugins/iOS/Tapjoy*
```
The `*` means 0 or more characters after the previous character. All the files that match this pattern should be deleted.

To simplify this process we developed a script that will delete everything for you. This script is only guaranteed to remove everything that is necessary if the installed version of MUAds is 2.11.1 or greater. You can find the script in a file called `cleanup.sh` ([link](cleanup.sh)).

Copy the script to the root of your project, along side with the Assets folder (not inside it) and run it as follows:

```
sh cleanup.sh
```

Once finished, remove all the advertising related activities from `AndroidManifest.xml`.

### Insert

You will also need to add Java 8 compatibility. This can be done by adding the following snippet to your `mainTemplate.gradle`:  

```groovy
// You can add the complileOptions closure to an already existing android closure.

android {
    compileOptions {
        sourceCompatibility 1.8
        targetCompatibility 1.8
    }
}
```

### Update

All the interface names were change to accommodate the interface naming standards. Please replace of the occurrences of the old names with the new names as follows:

| Old Name               | New Name                |
|------------------------|-------------------------|
| ConfiguratorListener   | IConfiguratorListener   |
| BannersListener        | IBannersListener        |
| InterstitialsListener  | IInterstitialsListener  |
| RewardedVideosListener | IRewardedVideosListener |

The GDPR API was deprecated in favor to one that will support new data protection policies that may come.
Remove any usages of the follwing methods:

```c#
Configurator.GetUserConsentGDPR()
Configurator.SetUserConsentGDPR(<true/false>)
```

You will need to let MCAds know from which data protection policy the user is protected by. MCAds, right now, supports the following policies:

```c#
//User is not protected by any data protection policy
NoDataProtection();

//User is protected by GDPR
GDPRDataProtection(<true/false>);
//true means the user gave consent for targeted ads, false otherwise

//User is protected by CCPA
CCPADataProtection(<true/false>);
//true means the user allows selling their data, false otherwise
```

Select the one that represents the policy the user is protected by and add it to the current mediation setup you have, like so:

```c#
bool setupMoPubSuccess = Configurator.SetupMoPub("<Ad Unit ID>", new GDPRDataProtection(<true/false>));
bool setupIronSourceSuccess = Configurator.SetupIronSource("<AppKey>", new GDPRDataProtection(<true/false>));
bool setupAdMobSuccess = Configurator.SetupAdMob("<AdMob App Key>", "<MoPub Ad Unit Id>", new GDPRDataProtection(<true/false>));
```

You can now edit your AdMob configuration on the Unity editor top menu `Miniclip/MUAds/ConfigurationHelper`. We recommend that Delay App Measurement should be enabled.

If you want to remove networks or mediations from the build to reduce build size, we now have a tool for that in the Unity Editor top menu `Miniclip/MUAds/Ad Networks Selector`.