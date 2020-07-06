---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-17"

keywords: swift app service, swift versioning, audience swift, messages swift, in-app swift, engagement swift, create feature swift, feature control swift, swift feature release

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:gif: data-image-type='gif'}

# Managing app feature releases
{: #mobile_applaunch}

{{site.data.keyword.engage_full}} lets developers build engaging apps by controlling reach, and roll out of app features that can provide measurable metrics. The service helps developers to remove the coupling that exists today between app feature rollout, and app updates to production. You can now publish features without exposing it to production to gradually release new versions of an app in a controlled manner. With the {{site.data.keyword.engage_short}} service, app owners have full control over feature rollout for a targeted segment.

The {{site.data.keyword.engage_short}} service defines a feature, creates an audience based on device platforms (including custom audience attributes), and finally defines an engagement that choreographs the timing and placement of the feature. After SDKs are used, along with the feature and metrics attributes that are incorporated in the application, the service starts measuring audience experiences. You can now use your app based on this information to create customized customer engagements across various categories of your app users.

![Cognitive Engage overview](images/process_app_launch.png "Cognitive Engage overview"){: caption="Figure 1. Overview of the {{site.data.keyword.engage_short}} service lifecycle caption-side="bottom"}

See the following features of the {{site.data.keyword.engage_short}} service:

- Accelerate feature deployment

  Accelerate delivery of features to your app through controlled release by mitigating risks. Release features to subset of audience segments and make larger rollout or roll back decisions based on real-time feedback. Separate feature rollouts from regular release cycles.

- Segment audience

  User segments can be defined based on demographic, contextual, and behavioral attributes. You can also roll out features to certain percentage of the entire user base. Key Performance indicators can be defined for each feature and client-side code to measure the results.

- Adapt application based on context

  Application behavior, user interface, and notifications can be customized for specific audience segments. For example, app background can be changed based on user location. This user personalization results in higher engagement of users with the application.

- A / B test features

  Gain confidence by experimentation. Two variants of application features can be compared against each other by rolling out both at the same time. Decisions can be made based on hard data.

- Increase Customer Engagement**

  Promote user engagement. Notifications can be targeted to all application users or to a specific set of users and devices. A message schedule can be defined. User interaction plays a vital role in customer relationships.

## Before you begin
{: #prereqs-applaunch}

First, ensure that you have the following prerequisites ready to go:

 - iOS 10+
 - Xcode 9
 - Swift 3.2 - 4
 - CocoaPods or Carthage

## Step 1. Creating an instance of {{site.data.keyword.engage_short}}
{: ##app_launch_create}

1. In the {{site.data.keyword.cloud_notm}} catalog, click **Mobile** > **App Launch**. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Click **Create**.
4. In the navigation pane, click **Connections** to select an app and bind it to your service. You can bind the service instance to your app later if you leave it unbound during creation.

## Step 2. Initializing your app
{: #initialize-applaunch}

The service provides platform-specific SDKs to simplify application development. The {{site.data.keyword.cloud_notm}} Mobile Services Swift SDKs can be installed with either CocoaPods or Carthage.

1. Click **Settings**.
2. Install the [SDK](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-applaunch){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon"). For more information, see the `README` file that includes installation steps and technical concepts.
3. Copy the configuration keys to initialize your app. Use the app secret, app GUID, and client secret to configure your app and create engagements.

## Step 3. Creating a feature
{: #create-feature-applaunch}

The {{site.data.keyword.engage_short}} service creates and tests responses to features.

![Feature Details](images/feature_creation_animated.gif){: gif}

To create a feature, complete the following steps: 
1. In the navigation pane, click **Features** > **Create New Feature**.
2. Update the Create New Feature and Metrics form with a feature name and description. You can also define the feature properties and add metrics to measure the impact of your engagement. Click **Bulk edit** to add multiple properties by editing the JSON.
3. Click **Create**. The new feature now appears on the Features panel.
4. Enable the feature after it's developed.
5. To enable a feature to be used as an engagement, click the Feature that you created.
6. In the Feature Details window, choose to Update Status of your feature to **Ready**.
7. Click **Update Status**.
8. Update your app to include the newly created attributes and feature codes in your iOS app.
9. The feature is now ready to be used.

The Feature Details window can export the feature as a JSON file, which can be used in the client application to load the default values.

## Step 4. Creating an audience
{: #audience-applaunch}

To create an audience, complete the following steps:

### Creating an **audience attribute**:
{: #audience-attrib-applaunch}

1. Click **Audience** > **Create Attribute**.
2. Provide the following values:
  * **Name**: Provide an appropriate name for the attribute.
  * **Description**: A brief description on the attribute.
  * **Type**: Choose the attribute type.
  * **Allowed values**: Enter the attribute values that you would want to use.

  You can choose to create more than one audience attribute, as listed in the following image, based on your requirement.

### Creating an **audience**:
{: #audience-create-applaunch}

1. Click **Create Audience**.
2. Provide an appropriate name and description on the New Audience window.
3. Select an attribute, and click **Add**.
4. Choose the required options from the listed attributes.
5. Click **Save**.

You can now create an Engagement.

## Step 5. Creating an Engagement
{: #engagement-applaunch}

An Engagement is an instantiation of a Feature with properties initialized and attaching one of the pre-defined audiences. You can either create an engagement by using **Feature Control** or **In-App Messaging**.

### Enabling Feature Control Capability
{: #feature-control-applaunch}

Through this engagement, an app owner can control the visibility of a feature by enabling or disabling it at run time. A feature can be enabled or disabled to all the application users or to a specific set of users and devices.

Feature roll outs can be scheduled and coordinated by defining a start or end time and date. You can also choose a specific day on which a defined feature is to be enabled or disabled.

Complete the following steps to create an engagement by using the Feature Control:

1. You can create an engagement by using either of the following methods:
  - Click **Engagements** in the navigation pane.
  - Select **Create Engagements** on the new Feature that you created.
  - In the navigation pane, click **Overview** > **Create New Engagement**.

  The New Engagement window appears.

2. Provide a name and description to your new engagement. Ensure that you give a unique engagement name, and not one that is already listed in Engagements.
  - **Select Engagement type** as **Feature Control**.
  - To do a controlled experiment with more than one variant of the feature, select **A/B testing** on the **Select Experimentation Type**. Click **Next**.

3. Select the Feature that you created. You can also choose to add and define the variants that you might want to experiment with. Click **Next**.

4. Select an audience. Click **Next**.

5. Define a trigger by choosing Time and the **Start** date or time and an **End** date or time. Click **Save**.

  The new engagement now appears in the Engagement Details window.

You can now measure the [performance](/docs/services/app-launch?topic=app-launch-applaunch_type#applaunch_type) of your engagement.

### Enabling In-App Messaging Capability
{: #app-message-applaunch}

Through this engagement, an app owner can send notifications to the app users while they're actively using the application.

Messages can be targeted to all application users or to a specific set of users and devices. For every message that you submit to the service, the intended audience receives a notification.

In-app messages can be scheduled by defining a start or end date and time. You might also schedule it based on an event. These messages are more customized as they're based on analytics insights about the user's choice, interactions, device, application logs, and so on.

See the following In-app message examples:

- Send customized messages.
- Send messages to users that turned off push notifications.
- Request feedback or engage users in a conversation.
- Send relevant messages by based on what the user is looking for.
- Engage active and loyal customers.
- Inform users of app updates (launch of a new feature).

![animated gif](images/in-app-engagement_animated.gif){: gif}

Complete the following steps to create an engagement that uses the Messaging option:

1. You can create an engagement by using either of the following methods:
  - Click **Engagements** in the navigation pane.
  - Select **Create Engagement** on the new Feature that you created.
  - In the navigation pane, click **Overview** > **Create New Engagement**.

  The New Engagement window appears.

2. Provide a name and description to your new engagement. Ensure that you give a unique engagement name and not one that is already listed in Engagements.
  - **Select Engagement type** as **In-App Messaging**
  - To do a controlled experiment with multiple variants of the messaging feature, select **A/B testing** on the **Select Experimentation Type**. Click **Next**.

3. Enter the message properties and click **Next**.

4. **Select Audience** and the percentage of audience you want to reach. Click **Next**.

5. Define a trigger by selecting a **Start/End Date and Time**.

6. Select **Event** and click **Next**.

7. Map the elements to the metrics you want to measure. Select the element and enter the metric details. Click **Save**.

  The new engagement now appears in the Engagement Details window.

You can now measure the [performance](/docs/services/app-launch?topic=app-launch-applaunch_type) of your engagement.

## Quick Links
{: #links-applaunch notoc}

Check out the following links to gain insight, and understand the features of {{site.data.keyword.engage_short}}:

 - Try out the [App Launch Service](https://{DomainName}/catalog/services/app-launch){: new_window} ![External link icon](../../icons/launch-glyph.svg "External link icon").
 - [Blogs and Videos](/docs/services/app-launch?topic=app-launch-blogs-and-videos)
 - For more information, see the [App Launch documentation](/docs/services/app-launch?topic=app-launch-gettingstartedtemplate).
