---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: watson swift, tone anaylzer swift, cocoapods swift, swift sdk install, starter kit watson

subcollection: swift

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:external: target="_blank" .external}

# {{site.data.keyword.toneanalyzershort}}
{: #tone}

The {{site.data.keyword.ibmwatson}} {{site.data.keyword.toneanalyzershort}} service enables your app to understand emotions and tones in text. You can use the service to better understand your user's conversations or help users understand how their written communication is perceived.

## How it works
{: #how-it-works-tone}

1. Your app chooses a selection of text to analyze (for example, a text message or Twitter feed).
2. Your app sends the text to the {{site.data.keyword.toneanalyzershort}} service by using the Watson Swift SDK.
3. The service analyzes the text by using linguistic analysis to identify emotions and tones.
4. The service's analysis is returned to your app through the [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk).

## Before you begin
{: #prereqs-tone}

Ensure that you have the following prerequisites:

* OS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage, or Swift Package Manager

You can install the [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: external} by using [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: external}, [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: external}, or the [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: external}. By using CocoaPods to manage dependencies, you get only the frameworks that you need, as opposed to the entire Watson Swift SDK. If you are new to CocoaPods, you can install it easily:

```bash
$ sudo gem install cocoapods
```
{: codeblock}

## Step 1. Creating an instance of Tone Analyzer
{: #create-instance-tone}

Provision an instance of the {{site.data.keyword.toneanalyzershort}} service:

1. In the {{site.data.keyword.cloud_notm}} catalog, select {{site.data.keyword.toneanalyzershort}}. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Select an app from the Connect menu if you'd like to bind your instance to an app.
4. Select a pricing plan and click **Create**.
5. Select the **Credentials** tab to view your service credentials. These values are used to connect to the service from your app.

## Step 2. Downloading and building dependencies
{: #download-depend-tone}

Using your favorite text editor, create a new `Podfile` in the root directory of your project (where your `.xcodeproj` file is located) by running `pod init`. Then, add a line to specify the {{site.data.keyword.conversationshort}} framework of the Watson Swift SDK as a dependency:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonToneAnalyzerV3'
```
{: codeblock}

For a production app, you might also want to specify a particular [version requirement](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: external} to avoid unexpected changes from new releases of the Watson Swift SDK.

With your `Podfile` in place, now you can download the dependencies. Use a terminal to navigate to the root directory of your project, then run CocoaPods:

```console
pod install
```
{: codeblock}

Cocoapods downloads the {{site.data.keyword.toneanalyzershort}} framework, and builds it in the `Pods/` folder of your project.

To prevent Pod build failures, open the file that ends in `.xcworkspace` instead of the `.xcodeproj` when you open the project in Xcode.
{: tip}

## Step 3. Analyzing text in your app
{: #analyze-text-tone}

The following samples help you add {{site.data.keyword.toneanalyzershort}} capabilities to your application, typically in the `ViewController.swift`. Using the following examples, you can extend the Tone Analyzer calls for your use case.

1. Add an import statement for Tone Analyzer:
  ```swift
  import ToneAnalyzer
  ```
  {: codeblock}

2. Instantiate the Tone Analyzer service:
  ```swift
  let toneAnalyzer = ToneAnalyzer(version: "yyyy-mm-dd", apiKey: "your-api-key-here")
  ```
  {: codeblock}

  Check out the [version parameter documentation](https://{DomainName}/apidocs/tone-analyzer#versioning){: external} or use the date that the {site.data.keyword.conversationshort}} service was created.
  {: tip}

3. Provide text for analysis and process the results:
  ```swift
  // Text to analyze
  let review = """
      I was asked to sign a third party contract a week out from stay. If it wasn't an 8 person group that
      took a lot of wrangling I would have cancelled the booking straight away. Bathrooms - there are no
      stand alone bathrooms. Please consider this - you have to clear out the main bedroom to use that
      bathroom. Other option is you walk through a different bedroom to get to its en-suite. Signs all
      over the apartment - there are signs everywhere - some helpful - some telling you rules. Perhaps
      some people like this but It negatively affected our enjoyment of the accommodation. Stairs - lots
      of them - some had slightly bending wood which caused a minor injury.
  """

  // Analyze text
  let input = ToneInput(text: review)
  toneAnalyzer.tone(toneInput: .toneInput(input)) { response, error in
      if let error = error {
          print(error)
          return
      }

      guard let tones = response?.result?.documentTone.tones else {
          print("Failed to analyze the tone input")
          return
      }

      for tone in tones {
          print("\(tone.toneName): \(tone.score)")
      }
  }
  ```
  {: codeblock}

  When you run the code, you see the following analysis in the console:
  ```
  Sadness: 0.575803
  Tentative: 0.867377
  ```
  {: screen}

4. Explore the Watson SDK [Tone Analyzer documentation](https://watson-developer-cloud.github.io/swift-sdk/services/ToneAnalyzerV3/index.html){: external} to build out your application's functionality.

## Using starter kits
{: #tone_starterkits}

[Starter kits](https://{DomainName}/developer/appledevelopment/starter-kits){: external} are one of the fastest ways to use the capabilities of {{site.data.keyword.cloud_notm}}. You can use the {{site.data.keyword.toneanalyzershort}} service by selecting the **Tone Analyzer for iOS with Watson** starter kit. This service utilizes deep learning capabilities to evaluate passages of text. The Tone Analyzer application identifies the speaker's tone (happy, sad, confident, and more) as it relates to a number of categories.

To get started with this starter kit:

1. Select the starter kit found [here](https://{DomainName}/developer/appledevelopment/starter-kits/tone-analyzer-for-ios-with-watson){: external}.
2. Create the project with the default services.
3. Download the project by clicking **Download Code**. Service credentials are injected into the `BMSCredentials.plist` file in the corresponding key fields.

## Next steps
{: #tone_next notoc}

Great job! {{site.data.keyword.toneanalyzershort}} is now added to your app. Keep the momentum by trying one of the following options:

* View the [Watson Swift SDK on GitHub](https://github.com/watson-developer-cloud/swift-sdk){: external} and explore the other supported Watson services.
* For more information, see [IBM Watson {{site.data.keyword.toneanalyzershort}}](https://www.ibm.com/watson/services/tone-analyzer/){: external}.
