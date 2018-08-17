---

copyright:
  years: 2018
lastupdated: "2018-08-17"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Using Application Metrics with Swift apps
{: #metrics}

Application metrics are important for monitoring the performance of your application. Monitoring the performance of the environment, including CPU, Memory, Latency, and HTTP metrics; can seem like a monumental effort, but it is essential to ensure that your Swift application is running effectively over time. Cloud Native services, such as autoscaling, can rely on these metrics to scale your app to perform under peak load and scale down to keep costs low.

Application metrics can help you to identify common performance problems such as:

* Slow HTTP response times on some or all routes
* Poor throughput in the application
* Spikes in demand that cause slowdown
* Higher than expected CPU usage for the level of throughput/load
* High and/or growing memory usage (potential memory leak)

## Adding Application Metrics to your existing Swift application
{: #add-appmetrics-existing}

Use [Application Metrics for Swift](https://developer.ibm.com/swift/monitoring-diagnostics/application-metrics-for-swift/) to add performance monitoring to your Swift application. Application Metrics for Swift is comprised of two libraries: `SwiftMetrics`, and `SwiftMetricsDash`.

* The `SwiftMetrics` library is a comprehensive instrumentation library that gathers and aggregates metrics for your application. It has several extensions, including a Kitura module for HTTP metrics, [Prometheus support](https://github.com/RuntimeTools/SwiftMetrics#prometheus-support), and a stand-alone [emitter](https://github.com/RuntimeTools/SwiftMetrics#application-metrics-for-swift-agent).

* The `SwiftMetricsDash` library consumes the metrics that are produced by `SwiftMetrics`, and provides a built-in dashboard for visualization.


To enable the base monitoring API, add `SwiftMetrics` to the **dependencies:** section in your `Package.swift`, and make sure to add it to the desired target:
```swift
.package(url: "https://github.com/RuntimeTools/SwiftMetrics.git", from: "2.4.0")
```
{: codeblock}

Add the following instrumentation code to initialize the `SwiftMetrics` code:
```swift
import SwiftMetrics
import SwiftMetricsKitura

let metrics = try SwiftMetrics()
SwiftMetricsKitura(swiftMetricsInstance: metrics)
let monitoring = metrics.monitor()
```
{: codeblock}

Add the following code to the original sample to provide metrics in the monitoring dashboard:
```swift
import SwiftMetricsDash

let smd = try SwiftMetricsDash(swiftMetricsInstance : metrics)
```  
{: codeblock}

By default, `SwiftMetricsDash` starts its own Kitura server, and serves up the page under  `http://<hostname>:<port>/swiftmetrics-dash`. Access the dashboard to see your new application metrics, including HTTP requests, and event loop latency.

## Using Application Metrics in Starter Kits

The server-side Swift applications that are created from Starter Kits include `SwiftMetrics`, `SwiftMetricsDash`, and `SwiftMetricsPrometheus`, so they are ready for use in Kubernetes environments that use Prometheus endpoints for gathering metrics.

The `SwiftMetrics` code can be found in `/Sources/Application/Metrics.swift`:
```swift
import Kitura
import SwiftMetrics
import SwiftMetricsDash
import SwiftMetricsPrometheus
import LoggerAPI

var swiftMetrics: SwiftMetrics?
var swiftMetricsDash: SwiftMetricsDash?
var swiftMetricsPrometheus: SwiftMetricsPrometheus?

func initializeMetrics(router: Router) {
    do {
        let metrics = try SwiftMetrics()
        let dashboard = try SwiftMetricsDash(swiftMetricsInstance: metrics, endpoint: router)
        let prometheus = try SwiftMetricsPrometheus(swiftMetricsInstance: metrics, endpoint: router)

        swiftMetrics = metrics
        swiftMetricsDash = dashboard
        swiftMetricsPrometheus = prometheus
        Log.info("Initialized metrics.")
    } catch {
        Log.warning("Failed to initialize metrics: \(error)")
    }
}
```
{: codeblock}

Once your application is running, you can access the dashboard by using the `/swiftmetrics-dash` endpoint.

By default, `SwiftMetricsPrometheus` provides the [Prometheus endpoint](https://prometheus.io/) under `http://<hostname>:<port>/metrics`.