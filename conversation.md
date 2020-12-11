---

copyright:
  years: 2018, 2020
lastupdated: "2020-12-10"

keywords: chatbot swift, virtual assistant swift, assistant swift, watson swift starter, assistantv2 swift, watson sdk swift, add chatbot swift, add assistant swift

subcollection: swift

---

{:tip: .tip}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:external: target="_blank" .external}

# Adding a chatbot
{: #assistant}

You can use the {{site.data.keyword.conversationshort}} service to build applications that understand natural-language input and respond to users with human-like conversation.

The following list outlines the flow of how the integration works:

1. Users interact with the interface that is implemented in your app.
2. Your app sends user input to the {{site.data.keyword.conversationshort}} by using the {{site.data.keyword.watson}} Swift SDK.
3. The {{site.data.keyword.watson}} Swift SDK connects to a workspace, which is a container for your dialog flow and training data.
4. The workspace interprets the user input and directs the flow of the conversation, sending a response to your app.
5. Your app displays the response for the user.

## Before you begin
{: #prereqs-chatbot}

Ensure that you have the following prerequisites:

* An instance of the [{{site.data.keyword.conversationshort}} service](/docs/assistant?topic=assistant-getting-started#getting-started)
* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage, or Swift Package Manager

You can install the [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: external} by using [CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: external}, [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: external}, or the [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: external}. By using CocoaPods to manage dependencies, you get only the frameworks that you need, as opposed to the entire Watson Swift SDK. If you are new to CocoaPods, you can install it easily:

```console
sudo gem install cocoapods
```
{: codeblock}

## Step 1. Creating an instance of Watson Assistant
{: #instance-watson-chatbot}

Provision an instance of the {{site.data.keyword.conversationshort}} service:

1. In the {{site.data.keyword.cloud_notm}} catalog, select **{{site.data.keyword.conversationshort}}**. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Select an app from the **Connect** menu if you'd like to bind your instance to an app.
4. Select a pricing plan, and click **Create**.
5. Select the **Credentials** tab to view your service credentials. These values are used to connect to the service from your app.
6. Click **Launch tool** to build your chatbot assistant. Follow the instructions on the landing page to build your own chatbot, or go to the **Skills** tab and select **Create new**. On the **Add Dialog Skill** page, choose the **Use sample skill** tab and select the **Customer Care Sample Skill** to get started quickly. You can continue to refine this skill or create others to be used by your app later.
7. Click the menu on your new skill, and select **View API Details**. You can now see the `Workspace ID` that you need in addition to the service credentials.

## Step 2. Downloading and building dependencies
{: #download-depend-chatbot}

The following example uses AssistantV1. For more information about the AssistantV2 framework, see the [Watson SDK AssistantV2 documentation](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV2/index.html){: external}.

Using your favorite text editor, create a new `Podfile` in the root directory of your project (where your `.xcodeproj` file is located) by running `pod init`. Then, add a line to specify the {{site.data.keyword.conversationshort}} framework of the Watson Swift SDK as a dependency:

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonAssistantV1'
```
{: codeblock}

For a production app, you might also want to specify a particular [version requirement](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: external} to avoid unexpected changes from new releases of the Watson Swift SDK.

With your `Podfile` in place, now you can download the dependencies. Use a terminal to navigate to the root directory of your project, then run CocoaPods:

```console
pod install
```
{: codeblock}

Cocoapods downloads the {{site.data.keyword.conversationshort}} framework, and builds it in the `Pods/` folder of your project.

To prevent Pod build failures, open the file that ends in `.xcworkspace` instead of the `.xcodeproj` when you open the project in Xcode.
{: tip}

## Step 3. Adding a virtual assistant to your app
{: #virtual-assist-chatbot}

The following samples help you add {{site.data.keyword.conversationshort}} capabilities to your application, typically in the `ViewController.swift`. Using the following examples, you can extend the Assistant calls for your use case.

1. Add an import statement for {{site.data.keyword.conversationshort}}, for example `import Assistant`.
  ```swift
  import Assistant
  ```
  {: codeblock}

2. Instantiate the {{site.data.keyword.conversationshort}} service:
  ```swift
  let assistant = Assistant(version: "yyyy-mm-dd", apikey: "your-api-key-here")

  // save context to state to continue the conversation later
  var context: Context?
  ```
  {: codeblock}

  **Tip**: This example saves the context to state. For a better understanding of this object and how to adapt it for your use case, see the [context variable documentation](/docs/assistant?topic=assistant-dialog-runtime-context). Check out the [version parameter documentation](https://{DomainName}/apidocs/assistant#versioning){: external} or use the date that the {site.data.keyword.conversationshort}} service was created.

3. Initialize the conversation. Depending on how your assistant is configured, it can provide an initial response to the user:
  ```swift
  // Start a conversation
  assistant.message(workspaceID: "your-workspace-ID-here) { response, error in
      if let error = error {
         print(error)
      }

      guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Set the context to state
      self.context = message.context    
  }
  ```
  {: codeblock}

4. Send a message and the context to the assistant:
  ```swift
  print("Request: When are you open?")
  let input = InputData(text: "When are you open?")

  assistant.message(workspaceID: workspace, input: input, context: self.context) { response, error in
      if let error = error {
         print(error)
      }

      guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Update the context
      self.context = message.context
  }
  ```
  {: codeblock}

When you run your app with these examples with the default assistant, the following messages are displayed in the console:
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hello, I’m a demo customer care virtual assistant to show you the basics.  I can help with directions to my store, hours of operation and booking an in-store appointment.
Request: When are you open?
Response: Our hours are Monday to Friday 10am to 8pm and Friday and Saturday 11am to 6pm.
```
{: screen}

5. Explore the Watson SDK [Assistant documentation](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV1/index.html){: external} to build out your application's functionality.

## Using starter kits
{: #starterkits-chatbot}

With starter kits, you can quickly and easily use the capabilities of {{site.data.keyword.cloud_notm}}. You can add {{site.data.keyword.conversationshort}} to any server-side back end by using the starter kits. The Chatbot for iOS with Watson Starter Kit illustrates how to use the deep learning capabilities of {{site.data.keyword.conversationshort}} by adding a natural language interface to your application that automates interactions with your users.

1. Select the [starter kit](https://{DomainName}/developer/appledevelopment/starter-kits){: external} with which you want to work.
2. Create the app with the default services.
3. Click **Add service > Watson > {{site.data.keyword.conversationshort}}**.
4. Download the project by clicking **Download code**. You can find the service credentials in the `config/local-dev.json` file.

## Next steps
{: #next-chatbot notoc}

Great job! You added an AI assistant to your app. Keep the momentum by trying one of the following options:

* Check out the [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: external} and explore the other supported Watson services.
* Take advantage of all of the features that [{{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-index#index) offers.
* View the source code for the [Simple Chat sample app](https://github.com/watson-developer-cloud/simple-chat-swift){: external}, demonstrating the {{site.data.keyword.watson}} Swift SDK on GitHub.
