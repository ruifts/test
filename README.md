<img style="float: right;" width="120px" src="./Documentation~/images/MiniclipLogo.jpg"/>

# MUAds

MUAds provides support to display advertisements in Unity games. It enables game to better use third party mediations, providing access to Ad Lifetime Callbacks and metric collection (via Standard Ad Events) for multiple ad formats.

### Quick Links

* [Getting Started](Documentation~/getting_started.md)
    * [API Integration - Interstitials](Documentation~/interstitials.md)
    * [API Integration - Rewarded Videos](Documentation~/rewarded_videos.md)
    * [API Integration - Event Reporting (Standard Ad Events)](Documentation~/event_report.md)
* [FAQ](Documentation~/faq.md)
* [Technology Versions](Documentation~/tech_versions.md)
* [Migration Guide](MIGRATION.md)

### Dependencies

* MUCore - Required to run threads on the Unity main thread.
* [External Dependency Manager for Unity](https://github.com/googlesamples/unity-jar-resolver) - Required to resolve the Google Play Services dependencies.
* Third party Network SDKs - Both Android and iOS depend on several third party SDKs corresponding to the ad providers.

### Minimum Requirements

Please refer to the [requirements documentation](Documentation~/requirements.md) to check what are the minimum and recommended settings to integrate MUAds.

### How it works

MUAds eases the integration of several types of advertisements, simplifying and abstracting the API differences of each mediator and network.  
The third party Network SDKs are managed automatically via the External Dependency Manager for Unity.


### Changes to the MUAds code

Since the code is open source, it can be changed and reviewed - we believe this will help our partners with their integration, debugging and understanding of the technology.   
It is acceptable to change the code where fit, but Miniclip must be notified of it. Should Miniclip not agree with said code change, ultimately, it can stop providing support to the integration.
