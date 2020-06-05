---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-05"

keywords: push swift, swift notifications, push notifications swift, sending push swift, configure service instance swift, provider credentials swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:external: target="_blank" .external}

# Sending {{site.data.keyword.mobilepushshort}}
{: #push_notifications}

Enhance your Swift app by using {{site.data.keyword.mobilepushshort}} service on {{site.data.keyword.cloud}} to send live notifications to mobile devices and web applications.

 - Notifications can be delivered to either all application users, or to a selected set of users or devices.
 - Supports both interactive and silent notifications.
 - Customers can choose to subscribe to specific tags or topics for notification.
 - Enables the app owner to analyze the number of devices that are registered to receive notifications and the number of notifications sent.

You can choose to use the {{site.data.keyword.mobilepushshort}} service either as a part of MobileFirst Services Starter Boilerplate or as {{site.data.keyword.cloud_notm}} [Dedicated Services](/docs/dedicated?topic=dedicated-dedicated#dedicated). You can also use an SDK (software development kit) and [REST APIs](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/notifications/rest-apis/){: external} to further develop your client applications.

![Push overview](images/push_notification_lifecycle.jpg "Push overview"){: caption="Figure 1. Overview of the {{site.data.keyword.mobilepushshort}} service lifecycle" caption-side="bottom"}

## Before you begin
{: #prereqs-push}

First, be sure that you have the following prerequisites ready to go:

 - iOS 8.0+
 - Xcode 7.3, 8.0
 - Swift 2.3 - 4.0
 - CocoaPods or Carthage

## Step 1. Creating an instance of {{site.data.keyword.mobilepushshort}}
{: #push_create}

1. In the {{site.data.keyword.cloud_notm}} catalog, click **Mobile** > **{{site.data.keyword.mobilepushshort}}**. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Click **Create**.
4. In the navigation pane, click **Connections** to select an app and bind it to your service. You can bind the service instance to your app later if you leave it unbound during creation.


## Step 2. Obtain your notification provider credentials
{: #get_creds-push}

To set up Push Notifications service, you need to get the required credentials from the Apple Push Notification Service (APNs). Follow the steps here to [obtain and configure your APNs credentials](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1).


## Step 3. Configure a service instance
{: #config-resource-push}

To use the {{site.data.keyword.mobilepushshort}} service to send notifications, upload the `.p12` keystore that you created, which has the private key and SSL certificates that are required to build and publish your application. You can also use the REST API to upload an APNs certificate.

After the `.cer` file is in your key chain access, export it to your computer to create a `.p12` certificate.

For more information about using the APNs, see [iOS Developer Library: Local and Push Notification Programming Guide](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1){: external}.

To set up APNs on the Push Notification services console, complete the steps:

1. Select **Configure** on the {{site.data.keyword.mobilepushshort}} services console.
2. Choose the **Mobile** option to update the information in the **APNs Push Credentials** form.
3. Choose either of the following options:
	- For **Mobile** option
		1. Select **Sandbox** (development) or **Production** (distribution), and then upload the `p.12` certificate that you created.
		  ![{{site.data.keyword.mobilepushshort}} console](images/wizard.jpg "Push notification mobile configuration"){: caption="Figure 2. Push notification mobile configuration" caption-side="bottom"}

		2. In the **Password** field, enter the password that is associated with the `.p12` certificate file, then click **Save**.
	- For **Web** option
		- In the Safari Push section, update the form with the required information.
		- **Website Name**: The website name provided in the Notification center.
		- **Website Push ID**: Update with the reverse-domain string for your Website Push ID. For example, `web.com.example.www`.
		- **Website URL**: Provide the URL of the website that is subscribed to push notifications. For example, `https://www.example.com`.
		- **Allowed Domains**: (optional parameter) A list of websites that request permission from the user. Ensure that the URLs are comma separated values. The values in Website URL are used if the info isn't provided.
		- **URL Format String**: The URL to resolve when the notification is clicked. For example, `https://www.example.com`. Ensure that the URL uses http or https schema.
		-**Safari web push certificate**: Upload the `.p12` certificate and provide the password.
4. Click **Save**.

  ![{{site.data.keyword.mobilepushshort}} console](images/push_configure_safari.jpg "Push notification web configuration"){: caption="Figure 3. Push notification web configuration" caption-side="bottom"}

## Step 4. Set up service client SDK
{: #service-client-push}

To enable iOS applications to receive push notifications to your devices, you need to configure the iOS SDK for the {{site.data.keyword.mobilepushshort}} service.

The {{site.data.keyword.cloud_notm}} Mobile Services Swift SDKs can be installed with either Cocoapods or Carthage. For more information, see [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application){: external}.

## Step 5. Sending a notification
{: #send-notify-push}

After you develop your application, you can send basic push notifications.

To send basic push notifications, complete the following steps:

1. Select **Send Notifications**, and compose a message by choosing a **Send to** option. The supported options are **Device by Tag**, **Device Id**, **User Id**, **iOS devices**, **Web Notifications**, and **All Devices**.
**Note**: When you select the **All Devices** option, all devices that are subscribed to {{site.data.keyword.mobilepushshort}} receive notifications.

  ![Send Notifications screen](images/tag_notification.jpg "Send Notifications screen"){: caption="Figure 4. Send Notifications screen" caption-side="bottom"}

2. In the **Message** field, compose your message. Choose to configure the optional settings as required.
3. Click **Send**.
3. Verify that your devices or browser received the notification.

  The following screen capture shows an alert box that handles a push notification in the foreground on the device.
  
  ![Foreground push notification on Android](images/Android_Screenshot.jpg "Foreground notification alert"){: caption="Figure 5. Foreground notification alert on Android" caption-side="bottom"}

  The following screen capture shows a push notification in the background.
  
  ![Background push notification on iOSd](images/background.png "Background notification alert"){: caption="Figure 6. Background notification alert on iOS" caption-side="bottom"}

### Optional settings
{: #optional-push}

You can customize the {{site.data.keyword.mobilepushshort}} settings for sending notifications to iOS devices. The following optional customization options are supported.

- **Badge**: Indicates the number that is displayed on the application badge. The default value is zero (0), and does not display a badge.
- **Sound**: Indicates a sound clip to be played on the receipt of a notification. Supports default or the name of a sound resource that is bundled in the app.
- **Additional payload**: Specifies the custom payload values for your notifications.

You can also choose to enable [interactive notifications](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#interactive-notifications){: external} and [rich media notifications](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enabling-rich-media-notifications){: external}.

## Step 6. Monitoring for delivered notifications
{: #monitor-push}

The {{site.data.keyword.mobilepushshort}} service provides a monitoring utility to help you check the status of messages that are sent. To configure your monitoring utility, see [Enable monitoring for iOS applications](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enable-monitoring){: external}.

## Next steps
{: #next-push notoc}

 - To learn more about the service and take advantage of all of the features, read through our [documentation](/docs/mobilepush?topic=mobilepush-getting-started).

 - For an introduction to working with Mobile services and {{site.data.keyword.cloud_notm}}, see [Getting started with Mobile apps on {{site.data.keyword.cloud_notm}}](/docs/services/mobile?topic=mobile-about).

 - Starter kits are one of the fastest ways to use the features of {{site.data.keyword.cloud_notm}}. View our available starter kits in the [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: external}.

 - You can use the [Swagger UI](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/notifications/rest-apis/){: external} to quickly review REST API documentation.
