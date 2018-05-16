---
copyright:
  years: 2017, 2018
lastupdated: "2018-05-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Integrating back-end services to your app with a generated SDK
{: #sdk-cli}

The {{site.data.keyword.IBM}} SDK Generator plug-in can be installed in the [{{site.data.keyword.Bluemix_notm}} CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/cli/reference/bluemix_cli/index.html){: new_window}.


This {{site.data.keyword.IBM_notm}} SDK Generator plug-in enables you to easily integrate your back-end services to your app with a generated SDK. When a change to a REST API occurs, you can regenerate the SDK and replace the old one for a seamless SDK upgrade. You can also integrate the CLI into a devops pipeline and ensure that the SDK is always consistent with the API spec each time the app is built.

The REST API definition must be valid and either hosted on a live server endpoint or a local file on your system.


## Before you begin
{: #prereqs}

Ensure that you have:

* An [{{site.data.keyword.Bluemix_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){: new_window} account
* A valid API definition that conforms to the [Open API ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.openapis.org/){: new_window} specification


## Installing the SDK plug-in
{: #installation}

1. [Install the {{site.data.keyword.Bluemix}} CLI](/docs/cli/reference/bluemix_cli/get_started.html).

2. [Install the plug-in ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/cli/reference/bluemix_cli/index.html#install_plug-in){: new_window}.

  ```
  bx plugin install sdk-gen
  ```
  {: codeblock}


## Generating the SDK
{: #commands}

Generate an SDK by entering: `bx sdk generate [arguments...] [command options]`


### Arguments
{: #gen-args}

* `APP_NAME` - the name of the Cloud Foundry app in your current space
* `OPENAPI_DOC_LOCATION` - a URL or a relative file path to the raw REST API definition JSON or Yaml
* `GENERATED_SDK_NAME` (optional) - the name of the generated SDK


### Options
{: #gen-options}

* `PLATFORM` (required)
   * `--ios` - generate an iOS Swift SDK
   * `--swift` - generate a Swift server SDK
   * `--js` - generate a JavaScript SDK
* `LOCATION` (required) - specifies the type for `OPENAPI_DOC_LOCATION`
   * `-r` - remote URL
   * `-f` - file
   * `-a` - app that runs on {{site.data.keyword.Bluemix_notm}}
   * `-l` - localhost URL
* `--output "YOUR_RELATIVE_PATH"` (optional) - places the generated SDK in the directory that is specified by `YOUR_RELATIVE_PATH` (overwrites if existing SDK is present)
* `--unzip` (optional) - extracts the generated SDK (overwrites if existing SDK artifacts are present)


### Usage
{: #gen-usage}

To generate an SDK from a Cloud Foundry app that is running in {{site.data.keyword.Bluemix_notm}}, you can use the app's name as a parameter to the CLI. The following command uses the app's name as the `SDK_Name`.

```
bx sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

To generate an SDK from a URL to an Open API definition file or a local JSON or Yaml file, use the following command.

```
bx sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}


## Validating the Open API definitions
{: #validating}

Run the following command: `bx sdk validate [argument]`


### Arguments
{: #val-args}

* `APP_NAME` - the name of the Cloud Foundry app in your current space
* `OPENAPI_DOC_LOCATION` - a URL or a relative file path to the raw REST API definition JSON or Yaml


### Usage
{: #val-usage}

To validate a Cloud Foundry app's API spec that is running in {{site.data.keyword.Bluemix_notm}}, you can use the app's name as a parameter to the CLI.
```
bx sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

To validate an SDK from the URL to an API spec document or a local JSON or Yaml file, use the following command:
```
bx sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}
