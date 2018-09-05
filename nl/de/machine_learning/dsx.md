---

copyright:
  years: 2018
lastupdated: "2018-06-04"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Datenbestände mit benutzerdefiniert generierten Modellen analysieren

Watson Studio stellt Ihnen die Umgebung und die Tools zur Verfügung, mit
deren Hilfe Sie geschäftsbezogene Problemstellungen durch die Analyse von Daten
lösen können. Sie können die Tools auswählen, die Sie zum Analysieren,
Bereinigen und Organisieren von Daten benötigen. In diesem Abschnitt erfahren
Sie, wie Sie Datenströme einpflegen oder Modelle für maschinelles Lernen
erstellen, trainieren und bereitstellen. Watson Studio kann bei vielen
{{site.data.keyword.cloud}}-Services sowie Watson Knowledge Catalog
integriert werden, von dem das Richtlinienmanagement für die Steuerung von
Assets bereitgestellt und der Index für die Suche nach diesen Assets
katalogisiert wird. Weitere Informationen hierzu finden Sie unter der
Adresse "https://dataplatform.ibm.com/".

Das Zentrum der Struktur von Watson Studio bildet eine projektbasierte
Architektur, die Ihre Ressourcen für die Lösung eines Geschäftsproblems
verwaltet. Zu diesen Ressourcen gehören Verbindungen zu Datenspeichern vor Ort
und in der Cloud, Datendateien, Collaborators und Analyseassets wie Modelle. Weitere
Informationen finden Sie unter der Adresse
"https://datascience.ibm.com/docs/content/getting-started/overview-ws.html?context=analytics".

## Maschinelles Lernen für {{site.data.keyword.DSX}}
{: #dsx}

Mit {{site.data.keyword.DSX}} können Modelle trainiert und
bereitgestellt werden; anschließend werden die Ergebnisse mithilfe von APIs
verarbeitet. Diese APIs können dann in Ihren iOS- oder Swift-Anwendungen
eingesetzt werden.

Mit IBM Watson Machine Learning können Sie nach dem Einrichten der
Umgebung Modelle erstellen, in der Cloud bereitstellen und trainieren.
Weitere Informationen finden Sie auf der Seite
[Create, deploy, and train models with {{site.data.keyword.pm_full}} and {{site.data.keyword.DSX}}](https://datascience.ibm.com/docs/content/analyze-data/wml-ai.html?context=analytics).

### Lernprogramme
- [Logistisches
Regressionsmodell mit {{site.data.keyword.pm_short}} erstellen](https://datascience.ibm.com/docs/content/analyze-data/ml-example-log-regress.html?context=analytics)
- [Naive-Bayes-Modell
mit {{site.data.keyword.pm_short}} erstellen](https://datascience.ibm.com/docs/content/analyze-data/ml-example-naive-bayes.html?context=analytics)

## {{site.data.keyword.DSX}} mit iOS und Swift einrichten
{: #dsx_ios}

1. Damit die Berechtigungsnachweise leichter integriert werden können,
müssen Sie die
{{site.data.keyword.pm_short}}-Instanz zu Ihrer iOS-App oder
Back-End-App hinzufügen. Um den Zugriff einfach durchführen zu können, werden
Ihre Berechtigungsnachweise in Ihr Projektdashboard einbezogen.

![Maschinelles Lernenin Ihrer App](images/ios-machinelearning-app.png)


2. Laden Sie den App-Code herunter.
3. Initialisierung
  * Bei einem iOS-Projekt werden die Berechtigungsnachweise durch das
einfache Hinzufügen der
{{site.data.keyword.pm_short}}-Ressource zu Ihrem iOS-Projekt sofort in
Ihre App eingefügt. Kopieren Sie das folgende Code-Snippet und fügen Sie es
ein, um von Ihrer Anwendung aus auf die Berechtigungsnachweise zuzugreifen. Denken
Sie auch daran, den Endpunkt für die Bewertung (scoring) zu Ihrer App
hinzuzufügen, der auf der Registerkarte `Implementierung` für
die Bereitstellung Ihres Modells zu finden ist.

    ```Swift
    // The url to your model's scoring endpoint
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"

    // Your credentials
    var machineLearningUsername: String!
    var machineLearningPassword: String!

    // Machine Learning initialization
    if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"),
       let dictionary = NSDictionary(contentsOfFile: contents),
       let username = dictionary["machinelearningUsername"] as? String,
       let password = dictionary["machinelearningPassword"] as? String {

           machineLearningUsername = username
           machineLearningPassword = password

    }
    ```
    {: codeblock}

  * Fügen Sie bei einer serverseitigen Anwendung Ihren Benutzernamen und
das zugehörige Kennwort zur Anwendung wie auch den Endpunkt für die
Bewertung (scoring) hinzu, der auf der Registerkarte
`Implementierung` für die Bereitstellung Ihres Modells zu
finden ist.

    ```Swift
    // Your Machine Learning Credentials
    let machineLearningUsername: String = "<your-ml-service-username>"
    let machineLearningPassword: String = "<your-ml-service-password>"

    // The url to your model's scoring endpoint
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"
    ```
    {: codeblock}

4. Rufen Sie Zugriffstokens ab und führen Sie die Vorhersageanalyse für
Datenbestände aus Ihrer Anwendung heraus mit einem einfachen Client-SDK aus.

  ```Swift
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

### Beispiel
{: #example}

**Szenarioname:** Produktlinienprognose

**Szenariobeschreibung:** Ein Unternehmen, das
Outdoorausrüstungen vertreibt, erstellt und implementiert ein Modell, um das
Kundeninteresse an seiner Produktlinie vorherzusagen. Ihre Aufgabe ist es,
Bewertungsanforderungen für das implementierte Modell auszuführen.

Nachdem das Modell bereitgestellt wurde, können Sie die Vorhersageanalyse
mithilfe des Bewertungsendpunkts vornehmen.

```Swift
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

## Nächste Schritte
{: #dsx_next}

Hervorragend! Durch die Verwendung von benutzerdefiniert generierten
Modellen für maschinelles Lernen können Sie jetzt Datenbestände analysieren. Nutzen Sie diesen Schwung und informieren Sie sich auf der Seite
[Data
Science und maschinelles Lernen](https://www.ibm.com/analytics/data-science/machine-learning) genauer über die Funktionen, die
{{site.data.keyword.pm_short}} zu bieten hat.

### Zugehörige Links
* [{{site.data.keyword.pm_short}}](/docs/services/PredictiveModeling/index.html#using-machine-learning-with-data-science-experience)
* [{{site.data.keyword.DSX}}](https://datascience.ibm.com/)
* [Dokumentation
zu {{site.data.keyword.DSX}}](https://datascience.ibm.com/docs/content/getting-started/welcome-main.html?context=analytics)
