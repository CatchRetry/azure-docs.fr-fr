---
title: Analyser les données Log Analytics dans Azure Monitor | Microsoft Docs
description: Vous avez besoin d’effectuer une recherche dans les journaux pour récupérer des données à partir de Log Analytics.  Cet article décrit comment sont utilisées les nouvelles recherches dans les journaux dans Log Analytics, puis présente les concepts que vous devez comprendre avant de créer une recherche.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: bwren
ms.openlocfilehash: d3fc44456ac4f0df2bee35300c0f40728a40cb92
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54882233"
---
# <a name="analyze-log-analytics-data-in-azure-monitor"></a>Analyser les données Log Analytics dans Azure Monitor

Les données de journal collectées par Azure Monitor sont stockées dans un espace de travail Log Analytics, qui est basé sur [Azure Data Explorer](/azure/data-explorer). Il collecte des données de télémétrie à partir de diverses sources et utilise le [langage de requête de Data Explorer](/azure/kusto/query) pour récupérer et analyser les données.

> [!NOTE]
> Log Analytics a été précédemment traité comme un service propre dans Azure. Il fait désormais partie intégrante d’Azure Monitor et se concentre sur le stockage et l’analyse des données de journal par le biais de son langage de requête. Les fonctionnalités qui faisaient partie intégrante de Log Analytics, comme les agents Windows et Linux de collecte de données, les vues permettant d’afficher les données existantes, et les alertes vous informant des problèmes, n’ont pas changé, mais elles font aujourd’hui partie d’Azure Monitor.



## <a name="log-queries"></a>Requêtes dans les journaux

Vous devez effectuer une requête dans les journaux pour récupérer des données à partir de Log Analytics.  Que vous [analysiez des données dans le portail](../log-query/portals.md), [configuriez une règle d’alerte](../platform/alerts-metric.md) pour être averti d’une condition particulière ou récupériez des données à l’aide de l’[API Log Analytics](https://dev.loganalytics.io/), vous utiliserez une requête pour spécifier les données souhaitées.  Cet article décrit l’utilisation des requêtes dans les journaux dans Log Analytics, et présente les concepts que vous devez comprendre avant de créer une requête.



## <a name="where-log-queries-are-used"></a>Lorsque les requêtes dans les journaux sont utilisées

Voici plusieurs façons d’utiliser les requêtes dans Log Analytics :

- **Portails.** Vous pouvez effectuer une analyse interactive des données de journal dans le [portail Azure](../log-query/portals.md).  Cela vous permet de modifier votre requête et d’analyser les résultats dans divers formats et visualisations.  
- **Règles d’alerte.** Les [règles d’alerte](../platform/alerts-overview.md) identifient de façon proactive les problèmes à partir des données dans votre espace de travail.  Chaque règle d’alerte est basée sur une recherche dans les journaux qui est exécutée automatiquement à intervalles réguliers.  Les résultats sont inspectés pour déterminer si une alerte doit être créée.
- **Tableaux de bord.** Vous pouvez épingler les résultats de n’importe quelle requête dans un [tableau de bord Azure](../learn/tutorial-logs-dashboards.md) afin de visualiser les données de journal et les métriques ensemble et de partager ces informations avec d’autres utilisateurs Azure si vous le souhaitez. 
- **Vues.**  Vous pouvez créer des visualisations de données à inclure dans les tableaux de bord utilisateur avec le [Concepteur de vues](../platform/view-designer.md).  Les requêtes dans les journaux fournissent les données utilisées par les [vignettes](../platform/view-designer-tiles.md) et les [composants de visualisation](../platform/view-designer-parts.md) dans chaque vue.  
- **Exportation.**  Quand vous importez des données de l’espace de travail Log Analytics vers Excel ou [Power BI](../platform/powerbi.md), vous créez une requête de journal pour définir les données à exporter.
- **PowerShell.** Vous pouvez exécuter un script PowerShell à partir d’une ligne de commande ou d’un runbook Azure Automation qui utilise [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) pour récupérer des données à partir de Log Analytics.  Cette applet de commande nécessite une requête pour déterminer les données à récupérer.
- **API Log Analytics.**  L’[API de recherche dans les journaux Log Analytics](../platform/alerts-overview.md) permet à tout client d’API REST de récupérer des données de journal à partir de l’espace de travail.  La demande d’API comprend une requête qui est exécutée sur Log Analytics pour déterminer les données à récupérer.

![Recherches dans les journaux](media/log-query-overview/queries-overview.png)

## <a name="write-a-query"></a>Écrivez votre requête.
Log Analytics inclut [une version du langage de requête de Data Explorer](../log-query/get-started-queries.md) pour récupérer et analyser les données de journal de différentes façons.  Vous commencerez généralement par des requêtes de base, puis vous passerez à des fonctions plus avancées à mesure que vos exigences deviendront plus complexes.

La structure de base d’une requête est une table source suivie d’une série d’opérateurs séparés par une barre verticale `|`.  Vous pouvez chaîner plusieurs opérateurs pour affiner les données et effectuer des fonctions avancées.

Par exemple, supposez que vous souhaitez rechercher les dix ordinateurs ayant eu le plus d’événements d’erreur durant les dernières 24 heures,

```Kusto
Event
| where (EventLevelName == "Error")
| where (TimeGenerated > ago(1days))
| summarize ErrorCount = count() by Computer
| top 10 by ErrorCount desc
```

ou que vous souhaitez rechercher les ordinateurs qui n’ont pas eu de pulsation durant les dernières 24 heures.

```Kusto
Heartbeat
| where TimeGenerated > ago(7d)
| summarize max(TimeGenerated) by Computer
| where max_TimeGenerated < ago(1d)  
```

Et si vous génériez un graphique en courbes avec l’utilisation du processeur pour chaque ordinateur durant la semaine dernière ?

```Kusto
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
| render timechart    
```

Grâce à ces exemples rapides, vous pouvez constater que, quel que soit le type de données avec lequel vous travaillez, la structure de la requête est similaire.  Vous pouvez la décomposer en étapes distinctes où les données obtenues à partir d’une commande sont envoyées à la commande suivante par l’intermédiaire du pipeline.

Vous pouvez également interroger les données dans les espaces de travail Log Analytics au sein de votre abonnement.

```Kusto
union Update, workspace("contoso-workspace").Update
| where TimeGenerated >= ago(1h)
| summarize dcount(Computer) by Classification 
```

## <a name="how-log-analytics-data-is-organized"></a>Organisation des données Log Analytics
Quand vous générez une requête, commencez par déterminer les tables où figurent les données que vous recherchez. Les différents types de données sont séparés en tables dédiées dans chaque [espace de travail Log Analytics](../learn/quick-create-workspace.md).  La documentation des diverses sources de données inclut le nom du type de données créé et une description de chacune de ses propriétés.  De nombreuses requêtes nécessitent les données d’une seule table, mais d’autres peuvent utiliser diverses options pour inclure des données provenant de plusieurs tables.

Bien qu’[Application Insights](../app/app-insights-overview.md) stocke les données d’application telles que les requêtes, les exceptions, les traces et l’utilisation dans Log Analytics, ces données sont stockées dans une partition différente des autres données de journal. Le même langage de requête vous permet d’accéder à ces données, mais vous devez utiliser la [console Application Insights](../app/analytics.md) ou l’[API REST Application Insights](https://dev.applicationinsights.io/) pour y accéder. Vous pouvez utiliser les [requêtes interressources](../log-query/cross-workspace-query.md) pour combiner des données Application Insights avec d’autres données dans Log Analytics.


![Tables](media/log-query-overview/queries-tables.png)







## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur les [portails que vous utilisez pour créer et modifier des recherches dans les journaux](../log-query/portals.md).
- Consultez un [didacticiel sur l’écriture de requêtes](../log-query/get-started-queries.md) à l’aide du nouveau langage de requête.
