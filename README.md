# Salesforce Mobile SDK for iOS — Distribution

This repository contains the distribution (binary) packages for the Salesforce Mobile SDK for iOS.  Distributions will be tagged by SDK release version, so grab the binaries you want associated with the version you support.

## Different SDK flavors
- If you would like to create a new native or hybrid Mobile SDK app for iOS, take a look at our [forceios npm package](https://npmjs.org/package/forceios). - Checkout ~5 minutes video here:
[![Alt text for your video](http://img.youtube.com/vi/zNw59KEUF24/0.jpg)](http://www.youtube.com/watch?v=zNw59KEUF24)



- If you have an existing app and/or want to manually add Salesforce Authentication, **you should use THIS repo**. Check out ~10 mins video here:
[![Alt text for your video](http://img.youtube.com/vi/X4jhhmnvjAI/0.jpg)](http://www.youtube.com/watch?v=X4jhhmnvjAI)

- If you want to configure Salesforce communities login page, check out this ~3 minutes video:
[![Alt text for your video](http://img.youtube.com/vi/USFPo2u7jpU/0.jpg)](http://www.youtube.com/watch?v=USFPo2u7jpU)

- If you would like to work with the Mobile SDK and its sample apps directly, go to the [iOS source repo](https://github.com/forcedotcom/SalesforceMobileSDK-iOS).



## Adding the Salesforce Mobile SDK To Your Existing Native App

If you would like to leverage the Salesforce Mobile SDK functionality in your existing native app, you can add our pre-built binary packages.  You can either download the binaries directly from GitHub, or sync this repository locally, checking out the tag associated with the version you want.  The following sections describe how to add Salesforce Mobile SDK libraries to your existing native app.

### Git clone (recursively)
This repo has `Thirdparty` sub module. So please clone this repo recursively.

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
Last but not least,  you should also add SDK's header files to search path so your XCode project can search for it.

1. In your XCode, click on your app name > Click on your app name Target
2. Click on `Build Settings` and search for `Header Search Paths`
3. Double click on that value and add path to SDK libraries. It will look something like: `$(SRCROOT)/Your__App__Name/SalesforceOAuth` for SalesforceOAuth SDK. You need this at the bare minimum.
4. Select `Recursive` from the menu next to the path. This will make XCode to recursively search for all the header files in sub folders.
