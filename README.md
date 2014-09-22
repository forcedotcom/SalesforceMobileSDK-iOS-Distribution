# Salesforce Mobile SDK for iOS — Distribution

This repository contains the distribution (binary) packages for the Salesforce Mobile SDK for iOS.  Distributions will be tagged by SDK release version, so grab the binaries you want associated with the version you support.

- If you would like to work with the Mobile SDK and its sample apps directly, go to the [iOS source repo](https://github.com/forcedotcom/SalesforceMobileSDK-iOS).
- If you would like to create a new native or hybrid Mobile SDK app for iOS, take a look at our [forceios npm package](https://npmjs.org/package/forceios).

## Adding the Salesforce Mobile SDK To Your Existing Native App

If you would like to leverage the Salesforce Mobile SDK functionality in your existing native app, you can add our pre-built binary packages.  You can either download the binaries directly from GitHub, or sync this repository locally, checking out the tag associated with the version you want.  The following sections describe how to add Salesforce Mobile SDK libraries to your existing native app.

If syncing this repository locally, make sure to also sync the submodules using following commands:

    $ git submodule update --init

### Libraries and Resources

The following Mobile SDK libraries and resources are required for native apps.  For library archives, choose the -Debug.zip or -Release.zip file according to whether you want debug symbols included with the libraries.  The libraries in ThirdParty folder are already uncompressed, so just add those folders to your app directly.

- openssl (ThirdParty/openssl)
- sqlcipher (ThirdParty/sqlcipher)
- SalesforceCommonUtils (ThirdParty/SalesforceCommonUtils)
- SalesforceNativeSDK (SalesforceNativeSDK-[Debug/Release].zip)
- SalesforceNetworkSDK (SalesforceNetworkSDK-[Debug/Release].zip)
- MKNetworkKit (MKNetworkKit-iOS-[Debug/Release].zip)
- SalesforceOAuth (SalesforceOAuth-[Debug/Release].zip)
- SalesforceSDKCore (SalesforceSDKCore-[Debug/Release].zip)
- SalesforceSecurity (SalesforceSecurity-[Debug/Release].zip)

In addition, the following iOS dependencies are required. Go to the **"Build Phases"** tab of the project settings and add these dependencies under **"Link Binary with Libraries"** section.

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

Under the project build settings, add **"Header Search Paths"** for the SDK libraries (*SalesforceCommonUtils, SalesforceNativeSDK, SalesforceNetworkSDK, SalesforceOAuth, SalesforceSDKCore and SalesforceSecurity*) added earlier. Also make sure to set the `-ObjC` and `-all_load` flags in the "Other Linker Flags" section.


Now you are ready to use the Salesforce Mobile SDK in your existing app. To launch authentication flow, add the following code to your class:

- Import headers: `SFUserAccountManager.h`, `SFAuthenticationManager.h`

- Set your Connected App Consumer Key

    `[SFUserAccountManager sharedInstance].oauthClientId = @"3MVG9Iu66FKeHhINkB1l7xt7kR8czFcCTUhgoA8Ol2Ltf1eYHOU4SqQRSEitYFDUpqRWcoQ2.dBv_a1Dyu5xa";`

- Set your Connected App's Callback URL

    `[SFUserAccountManager sharedInstance].oauthCompletionUrl = @"testsfdc:///mobilesdk/detect/oauth/done";`

- Set the OAuth scopes that your Connected App will request

    `[SFUserAccountManager sharedInstance].scopes = [NSSet setWithObjects:@"web", @"api", nil];`

- Launch authentication

    ```
[[SFAuthenticationManager sharedManager]
    loginWithCompletion:(SFOAuthFlowSuccessCallbackBlock)^(SFOAuthInfo *info) {
        NSLog(@"Authentication Done");
    }
    failure:(SFOAuthFlowFailureCallbackBlock)^(SFOAuthInfo *info, NSError *error) {
        NSLog(@"Authentication Failed");
       [[SFAuthenticationManager sharedManager] logout];
    }
];
    ```