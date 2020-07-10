---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-05"

keywords: liveness probe swift, readiness probe swift, health swift, healthcheck swift, swift app status, kubernetes endpoint swift, health endpoint swift, swift health check

subcollection: swift

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:external: target="_blank" .external}

# Using a health check in your Swift app
{: #healthcheck}

Health checks provide a simple mechanism to determine whether a server-side application is behaving properly. Cloud environments like [Kubernetes](https://www.ibm.com/cloud/container-service){: external} and [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry){: external} can be configured to poll health endpoints periodically to determine whether an instance of your service is ready to accept traffic.
{: shortdesc}

## Health check overview
{: #overview}

Health checks provide a simple mechanism to determine whether a server-side application is behaving properly. They're typically consumed over HTTP and use standard return codes to indicate UP or DOWN status. The return value of a health check is variable, but a minimal JSON response, like `{"status": "UP"}`, is typical.

Kubernetes has a nuanced notion of process health. It defines two probes:

- A _**readiness**_ probe is used to indicate whether the process can handle requests (is routable).

  Kubernetes doesn't route work to a container with a failing readiness probe. A readiness probe can fail if a service isn't finished initializing, or is otherwise busy, overloaded, or unable to process requests.

- A _**liveness**_ probe is used to indicate whether the process is to be restarted.

  Kubernetes stops and restarts a container with a failing liveness probe. Use liveness probes to clean up processes in an unrecoverable state, for example, if memory is exhausted, or if the container didn't stop properly after an internal process crashed.

As a note for comparison, Cloud Foundry uses one health endpoint. If this check fails, the process is restarted, but if it succeeds, requests are routed to it. In this environment, the endpoint minimally succeeds when the process is live. An initial wait time is configured to defer health checking until the service is finished initializing to avoid restart cycles.

The following table provides guidance on the responses that readiness, liveness, and singular health endpoints can provide.

| State    | Readiness                   | Liveness                   | Health                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | Non-OK causes no load       | Non-OK causes restart      | Non-OK causes restart     |
| Starting | 503 - Unavailable           | 200 - OK                   | Use delay to avoid test   |
| Up       | 200 - OK                    | 200 - OK                   | 200 - OK                  |
| Stopping | 503 - Unavailable           | 200 - OK                   | 503 - Unavailable         |
| Down     | 503 - Unavailable           | 503 - Unavailable          | 503 - Unavailable         |
| Errored  | 500 - Server Error          | 500 - Server Error         | 500 - Server Error        |
{: caption="Table 1. HTTP status codes." caption-side="bottom"}

## Adding a health check to an existing Swift app
{: #existing-app}

The [Health](https://github.com/IBM-Swift/Health){: external} library makes it easy add a health check to your Swift application. Health checks are extensible. For more information about [caching](https://github.com/IBM-Swift/Health#caching){: external} to prevent DoS attacks or adding [custom checks](https://github.com/IBM-Swift/Health#implementing-a-health-check){: external}, see the [Health](https://github.com/IBM-Swift/Health){: external} library.

To add the Health library to an existing Swift app, see the following steps:

1. Specify it in the *dependencies:* section of your `Package.swift` file, and add it to all appropriate targets:

    ```swift
    .package(url: "https://github.com/IBM-Swift/Health.git", .from: "1.0.0"),
    ```
    {: codeblock}

2. Add the following initialization code to your application:

    ```swift
    import Health

    let health = Health()
    ```
    {: codeblock}

3. Add the route definition to define the health check endpoint:

    ```swift
    router.get("/health") { request, response, next in
        /* let status = health.status.toDictionary() */
        let status = health.status.toSimpleDictionary()
        if health.status.state == .UP {
            try response.send(json: status).end()
        } else {
            try response.status(.serviceUnavailable).send(json: status).end()
        }
    }
    ```
    {: codeblock}

4. Check the status of the app with a browser by accessing the `/health` endpoint. The code returns a payload `{"status": "UP"}`, as defined by the simple dictionary.

## Checking the health of a server-side Swift starter kit app
{: #healthcheck-starterkit}

When you generate a Kitura-based Swift app by using a starter kit, a basic health check endpoint, `/health`, is included by default. The endpoint uses the Codable protocol available in Swift 4, as supported by the [Health](https://github.com/IBM-Swift/Health){: external} library.

Basic initialization code, such as the initialization of the Health object occurs in `Sources/Application.swift`. The health check endpoint itself is provided by the `/Sources/Application/Routes/HealthRoutes.swift` file, and uses the following code:

```swift
import LoggerAPI
import Health
import KituraContracts

func initializeHealthRoutes(app: App) {

    app.router.get("/health") { (respondWith: (Status?, RequestError?) -> Void) -> Void in
        if health.status.state == .UP {
            respondWith(health.status, nil)
        } else {
            respondWith(nil, RequestError(.serviceUnavailable, body: health.status))
        }
    }

}
```
{: codeblock}

The example uses the standard dictionary, which yields a payload such as:
```
{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}
```

when you access the `/health` endpoint.

## Recommendations for readiness and liveness probes
{: #recommend-probes}

Readiness checks must include the viability of connections to downstream services in their result if an no acceptable fallback exists for when the downstream service is unavailable. This doesn't mean calling the health check that is provided by the downstream service directly, as infrastructure checks that for you. Instead, consider verifying the health of the existing references your application has to downstream services: this might be a JMS connection to WebSphere MQ, or an initialized Kafka consumer or producer. If you do check the viability of internal references to downstream services, cache the result to minimize the impact health checking has on your application.

A liveness probe, by contrast, can be deliberate about what is checked, as a failure results in immediate termination of the process. A simple HTTP endpoint that always returns `{"status": "UP"}` with status code `200` is a reasonable choice.

### Add support for Kubernetes Readiness and Liveness to a Swift app
{: #kube-readiness-swift}

For alternative implementations, such as using **Codable** or the standard dictionary, see [Health library examples](https://github.com/IBM-Swift/Health#usage){: external}. Some of these implementations simplify the creation of extensible health checks with support for caching checks that are performed against backing services. In this scenario, you would want to separate the simple liveness test from the more robust, detailed readiness check.

## Configuring readiness and liveness probes in Kubernetes
{: #config-kube-readiness}

Declare liveness and readiness probes alongside your Kubernetes deployment. Both probes use the same configuration parameters:

* The kubelet waits for `initialDelaySeconds` before the first probe.

* The kubelet probes the service every `periodSeconds` seconds. The default is 1.

* The probe times out after `timeoutSeconds` seconds. The default and minimum value is 1.

* The probe is successful if it succeeds `successThreshold` times after a failure. The default and minimum value is 1. The value must be 1 for liveness probes.

* When a pod starts and the probe fails, Kubernetes tries `failureThreshold` times to restart the pod and then giving up. The minimum value is 1 and the default value is 3.
    - For a liveness probe, "giving up" means restarting the pod.
    - For a readiness probe, "giving up" means marking the pod `Unready`.

To avoid restart cycles, set `livenessProbe.initialDelaySeconds` to be safely longer than it takes your service to initialize. You can then use a shorter value for `readinessProbe.initialDelaySeconds` to route requests to the service as soon as it's ready.

See the following `yaml` example:
```yaml
spec:
  containers:
  - name: ...
    image: ...
    readinessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 120
      timeoutSeconds: 5
    livenessProbe:
      httpGet:
        path: /liveness
        port: 8080
      initialDelaySeconds: 130
      timeoutSeconds: 10
      failureThreshold: 10
```

For more information, see how to [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/){: external}.
