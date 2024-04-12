![alt text][logo]

[logo]: images/MiniclipLogo.png "Miniclip Logo"
# MUAds

## API Integration - Event Report

### Introduction

In order to be able to track important KPIs related to revenue and user experience, **MUAds** is equipped with a feature called `Standard Ad Events`.  
This feature  makes use of two events: `interstitial_banner_funnel` and `video_ad_funnel`. For each format (Interstitial or Rewarded Video) MUAds will send these events at every meaningful step of an Ad lifecycle.  
  
**Note: Not all games are required to integrate the `Standard Ad Events` feature. If your game is not integrating it, feel free to ignore this document** . 

### Requirements

**MUAds** does not report metrics by itself. In order to report metrics you will need to integrate and initializate **MUGoliath** into your project and ensure the required events (`interstitial_banner_funnel` and `video_ad_funnel`) have been added to your game's Goliath Schema. Please follow the provided instructions to integrate **MUGoliath** before starting reporting events.


### Overview

**MUAds will automatically report a series of steps** without any action required from your part. These steps are:

*     Load Request
*     Load Success
*     Load Failure
*     Show
*     Show Failure
*     Click
*     Dismiss
*     Reward Trigger

**Certain steps MUAds can't automatically report as they are dependent on application logic**. They are:

##### Rewarded Video:

*     Button Press
*     Reward Attribution

##### Interstitials:

*     Ad Opportunity

**Button Press** is meant to be triggered when the user presses a button in order to watch a video.  
**Reward Attribution** is to be triggered when the user receives the reward in their account as compensation for watching a video.  
**Ad Opportunity** is meant to be triggered when the game (according to internal logic) decides that there is an opportunity to show an interstitial - even if the interstitial is not yet ready or fails for some reason.


### How to use
Event reporting is **disabled** by default. If you wish to report events and have MUGoliath integrated you can enable this feature using:

```c#
Configurator.SetEventReportingEnabled(true);
```

In order to report the app depending events you can use the methods provided in our `EventReport API`:

**Note:** It's important that you set the EventReportingName of the placement before the user interacts with the placement (i.e before the `press_button` event for Rewarded Video or the `show` event for Interstitials). This will guarantee that the corresponding Goliath event will have the parameter `placement_name` filled.

```c#
// Rewarded Video
Placement rewardedVideoPlacement;
(...)
rewardedVideoPlacement.EventReportingName = "<PLACEMENT_NAME>";

void OnRewardedVideoButtonPress()
{
   EventReport.RewardedVideosButtonPress(rewardedVideoPlacement); 
   (...) //Show the rewarded video
}

void RewardUser(string currency, int amount)
{  
   (...) //Reward the user for watching the video ad 
   EventReport.RewardedVideosRewardAttribution(rewardedVideoPlacement, currency, amount);
}
    
```

```c#
// Interstitial
Placement interstitialPlacement;
(...)
interstitialPlacement.EventReportingName = "<PLACEMENT_NAME>";

void ShouldShowInterstitialNow()
{  
	EventReport.AdOpportunity(interstitialPlacement);
   (...) // Show the interstitial if possible
}

```

### Extra debugging options

**MUAds** provides a way to log the contents of every Standard Ad Event step that is being set via **MUGoliath**.
In order to do so, you need to enable the `ExtraLoggingOptions` flag in the `EventReport API`:

```c#
    EventReport.ExtraLoggingOptions = <true/false>; // By default the value is false.
```

**Note:** This option is only available for `DEBUG` builds. 
