# Salesforce Mobile SDK for iOS — Distribution

This repository contains the distribution (binary) packages for the Salesforce Mobile SDK for iOS.  Distributions will be tagged by SDK release version, so grab the binaries you want associated with the version you support.

 You should use this repo if you have an existing app and/or want to manually add Salesforce Authentication to an iOS app. Check out a video demo here:
[![Alt text for your video](http://img.youtube.com/vi/X4jhhmnvjAI/0.jpg)](http://www.youtube.com/watch?v=X4jhhmnvjAI)

Be sure to check out other flavors of iOS SDK like using [forceios npm package](https://npmjs.org/package/forceios) at our source repo here [SalesforceMobileSDK-iOS source repo](https://github.com/forcedotcom/SalesforceMobileSDK-iOS). 





## Adding the Salesforce Mobile SDK To Your Existing Native App

If you would like to leverage the Salesforce Mobile SDK functionality in your existing native app, you can add our pre-built binary packages.  You can either download the binaries directly from GitHub, or sync this repository locally, checking out the tag associated with the version you want.  The following sections describe how to add Salesforce Mobile SDK libraries to your existing native app.

### Git clone (recursively)
Because this repo has `Thirdparty` sub module, be sure to clone this repo recursively.

` git clone https://github.com/forcedotcom/SalesforceMobileSDK-iOS-Distribution.git --recursive`

### Libraries and Resources

The following Mobile SDK libraries and resources are required for native apps.  For library archives, choose the `-Debug.zip` or `-Release.zip` file according to whether you want debug symbols included with the libraries.  The libraries in ThirdParty are already uncompressed, so just add those folders to your app directly.

- openssl (ThirdParty/openssl)
- sqlcipher (ThirdParty/sqlcipher)
- SalesforceCommonUtils (ThirdParty/SalesforceCommonUtils)
- SalesforceNativeSDK (SalesforceNativeSDK.zip)
- SalesforceNetworkSDK (SalesforceNetworkSDK.zip)
- MKNetworkKit (MKNetworkKit-iOS.zip)
- SalesforceOAuth (SalesforceOAuth.zip)
- SalesforceSDKCore (SalesforceSDKCore.zip)

In addition, the following iOS dependencies are required:

- libxml2.dylib
- libz.dylib
- UIKit.framework
- ImageIO.framework
- Foundation.framework
- CFNetwork.framework
- CoreData.framework
- CoreGraphics.framework
- MessageUI.framework
- MobileCoreServices.framework
- QuartzCore.framework
- Security.framework
- SystemConfiguration.framework

Add the following resource bundle:

- SalesforceSDKResources.bundle

### Configuration

#### Part 1

In addition to adding binary dependencies, you need to implement a minimum amount of app configuration, before you can use any Mobile SDK options that require authentication:

- Include `SFAccountManager.h`
- Set your Connected App Consumer Key
- Set your Connected App's Callback URL
- Set the OAuth scopes that your Connected App will request

For example:

        [SFAccountManager setClientId:@"3MVG9Iu66FKeHhINkB1l7xt7kR8czFcCTUhgoA8Ol2Ltf1eYHOU4SqQRSEitYFDUpqRWcoQ2.dBv_a1Dyu5xa"];
        [SFAccountManager setRedirectUri:@"testsfdc:///mobilesdk/detect/oauth/done"];
        [SFAccountManager setScopes:[NSSet setWithObjects:@"api", nil]];

Finally, make sure to set the `-ObjC` and `-all_load` flags in the `Other Linker Flags` section of your app's `Build Settings`.

#### Part 2
Last but not least,   add the Mobile SDK header file directories to your project's search path so Xcode can search for them.

1. In your Xcode, click on your app name > Click on your app name Target
2. Click on `Build Settings` and search for `Header Search Paths`
3. Double click on that value and add path to SDK libraries. It will look something like: `$(SRCROOT)/Your__App__Name/SalesforceOAuth` for SalesforceOAuth SDK. You need this at the bare minimum.
4. Select `Recursive` from the menu next to the path. This will make Xcode to recursively search for all the header files in sub folders.
5.  Repeat steps 3 and 4 for all other libraries including `Thirdparty` libraries.

#### Part 3 (Salesforce PIN Security)
If you want to enable [PIN security](https://developer.salesforce.com/docs/atlas.en-us.mobile_sdk.meta/mobile_sdk/connected_apps_security_pin.htm), make sure to also add the following:

1. Drag and drop `SalesforceSDKResources.bundle` file to your Xcode project.
2. Open `main.m` file and import `SFApplication.h` and pass the SFApplication's class name as the 3rd parameter to `UIApplicationMain` method.


It should look something like:

```

#import <UIKit/UIKit.h>
#import "AppDelegate.h"
#import "SFApplication.h" ## <-- Add this

int main(int argc, char *argv[])
{
    @autoreleasepool {
        return UIApplicationMain(argc, argv, NSStringFromClass([SFApplication class]), NSStringFromClass([AppDelegate class]));
    }
}
```

Note; If you are using [forceios npm package](https://npmjs.org/package/forceios) tool, it automatically setups up everything mentioned above for a sample app.
