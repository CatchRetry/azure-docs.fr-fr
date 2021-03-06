---
title: Le chargement d’une application Proxy d’application prend trop de temps | Microsoft Docs
description: Résolution des problèmes de performances associés au chargement de page avec le Proxy d’application Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: ad2190f3bfa9e943258a55a8b8ecdff429d6576e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165776"
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Le chargement d’une application Proxy d’application prend trop de temps

Cet article vous aide à comprendre pourquoi le chargement d’une application Proxy d’application peut prendre beaucoup de temps. Il explique également ce que vous pouvez faire pour résoudre ce problème.

## <a name="overview"></a>Vue d'ensemble
Bien que vos applications fonctionnent, elles peuvent subir un temps de latence. Il peut y avoir des modifications de topologie de réseau que vous pouvez effectuer pour améliorer la vitesse. Pour une évaluation des différentes topologies, consultez le [document des considérations sur la topologie du réseau](application-proxy-network-topology.md).

Outre la topologie du réseau, il n’existe actuellement aucune autre recommandation pour l’optimisation des performances. Lorsque le service de proxy d’application se développe, il peut s’étendre à un centre de données qui est physiquement plus proche. La plus grande proximité peut améliorer la latence. Pour obtenir la liste de centres de données Azure, consultez la [page de test de latence](http://www.azurespeed.com/Azure/Latency). 

Pour trouver les centres de données avec le service Proxy d’application, utilisez l’outil [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Commentaires sur les emplacements des centres de données du Proxy d’application 
Même si certains centres de données Azure n’incluent pas encore le Proxy d’application, ils peuvent néanmoins améliorer considérablement vos temps de latence. Envoyez l’emplacement du centre de données à aadapfeedback@microsoft.com. Microsoft utilise vos commentaires pour ses plans d’expansion.

Microsoft travaille sur des fonctionnalités supplémentaires pour améliorer la latence. Dès que ces améliorations seront disponibles, la documentation sera mise à jour.

## <a name="next-steps"></a>Étapes suivantes
[Travailler avec des serveurs proxy locaux existants](application-proxy-configure-connectors-with-proxy-servers.md)
