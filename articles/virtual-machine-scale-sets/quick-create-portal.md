---
title: Guide de démarrage rapide - Créer un groupe de machines virtuelles identiques dans le portail Azure | Microsoft Docs
description: Apprendre à créer rapidement un groupe de machines virtuelles identiques dans le portail Azure
keywords: Jeux de mise à l’échelle de machine virtuelle
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: quickstart
ms.custom: H1Hack27Feb2017
ms.date: 03/27/18
ms.author: cynthn
ms.openlocfilehash: 7644940418b7322e5ec4b02b793219b44ae3aa97
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54885147"
---
# <a name="quickstart-create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Démarrage rapide : Créer un groupe de machines virtuelles identiques dans le portail Azure
Un groupe de machines virtuelles identiques vous permet de déployer et de gérer un ensemble de machines virtuelles identiques prenant en charge la mise à l’échelle automatique. Vous pouvez mettre à l’échelle manuellement le nombre de machines virtuelles du groupe identique ou définir des règles de mise à l’échelle automatique en fonction de l’utilisation des ressources telles que l’UC, la demande de mémoire ou le trafic réseau. Un équilibreur de charge Azure distribue ensuite le trafic vers les instances de machine virtuelle du groupe identique. Dans ce guide de démarrage rapide, vous créez un groupe de machines virtuelles identiques dans le portail Azure.

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.


## <a name="log-in-to-azure"></a>Connexion à Azure
Connectez-vous au portail Azure sur http://portal.azure.com.


## <a name="create-virtual-machine-scale-set"></a>Créer un groupe de machines virtuelles identiques
Vous pouvez déployer un groupe identique avec une image Windows Server ou une image Linux, comme RHEL, CentOS, Ubuntu ou SLES.

1. Cliquez sur **Créer une ressource** en haut à gauche du portail Azure.
2. Recherchez *groupe identique*, choisissez **Groupe de machines virtuelles identiques**, puis sélectionnez **Créer**.
3. Entrez un nom pour le groupe identique, par exemple, *myScaleSet*.
4. Sélectionnez le type de système d’exploitation approprié, par exemple, *Windows Server 2016 Datacenter*.
5. Entrez le nom souhaité pour votre groupe de ressources (par exemple, *myResourceGroup*) ainsi que son emplacement (par exemple, *USA Est*).
6. Entrez le nom d’utilisateur de votre choix, puis sélectionnez le type d’authentification que vous préférez.
    - Un **mot de passe** doit comporter au moins 12 caractères, avec au moins trois des quatre caractères suivants : une minuscule, une majuscule, un chiffre et un caractère spécial. Pour plus d’informations, consultez les [critères de nom d’utilisateur et de mot de passe](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).
    - Si vous sélectionnez une image de disque du système d’exploitation Linux, vous pouvez choisir à la place **Clé publique SSH**. Entrez uniquement votre clé publique, comme *~/.ssh/id_rsa.pub*. Vous pouvez utiliser Azure Cloud Shell à partir du portail pour [créer et utiliser des clés SSH](../virtual-machines/linux/mac-create-ssh-keys.md).

7. Entrez un **nom d’adresse IP publique**, tel que *myPublicIP*.
8. Entrez une valeur unique dans **Étiquette du nom de domaine** , par exemple, *myuniquedns*. Cette étiquette DNS constitue la base du nom de domaine complet (FQDN) pour l’équilibreur de charge placé devant le groupe identique.
9. Pour confirmer les options du groupe identique, sélectionnez **Créer**.

    ![Créer un groupe de machines virtuelles identiques dans le portail Azure](./media/virtual-machine-scale-sets-create-portal/create-scale-set.png)


## <a name="connect-to-a-vm-in-the-scale-set"></a>Se connecter à une machine virtuelle dans le jeu de mise à l’échelle
Quand vous créez un groupe identique dans le portail, un équilibreur de charge est créé. Les règles de traduction d’adresses réseau (NAT) permettent de distribuer le trafic sur les instances du groupe identique pour la connectivité à distance comme RDP ou SSH.

Pour afficher ces règles NAT et les informations de connexion des instances du groupe identique :

1. Sélectionnez le groupe de ressources que vous avez créé à l’étape précédente (par exemple, *myResourceGroup*).
2. Dans la liste des ressources, sélectionnez votre **équilibreur de charge**, tel que *myScaleSetLab*.
3. Choisissez **Règles NAT de trafic entrant** dans le menu à gauche de la fenêtre.

    ![Les règles NAT de trafic entrant vous permettent de vous connecter aux instances du groupe de machines virtuelles identiques](./media/virtual-machine-scale-sets-create-portal/inbound-nat-rules.png)

Vous pouvez vous connecter à chaque machine virtuelle dans le jeu de mise à l’échelle en utilisant des règles NAT. Chaque instance de machine virtuelle affiche une adresse IP de destination et une valeur de port TCP. Par exemple, si l’adresse IP de destination est *104.42.1.19* et que le port TCP est *50001*, vous vous connectez à l’instance de machine virtuelle comme ceci :

- Pour un groupe identique Windows, connectez-vous à l’instance de machine virtuelle avec RDP sur `104.42.1.19:50001`
- Pour un groupe identique Linux, connectez-vous à l’instance de machine virtuelle avec SSH sur `ssh azureuser@104.42.1.19 -p 50001`

Quand vous y êtes invité, entrez les informations d’identification que vous avez spécifiées à l’étape précédente lors de la création du groupe identique. Les instances du groupe identique sont des machines virtuelles standard qui s’utilisent normalement. Pour plus d’informations sur le déploiement et l’exécution d’applications sur les instances d’un groupe identique, consultez [Déployer votre application sur des groupes de machines virtuelles identiques](virtual-machine-scale-sets-deploy-app.md)


## <a name="clean-up-resources"></a>Supprimer des ressources
Quand vous n’en avez plus besoin, supprimez le groupe de ressources, le groupe identique et toutes les ressources associées. Pour ce faire, sélectionnez le groupe de ressources de la machine virtuelle et cliquez sur **Supprimer**.


## <a name="next-steps"></a>Étapes suivantes
Dans ce guide de démarrage rapide, vous avez créé un groupe de machines virtuelles identiques dans le portail Azure. Pour en savoir plus, passez au didacticiel pour savoir comment créer et gérer des machines virtuelles identiques Azure.

> [!div class="nextstepaction"]
> [Créer et gérer des groupes de machines virtuelles identiques Azure](tutorial-create-and-manage-powershell.md)