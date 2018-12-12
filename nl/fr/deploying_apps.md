---

copyright:
  years: 2018
lastupdated: "2018-11-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Déploiement et intégration d'applications Swift
{: #deploy_apps}

Vous pouvez déployer vos applications Swift avec une chaîne d'outils ou en utilisant l'interface de ligne de commande. Une chaîne d'outils est un ensemble d'intégrations d'outils. L'interface de ligne de commande constitue un moyen simple de déployer vos applications et vos instances de service.

Pour plus d'informations, voir [Déploiement d'applications](../apps/dep-app-tool.html).

## Développement d'applications Swift sans serveur
{: #serverless}

Pour développer dans un développement sans serveur, vous pouvez utiliser l'offre Faas (Functions as a ServiceS) d'IBM, {{site.data.keyword.openwhisk}}. {{site.data.keyword.openwhisk_short}} permet d'exécuter la logique d'application en réponse à des événements ou à des appels directs du Web ou d'applications mobiles sur HTTP sans avoir à créer ou à gérer des serveurs.

{{site.data.keyword.openwhisk_short}} effectue des tâches d'administration système, comme la mise à l'échelle automatique, la gestion de la disponibilité et la maintenance, ce qui permet aux développeurs de rester concentrés sur l'écriture de la logique d'application.

Pour plus d'informations, voir [Développement d'applications sans serveur](../apps/deploying/functions.html).

## Intégration de services de back-end avec un logiciel SDK généré
{: #backend_gensdk}

Le plug-in {{site.data.keyword.IBM_notm}} SDK Generator intègre aisément des services de back end à votre application à l'aide d'un logiciel SDK généré. Lorsqu'un changement se produit au niveau d'une API REST, vous pouvez regénérer le logiciel SDK et remplacer l'ancien pour mettre à niveau le logiciel SDK. Vous pouvez aussi intégrer l'interface de ligne de commande dans un pipeline DevOps et vous assurer que le SDK est toujours cohérent avec la spécification d'API chaque fois que l'application est générée.

Pour plus d'informations, voir [Intégration de services de back-end avec un logiciel SDK généré](/docs/swift/backend/cli_sdkgen.html).

## Déploiement dans un cluster Kubernetes
{: #deploy_kube}

Vous pouvez apprendre comment utiliser le service Kubernetes {{site.data.keyword.cloud_notm}} pour déployer une application Node.js conteneurisée qui optimise Watson Tone Analyzer. Dans le scénario fourni, une firme PR fictive utilise le service {{site.data.keyword.cloud_notm}} pour analyser ses communiqués de presse et recevoir des commentaires en retour sur la tonalité de ses messages. Pour plus d'informations, voir le tutoriel [Déploiement d'applications dans les clusters](../containers/cs_tutorials_apps.html).

## Déploiement sur un serveur virtuel
{: #virtual_deploy}

Un service virtuel offre un plus haut degré de transparence, de prévisibilité et d'automatisation pour tous les types de charge de travail. Terraform est utilisé pour générer, changer et gérer les versions de votre infrastructure en toute sécurité et de manière efficace.

Pour plus d'informations, voir [Déploiement sur un serveur virtuel](../apps/vsi-deploy.html).
