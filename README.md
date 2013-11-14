# Salesforce Mobile SDK for iOS — Distribution

This repository contains the distribution (binary) packages for the Salesforce Mobile SDK for iOS.  Distributions will be tagged by SDK release version, so grab the binaries you want associated with the version you support.

- If you would like to work with the Mobile SDK and its sample apps directly, go to the [iOS source repo](https://github.com/forcedotcom/SalesforceMobileSDK-iOS).
- If you would like to create a new native or hybrid Mobile SDK app for iOS, take a look at our [forceios npm package](https://npmjs.org/package/forceios).

## Adding the Salesforce Mobile SDK To Your Existing Native App

If you would like to leverage the Salesforce Mobile SDK functionality in your existing native app, you can add our pre-built binary packages.  You can either download the binaries directly from GitHub, or sync this repository locally, checking out the tag associated with the version you want.  The following sections describe how to add Salesforce Mobile SDK libraries to your existing native app.

### Libraries and Resources

The following Mobile SDK libraries and resources are required for native apps.  For library archives, choose the -Debug.zip or -Release.zip file according to whether you want debug symbols included with the libraries.  The libraries in ThirdParty are already uncompressed, so just add those folders to your app directly.

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

In addition to adding binary dependencies, you need to implement a minimum amount of app configuration, before you can use any Mobile SDK options that require authentication:

- Include `SFAccountManager.h`
- Set your Connected App Consumer Key
- Set your Connected App's Callback URL
- Set the OAuth scopes that your Connected App will request

For example:

        [SFAccountManager setClientId:@"3MVG9Iu66FKeHhINkB1l7xt7kR8czFcCTUhgoA8Ol2Ltf1eYHOU4SqQRSEitYFDUpqRWcoQ2.dBv_a1Dyu5xa"];
        [SFAccountManager setRedirectUri:@"testsfdc:///mobilesdk/detect/oauth/done"];
        [SFAccountManager setScopes:[NSSet setWithObjects:@"api", nil]];

Finally, make sure to set the `-ObjC` and `-all_load` flags in the "Other Linker Flags" section of your app's Build Settings.
