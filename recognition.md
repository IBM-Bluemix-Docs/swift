---

copyright:
  years: 2018, 2021
lastupdated: "2021-01-07"

keywords: swift visual recognition, train swift, cocoapods swift, swift sdk install, starter kit watson swift, image swift classify, machine learning swift

subcollection: swift

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:external: target="_blank" .external}
{:deprecated: .deprecated}

# {{site.data.keyword.visualrecognitionshort}}
{: #recognition}

{{site.data.keyword.visualrecognitionfull}} is discontinued. Existing instances are supported until 1 December 2021, but as of 7 January 2021, you can't create instances. Any instance that exists on 1 December 2021 will be deleted. {: deprecated}

The {{site.data.keyword.visualrecognitionfull}} service enables your app to quickly and accurately tag, classify, and train visual content by using machine learning. The service can help to classify virtually any visual content, train your own custom model in minutes, and detect faces.

## How it works
{: #how-it-works-recognition}

1. Your app chooses a selection of images to analyze.
2. Your app sends the image to the {{site.data.keyword.visualrecognitionshort}} service by using the Watson Swift SDK.
3. The service analyzes the image by using classification analysis to identify scenes, objects, faces, and more.
4. The service's analysis is returned to your app by the Watson Swift SDK.

## Before you begin
{: #prereqs-recognition}

Ensure that you have the following prerequisites:

* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage, or Swift Package Manager

You can install the [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: external} by using [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: external}, [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: external}, or the [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: external}. By using CocoaPods(https://cocoapods.org/) to manage dependencies, you get only the frameworks that you need, as opposed to the entire Watson Swift SDK. If you are new to CocoaPods, you can install it easily:
```console
sudo gem install cocoapods
```
{: codeblock}

## Step 1. Creating an instance of Visual Recognition
{: #create-instance-recognition}

Provision an instance of the {{site.data.keyword.visualrecognitionshort}} service:

1. In the {{site.data.keyword.cloud_notm}} catalog, select **{{site.data.keyword.visualrecognitionshort}}**. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Select an app from the **Connect** menu if you'd like to bind your instance to an app.
4. Select a pricing plan, and click **Create**.
5. Select the **Credentials** tab to view your service credentials. These values are used to connect to the service from your app.

## Step 2. Downloading and building dependencies
{: #download-depend-recognition}

Using your favorite text editor, create a new `Podfile` in the root directory of your project (where your `.xcodeproj` file is located) by running `pod init`. Then, add a line to specify the {{site.data.keyword.visualrecognitionshort}} framework of the Watson Swift SDK as a dependency:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonVisualRecognitionV3'
```
{: codeblock}

For a production app, you might also want to specify a particular [version requirement](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: external} to avoid unexpected changes from new releases of the Watson Swift SDK.

With your `Podfile` in place, now you can download the dependencies. Use a terminal to navigate to the root directory of your project, then run CocoaPods:

```console
pod install
```
{: codeblock}

Cocoapods downloads the {{site.data.keyword.visualrecognitionshort}} framework, and builds it in the `Pods/` folder of your project.

To prevent Pod build failures, open the file that ends in `.xcworkspace` instead of the `.xcodeproj` when you open the project in Xcode.
{: tip}

## Step 3. Analyzing images in your app
{: #analyze-images-recognition}

The following samples help you add {{site.data.keyword.visualrecognitionshort}} capabilities to your application, typically in the `ViewController.swift`. Using the following examples, you can extend the Visual Recognition calls for your use case.

1. Add an import statement for Visual Recognition:
  ```swift
  import VisualRecognition
  ```
  {: codeblock}

2. Pass the API key and version (you can use today's date) to initialize the SDK:
  ```swift
  let visualRecognition = VisualRecognition(version: "yyyy-mm-dd", apiKey: "your-api-key")
  ```
  {: codeblock}

  You can check out [version parameter documentation](https://{DomainName}/apidocs/visual-recognition#versioning){: external} or use the date that the {site.data.keyword.conversationshort}} service was created.
  {: tip}

3. Add the following code to classify an image:
  ```swift
  let url = "your-image-url"
  visualRecognition.classify(url: url) { response, error in
      if let error = error {
          print(error)
      }
      guard let classifiedImages = response?.result else {
          print("Failed to classify the image")
          return
      }
      print(classifiedImages)
  }
  ```
  {: codeblock}

There are multiple classification methods that are supported by the Visual Recognition framework. Explore the Watson SDK [Visual Recognition documentation](https://watson-developer-cloud.github.io/swift-sdk/services/VisualRecognitionV3/index.html){: external} to find which one best suits your application.
{: tip}

## Using starter kits
{: #recognition_starterkits}

[Starter kits](https://{DomainName}/developer/appledevelopment/starter-kits){: external} are one of the fastest ways to use the capabilities of {{site.data.keyword.cloud_notm}}. You can use the {{site.data.keyword.visualrecognitionshort}} service by selecting the **Visual Recognition for iOS with Watson** starter kit. This service evaluates and classifies your images. Upload new or existing images from your mobile device, and the Visual Recognition app quickly tags and classifies the image content.

To get started:
1. Select the starter kit found [here](https://{DomainName}/developer/appledevelopment/starter-kits/visual-recognition-for-ios-with-watson){: external}.
2. Create the project with the default services.
3. Download the project by clicking **Download Code**. Service credentials are injected into the `BMSCredentials.plist` file in the corresponding key fields.

## Next steps
{: #recognition_next notoc}

Great job! Visual Recognition is now available to your app. Keep the momentum by trying one of the following options:
* Check out the [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: external} and explore the other supported Watson services.
* For more information, see [IBM Watson {{site.data.keyword.visualrecognitionshort}}](https://www.ibm.com/watson/services/visual-recognition/){: external}.
