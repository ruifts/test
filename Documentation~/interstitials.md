![alt text][logo]

[logo]: images/MiniclipLogo.png "Miniclip Logo"
# MUAds

## API Integration - Interstitials

### Step 1 - Ad placement creation

Create an IronSource interstitials placement

```c#
Placement placement =
        Interstitials.CreateIronSourcePlacement("<Placement Name>");
```

Or create an AdMob interstitials placement

```c#
Placement placement =
        Interstitials.CreateAdMobPlacement("<Ad Unit Id>");
```

### Step 2 - Loading and showing an ad

Showing an ad will require it to be loaded first.
You can query if a placement is ready to be shown or not, or just show it asynchronously if you don't want to worry about loading the placement.

1. Preload and then show.

        // Preloads the ad placement
        placement.Preload();
        
        // The status of the placement will only switch to Ready
        // once it receives the OnInterstitialLoaded(success = true) callback.

        // Shows an ad placement if it's ready
        if(placement.IsReady()) {
            placement.Show();
        }

2. Request an asynchronous show of the ad placement that will load it if necessary.<br>
When provided, the optional `showAvailabilityCallback` callback will be invoked before showing the ad.<br>
The callback must return `true` only if the application is still in a valid state to show the ad (for example, it should return `false` if the user is now playing a game and you don't want to interrupt it).

        // When the ad is ready, this callback will tell MUAds if we are
        // still in a state where the interstitial should be shown
        Func<bool> shouldShow = () => {
            return IsValidStateToShowAd();
        };
        
        // If the ad is ready to be shown, this will show it immediately;
        // if not, it will load it first
        placement.ShowAsync(shouldShow);
        
**Note:** The usage of `showAsync` should happen only in very specific situations - like when there is very little time between loading and showing an ad. As a default approach, we recommend the usage of the `preload` and `show` APIs because they work much better with the recommended pre-caching practices.  
You can check these practices [here](extra/precaching.pdf).

### Step 3 - Listening to events

A listener can be registered in order to receive interstitials events.
These can be used to know when an ad is loaded, shown, dismissed, etc.

```c#
class MyInterstitialsListener : IInterstitialsListener {
    public void OnInterstitialLoaded(Placement placement,
            string providerName, bool success) {
        if(success) {
            Debug.Log("Interstitial loaded.");
        } else {
            Debug.Log("Interstitial failed to load.");
        }
    }

    public void OnInterstitialShown(Placement placement,
            string providerName, bool success) {
        if(success) {
            Debug.Log("Interstitial shown.");
        } else {
            Debug.Log("Interstitial failed to show.");
        }
    }

    public void OnInterstitialDismissed(Placement placement,
            string providerName) {
        Debug.Log("Interstitial dismissed.");
    }

    public void OnInterstitialClicked(Placement placement,
            string providerName) {
        Debug.Log("Interstitial clicked.");
    }
    
    public void OnInterstitialImpressionRegistered(Placement placement, string providerName, ImpressionData impressionData) {
      Debug.Log("Interstitial Impression Tracked.");
    }
}
```

```c#
IInterstitialsListener interstitialsListener = new MyInterstitialsListener();
Interstitials.SetListener(interstitialsListener);
```
