<img style="float: right;" width="120px" src="./Documentation~/images/MiniclipLogo.jpg"/>

# MUAds
## v7.2.0 - 2022-12-29
* Built and tested on Xcode 14.0 and 14.2.
* Built and tested on Unity 2021.3.

### Updated
* Ad Network SDKs have been updated - see [Tech Versions](Documentation~/tech_versions.md) for more info.
  * Current `FBLPromises.framework` version (dependency from AdMob) doesn't allow to be built with `Bitcode Enabled` on iOS.

### Added
* New menu in the `Configuration Helper` helps with the disabling of Bitcode builds on iOS.

## v7.1.0 - 2022-07-20
### Updated
* Ad Network SDKs have been updated - see [Tech Versions](Documentation~/tech_versions.md) for more info.
* Due to the Ad Network update, min Xcode version has risen to 13.2.1 - see [Migration](MIGRATION.md) for more info.

## v7.0.1 - 2022-05-30
### Added
* Impression Level Revenue API now also contains the `Precision` for the value given.

## v7.0.0 - 2022-05-11
### Added
* [BREAKING] AppLovin has been added as a supported network for both AdMob and IronSource mediations.
  * This will add a new parameter to the `MUAds Ad Network Selector`. A way to ensure that the configuration is correct is to check if the define `MU_APPLOVIN` is present (if the network should be enabled) as a build define.

## v6.0.3 - 2022-04-08
### Fixed
* The `AdNetworkManagerSettings.asset` should have a more predictable behaviour - keeping its stored settings between computers.

## v6.0.2 - 2022-03-28
### Updated
* Updated ironSource Android SDK to `7.2.1.1` to include more fixes to ANRs.
* Update Vungle Android SDK to `6.10.5` and mediation adapters to include a crash fix.

## v6.0.1 - 2022-03-21
### Updated
* Updated AdMob C# API - this was causing `OnRewardedVideoShown` event not to be called.

## v6.0.0 - 2022-03-11
### Removed
* [BREAKING] Removed the MoPub ad network. This is breaking because AdMob mediation initialization no longer requires the MoPub `app_key` to be supplied on setup.  
* [BREAKING] Removed `OnIronSourceInitializedForInterstitials` initialization callback for ironSource Mediation. IronSource now provides an agnostic callback that we can use.

### Updated
* Ad Network SDKs have been updated - see [Tech Versions](Documentation~/tech_versions.md) for more info.
* [BREAKING] Due to the Ad Network update, min iOS version has risen to 11.

### Added
* `ConfigurationHelper` editor menu now supports the feature to `Setup Folders on Demand`. This feature allows folder setup to be decoupled from build preprocessing (it is disabled by default).

### Fixed
* Minor bug fixes and optimizations.

## v5.0.0 - 2022-01-11
### Added
* [BREAKING] Impression level revenue is now accessible on all supported mediations via the `On<AdFormat>ImpressionRegistered` callback.

### Updated
* Ad Network SDKs have been updated - see [Tech Versions](Documentation~/tech_versions.md) for more info.
* This version is compliant with iOS 15 and Android 12.

### Changed
* On iOS, for AdMob Mediation, frameworks added via pods need to be statically linked - see [Getting Started](Documentation~/getting_started.md) for more info.
* Facebook Audience Network now requires XCode 13 for iOS builds.

## v4.0.2 - 2021-09-02
### Updated
* UnityAds SDK (on both formats) has been updated to 3.7.5 - including the Mediation Adapters.
* Facebook Audience Network iOS has been updated to 6.5.1 - including the Mediation Adapters.

### Added
* Facebook Audience Network iOS `setAdvertiserTrackingEnabled` is now supported.

### Fixed
* Minor fixes on AdMob Mediation C# API

## v4.0.1 - 2021-07-07

### Updated
* Bintray has been purged from the system.

## v4.0.0 - 2021-06-28
### Removed
* [BREAKING] `Configurator.SetAdMobDeviceForTesting` has been removed. Now MUAds automatically fetches and sets the test devices on `Debug` builds.
* [BREAKING] `(Obsolete) Configurator.GetUserConsentGDPR` has been removed - this API has been deprecated since `MUAds 3.0.0`.
* [BREAKING] `(Obsolete) Configurator.SetUserConsentGDPR` has been removed - this API has been deprecated since `MUAds 3.0.0`.

### Changed
* [BREAKING] `Configurator.setupAdMob` no longer requires an AdMob app key.

### Updated
* Full SDK update - see `tech_versions` for more info.

## v3.0.7 - 2021-04-19
### Fixed
* Make folder paths OS agnostic on Editor Menus.

### Changed
* Configuration Helper now allows games to include or not the Forced Linkage of the `UnityFramework.framework` on iOS.
* Use `dynamicUserId` instead of `userId` for IronSource Mediation - this allows user Id to be set at any point before showing an ad.

## v3.0.6 - 2021-03-2
### Fixed
* Fix bug when ironSource Rewarded Video Fails to show.
  
### Changed
* Use MUCore ApplicationEvents for ironSource event app event forwarding (including OnApplicationFocus).
* Force UnityFramework.framework on the Linked Binaries in iOS to prevent runtime Watchdog ANR.

## v3.0.5 - 2021-02-01
### Changed
* Fix link to the requirements file in the README.md.
* SKAdNetwork ids will now be automatically fetched on every iOS build.

## v3.0.4 - 2021-01-20
### Added
* Documentation for the minimum requirements for MUAds.

### Changed
* Updated the following iOS network SDKs: AdMob, Facebook, Fyber, Ironsource, UnityAds and Vungle.

## v3.0.3 - 2021-01-12
### Changed
* Update SKAdNetwork ids

## v3.0.2 - 2021-01-12
### Added
* M3 Support

### Removed
* MUCore plugin
* MUGoliath plugin

## v3.0.1 - 2020-11-06
### Changed
* Bug fix: include `.skadnetwork` in the end of every item on the SKAdNetworkItems List

## v3.0.0 - 2020-10-29
### Added
* Added Fyber network to Admob and IronSource mediations
* Added FAQ section to the documentation
* Added tool to manage the networks and mediations that will be compiled (Miniclip/MUAds/Ad Networks Selector)
* Added tool to facilitate the manipulation of AndroidManifest.xml and Info.plist (Miniclip/MUAds/Configuration Helper)
* Added support for the CCPA data protection policy

### Changed
* Updated all networks and adapters (including C# APIs) - see tech_versions for more information
* [BREAKING] iOS minimum deployment target is now 10.0.
* [BREAKING] Bump minimum Xcode version to Xcode 12.
* [BREAKING] MUAds interfaces have been renamed to match the C# Standard - IConfiguratorListener, IBannerListener, IInterstitialListener and IRewardedVideoListener
* [BREAKING] File structure has been changed - now everything MUAds related is under Assets/MUAds
* [BREAKING] The following methods now expect to receive a new parameter with the data protection policy for that user:

```csharp
public static bool SetupIronSource(string appkey,
                                   IDataProtection policy,
                                   string segmentName = null,
                                   Dictionary<string, string> customParams = null);

public static bool SetupAdMob(string appkey,
                              string mopubAdUnitId,
                              IDataProtection policy);
```

### Removed
* MyTarget network
* [BREAKING] MoPub Mediation
* [BREAKING] Offerwall API

### Deprecated
* e6bf7a7 - 2021/01/01 - **Configurator.GetUserConsentGDPR**
* e6bf7a7 - 2021/01/01 - **Configurator.SetUserConsentGDPR**

## v2.13.1 - 2020-04-01
### Removed
* Tapjoy Offerwall


## v2.13.0 - 2020-03-24
### Added
* AdMob Mediation Added
* AdMob Mediation Adapters - see tech_versions.md for information on the versions


## v2.12.0 - 2020-03-18
### Added
* Standard Ad Events - this requires MUGoliath 1.4.5 minimum to function

### Changed
* Placement callbacks have been refactored internally
* Use RequestBanner API from MoPub Unity SDK - instead of CreateBanner
* Increased Target Framework Version (C#) to 4.7.1 - from 3.5

### Removed
* Specific Placements per Ad Format (BannersPlacement, InterstitialsPlacement, RewardedVideosPlacement, OfferWallsPlacement)
* BannersPlacement.DestroyBanner() function


# v2.11.2
* Added MyTarget Network
* Added MoPub Adapters for MyTarget
* Added ironSource Adapters for MyTarget
* For more info see the Tech Versions documentation

# v2.11.1
* Tapjoy networks and respective mediation adapters have been updated per Tapjoy request.

# v2.11.0
* Removed AdMob Mediation
* Network SDK and Adapters have been updated - this version now targets AndroidX and iOS 13
* Added Tech Versions separated documentation
    * Please use this document to check the versions of the SDKs and Adapters

# v2.10.1
* Use placement reference logic to manage ironSource placement callbacks

# v2.10.0
* Update Vungle Android SDK to 6.4.11
    * Update Vungle Android Adapter for ironSource to 4.1.6
    * Update Vungle Android Adapter for MoPub to 6.4.11.4
* Update Vungle iOS SDK to 6.4.5
    * Update Vungle iOS Adapter for ironSource to 4.1.7
    * Update Vungle Android Adapter for MoPub to 6.4.5.0
* Remove Soom.la
* Remove AdTiming

# v2.9.0
* Banners introduced via MoPub Mediation

# v2.8.1
* Include MUCore.dll in the ExportPackage

# v2.8.0
* Rename most artifacts to MUAds (this includes the Plugin .dll)
* Add AdMob mediation for Interstitials and Rewarded Video
* Add dependency on MUCore 0.5.1
* Add AdTiming (Android v5.5.5 / iOS v3.6.3) to MoPub mediation
* Add PostProcess build script to enable Obj-C Modules on iOS build
* Update ironSource SDK (6.9.1 Android & 6.8.4.2 iOS)
* Update UnityAds SDK (3.1.0 Android & iOS)
* Update AdMob SDK (17.2.1 Android & 7.47.0 iOS)
* Add AdMob Adapters:
    * AdColony 1.0.5 - (3.3.10.1 Android & 3.3.7.2 iOS)
    * Facebook 2.2.0 - (5.3.0.0 Android & iOS)
    * ironSource 1.2.0 - (6.8.4.1 Android & 6.8.4.1.0 iOS)
    * MoPub 2.8.0 - (5.7.0.0 Android & 5.6.0.1 iOS)
    * TapJoy 2.2.0 - (12.2.1.0 Android & iOS)
    * UnityAds 2.1.0 - (3.1.0.0 Android & iOS)
    * Vungle 3.1.4 - (6.3.24.1 Android & 6.3.2.4 iOS)
* Update MoPub Adapters:
    * AdMob - (17.2.1.0 Android & 7.47.0.0 iOS)
    * ironSource - (6.9.1.0 Android & 6.8.4.1.0 iOS)
    * UnityAds - (3.1.0.0 Android & iOS)
* Update Soom.la:
    * Agent 4.14.0 (Android & iOS)
    * AdMob, Facebook, ironSource, UnityAds connectors updated


# v2.7.1
* Include IronSource Scripts in the MUAds package.

# v2.7.0
* IronSource Mediation for Interstitials and Rewarded Videos
* AdMob SDK (17.2.+ and 7.42.2) now requires parameters on AndroidManifest and Info.plist in order to work (Android and iOS respectively). Otherwise it will crash the game on startup.
* Update Soom.la Traceback Agent to 4.9.5
    * Update Soom.la's AdColony Connector to 5.1.2
    * Update Soom.la's AdMob Connector to 4.2.9
    * Update Soom.la's Facebook Connector to 4.1.4
    * Update Soom.la's ironSource Connector to 5.2.3
    * Update Soom.la's MoPub Connector to 4.1.2
    * Update Soom.la's Tapjoy Connector to 4.1.3
    * Update Soom.la's UnityAds Connector to 5.2.1
    * Update Soom.la's Vungle Connector to 5.1.2
* Update SDK's and MoPub Adapters
    * Update MoPub SDK to 5.6.0 (Android and iOS)
    * Update AdColony SDK to 3.3.10 (Android) and 3.3.7 (iOS)
    * Update AdMob SDK to 7.42.2 (iOS)
    * Update Facebook SDK to 5.3.0 (Android) and 5.2.0 (iOS)
    * Update ironSource SDK to 6.8.3 (Android) and 6.8.3.0 (iOS)
    * Update Tapjoy SDK to 12.2.1 (Android and iOS)
    * Update UnityAds SDK to 3.0.3 (Android and iOS)

    * Update MoPub's AdColony Adapter to 3.3.10.0 (Android) and 3.3.7.0 (iOS)
    * Update MoPub's AdMob Adapter to 7.42.2.0 (iOS)
    * Update MoPub's Facebook Adapter to 5.3.0.0 (Android) and 5.2.0.0 (iOS)
    * Update MoPub's ironSource Adapter to 6.8.3.0 (Android) and 6.8.3.0.0 (iOS)
    * Update MoPub's Tapjoy Adapter to 12.2.1.1 (Android) and 12.2.1.0 (iOS)
    * Update MoPub's UnityAds Adapter to 3.0.3.0 (Android and iOS)
    * Update MoPub's Vungle Adapter to 6.3.24.2 (Android) and 6.3.24.2 (iOS)
* Add IronSource Adapters
    * AdColony Adapter 4.1.6 (Android) and 4.1.4 (iOS)
    * AdMob Adapter 4.3.2 (Android) and 4.3.3 (iOS)
    * Facebook Adapter 4.3.2 (Android) and 4.3.3 (iOS)
    * Tapjoy Adapter 4.1.4 (Android) and 4.1.4 (iOS)
    * UnityAds Adapter 4.1.3 (Android) and 4.1.3 (iOS)
    * Vungle Adapter 4.1.8 (Android) and 4.1.7 (iOS)


# v2.6.2
* Downgrade AdMob SDK and MoPub Adapter to 17.0.0
* Use Tapjoy SDK for Android that does not collect Android ID (still 12.2.0)

# v2.6.1
* Fix MoPub initialization on SDK 5.5.0

# v2.6.0
* Update Soomla Traceback Agent to 4.6.1
 * Update Soomla Facebook Connector to 4.0.6
 * Update Soomla AdMob Connector to 4.1.4
 * Update Soomla MoPub Connector to 4.0.7
 * Update Soomla ironSource Connector to 5.1.2
 * Update Soomla AdColony Connector to 5.0.7
 * Update Soomla Tapjoy Connector to 4.0.3
 * Update Soomla UnityAds Connector to 5.1.4
 * Update Soomla Vungle Connector to 5.0.3
* Update Mopub Adapters and SDKs
 * Update Mopub SDK to 5.5.0 (Android and iOS)
 * Update AdColony SDK to 3.3.7 (Android)
 * Update Facebook Audience Network SDK to 5.1.0 (Android and iOS)
 * Update Google Ads (Admob) SDK to 17.1.2 and to 7.37.0 (iOS)
 * Update ironSource SDK to 6.8.1 (Android) and to 6.8.0.1(iOS)

# v2.5.0
* Update Soomla SDK to 4.2.2
 * Update Soomla AdColony Adapter to 5.0.1
 * Update Soomla Admob Adapter to 4.0.9
 * Update Soomla Facebook Audience Network Adapter to 4.0.3
 * Update Soomla IronSource Adapter to 5.0.3
 * Update Soomla Mopub Adapter to 4.0.3
 * Update Soomla Tapjoy Adapter to 4.0.2
 * Update Soomla Vungle Adapter to 5.1.1
 * Update Soomla UnityAds Adapter to 5.0.2
* Update Mopub Adapters
 * Update Mopub SDK to 5.4.1 (Android and iOS)
 * Update AdColony SDK to 3.3.5 (Android and iOS)
 * Update Facebook Audience Network SDK to 5.1.0 (Android and iOS)
 * Update Google Ads (AdMob) SDK to 17.0.0 (Android) and to 7.3.5 (iOS)
 * Update IronSource SDK to 6.7.12 (Android and iOS)
 * Update Tapjoy SDK to 12.2.0 (Android and iOS)
 * Update UnityAds SDK to 3.0.0 (Android and iOS)
 * Update Vungle SDK to 6.3.24 (Android) and to 6.3.2 (iOS)

# v2.4.0
* Include Soomla in order to track user revenue. 
* Documentation improvements.

# v2.3.5
* Fixed a NullReferenceException in the Configurator Class when using runtime version .NET 4.X.

# v2.3.4
* Removed unused MoPub SDK test assets.

# v2.3.3
* Removed AppLovin SDK and mediation adapters.

# v2.3.2
* Updated MoPub SDK to v5.2.0.
* Configured MoPub initialization to also initialize the Ad Networks SDK's. This initialization will only take effect after the first video request for each network. 

# v2.3.1
* Add support for Android builds using IL2CPP.

# v2.3.0
* Add ConfiguratorListener Interface. This will allow games to know when MoPub and Tapjoy finish their initialization.

# v2.2.0
* General Data Protection Regulation (GDPR) compliance.
Added a public API in order to set the user's consent. Be sure to call it before initializing the mediators. 

