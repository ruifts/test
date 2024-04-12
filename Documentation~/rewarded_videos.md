![alt text][logo]

[logo]: images/MiniclipLogo.png "Miniclip Logo"
# MUAds

## API Integration - Rewarded Videos

### Step 1 - Ad placement creation

Create an IronSource rewarded videos placement.

```c#
// Creates an IronSource placement
Placement placement =
        RewardedVideos.CreateIronSourcePlacement("<Placement Name>");
```

Or create an AdMob rewarded videos placement.

```c#
// Creates an AdMob placement
Placement placement =
        RewardedVideos.CreateAdMobPlacement("<Ad Unit ID>");
```

### Step 2 - Loading and showing an ad

Showing an ad will require it to be loaded first.
You can query if a placement is ready to be shown or not, or just show it asynchronously if you don't want to worry about loading the placement.

1. Preload and then show.

        // Preloads the ad placement
        placement.Preload();
        
        // The status of the placement will only switch to Ready
        // once it receives the OnRewardedVideoLoaded(success = true) callback.
        
        // Shows an ad placement if it's ready
        if(placement.IsReady()) {
            placement.Show();
        }

2. Request an asynchronous show of the ad placement that will load it if necessary.<br>
When provided, the optional `showAvailabilityCallback` callback will be invoked before showing the ad.<br>
The callback must return `true` only if the application is still in a valid state to show the ad (for example, it should return `false` if the user is now playing a game and you don't want to interrupt it).

        // When the ad is ready, this callback will tell MUAds if we are
        // still in a state where the rewarded video should be shown
        Func<bool> shouldShow = () => {
            return IsValidStateToShowAd();
        };
        
        // If the ad is ready to be shown, this will show it immediately;
        // if not, it will load it first
        placement.ShowAsync(shouldShow);
        
     
**Note:** The usage of `showAsync` should happen only in very specific situations - like when there is very little time between loading and showing an ad. As a default approach, we recommend the usage of the `preload` and `show` APIs because they work much better with the recommended pre-caching practices.  
You can check these practices [here](extra/precaching.pdf).

### Step 3 - Listening to events

A listener can be registered in order to receive rewarded videos events.
These can be used to know when an ad is loaded, shown, dismissed, etc. Make sure to only reward the user in the **OnRewardedVideoRewarded** callback. If you are interested in understanding better how the Rewards work (Server side vs. Client side) please consult the [FAQ](faq.md) for more information.

```c#
class MyRewardedVideosListener : IRewardedVideosListener {

    public void OnRewardedVideoLoaded(Placement placement,
            string providerName, bool success) {
        if(success) {
            Debug.Log("Rewarded video loaded.");
        } else {
            Debug.Log("Rewarded video failed to load.");
        }
    }
    
    public void OnRewardedVideoShown(Placement placement,
            string providerName, bool success) {
        if(success) {
            Debug.Log("Rewarded video shown.");
        } else {
            Debug.Log("Rewarded video failed to show.");
        }
    }
    
    public void OnRewardedVideoDismissed(Placement placement,
            string providerName) {
        Debug.Log("Rewarded video dismissed.");
    }
    
    public void OnRewardedVideoClicked(Placement placement, string providerName) {
    	  Debug.Log("Rewarded video clicked.");
    }
    
    public void OnRewardedVideoRewarded(Placement placement,
            string providerName, string currency, int amount) {
        Debug.LogFormat("Rewarded video rewarded {0} {1}.", amount, currency); 
    }
    
    public void OnRewardedVideoImpressionRegistered(Placement placement, string providerName, ImpressionData impressionData) {
        Debug.Log("Rewarded video Impression Tracked.");
    }
}
```

```c#
IRewardedVideosListener rewardedVideosListener = new MyRewardedVideosListener();
RewardedVideos.SetListener(rewardedVideosListener);
```

### Step 4 [OPTIONAL] - Server-to-Server Reward Validation

MUAds currently does not supply out-of-the-box Server-to-Server Reward Validation to the Third Party Mediations (AdMob and IronSource).
If this is a requirement, you will need to integrate directly with the Third Party. By doing this integration, Client-Side Reward Events become redundant.

* [IronSource Server-to-Server Guide](https://developers.ironsrc.com/ironsource-mobile/air/server-to-server-callback-setting/#step-1)
* [AdMob Server-to-Server Guide](https://developers.google.com/admob/android/rewarded-video-ssv)

Besides implementing one of these protocols, it will also be likely that you need to add some client code, in order for your server to be able to identify to what placement each callback belongs to.
The way MUAds supports this is via the `CustomParams` API that is present in the `Placement` class:

```c#
placement.CustomParams = new Dictionary<string, string> {{ "<identifier_name>", "<shared_info_with_server>" }};
```

These `CustomParams` will be set and sent on the `Show` call, so you can changed them at any point until then.
The key `<identifier_name>` can be the name of the parameter that the server will receive on the callback (some mediations ignore the key, others use it as parameter name).

Notes:  
* IronSource's server typically uses the key value and prepends `custom_` so you will get something like `custom_<identifier_name>=<shared_info_with_server>` on the Server Reward Callback.
* AdMob currently only supports one custom value.
