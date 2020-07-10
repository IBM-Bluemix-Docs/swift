---

copyright:
  years: 2018, 2020
lastupdated: "2020-06-05"

keywords: deploy swift app, deploy swift, serverless swift, deploy swift cloud foundry, swift kubernetes

subcollection: swift

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}

# Deploying and integrating Swift apps
{: #deploy_apps-swift}

You can deploy your Swift apps with a toolchain or by using the command-line interface. A toolchain is a set of tool integrations. The command-line interface is a simple way to deploy your apps and service instances.

For more information, see [Deploying apps](/docs/apps?topic=apps-deploying-apps).

## Developing serverless Swift apps
{: #serverless-swift}

To facilitate serverless development, you can use the IBM Function as a Service (FaaS) offering, [{{site.data.keyword.openwhisk}}](https://www.ibm.com/cloud/functions){: external}. {{site.data.keyword.openwhisk_short}} enables application logic to be run in response to events or direct invocations from web or mobile apps over HTTP without provisioning or managing servers.

{{site.data.keyword.openwhisk_short}} performs system administration like auto-scaling, availability management, and maintenance, allowing developers to remain focused on writing application logic.

For more information, see:
* [Developing serverless apps](/docs/apps?topic=apps-serverless)
* [IBM Cloud Functions](https://www.ibm.com/cloud/functions){: external}
* [FaaS (Function-as-a-Service)](https://www.ibm.com/cloud/learn/faas){: external}

## Deploying to a Kubernetes cluster
{: #deploy-kube}

[{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started) is an open source platform for managing containerized workloads and services across multiple hosts, and offers management tools for deploying, automating, monitoring, and scaling containerized apps with minimal to no manual intervention. [Learn more](https://www.ibm.com/cloud/learn/kubernetes). When you deploy your app to a Kubernetes cluster, you can select either Helm or [Knative](/docs/containers?topic=containers-serverless-apps-knative) as the deployment type.

## Deploying to an OpenShift cluster
{: #deploy-openshift}

[Red Hat OpenShift on {{site.data.keyword.cloud_notm}}](/docs/openshift?topic=openshift-getting-started) is a Kubernetes container platform that provides a trusted environment to run enterprise workloads. It extends the Kubernetes platform with built-in software to enhance app lifecycle development, operations, and security. With OpenShift, you can consistently deploy your workloads across hybrid cloud providers and environments. [View a comparison between Kubernetes and OpenShift clusters](https://cloud.ibm.com/docs/openshift?topic=openshift-cs_ov#openshift_kubernetes).

## Deploying to Cloud Foundry
{: #deploy-cf}

[{{site.data.keyword.cloud_notm}} Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-getting-started) is the premier industry standard Platform-as-a-Service (PaaS) that ensures fast, easy, and reliable deployment of cloud-native apps. Cloud Foundry ensures that the build and deploy aspects of coding remain carefully coordinated with any attached services — resulting in quick, consistent, and reliable iterating of applications. Cloud Foundry has a Lite plan that allows quick deployments for testing purposes.
