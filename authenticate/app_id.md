---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-05"

keywords: authentication swift, security swift, forgot password swift, social swift, identity provider swift, tentantid swift, cloud directory swift

subcollection: swift

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}

# Adding user authentication
{: #appid}

Application security is incredibly complicated. For most developers, it's one of the more difficult tasks of creating an app. How can you be sure that you're protecting your users information? By integrating {{site.data.keyword.appid_full}} into your apps, you can secure resources and add authentication, even when you don't have much security experience.

By requiring users to sign in, you can store user data such as app preferences (or information from public social profiles), and then use that data to customize each users experience within the app. {{site.data.keyword.appid_short_notm}} provides a log in framework for you, but you can also bring your own branded sign-in screens to use with cloud directory.

For all of the ways that you can use {{site.data.keyword.appid_short_notm}} and architecture information, see [About {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-about#about).

## Before you begin
{: #prereqs-appid}

First, be sure that you have the following prerequisites ready to go:
* CocoaPods (version 1.1.0 or higher)
* iOS (version 9 or higher)
* MacOS (version 10.11.5 or higher)
* Xcode (version 9.0.1 or higher)

## Step 1. Creating an instance of {{site.data.keyword.appid_short_notm}}
{: #create-instance-appid}

Create an instance of the {{site.data.keyword.appid_short_notm}} service:

1. In the [{{site.data.keyword.cloud_notm}} catalog](https://{DomainName}/catalog){: external}, select {{site.data.keyword.appid_short_notm}}. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Select your pricing plan and click **Create**.

## Step 2. Installing the iOS Swift SDK
{: #install-sdk-appid}

The service provides an SDK to help make coding your app easier. The SDK must be installed in your app code.

1. Open your existing Xcode project directory to the `Podfile`.

2. Under your projects target, add a dependency for the `IBMCloudAppID` pod. Ensure the `use_frameworks!` command is also under your target as shown in the following example.
    ```swift
    target '<yourTarget>' do
      use_frameworks!
        pod 'IBMCloudAppID'
    end
    ```
    {: pre}

3. Download the `IBMCloudAppID` dependency.
    ```
    pod install --repo-update
    ```
    {: pre}

## Step 3. Initializing the SDK
{: #initialize-sdk-appid}

After you initialize the SDK in your app, you can start configuring your {{site.data.keyword.appid_short_notm}} preferences.

1. Go to **Project Settings > Capabilities > Keychain sharing** and enable keychain sharing in your Xcode project.

2. Go to **Project Settings > Info >URL Types** and add the following value to both the **URL Scheme** and the **Identifier** text boxes.
  ```
  $(PRODUCT_BUNDLE_IDENTIFIER)
  ```
  {: codeblock}

3. Add the following import to your `AppDelegate.swift` file:
  ```swift
  import IBMCloudAppID
  ```
  {: codeblock}

4. Pass the `tenantID` and `AppID_region` parameters to initialize the SDK. It is common to place the code in the `application:didFinishLaunchingWithOptions` method of the `AppDelegate` in your app.
  
  ```swift
  AppID.getInstance().initialize(getApplicationContext(), <tenantId>, <AppID_region>);
  ```
  {: codeblock}

  The `tenantID` is a unique identifier that is used to initialize your app. You can find the value in the {{site.data.keyword.appid_short_notm}} dashboard by selecting the **Service Credentials** tab, and then click **View Credentials**.
  The `AppID_region` is the {{site.data.keyword.cloud_notm}} region in which you're working with the service. This region can be found in the service dashboard and can be `AppID.REGION_US_SOUTH`, `AppID.REGION_SYDNEY`, and `AppID.REGION_UK`.
  {: tip}

5. Add the following code to your `AppDelegate` file.
    ```swift
    func application(_ application: UIApplication, open url: URL, options :[UIApplicationOpenURLOptionsKey : Any]) -> Bool {
            return AppID.sharedInstance.application(application, open: url, options: options)
        }
    ```
    {: codeblock}

## Step 4. Managing the sign-in experience
{: #managing-signin-appid}

An identity provider provides the authentication information for your users so that you can authorize them. With {{site.data.keyword.appid_short_notm}}, you can use social identity providers such as Facebook and Google+ or you can manage a user registry with cloud directory.

You can update your configurations at any time without updating your app code.
{: tip}

### Social identity providers
{: #social-appid}

With {{site.data.keyword.appid_short_notm}}, you can use social identity providers such as Facebook and Google+ to secure your apps.

To configure social identity providers:

1. Open the {{site.data.keyword.appid_short_notm}} dashboard to **Identity Providers > Manage**.
2. Set the identity providers that you want to use to **On**. You can use any combination of identity providers, but if you'd like to bring customized sign-on screens, you need to enable Cloud Directory only.
3. Update the [default configuration](/docs/services/appid?topic=appid-social#social) to your own credentials. {{site.data.keyword.appid_short_notm}} provides IBM credentials that you can use to try out the service, but before you publish your app, you need to update the configuration.
4. Customize the sign-in screen to display the image and colors of your choice.
5. To call the login widget with your app, add the following command to your code.
    ```swift
    import IBMCloudAppID
    class delegate : AuthorizationDelegate {
        public func onAuthorizationSuccess(accessToken: AccessToken, identityToken: IdentityToken, response:Response?) {
            //User authenticated
        }

        public func onAuthorizationCanceled() {
            //Authentication cancelled by the user
        }

        public func onAuthorizationFailure(error: AuthorizationError) {
            //Exception occurred
        }
    }

    AppID.sharedInstance.loginWidget?.launch(delegate: delegate())
    ```
    {: codeblock}

### Cloud directory
{: #cloud-dir-appid}

With {{site.data.keyword.appid_short_notm}}, you can manage your own user registry called cloud directory. Cloud directory enables users to sign up and sign in to your mobile and web apps by using their email and a password.

To bring your own branded UI screens, only cloud directory can be enabled as an identity provider.
{: tip}

To configure cloud directory:

1. Open the {{site.data.keyword.appid_short_notm}} dashboard to the **Managing identity providers** tab and set cloud directory to **On**.
2. Configure your [directory and message settings](/docs/services/appid?topic=appid-cloud-directory).
4. Choose the combinations of sign-on screens you'd like to display and place the code to call those screens in your application.
    * Sign in
        ```swift
        class delegate : TokenResponseDelegate {
            public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
            /* User authenticated */
            }

            public func onAuthorizationFailure(error: AuthorizationError) {
            /* Exception occurred */
            }
        }

        AppID.sharedInstance.obtainTokensWithROP(username: username, password: password, delegate: delegate())
        ```
        {: codeblock}

    * Sign up
        ```swift
        class delegate : AuthorizationDelegate {
          public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             if accessToken == nil && identityToken == nil {
              /* email verification is required */
              return
             }
           /* User authenticated */
          }

          public func onAuthorizationCanceled() {
              /* Sign up cancelled by the user */
          }

          public func onAuthorizationFailure(error: AuthorizationError) {
              /* Exception occurred */
          }
        }

        AppID.sharedInstance.loginWidget?.launchSignUp(delegate: delegate())
        ```
        {: codeblock}

    * Forgot password
        ```swift
        class delegate : AuthorizationDelegate {
           public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
              /* forgot password finished, in this case accessToken and identityToken will be null. */
           }

           public func onAuthorizationCanceled() {
               /* forgot password canceled by the user */
           }

           public func onAuthorizationFailure(error: AuthorizationError) {
               /* Exception occurred */
           }
        }

        AppID.sharedInstance.loginWidget?.launchForgotPassword(delegate: delegate())
        ```
        {: codeblock}

    * Account details
        ```swift

         class delegate : AuthorizationDelegate {
             public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             }

             public func onAuthorizationCanceled() {
             }

             public func onAuthorizationFailure(error: AuthorizationError) {
             }
         }

         AppID.sharedInstance.loginWidget?.launchChangeDetails(delegate: delegate())
        ```
        {: codeblock}

    * Change password
        ```swift
         class delegate : AuthorizationDelegate {
             public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             }

             public func onAuthorizationCanceled() {
             }

             public func onAuthorizationFailure(error: AuthorizationError) {
             }
          }

          AppID.sharedInstance.loginWidget?.launchChangePassword(delegate: delegate())
        ```
        {: codeblock}


## Step 5. Testing your app
{: #testing-appid}

Is everything configured correctly? You can test it out!

1. Open your app with the Xcode emulator.
2. Using the GUI, walk through the process of signing into your application. If you configured cloud directory, be sure that all of your screens are displaying how you intend.
3. Update the identity providers or the login widget screen in the {{site.data.keyword.appid_short_notm}} dashboard.
4. Repeat steps 1 and 2 to see that the changes are immediately implemented. No updates to your app code are required.

Having trouble? Check out [troubleshooting {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-troubleshooting).

## Next steps
{: #next-appid notoc}

Great job! You added a level of security to your app. Keep the momentum by trying one of the following options:

* Learn more about and take advantage of all of the features that {{site.data.keyword.appid_short_notm}} has to offer, [check out the docs](/docs/services/appid?topic=appid-getting-started#getting-started)!
* Starter kits are one of the fastest ways to use the features of {{site.data.keyword.cloud_notm}}. View the available starter kits in the [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://{DomainName}/developer/appledevelopment/starter-kits){: external}.
