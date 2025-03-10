---
title: Prepare your iOS app for native authentication
description: Learn how to prepare your iOS app for native authentication using Microsoft Entra External ID for customers.

author: henrymbuguakiarie
manager: mwongerapk

ms.author: henrymbugua
ms.service: entra-external-id

ms.subservice: customers
ms.topic: tutorial
ms.date: 02/23/2024
ms.custom: developer, devx-track-dotnet
#Customer intent: As a dev, devops, I want to learn about how to configure prepare your iOS app for native authentication using Microsoft Entra External ID for customers.
---

# Tutorial: Prepare your iOS app for native authentication

This tutorial demonstrates how to add Microsoft Authentication Library (MSAL) native authentication SDK framework to your iOS Swift app.

In this tutorial, you learn how to:

- Add the MSAL framework to an iOS app.
- Create SDK instance.

## Prerequisites

- <a href="https://developer.apple.com/xcode/resources/" target="_blank">Xcode</a>
- [Sign in users in sample iOS (Swift) mobile app by using native authentication](how-to-run-native-authentication-sample-ios-app.md).
- iOS project

## Add the MSAL framework to an iOS app

1. Open your iOS project in Xcode.
1. Select **Add Package Dependencies...** from the **File** menu.
1. Enter `https://github.com/AzureAD/microsoft-authentication-library-for-objc` as the Package URL and choose **Add Package**

For more information and other mechanisms to add MSAL to your project, see the [project Readme file](https://github.com/AzureAD/microsoft-authentication-library-for-objc?tab=readme-ov-file#installation).

## Create SDK instance

1. Import the MSAL library into your view controller by adding `import MSAL` at the top of your `ViewController` class.
1. Add a `nativeAuth` member variable to your `ViewController` class by adding the following code just before the `viewDidLoad()` function:

   ```swift
   var nativeAuth: MSALNativeAuthPublicClientApplication!
   ```

1. Next, add the following code to the `viewDidLoad()` function:

   ```swift
    do {
       nativeAuth = try MSALNativeAuthPublicClientApplication(
           clientId: "Enter_the_Application_Id_Here",
           tenantSubdomain: "Enter_the_Tenant_Subdomain_Here",
           challengeTypes: [.OOB]
       )
    
       print("Initialized Native Auth successfully.")
    } catch {
       print("Unable to initialize MSAL \(error)")
    }
   ```

1. Replace the following values with the values from the Microsoft Entra admin center:
   1. Find the `Enter_the_Application_Id_Here` value and replace it with the **Application (client) ID** of the app you registered earlier.
   1. Find the `Enter_the_Tenant_Subdomain_Here` and replace it with the Directory (tenant) subdomain. For example, if your tenant primary domain is `contoso.onmicrosoft.com`, use `contoso`. If you don't have your Directory (tenant) subdomain, learn how to [read your tenant details](how-to-create-customer-tenant-portal.md#get-the-customer-tenant-details).
1. To build, select the **Product** > **Build** in your project’s toolbar.

### Optional: Logging configuration

MSAL provides a logging API that you can use to enable and configure logging. To see all debug output from MSAL add the following code at the start of the `viewDidLoad()` function:

```swift
MSALGlobalConfig.loggerConfig.logLevel = .verbose
MSALGlobalConfig.loggerConfig.setLogCallback { logLevel, message, containsPII in
   if !containsPII {
      print("MSAL: \(message ?? "")")
   }
}
```

This outputs all debug logs from MSAL, which can be helpful in diagnosing issues and learning how the native authentication flows work. To learn more about configuring log levels and best practices see [Logging in MSAL for iOS/macOS](../../identity-platform/msal-logging-ios.md?tabs=swift).

## Next steps

- [Tutorial: Add sign up with email one-time passcode](tutorial-native-authentication-ios-sign-up.md)

