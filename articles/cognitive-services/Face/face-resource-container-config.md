---
title: Configurer des conteneurs
titlesuffix: Face - Azure Cognitive Services
description: Paramètres de configuration des conteneurs.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: diberry
ms.openlocfilehash: 218343db4d8ac7e35f58951ee177f5c288aed3d9
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55476415"
---
# <a name="configure-face-docker-containers"></a>Configurer des conteneurs Docker Visage

L’environnement d’exécution de conteneur **Visage** est configuré à l’aide des arguments de la commande `docker run`. Ce conteneur a plusieurs paramètres obligatoires et quelques paramètres facultatifs. Plusieurs [exemples](#example-docker-run-commands) de commande sont disponibles. Les paramètres propres aux conteneurs correspondent aux paramètres de facturation. 

Les paramètres de conteneur sont [hiérarchiques](#hierarchical-settings) et peuvent être définis avec des [variables d’environnement](#environment-variable-settings) ou des [arguments de ligne de commande](#command-line-argument-settings) docker.

## <a name="configuration-settings"></a>Paramètres de configuration

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> Les paramètres [`ApiKey`](#apikey-setting), [`Billing`](#billing-setting) et [`Eula`](#eula-setting) sont utilisés conjointement, et vous devez fournir des valeurs valides pour les trois ; à défaut, votre conteneur ne démarrera pas. Pour plus d’informations sur l’instanciation d’un conteneur à l’aide de ces paramètres de configuration, consultez [Facturation](face-how-to-install-containers.md#billing).

## <a name="apikey-configuration-setting"></a>Paramètre de configuration ApiKey

Le paramètre `ApiKey` spécifie la clé de ressource Azure utilisée pour effectuer le suivi des informations de facturation pour le conteneur. Vous devez spécifier une valeur pour ApiKey, et il doit s’agir d’une clé valide pour la ressource _Visage_ spécifiée pour le paramètre de configuration [`Billing`](#billing-setting).

Vous trouverez ce paramètre à l’emplacement suivant :

* Portail Azure : Gestion des ressources de **Visage**, sous **Clés**

## <a name="applicationinsights-setting"></a>Paramètre ApplicationInsights

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Paramètre de configuration Billing

Le paramètre `Billing` permet de spécifier l’URI de point de terminaison de la ressource _Visage_ sur Azure servant à mesurer les informations de facturation du conteneur. Vous devez affecter à ce paramètre de configuration une valeur correspondant à un URI de point de terminaison valide pour une ressource _Visage_ sur Azure.

Vous trouverez ce paramètre à l’emplacement suivant :

* Portail Azure : Vue d’ensemble de **Visage**, étiqueté `Endpoint`

|Obligatoire| NOM | Type de données | Description |
|--|------|-----------|-------------|
|Oui| `Billing` | Chaîne | URI du point de terminaison de facturation<br><br>Exemple :<br>`Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0` |

## <a name="eula-setting"></a>Paramètre Eula

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Paramètres Fluentd

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Paramètres des informations d'identification du proxy HTTP

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Paramètres de journalisation
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Paramètres de montage

Utilisez des montages de liaisons pour lire et écrire des données vers et à partir du conteneur. Vous pouvez spécifier un montage d’entrée ou de sortie en spécifiant l’option `--mount` dans la commande [docker run](https://docs.docker.com/engine/reference/commandline/run/).

Les conteneurs Visage n’utilisent pas de montage d’entrée ou de sortie pour stocker des données d’entraînement ou de service. 

La syntaxe exacte de l’emplacement de montage d’hôte varie en fonction du système d’exploitation hôte. De plus, l’emplacement de montage de l’[ordinateur hôte](face-how-to-install-containers.md#the-host-computer) peut ne pas être accessible en raison d’un conflit entre les autorisations utilisées par le compte de service Docker et les autorisations de l’emplacement de montage de l’hôte. 

|Facultatif| NOM | Type de données | Description |
|-------|------|-----------|-------------|
|Non autorisé| `Input` | Chaîne | Les conteneurs Visage n’utilisent pas cet élément.|
|Facultatif| `Output` | Chaîne | Cible du montage de sortie. La valeur par défaut est `/output`. Il s’agit de l’emplacement des journaux. Les journaux de conteneur sont inclus. <br><br>Exemple :<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="hierarchical-settings"></a>Paramètres hiérarchiques

[!INCLUDE [Container shared configuration hierarchical settings](../../../includes/cognitive-services-containers-configuration-shared-hierarchical-settings.md)]

## <a name="example-docker-run-commands"></a>Exemples de commandes docker run 

Les exemples suivants utilisent les paramètres de configuration pour illustrer comment écrire et utiliser des commandes `docker run`.  Une fois en cours d’exécution, le conteneur continue à s’exécuter jusqu’à ce que vous l’[arrêtiez](face-how-to-install-containers.md#stop-the-container).

* **Caractère de continuation de ligne** : Les commandes Docker dans les sections suivantes utilisent la barre oblique inverse, `\`, comme caractère de continuation de ligne. Remplacez-la ou supprimez-la en fonction des exigences de votre système d’exploitation hôte. 
* **Ordre des arguments** : Ne changez pas l’ordre des arguments, sauf si vous avez une connaissance approfondie des conteneurs Docker.

Remplacez {_argument_name_} par vos propres valeurs :

| Placeholder | Valeur | Format ou exemple |
|-------------|-------|---|
|{BILLING_KEY} | Clé de point de terminaison de la ressource Visage. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT_URI} | Valeur de point de terminaison de facturation, région comprise.|`https://westcentralus.api.cognitive.microsoft.com/face/v1.0`|

> [!IMPORTANT]
> Vous devez spécifier les options `Eula`, `Billing` et `ApiKey` pour exécuter le conteneur, sinon il ne démarrera pas.  Pour plus d'informations, consultez [Facturation](face-how-to-install-containers.md#billing).
> La valeur ApiKey est la **Clé** présente dans la page des clés des ressources de Visage Azure. 

## <a name="face-container-docker-examples"></a>Exemples Docker de conteneurs Visage

Les exemples Docker suivants s’appliquent au conteneur Visage. 

### <a name="basic-example"></a>Exemple de base 

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example-with-command-line-arguments"></a>Exemple de journalisation avec arguments de ligne de commande

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} \
  Logging:Console:LogLevel=Information
  ```

### <a name="logging-example-with-environment-variable"></a>Exemple de journalisation avec variable d’environnement

  ```Docker
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY}
  ```

## <a name="next-steps"></a>Étapes suivantes

* Consultez [Guide pratique pour installer et exécuter des conteneurs](face-how-to-install-containers.md).
