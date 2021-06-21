## Feature flags in GitLab

GitLab has built-in support for feature flags. Since I've become a big supporter of this approach, I was happy to see this. 

GitLab uses [Unleash](https://github.com/Unleash/unleash-client-dotnet) as a feature toggle service. It has a free, open-sourced client for many platforms and can be found on NuGet. However, it is worth noticing that its .Net implementation requires Newtonsoft dependency. So let's try to make use of it in a .Net Core API.

First, we need to configure it. You can find settings in the Operations -> Feature Flags sidebar menu. And then press the _configure_ button.
![feature-flags-configure.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624271118869/VGt318xkB.png)
We are interested in the API URL and Instance ID settings from the dialog that pops up.
Now it's time to create a new feature flag called _feature-is-here_ setup for all users and all environments to keep it simple.

![new-feature.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624271134906/mi44sO_SQ.png)

It's worth examining the documentation because there are valuable strategies around the [flags configuration](https://docs.gitlab.com/ee/operations/feature_flags.html#feature-flag-strategies).

Armed with these variables, we are ready to write some code.
```
public class FeatureFlagsReader {
    readonly DefaultUnleash _defaultUnleash;

    public FeatureFlagsReader() =>
        _defaultUnleash = new DefaultUnleash(new()
        {
              AppName = "whathever-app-it-is",
            InstanceTag = "{Instance ID}",
            UnleashApi = new("{API URL}")
        });

    public bool ReadFor(string flagName) => 
         _defaultUnleash.IsEnabled(flagName);
}

```
It makes sense to have only one instance of the feature flag client during the whole application lifetime. Therefore I register it as a singleton in default .Net Core DI container.
```
services.AddSingleton<FeatureFlagsReader>();
```
Now we can inject it into our controller and use it as we see fit:
```
    [ApiController]
    public class BeautifulController : ControllerBase {
        readonly FeatureFlagsReader _featureFlagsReader;

        public BeautifulController(FeatureFlagsReader featureFlagsReader) =>
            _featureFlagsReader = featureFlagsReader;

        [HttpGet("/featureFlag")]
        public string GetFeatureFlags() => 
            _featureFlagsReader.ReadFor("feature-is-here").ToString();
    }
```
The whole code is available on my [GitHub](https://github.com/jciechowski/gitlab-feature-flags).

I think you agree that it's straightforward. Yet, please be aware that Unleash cache the feature flags values, so it takes some time to reflect the changes.