---

copyright:
  years: 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Swift アプリのデプロイと統合
{: #deploy_apps}

Swift アプリは、ツールチェーンやコマンド・ライン・インターフェースを使用してデプロイできます。ツールチェーンは、ツール統合の集合です。コマンド・ライン・インターフェースは、アプリとサービス・インスタンスをデプロイするための簡単な方法です。

詳しくは、[アプリのデプロイ](../apps/dep-app-tool.html)を参照してください。

## サーバーレス Swift アプリの開発
{: #serverless}

サーバーレス開発を円滑に行うため、IBM の Functions as a Service (FaaS) オファリングである {{site.data.keyword.openwhisk}} を使用できます。{{site.data.keyword.openwhisk_short}} を使用すると、サーバーのプロビジョニングや管理を行うことなく、HTTP 経由での Web アプリまたはモバイル・アプリからの直接呼び出しまたはイベントへの応答としてアプリケーション・ロジックを実行できます。

{{site.data.keyword.openwhisk_short}} が自動スケーリング、可用性管理、保守などのシステム管理を実行するため、開発者はアプリケーション・ロジックの作成に注力できます。

詳しくは、[サーバーレス・アプリの開発](../apps/deploying/functions.html)を参照してください。

## 生成された SDK によるバックエンド・サービスの統合
{: #backend_gensdk}

{{site.data.keyword.IBM_notm}} SDK Generator プラグインは、生成された SDK によって、容易にバックエンド・サービスをアプリに統合します。REST API が変更された場合は、SDK を再生成し、古い SDK を置換してシームレスな SDK アップグレードを行うことができます。また、CLI を devops パイプラインに統合し、アプリをビルドするたびに SDK が API 仕様と常に整合されるようにすることができます。

詳しくは、[生成された SDK によるバックエンド・サービスの統合](/docs/swift/backend/cli_sdkgen.html)を参照してください。

## Kubernetes クラスターへのデプロイ
{: #deploy_kube}

{{site.data.keyword.cloud_notm}} Kubernetes Service を使用して、Watson Tone Analyzer を活用したコンテナー化 Node.js アプリをデプロイする方法を学ぶことができます。用意されているシナリオでは、架空の PR 会社が {{site.data.keyword.cloud_notm}} サービスを使用して自社のプレス・リリースを分析し、自社のメッセージのトーンに関するフィードバックを受け取ります。詳しくは、チュートリアル: [クラスターにアプリをデプロイする方法](../containers/cs_tutorials_apps.html)を参照してください。

## 仮想サーバーへのデプロイ
{: #virtual_deploy}

仮想サービスでは、あらゆるワークロード・タイプのために、高いレベルの透過性、予測性、自動化を提供します。ご使用のインフラストラクチャーを安全かつ効率的にビルドし、変更し、バージョン管理するためには、Terraform が使用されます。

詳しくは、[仮想サーバーへのデプロイ](../apps/vsi-deploy.html)を参照してください。
