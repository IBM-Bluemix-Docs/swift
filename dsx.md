---

copyright:
  years: 2018, 2020
lastupdated: "2020-08-03"

keywords: watson studio swift, machine learning swift, custom model swift, data set swift, predictive swift, watson api swift, generated model swift, dataset swift

subcollection: swift

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:external: target="_blank" .external}

# Analyzing data sets with custom generated models
{: #dsx-overview}

Watson Studio provides you with the environment and tools to solve your business problems by collaboratively analyzing data. You can choose the tools that you need to analyze, cleanse, and organize data. Learn to ingest streaming data, or to create, train, and deploy machine learning models. Watson Studio integrates with a wide range of {{site.data.keyword.cloud}} services and Watson Knowledge Catalog, which provides policy management to control assets, and catalogs to index for locating them. [Learn more](https://dataplatform.cloud.ibm.com/){: external}.

Watson Studio is structured around a project-based architecture, which organizes your resources for solving a business problem. Resources include connections to cloud and on-premises data stores, data files, collaborators, and analytic assets like models. For more information, see the [Watson Studio overview](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/overview-ws.html){: external}.

## Machine learning for {{site.data.keyword.DSX}}
{: #dsx-learning}

By using the {{site.data.keyword.DSX}}, it is possible to train models and deploy them and then consume the results by using APIs. These APIs can then be used in your iOS or Swift applications.

With IBM Watson Machine Learning, after you set up your environment, you can create models, deploy them to the cloud, and train them. For more information, see [Create, deploy, and train models with {{site.data.keyword.pm_full}} and {{site.data.keyword.DSX}}](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/wml-ai.html){: external}.

### Tutorials
{: #dsx-tutorials}

## Setting up {{site.data.keyword.DSX}} with iOS and Swift
{: #dsx_ios}

1. To make the integration of the credentials easier, you must add the {{site.data.keyword.pm_short}} instance to your iOS app or back-end app. For ease of accessibility, your credentials are included on your project dashboard.

![App details](images/ios-machinelearning-app.png "App details"){: caption="Figure 1. App details" caption-side="bottom"}

2. Download the app code.
3. Initialization
  * For an iOS project, simply by adding the {{site.data.keyword.pm_short}} service to your iOS project, the credentials are instantly injected into your app.
    To access the credentials from your application, copy and paste the following code snippet. Also, be sure to add the scoring end point to your app, which can be found inside your model's deployment `implementation` tab.

    ```swift
    /* The url to your model's scoring endpoint */
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"

    /* Your credentials */
    var machineLearningUsername: String!
    var machineLearningPassword: String!

    /* Machine Learning initialization */
    if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"),
       let dictionary = NSDictionary(contentsOfFile: contents),
       let username = dictionary["machinelearningUsername"] as? String,
       let password = dictionary["machinelearningPassword"] as? String {

           machineLearningUsername = username
           machineLearningPassword = password

    }
    ```
    {: codeblock}

  * For server-side application, manually add your username and password to your application as well as the scoring end point, which can be found inside your model's deployment `implementation` tab.

    ```swift
    /* Your Machine Learning Credentials */
    let machineLearningUsername: String = "<your-ml-service-username>"
    let machineLearningPassword: String = "<your-ml-service-password>"

    /* The url to your model's scoring endpoint */
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"
    ```
    {: codeblock}

4. Retrieve access tokens and perform predictive analysis on data sets from your application with a simple client SDK.

  ```swift
  public class MachineLearning {

      private let username: String
      private let password: String

      public init(username: String, password: String) {
          self.username = username
          self.password = password
      }

      public static func failure(_ error: Error) {
          print("Error:", error.localizedDescription)
      }

      public func retrieveToken(failure: @escaping (Error) -> Void = MachineLearning.failure, success: @escaping (String) -> Void) {

          guard var request = createRequest(url: "https://ibm-watson-ml.mybluemix.net/v3/identity/token") else {
              failure(NSError(domain: "Could not create url", code: 1, userInfo: nil))
              return
          }

          guard let authString = (username + ":" + password).data(using: .utf8)?.base64EncodedString() else {
              failure(NSError(domain: "Could not encode credentials", code: 1, userInfo: nil))
              return
          }

          request.setValue("Basic \(authString)", forHTTPHeaderField: "Authorization")

          execute(request, failure: failure) { dict in

              guard let token = dict["token"] as? String else {
                  failure(NSError(domain: "Invalid Response", code: 1, userInfo: nil))
                  return
              }

              success(token)
          }
      }

      public func retrieveScore(_ scoringUrl: String, token: String, payload: [String: Any], failure: @escaping (Error) -> Void  = failure, success: @escaping ([String: Any]) -> Void) {

          guard let data = try? JSONSerialization.data(withJSONObject: payload, options: .prettyPrinted) else {
              failure(NSError(domain: "Could not encode data", code: 1, userInfo: nil))
              return
          }

          guard var request = createRequest(method: "POST", url: scoringUrl, body: data) else {
              failure(NSError(domain: "Could not create url", code: 1, userInfo: nil))
              return
          }

          request.setValue("Bearer " + token, forHTTPHeaderField: "Authorization")
          request.setValue("application/json", forHTTPHeaderField: "Content-Type")

          execute(request, failure: failure, success: success)
      }

      public func createRequest(method: String = "GET", url: String, body: Data? = nil) -> URLRequest? {
          guard let url = URL(string: url) else {
              return nil
          }
          var req = URLRequest(url: url)
          req.httpMethod = method
          req.httpBody = body
          return req
      }

      private func execute(_ request: URLRequest, failure: @escaping (Error) -> Void, success: @escaping ([String: Any]) -> Void) {
          URLSession.shared.dataTask(with: request) { data, _, error in

              guard let data = data, error == nil else {
                  let error = error ?? NSError(domain: "No data was returned", code: 1, userInfo: nil)
                  failure(error)
                  return
              }

              guard let body = try? JSONSerialization.jsonObject(with: data, options: .allowFragments) as? [String: Any],
                    let dict = body else {
                  failure(NSError(domain: "Could not create dictionary", code: 1, userInfo: nil))
                  return
              }

              guard let resp = response as? HTTPURLResponse , resp.statusCode == 200 else {
                 let msg = (dict["errors"] as? [[String: Any]])?.first?["message"] as? String ?? "An error occurred"
                 let code = (response as? HTTPURLResponse)?.statusCode ?? 1
                 failure(NSError(domain: msg, code: code, userInfo: nil))
                 return
             }

              success(dict)

          }.resume()
      }
  }
  ```
  {: codeblock}

### Example
{: #dsx-example}

**Scenario name:** Product line prediction

**Scenario description:** A company that sells outdoor equipment builds and deploys a model to predict client interest in its product line. Your task is to make score requests against the deployed model.

Once the model is deployed, you can perform predictive analysis by using the scoring end point.

```swift
// The data you want to have analyzed
let examplePayload: [String: Any] = [
    "fields": ["GENDER", "AGE", "MARITAL_STATUS", "PROFESSION"],
    "values": [["M", 23, "Single", "Student"],["M", 55, "Single", "Executive"]]
]

// Your service credentials
let machineLearningUsername: String = "<your-ml-service-username>"
let machineLearningPassword: String = "<your-ml-service-password>"

// Your scoring end-point
let scoringUrl: String = "<your-model-scoring-url>"

// Define your client
let client = MachineLearning(username: username, password: password)

// Retrieve your access token and score your payload!
client.retrieveToken { token in
    client.retrieveScore(scoringUrl, token: token, payload: examplePayload) { dict in
        print(dict)
    }
}
```
{: codeblock}

## Next steps
{: #dsx_next notoc}

Great job! Now you can analyze data sets by using custom generated machine learning models. Keep the momentum by learning more about the features that {{site.data.keyword.pm_short}} has to offer at [Data science and machine learning](https://www.ibm.com/analytics/machine-learning){: external}.

### Related links
{: #dsx-related}

* [{{site.data.keyword.DSX}}](https://www.ibm.com/cloud/watson-studio){: external}
* [{{site.data.keyword.DSX}} documentation](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/welcome-main.html){: external}
