---
title: Réplication transactionnelle avec Azure SQL Database | Microsoft Docs
description: Découvrez comment utiliser la réplication transactionnelle SQL Server avec des bases de données autonomes, regroupées et d’instance dans Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 548bc9afb37f8c4a1c6c208a8741d1e3da0a784c
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469394"
---
# <a name="transactional-replication-with-standalone-pooled-and-instance-databases-in-azure-sql-database"></a>Réplication transactionnelle avec des bases de données autonomes, regroupées et d’instance dans Azure SQL Database

La réplication transactionnelle est une fonctionnalité d’Azure SQL Database, de Managed Instance et de SQL Server qui vous permet de répliquer les données d’une table dans Azure SQL Database ou SQL Server sur des tables placées dans des bases de données distantes. Cette fonctionnalité vous permet de synchroniser plusieurs tables dans différentes bases de données.

## <a name="when-to-use-transactional-replication"></a>Quand utiliser la réplication transactionnelle

La réplication transactionnelle est utile dans les scénarios suivants :

- Publier les changements apportés dans une ou plusieurs tables d’une base de données et les distribuer à une ou plusieurs bases de données SQL Server ou SQL Azure abonnées aux changements.
- Maintenir plusieurs bases de données distribuées dans un état synchronisé.
- Migrer des bases de données d’un serveur SQL Server ou d’une instance Managed Instance vers une autre base de données en publiant les modifications en continu.

## <a name="overview"></a>Vue d’ensemble

Les composants clés de la réplication transactionnelle sont présentés dans l’image suivante :  

![réplication avec SQL Database](media/replication-to-sql-database/replication-to-sql-database.png)


Le **serveur de publication** est une instance ou un serveur qui publie les changements apportés à des tables (articles) en envoyant les mises à jour au serveur de distribution. La publication sur une base de données SQL Azure à partir d’un serveur SQL Server local est prise en charge sur les versions suivantes de SQL Server :

    - SQL Server 2019 (préversion)
    - SQL Server 2016 à SQL 2017
    - SQL Server 2014 SP1 CU3 ou ultérieur (12.00.4427)
    - SQL Server 2014 RTM CU10 (12.00.2556)
    - SQL Server 2012 SP3 ou ultérieur (11.0.6020)
    - SQL Server 2012 SP2 CU8 (11.0.5634.0)
    - Pour les autres versions de SQL Server qui ne prennent pas en charge la publication sur des objets dans Azure, il est possible d’utiliser la méthode de [republication des données](https://docs.microsoft.com/sql/relational-databases/replication/republish-data) pour déplacer des données vers des versions plus récentes de SQL Server. 

Le **serveur de distribution** est une instance ou un serveur qui collecte les changements apportés aux articles à partir d’un serveur de publication et qui les distribue aux Abonnés. Le serveur de distribution peut être Azure SQL Database Managed Instance ou SQL Server (n’importe quelle version, tant qu’elle est égale ou supérieure à la version du serveur de publication). 

L’**Abonné** est une instance ou un serveur qui reçoit les changements apportés sur le serveur de publication. Les abonnés peuvent être des bases de données autonomes, regroupées et d’instance dans Azure SQL Database ou des bases de données SQL Server. Un abonné sur une base de données autonome ou regroupée doit être configuré en tant qu’abonné d’envoi (push). 

| Rôle | Bases de données autonomes et regroupées | Bases de données d’instance |
| :----| :------------- | :--------------- |
| **Publisher** | Non  | Oui | 
| **Serveur de distribution** | Non  | Oui|
| **Abonné de type pull** | Non  | Oui|
| **Abonné de type push**| Oui | Oui|
| &nbsp; | &nbsp; | &nbsp; |

Il existe différents [types de réplications](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication?view=sql-server-2017) :


| Réplication | Bases de données autonomes et regroupées | Bases de données d’instance|
| :----| :------------- | :--------------- |
| [**Transactionnelle**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) | Oui (uniquement en tant qu’Abonné) | Oui | 
| [**Capture instantanée**](https://docs.microsoft.com/sql/relational-databases/replication/snapshot-replication) | Oui (uniquement en tant qu’Abonné) | Oui|
| [**Réplication de fusion**](https://docs.microsoft.com/sql/relational-databases/replication/merge/merge-replication) | Non  | Non |
| [**Pair à pair**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/peer-to-peer-transactional-replication) | Non  | Non |
| **Unidirectionnelle** | Oui | Oui|
| [**Bidirectionnelle**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/bidirectional-transactional-replication) | Non  | Oui|
| [**Abonnements pouvant être mis à jour**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication) | Non  | Non |
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > - Si vous configurez la réplication avec une version antérieure, les erreurs suivantes peuvent se produire : MSSQL_REPL20084 (le processus n’a pas pu se connecter à l’abonné) et MSSQL_REPL40532 (impossible d’ouvrir le serveur \<nom> demandé par la connexion. La connexion a échoué).
  > - Pour bénéficier de toutes les fonctionnalités d’Azure SQL Database, vous devez utiliser les dernières versions de [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) et de [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017).

## <a name="requirements"></a>Configuration requise

- La connectivité doit utiliser l’authentification SQL entre les participants de la réplication. 
- Un partage de compte de stockage Azure pour le répertoire de travail utilisé par la réplication. 
- Le port 445 (TCP sortant) doit être ouvert dans les règles de sécurité du sous-réseau Managed Instance pour accéder au partage de fichiers Azure. 
- Le port 1433 (TCP sortant) doit être ouvert si les serveurs de publication/distribution se trouvent sur une instance Managed Instance et que l’Abonné est local. 

## <a name="common-configurations"></a>Configurations courantes

En règle générale, le serveur de publication et le serveur de distribution doivent se trouver dans le cloud ou en local. Les configurations suivantes sont prises en charge : 

### <a name="publisher-with-local-distributor-on-a-managed-instance"></a>Serveur de publication avec un serveur de distribution local sur une instance Managed Instance

![Instance unique faisant office de serveur de publication et de serveur de distribution ](media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

Le serveur de publication et le serveur de distribution sont configurés dans une même instance Managed Instance, et les changements sont distribués à d’autres instances Managed Instance, bases de données uniques, bases de données regroupées ou serveurs SQL Server en local. Dans cette configuration, l’instance Managed Instance faisant office de serveur de publication et de serveur de distribution ne peut pas être configurée avec [la géoréplication et des groupes de basculement automatique](sql-database-auto-failover-group.md).

### <a name="publisher-with-remote-distributor-on-a-managed-instance"></a>Serveur de publication avec un serveur de distribution distant sur une instance Managed Instance

Dans cette configuration, une instance Managed Instance publie les changements sur le serveur de distribution placé sur une autre instance Managed Instance qui peut servir plusieurs instances Managed Instance sources et distribuer les changements à une ou plusieurs cibles sur une instance Managed Instance, une base de données unique, une base de données regroupée ou un serveur SQL Server.

![Instances séparées pour le serveur de publication et le serveur de distribution](media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

Le serveur de publication et le serveur de distribution sont configurés sur deux instances Managed Instance. Dans cette configuration

- Les deux instances Managed Instance sont sur le même réseau virtuel.
- Les deux instances Managed Instance se trouvent au même emplacement.
- Les instances Managed Instance qui hébergent les bases de données du serveur de publication et du serveur de distribution ne peuvent pas être [géorépliquées à l’aide de groupes de basculement automatique](sql-database-auto-failover-group.md).

### <a name="publisher-and-distributor-on-premises-with-a-subscriber-on-a-standalone-pooled-and-instance-database"></a>Serveurs de publication et de distribution locaux avec un abonné sur une base de données autonome, regroupée et d’instance 

![Base de données SQL Azure en tant qu’Abonné](media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)
 
Dans cette configuration, une Azure SQL Database (base de données autonome, regroupée et d’instance) est un abonné. Cette configuration prend en charge la migration de données locales vers Azure. Si un abonné se trouve sur une base de données autonome ou regroupée, il doit être en mode transmission de type push.  

## <a name="next-steps"></a>Étapes suivantes

1. [Configurer la réplication transactionnelle pour une instance Managed Instance](replication-with-sql-database-managed-instance.md#configure-publishing-and-distribution-example). 
1. [Créer une publication](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication).
1. [Créer un abonnement par émission de données](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription) en utilisant le nom du serveur Azure SQL Database en tant qu’abonné (par exemple, `N'azuresqldbdns.database.windows.net`) et le nom d’Azure SQL Database en tant que base de données de destination (par exemple, **Adventureworks**). )


## <a name="see-also"></a>Voir aussi  

- [Réplication sur SQL Database](replication-to-sql-database.md)
- [Réplication sur une instance Managed Instance](replication-with-sql-database-managed-instance.md)
- [Créer une publication](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Créer un abonnement par émission de données](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Types de réplication](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [Surveillance (réplication)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Initialiser un abonnement](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
