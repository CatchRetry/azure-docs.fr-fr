---
title: Authentification à l’aide d’Azure AD dans les clouds nationaux
description: Découvrez l’inscription d’application et les points de terminaison d’authentification pour les clouds nationaux.
services: active-directory
documentationcenter: ''
author: negoe
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/20/2018
ms.author: negoe
ms.reviewer: negoe,andret,saeeda,CelesteDG
ms.custom: aaddev
ms.openlocfilehash: 2130cc4d51965a087ccdbaeb922eda5059ee1c82
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55091578"
---
# <a name="national-clouds"></a>Clouds nationaux

Les clouds nationaux (également appelé clouds souverains) sont des instances d’Azure physiquement isolées. Ces régions d’Azure sont conçues pour garantir que les conditions de résidence, de souveraineté et de conformité des données sont respectées au sein de limites géographiques.

Outre le cloud global, Azure Active Directory est déployé dans les clouds nationaux suivants :  

- Azure US Government
- Azure Germany
- Azure China 21Vianet

Les clouds nationaux sont des environnements uniques, différents d’Azure mondial. Par conséquent, vous devez bien comprendre les principales différences quand vous développez votre application pour ces environnements, comme l’inscription des applications, l’acquisition des jetons et la configuration des points de terminaison.

## <a name="app-registration-endpoints"></a>Points de terminaison d’inscription d’application

Il existe un portail Azure distinct pour chacun des clouds nationaux. Pour intégrer des applications à la plateforme d’identités Microsoft dans un cloud national, vous devez inscrire votre application séparément dans chaque portail Azure propre à l’environnement.

Le tableau suivant répertorie les URL de base des points de terminaison Azure Active Directory (Azure AD) utilisés pour inscrire une application pour chaque cloud national.

| Cloud national | Point de terminaison du Portail Azure AD
| --- | --- |
| Azure AD pour le gouvernement des États-Unis |https://portal.azure.us
|Azure AD Allemagne |https://portal.microsoftazure.de
|Azure AD Chine géré par 21Vianet |https://portal.azure.cn
|Azure AD (service mondial)|https://portal.azure.com 

## <a name="azure-ad-authentication-endpoints"></a>Points de terminaison d’authentification Azure AD

Tous les clouds nationaux authentifient les utilisateurs séparément dans chaque environnement et ont des points de terminaison d’authentification distincts.

Le tableau suivant liste les URL de base des points de terminaison Azure Active Directory (Azure AD) utilisés pour obtenir des jetons pour chaque cloud national.

| Cloud national | Point de terminaison d’authentification Azure AD
| --- | --- |
| Azure AD pour le gouvernement des États-Unis |`https://login.microsoftonline.us`
|Azure AD Allemagne| `https://login.microsoftonline.de`
|Azure AD Chine géré par 21Vianet | `https://login.chinacloudapi.cn`
|Azure AD (service mondial)|`https://login.microsoftonline.com`

- Les requêtes adressées aux points de terminaison de jeton ou d’autorisation Azure AD peuvent être formées à l’aide de l’URL de base spécifique à la région appropriée. Par exemple, pour Azure Allemagne :

  - Le point de terminaison d’autorisation commun est `https://login.microsoftonline.de/common/oauth2/authorize`.
  - Le point de terminaison de jeton commun est `https://login.microsoftonline.de/common/oauth2/token`.

- Pour les applications monolocataires, remplacez « common » dans les URL ci-dessus par l’ID ou le nom de votre locataire, par exemple, `https://login.microsoftonline.de/contoso.com`.

>[!NOTE]
> Les points de terminaison de jeton et [d’autorisation Azure AD v2.0]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) sont disponibles uniquement pour le service mondial. Ils ne sont pas encore pris en charge pour les déploiements de clouds nationaux.

## <a name="microsoft-graph-api"></a>API Microsoft Graph

Pour savoir comment appeler les API Microsoft Graph dans un environnement de cloud national, accédez à [Microsoft Graph dans un cloud national](https://developer.microsoft.com/graph/docs/concepts/deployments).



>[!IMPORTANT]
Certains services et fonctionnalités dans des régions spécifiques du service mondial peuvent ne pas être disponibles dans l’ensemble des clouds nationaux. Pour connaître les services disponibles, accédez aux [produits disponibles par région](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

## <a name="next-steps"></a>Étapes suivantes

- Découvrez plus d’informations sur [Azure Government](https://docs.microsoft.com/azure/azure-government/).
- Découvrez plus d’informations sur [Azure Chine géré par 21Vianet](https://docs.microsoft.com/azure/china/).
- Découvrez plus d’informations sur [Azure Allemagne](https://docs.microsoft.com/azure/germany/).
- Découvrez plus d’informations sur les [principes de base de l’authentification Azure AD](authentication-scenarios.md).
