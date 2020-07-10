---

copyright:
  years: 2018, 2020
lastupdated: "2020-06-05"

keywords: swift logging, ios logging, debug swift, add logging swift, heliumlogger swift, loggerapi swift, logger swift, starter kit swift logger

subcollection: swift

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:external: target="_blank" .external}

# Logging in Swift
{: #logging_swift}

Log messages are strings with contextual information about the state and activity of the microservice at the time that the log entry is made. Logs are required to diagnose how and why services fail, and plays a supporting role in monitoring application health.

Given the transient nature of processes in cloud environments, logs must be collected and sent elsewhere, usually to a centralized location for analysis. The most consistent way to log in cloud environments is to send log entries to standard output and error streams, which leaves the infrastructure to handle the rest.

## Adding logging to your Swift app
{: #logging-add}

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger){: external} is a popular lightweight logging framework for Swift, and provides many native benefits such as logging to standard output and different log levels.

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI){: external} is the logger protocol that provides a common logging interface for different kinds of loggers in Swift. Kitura uses the `LoggerAPI` throughout its implementation.

To use `HeliumLogger`, add the following code to the **dependencies:** section in your `Package.swift` for all appropriate targets:
```swift
.package(url: "https://github.com/IBM-Swift/HeliumLogger.git", from: "1.7.1")
```
{: codeblock}

Add the following instrumentation code to initialize `HeliumLogger` and set it as the logger used by the `LoggerAPI`:
```swift
import HeliumLogger
import LoggerAPI

let logger = HeliumLogger(.verbose)
Log.logger = logger

Log.info("This is an informational log message.")
```
{: codeblock}

In the provided example, the [log level](http://ibm-swift.github.io/HeliumLogger/){: external} was explicitly set to `.verbose`, which is the default.

For more information about customizing the log messages, see the official [HeliumLogger API reference documentation](http://ibm-swift.github.io/HeliumLogger/){: external}.

## Logging with starter kits
{: #logging-starterkits}

Swift apps that are created by using the {{site.data.keyword.cloud_notm}} App Service come with `HeliumLogger` by default. Running the app natively or in a cloud environment produces the following output:
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

These messages are found in `stdout` (standard output) locally, or in the logs for Cloud Foundry and Kubernetes deployments, which are accessed by `[ibmcloud app logs --recent <APP_NAME>]`(/docs/cli?topic=cli-ibmcloud_commands_apps#ibmcloud_app_logs) and `[kubectl logs <deployment name>`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}.

In the `/Sources/AppName/main.swift` file, you can see the following code:
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

The log level is explicitly set to `.info` to log informational level messages like the application startup logs.
{: tip}

## Next steps
{: #next-logging notoc}

Learn more about viewing logs in each deployment environment:
* [Managing Kubernetes cluster logs with IBM Log Analysis with LogDNA](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-kube)
* [Managing logs from CF resources](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-monitor_cfapp_logs)
* [{{site.data.keyword.openwhisk}} Logs & Monitoring](/docs/openwhisk?topic=openwhisk-logs)
