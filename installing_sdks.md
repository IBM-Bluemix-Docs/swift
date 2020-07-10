---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-21"

keywords: install sdk swift, sdk client swift, carthage, cocoapods, swift package manager, ios sdk

subcollection: swift

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:external: target="_blank" .external}

# Installing SDKs into client apps
{: #install-sdks}

{{site.data.keyword.cloud}} iOS SDKs support various of popular dependency managers, enabling you to easily install, and use {{site.data.keyword.cloud_notm}} services within your own applications.

## Installing with CocoaPods
{: #install_cocoapods}

To install an SDK by using CocoaPods, add it to your `Podfile`. If your project doesn't have a `Podfile` yet, use the `pod init` command.
```ruby
use_frameworks!

target 'MyApp' do
    pod '<SDK Name>'
end
```
{: codeblock}

Run `pod install`, and open the generated `.xcworkspace` file.

For more information, see the [CocoaPods Guides](https://guides.cocoapods.org/using/index.html){: external}).

## Installing with Carthage
{: #install_carthage}

To install an SDK with Carthage, add this line to your `Cartfile`:
```
github "<github org name>/<github project name>"
```
{: codeblock}

Run `carthage update` to start the build process. After the build is finished, add the generated frameworks to your project. 

For more information, see the [Carthage getting started](https://github.com/Carthage/Carthage#getting-started){: external} documentation.

## Installing with the Swift package manager
{: #install_swift_package}

To install an SDK by using the Swift Package Manager, add the following line to your dependencies in your `Package.swift`:
```swift
.Package(url: "<SDK git url>")
```
{: codeblock}

Run `swift build` to start the build process.

For more information, see the [Swift Package Manager Overview](https://swift.org/package-manager/){: external}.

## Installing manually
{: #install_manually}

To install an SDK manually, download the SDK and manually add the source files into your project.
