---
title: Fichier Include
description: Fichier Include
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/24/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a948a40e638b5f6e042c62ab58c2b7b65a49cd4e
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741978"
---
Le stockage localement redondant (LRS) offre une durabilité des objets d’au moins 99,999999999 % (11 « neuf ») sur une année donnée. Le stockage localement redondant fournit la durabilité de cet objet en répliquant vos données sur une unité d’échelle de stockage. Un centre de données, qui se trouve dans la région où vous avez créé votre compte de stockage, héberge l’unité d’échelle de stockage. Une demande d’écriture adressée à un compte de stockage LRS retourne un indicateur de réussite seulement une fois que les données sont écrites dans tous les réplicas. Chaque réplica se trouve dans des domaines d’erreur et de mise à jour distincts au sein d’une unité d’échelle de stockage.

Une unité d’échelle de stockage est un ensemble de racks de nœuds de stockage. Un domaine d’erreur est un groupe de nœuds qui représente une unité physique de défaillance. Vous pouvez considérer qu’un domaine d’erreur est un ensemble de nœuds appartenant au même rack physique. Un domaine de mise à niveau est un groupe de nœuds qui sont mis à jour ensemble au cours d’un processus de mise à niveau de service (déploiement). Les réplicas sont répartis entre les domaines de mise à niveau et d’erreur au sein d’une même unité d’échelle de stockage. Cette architecture garantit que vos données sont disponibles si une défaillance matérielle affecte un rack, ou quand des nœuds sont mis à niveau lors d’une mise à niveau de maintenance.

LRS est l’option de réplication la moins coûteuse et offre la durabilité la plus faible en comparaison des autres options. Si un sinistre se produit au niveau du centre de données (par exemple, un incendie ou une inondation), tous les réplicas risquent d’être perdus ou irrécupérables. Pour atténuer ce risque, Microsoft recommande d’utiliser le stockage redondant interzone (ZRS) ou le stockage géoredondant (GRS).

* En revanche, si votre application stocke des données qui peuvent être recréées facilement en cas de perte, vous pouvez opter pour un stockage LRS.
* Certaines applications sont limitées à la réplication des données dans un pays en raison des exigences de gouvernance des données. Dans certains cas, les régions appairées dans lesquelles les données sont répliquées pour les comptes GRS peuvent se trouver dans un autre pays. Pour plus d’informations sur les régions associées, consultez la page [Régions Azure](https://azure.microsoft.com/regions/).
