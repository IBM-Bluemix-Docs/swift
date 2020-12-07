---

copyright:
  years: 2018, 2020
lastupdated: "2020-12-07"

keywords: getting started swift, custom app, create app swift, stater kit swift, apple app swift, swift dependency, ios development
content-type: tutorial
services: 
account-plan: lite
completion-time: 30m
subcollection: swift

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}
{:step: data-tutorial-type='step'}

# Getting started tutorial
{: #getting-started}
{: toc-content-type="tutorial"} 
{: toc-services=""} 
{: toc-completion-time="30m"}

The following tutorial shows you how to create a Swift mobile app by using a starter kit from the [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: external}. From the console, you add the Cloudant service, download the code, run the iOS app locally in Xcode, configure, and monitor the app.
{: shortdesc}

{{site.data.keyword.cloud}} offers solutions and services to enable Swift developers to build applications that are integrated with the security, AI, and value that your customers demand. With a broad portfolio of offerings and SDKs, you can use these services and bring cutting-edge applications to market quickly. This Swift programming explains how to add services to a new or existing Swift application, whether it's an iOS client or server-side Swift.

## Requirements for developers
{: #dev-requirements-swift}
{: step}

To get started with iOS development on {{site.data.keyword.cloud_notm}}, make sure that you meet the following requirements.

### Operating system
{: #swift-os-requirements}

The best practice for developing Swift apps is by using the latest macOS supported hardware, and testing with the latest iOS releases. Sign up for an [Apple Developer](https://developer.apple.com/){: external} account to enable testing on a physical device. 

### iOS and Xcode
{: #ios_and_xcode}

- Install [Xcode 8+](https://developer.apple.com/xcode/){: external} (or higher).
- Deploy to [iOS 8 devices](https://support.apple.com/downloads/ios){: external} (or higher).
- Review the [App Store Submission Guidelines](https://developer.apple.com/app-store/resources/){: external} before you submit apps to Apple.

### SDKs and dependency management
{: #swift-sdk-management}

The following tools ensure that you can install the native SDKs to work with the various {{site.data.keyword.cloud_notm}} Services. You can use CocoaPods or Carthage for dependency management.

* **Using CocoaPods** - Install [CocoaPods](https://cocoapods.org/) to support {{site.data.keyword.cloud_notm}} SDKs:
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}

* **Using Carthage** - Some SDKs are also available through the [Carthage](https://github.com/Carthage/Carthage){: external} or [Swift Package Manager](https://swift.org/package-manager/){: external} dependency managers. To use Carthage for dependency management, do the following steps:

  Install [Homebrew](https://brew.sh/){: external} to assist Carthage installation:
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

  Install Carthage:
  ```
  brew install carthage
  ```
  {: codeblock}

## Create a custom iOS Swift app
{: #create-ios-app-swift}
{: step}

1. Log in to your {{site.data.keyword.cloud_notm}} account. If you don't have an account, you can [register for a free account](https://{DomainName}/registration){: external}. For more information, see [Setting up your IBM Cloud account](/docs/account?topic=account-account-getting-started).
2. Log in to the [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: external}.
3. Click **Create App**, and then select the **Create** tab.
4. On the Create App page, you can use the default configuration, or update the fields as needed. Ensure that **iOS Swift** is the selected language. Click **Create**.

## Add the {{site.data.keyword.cloudant_short_notm}} service
{: #resources-swift}
{: step}

You can now add services to your Swift application. For this tutorial, add the {{site.data.keyword.cloudant_short_notm}} service to your Swift app, which creates a fully managed, distributed `JSON` document database. Cloudant powers your app with scalability, high availability, and durability in a lightweight framework that keeps your data safe and in sync.

1. On the App details page, click **Create service**.
2. Select **Databases**, and click **Next**.
3. Select **Cloudant**, and click **Next**.
4. Select the Lite pricing plan, and click **Create**.
5. In the **Services** card, click the Actions icon ![Actions icon](../icons/actions-icon-vertical.svg), and select **Open dashboard**.
6. Select **Launch Cloudant Dashboard** to begin creating a database and JSON documents.

## Download the code and setting up client SDKs
{: #run-locally-swift}
{: step}

To download the code, click **Download code** on the App details page. The downloaded code includes the [SwiftCloudant SDK](https://github.com/cloudant/swift-cloudant){: external}, as well as some basic initialization code. The client SDKs are available on CocoaPods and Swift Package Manager. This solution uses CocoaPods.

1. Extract the downloaded code. Then, using a terminal, navigate to the extracted folder.
  ```
  cd <Name of Project>
  ```

2. The folder includes a podfile with the required dependencies. Run the following command to install the dependencies (Client SDKs):
  ```
  pod install
  ```
  {: codeblock}

## Configure the app to use your new database
{: #config-db-swift}
{: step}

1. Open the file name that ends in `.xcworkspace` with Xcode, and navigate to `ViewController.swift`. `SwiftCloudant` is the core Cloudant SDK. `CouchDBClient` is a class of `SwiftCloudant` and initialized in `ViewController.swift`.

  Always open the new Xcode workspace, instead of the original Xcode project file: `MyApp.xcworkspace`.
  {: tip}

2. The database initialization code is already included as shown in the following snippet:
  ```swift
  /* Read the Cloudant credentials and intialize database connection */
  if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"), let dictionary = NSDictionary(contentsOfFile: contents) {
            let url = URL(string: dictionary["cloudantUrl"] as! String)
            let client = CouchDBClient(url:url!,
                                       username:dictionary["cloudantUsername"] as? String,
                                       password:dictionary["cloudantPassword"] as? String)
        }
  ```
  {: codeblock}

  The service credentials are part of the `BMSCredentials.plist` file.
  {: note}

## Build out your database operations
{: #build_ops-swift}
{: step}

Now that you have a working database connection and SDK set up, you can begin building out the basic [create, read, update, and destroy operations](/docs/swift/data?topic=swift-cloudant) for your particular use case.

## Next steps
{: #next-swift}

### Add more services
{: #moreresources-swift}

You can add more services to your iOS app directly from the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}){: external}, such as the following commonly used services:

* [Adding the Push Notifications service](/docs/mobilepush?topic=mobilepush-gettingstartedtemplate)
* [Adding user authentication with App ID](/docs/services/appid?topic=appid-getting-started)

### {{site.data.keyword.cloud_notm}} developer tools
{: #devtools-swift}

You can also learn how to develop Swift apps by using the [{{site.data.keyword.cloud_notm}} Developer Tools commands](/docs/cli?topic=cli-getting-started), which offer a command-line approach to creating, developing, and deploying complete web, mobile, and microservice applications.
