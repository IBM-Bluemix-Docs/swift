---

copyright:
  years: 2018, 2020
lastupdated: "2020-06-05"

keywords: swift-cfenv, service bindings swift, environment swift, swift configuration, cloudenvironment swift, VCAP_SERVICES swift, swift credentials

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}

# Configuring the Swift environment
{: #configuration}

Cloud-native development has two closely related practices that intersect in how you handle configuration data. The first is that you must build immutable artifacts to minimize the likelihood that errors are introduced as your application journeys from development to production. The second is that you must strive for parity between your development, and production environments, to avoid problems created by environment-specific code. 

Management of service configuration and credentials (service bindings) varies between platforms. Cloud Foundry stores service binding details in a stringified JSON object that is passed to the application as an environment variable `VCAP_SERVICES`. Kubernetes stores service bindings as stringified JSON or flat attributes in `ConfigMaps` or `Secrets`, which can be passed to the containerized application as environment variables or mounted as a temporary volume. Local development has its own configuration, as local testing is often a simplified derivative of whatever is running in the cloud. Working across these variations in a portable way without having environment-specific code paths can be challenging.

You can follow simple guidelines to help you write portable applications, and utilities that you can use to encapsulate finding service bindings (or other configuration) in environment-specific locations. Whether you need to add cloud support to existing applications or create apps with Starter Kits, the goal is to provide portability for Swift apps to be used on many deployment platforms.

## Adding {{site.data.keyword.cloud_notm}} to existing Swift applications
{: #addcloud-env}

The path for abstracting environment values can differ from one cloud environment to another. The [CloudEnvironment](https://github.com/IBM-Swift/CloudEnvironment){: external} library abstracts environment configuration and credentials from various cloud providers so that your Swift app can consistently access the information by running locally, in Cloud Foundry, Kubernetes, or OpenShift. The credentials abstraction is provided by the `CloudEnvironment` library, which internally uses [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: external} for Cloud Foundry configuration and [Configuration](https://github.com/IBM-Swift/Configuration){: external} as a configuration manager.

With `CloudEnvironment`, you can abstract low-level details from your application's source code by defining a lookup key that your Swift application can use for searching its corresponding value.

The `CloudEnvironment` library provides a consistent lookup key that can be used in your source code. The library then searches across an array of search patterns to find a JSON object with the configuration attributes or service credentials. 

### Adding the CloudEnvironment package to your Swift application
{: #add-cloudenv}

To use the `CloudEnvironment` package in your Swift application, specify it in the **dependencies:** section of your `Package.swift` file:
```swift
.package(url: "https://github.com/IBM-Swift/CloudEnvironment.git", from: "8.0.0"),
```
{: codeblock}

Then, add the following instrumentation code to your application:
```swift
import CloudEnvironment

let cloudEnv = CloudEnv()
```
{: codeblock}

### Accessing credentials
{: #access-credentials}

Now that the `CloudEnvironment` library is initialized, you can access your credentials as shown in the following examples:
```swift
let cloudantCredentials = cloudEnv.getCloudantCredentials(name: "cloudant-credentials")
// cloudantCredentials.username, cloudantCredentials.password, cloudantCredentials.url, etc.
let objStorageCredentials = cloudEnv.getObjectStorageCredentials(name: "object-storage-credentials")
// objStorageCredentials.username, objStorageCredentials.password, objStorageCredentials.projectID, etc.

let service1Credentials = cloudEnv.getDictionary("service1-credentials")
let service1CredentialsStr = cloudEnv.getString("service1-credentials")

// Get a default PORT and URL
let port = cloudEnv.port
let url = cloudEnv.url
```
{: codeblock}

This example provides access to the credential sets for services, which can now be used to initialize connections to these [supported services or a generic dictionary](https://github.com/IBM-Swift/CloudEnvironment#supported-services){: external}. Check out [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api){: external} for Cloud Foundry specific configuration, and [configuration details](https://github.com/IBM-Swift/Configuration){: external} on loading configuration data.

## Understanding service credentials
{: #service_creds}

The `CloudEnvironment` library uses a file that is named `mappings.json`, found in the `config` directory, to communicate where the credentials are stored for each service. The `mappings.json` file supports searching for values that use the following three search pattern types:
- **`cloudfoundry`** - A pattern type used to search for a value in Cloud Foundry's services environment variable (`VCAP_SERVICES`). For Cloud Foundry Enrprise Edition, see this [getting started tutorial](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started) for more information.
- **`env`** - A pattern type used to search for a value that is mapped to an environment variable, as in Kubernetes or {{site.data.keyword.openwhisk}}.
- **`file`** - A pattern type used to search for a value in a JSON file. The path must be relative to the root folder of your Swift application.

The array of values that are associated with a lookup key is searched in the order they appear.

The following shows an example of a `mappings.json` file:
```javascript
{
    "cloudant-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-cloudant-db",
                "env:my_awesome_cloudant_db_credentials",
                "file:localdev/my-awesome-cloudant-db-credentials.json"
            ]
        }
    },
    "object-storage-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-object-storage",
                "env:my_awesome_object_storage_credentials",
                "file:localdev/my-awesome-object-storage-credentials.json"
            ]
        }
    }
}
```
{: codeblock}

In this example, `cloudant-credentials` and `object-storage-credentials` are the lookup keys that your Swift application uses to look up the corresponding credentials. According to the array of search patterns, the configuration manager first looks for `my-awesome-cloudant-db` in `VCAP_SERVICES`, then it looks for `my_awesome_cloudant_db_credentials` as an environment variable. If that isn't defined, it searches the contents of `my-awesome-object-storage-credentials.json` in the `localdev` folder. 

When the application runs locally, it can use credentials that are stored in a file, like `localdev/my-awesome-object-storage-credentials.json` as shown in the previous example. The connection information for services you want to access locally, such as username, password, and hostname, are all to be stored in this file. 

For security reasons, credential files don't belong in repositories. In the previous example, a `localdev` folder is used to store local credentials so you must add this folder to the `.gitignore` file to avoid an accidental commit. If you're using a Starter Kit app, this folder is created for you, and is present in the `.gitignore` file.

For more information about the `mappings.json` file, check out the [Understanding service credential](#service_creds) section.

## Using the Swift configuration manager from starter kit apps
{: #configmanager-swift}

Swift apps that are created with [starter kits](https://{DomainName}/developer/appledevelopment/starter-kits){: external} automatically come with the credentials and configuration that is needed to run locally, and also in many cloud deployment targets, such as [Kubernetes](/docs/containers?topic=containers-getting-started) and [Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-getting-started).

The basic creation of the configuration manager can be found in `Sources/Application/Application.swift`. When you create a Swift-based Starter Kit app with services, a `config` folder and `mappings.json` file is created for you. If you download your app, the `config` folder includes a `localdev-config.json` file that has all of the credentials for your services, and is present in the `.gitignore` file.

## Next steps
{: #next-configß notoc}

Check out our three libraries to help your applications abstract themselves from their environments:

* [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env){: external}
* [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: external}
* [Configuration](https://github.com/IBM-Swift/Configuration){: external}
