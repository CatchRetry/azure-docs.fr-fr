---
title: Meilleures pratiques de l’opérateur – Connectivité réseau dans Azure Kubernetes Service (AKS)
description: Découvrez les meilleures pratiques de l’opérateur pour les ressources de réseau virtuel et la connectivité dans Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: iainfou
ms.openlocfilehash: 0ad6ab27a51cf082be71262b887a459f6c7cc906
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54101970"
---
# <a name="best-practices-for-network-connectivity-and-security-in-azure-kubernetes-service-aks"></a>Meilleures pratiques pour la connectivité réseau et la sécurité dans Azure Kubernetes Service (AKS)

Lorsque vous créez et gérez des clusters dans Azure Kubernetes Service (AKS), vous assurez une connectivité réseau à vos nœuds et à vos applications. Parmi ces ressources réseau figurent des plages d’adresses IP, des équilibreurs de charge et des contrôleurs d’entrée. Pour maintenir une qualité de service élevée dans les applications, il faut planifier, puis configurer ces ressources.

Cet article porte sur les meilleures pratiques en matière de connectivité réseau et de sécurité pour les opérateurs de clusters. Dans cet article, vous apprendrez comment :

> [!div class="checklist"]
> * comparer le mode réseau de base et le mode réseau avancé dans AKS ;
> * prévoir l’adressage IP et la connectivité nécessaires ;
> * distribuer le trafic à l’aide d’équilibreurs de charge, de contrôleurs d’entrée ou de pare-feu d’applications web ;
> * se connecter en toute sécurité aux nœuds de cluster.

## <a name="choose-the-appropriate-network-model"></a>Choisir le modèle réseau adapté

**Meilleures pratiques** : pour une intégration avec des réseaux virtuels existants ou des réseaux locaux, utilisez la mise en réseau avancée dans AKS. Ce modèle réseau offre également une meilleure séparation des ressources et plus de contrôle dans un environnement d’entreprise.

Les réseaux virtuels assurent une connectivité de base pour les nœuds AKS et les clients qui accèdent à vos applications. Il existe deux façons différentes de déployer des clusters AKS dans des réseaux virtuels :

* **Mise en réseau de base** : Azure gère les ressources de réseau virtuel pendant le déploiement du cluster et utilise le plug-in Kubernetes [KubeNet][kubenet].
* **Mise en réseau avancée** : le déploiement se fait dans un réseau virtuel existant, avec le plug-in Kubernetes [Azure Container Networking Interface (CNI)][cni-networking]. Les pods reçoivent des adresses IP individuelles qui peuvent communiquer avec d’autres services réseau ou ressources locales.

L’interface Azure CNI est un protocole indépendant du fournisseur qui permet au runtime du conteneur d’adresser des demandes à un fournisseur de réseau. Elle affecte des adresses IP aux pods et aux nœuds et offre des fonctionnalités de Gestion des adresses IP (IPAM) pour la connexion à des réseaux virtuels Azure existants. Chaque ressource de type nœud ou pod reçoit une adresse IP dans le réseau virtuel Azure, sans qu’aucun routage supplémentaire soit nécessaire pour communiquer avec d’autres ressources ou services.

![Diagramme représentant 2 nœuds avec des ponts les reliant chacun à un réseau virtuel Azure](media/operator-best-practices-network/advanced-networking-diagram.png)

La mise en réseau avancée est recommandée dans la plupart des déploiements de production. Ce modèle de réseau permet de séparer le contrôle et la gestion des ressources. Du point de vue de la sécurité, il est souvent préférable que différentes équipes gèrent et sécurisent ces ressources. La mise en réseau avancée permet de se connecter directement à des ressources Azure existantes, à des ressources locales ou à d’autres services avec les adresses IP affectées à chacun des pods.

Avec la mise en réseau avancée, la ressource de réseau virtuel se trouve dans un groupe de ressources distinct du cluster AKS. Déléguez des permissions au principal de service AKS pour qu’il puisse accéder à ces ressources et les gérer. Le principal du service utilisé par le cluster AKS doit disposer au moins des autorisations [Contributeur de réseau](../role-based-access-control/built-in-roles.md#network-contributor) sur le sous-réseau de votre réseau virtuel. Si vous souhaitez définir un [rôle personnalisé](../role-based-access-control/custom-roles.md) au lieu d’utiliser le rôle de contributeur de réseau intégré, les autorisations suivantes sont nécessaires :
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

Pour plus d’informations sur la délégation du principal du service AKS, voir [Déléguer l’accès à d’autres ressources Azure][sp-delegation].

Dans la mesure où chaque nœud et chaque pod reçoit sa propre adresse IP, planifiez les plages d’adresses des sous-réseaux AKS. Le sous-réseau doit être assez grand pour offrir une adresse IP à chacun des nœuds, pods et ressources réseau déployés. Chaque cluster AKS doit être placé dans son propre sous-réseau. Pour autoriser la connexion à des réseaux locaux ou en peering dans Azure, n’utilisez pas de plages d’adresses IP qui recouvrent des ressources réseau existantes. Des limites par défaut s’appliquent au nombre de pods qu’exécute chaque nœud avec la mise en réseau de base et avancée. Pour gérer les événements de montée en puissance et les mises à niveau de cluster, des adresses IP supplémentaires sont également nécessaires dans le sous-réseau attribué.

Pour calculer l’adresse IP requise, voir [Configurer la mise en réseau avancée dans AKS][advanced-networking].

### <a name="basic-networking-with-kubenet"></a>Mise en réseau de base avec KubeNet

Même si la mise en réseau de base n’impose pas de configurer les réseaux virtuels avant le déploiement du cluster, elle présente des inconvénients :

* Les nœuds et les pods sont placés sur différents sous-réseaux IP. Le routage défini par l’utilisateur (UDR) et le transfert IP sont utilisés pour acheminer le trafic entre les nœuds et les pods. Ce routage supplémentaire réduit les performances du réseau.
* Les connexions à des réseaux locaux existants et le peering avec d’autres réseaux virtuels Azure sont complexes.

La mise en réseau de base convient aux petites charges de travail développement et de test, dans la mesure où il n’est pas nécessaire de créer le réseau virtuel et les sous-réseaux séparément du cluster AKS. Les sites web simples recevant peu de trafic et le lift-and-shift de charges de travail dans des conteneurs peuvent également tirer avantage de la simplicité des clusters AKS déployés avec la mise en réseau de base. La mise en réseau avancée est planifiée et utilisée dans la plupart des déploiements de production.

## <a name="distribute-ingress-traffic"></a>Distribuer le trafic d’entrée

**Meilleures pratiques** : pour répartir le trafic HTTP ou HTTPS sur vos applications, utilisez des contrôleurs et des ressources d’entrée. Les contrôleurs d’entrée offrent des fonctionnalités supplémentaires par rapport à un équilibreur de charge Azure standard. Ils peuvent être gérés comme des ressources Kubernetes natives.

Un équilibreur de charge Azure peut distribuer le trafic de client aux applications du cluster AKS, mais sa compréhension du trafic reste limitée. Une ressource d’équilibrage de charge, qui agit au niveau de la couche 4, répartit le trafic en fonction du protocole ou des ports. La plupart des applications web qui utilisent HTTP ou HTTPS doivent de préférence utiliser des contrôleurs et des ressources d’entrée Kubernetes, qui fonctionnent au niveau de la couche 7. L’entrée peut distribuer le trafic en fonction de l’URL de l’application et gérer l’arrêt TLS/SSL. Cette capacité réduit également le nombre d’adresses IP exposées et mappées. Avec un équilibreur de charge, une adresse IP publique doit généralement être affectée et mappée au service dans le cluster AKS pour chaque application. Avec une ressource d’entrée, une seule adresse IP peut répartir le trafic entre plusieurs applications.

![Diagramme montrant le flux de trafic d’entrée dans un cluster AKS](media/operator-best-practices-network/aks-ingress.png)

 Il existe deux composants d’entrée :

 * une *ressource* d’entrée ;
 * un *contrôleur* d’entrée.

La ressource d’entrée est un manifeste YAML de `kind: Ingress` qui définit l’hôte, les certificats et les règles de routage du trafic vers les services qui s’exécutent dans le cluster AKS. L’exemple de manifeste YAML suivant distribuerait le trafic de *myapp.com* à un des deux services, *blogservice* ou *storeservice*. Le client est dirigé vers l’un ou l’autre selon l’URL consultée.

```yaml
kind: Ingress
metadata:
 name: myapp-ingress
   annotations: kubernetes.io/ingress.class: "PublicIngress"
spec:
 tls:
 - hosts:
   - myapp.com
   secretName: myapp-secret
 rules:
   - host: myapp.com
     http:
      paths:
      - path: /blog
        backend:
         serviceName: blogservice
         servicePort: 80
      - path: /store
        backend:
         serviceName: storeservice
         servicePort: 80
```

Un contrôleur d’entrée est un démon qui s’exécute sur un nœud AKS et surveille les demandes entrantes. Le trafic est ensuite distribué selon les règles définies dans la ressource d’entrée. Le contrôleur d’entrée le plus courant est basé sur [NGINX], mais AKS ne limite pas à un contrôleur spécifique : vous pouvez utiliser d’autres contrôleurs comme [Contour][contour], [HAProxy][haproxy] ou [Traefik][traefik].

Il existe de nombreux scénarios pour l’entrée, notamment ceux des guides pratiques suivants :

* [Créer un contrôleur d’entrée de base avec une connectivité réseau externe][aks-ingress-basic]
* [Créer un contrôleur d’entrée qui utilise un réseau privé interne et une adresse IP][aks-ingress-internal]
* [Créer un contrôleur d’entrée qui utilise vos propres certificats TLS][aks-ingress-own-tls]
* Créer un contrôleur d’entrée qui utilise Let’s Encrypt pour générer automatiquement des certificats TLS [avec une adresse IP publique dynamique][aks-ingress-tls] ou [avec une adresse IP publique statique][aks-ingress-static-tls]

## <a name="secure-traffic-with-a-web-application-firewall-waf"></a>Sécuriser le trafic avec un pare-feu d’applications web (WAF)

**Meilleures pratiques** : pour analyser le risque d’attaques dans le trafic, utilisez un pare-feu d’applications web (WAF) comme [Barracuda pour Azure][barracuda-waf] ou Azure Application Gateway. Ces ressources réseau plus avancées peuvent également acheminer le trafic au-delà des connexions HTTP et HTTPS et de l’arrêt SSL de base.

Un contrôleur d’entrée qui distribue le trafic aux services et applications est généralement une ressource Kubernetes du cluster AKS. Le contrôleur s’exécute en tant que démon sur un nœud AKS et consomme des ressources du nœud, comme le processeur, la mémoire et la bande passante réseau. Dans les environnements de grande taille, il est souvent nécessaire de décharger une partie du routage du trafic ou de l’arrêt TLS sur une ressource réseau située en dehors du cluster AKS. L’objectif est également d’analyser le risque d’attaques dans le trafic entrant.

![Un pare-feu d’applications web (WAF) comme Azure Application Gateway peut protéger et distribuer le trafic d’un cluster AKS](media/operator-best-practices-network/web-application-firewall-app-gateway.png)

Un pare-feu d’applications web (WAF) ajoute une couche de sécurité supplémentaire en filtrant le trafic entrant. Le projet OWASP (Open Web Application Security Project) propose un ensemble de règles pour surveiller les attaques de type script intersites ou cookie poisoning. [Azure Application Gateway][app-gateway] est un pare-feu WAF capable d’intégrer ces fonctionnalités de sécurité à des clusters AKS, avant que le trafic n’atteigne les clusters et les applications. D’autres solutions tierces assurent également ces fonctions, ce qui peut permettre de continuer à utiliser les investissements et l’expertise déjà acquis sur un produit donné.

Les ressources d’équilibrage de charge ou d’entrée continuent de s’exécuter dans le cluster AKS pour affiner davantage la distribution du trafic. App Gateway peut être géré de manière centralisée comme contrôleur d’entrée avec une définition de ressource. Pour commencer, voir [Créer un contrôleur d’entrée Application Gateway][app-gateway-ingress].

## <a name="securely-connect-to-nodes-through-a-bastion-host"></a>Se connecter en toute sécurité aux nœuds à travers un hôte bastion

**Meilleures pratiques** : n’exposez pas de connectivité à distance sur vos nœuds AKS. Créez un hôte bastion, ou une jumpbox, dans un réseau virtuel de gestion. Utilisez l’hôte bastion pour acheminer le trafic en toute sécurité dans votre cluster AKS aux tâches de gestion à distance.

La plupart des opérations effectuées dans AKS peuvent exploiter des outils de gestion Azure ou le serveur d’API Kubernetes. Les nœuds AKS, non connectés à l’Internet public, ne sont disponibles que sur un réseau privé. Pour vous connecter aux nœuds et effectuer des tâches de maintenance ou résoudre des problèmes, acheminez vos connexions à travers un hôte bastion ou une jumpbox. Cet hôte doit se trouver dans un réseau virtuel de gestion distinct, qui offre un peering sécurisé avec le réseau virtuel du cluster AKS.

![Se connecter à des nœuds AKS à l’aide d’un hôte bastion ou d’une jumpbox](media/operator-best-practices-network/connect-using-bastion-host-simplified.png)

Le réseau de gestion de l’hôte bastion doit lui aussi être sécurisé. Utilisez [Azure ExpressRoute][expressroute] ou une [passerelle VPN][vpn-gateway] pour vous connecter à un réseau local et contrôler l’accès avec des groupes de sécurité réseau.

## <a name="next-steps"></a>Étapes suivantes

Cet article porte sur la sécurité et la connectivité réseau. Pour plus d’informations sur les principes fondamentaux des réseaux dans Kubernetes, voir [Concepts relatifs au réseau des applications dans Azure Kubernetes Service (AKS)][aks-concepts-network].

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[app-gateway-ingress]: https://github.com/Azure/application-gateway-kubernetes-ingress
[nginx]: https://www.nginx.com/products/nginx/kubernetes-ingress-controller
[contour]: https://github.com/heptio/contour
[haproxy]: https://www.haproxy.org
[traefik]: https://github.com/containous/traefik
[barracuda-waf]: https://www.barracuda.com/products/webapplicationfirewall/models/5

<!-- INTERNAL LINKS -->
[aks-concepts-network]: concepts-network.md
[sp-delegation]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[expressroute]: ../expressroute/expressroute-introduction.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-own-tls]: ingress-own-tls.md
[app-gateway]: ../application-gateway/overview.md
[advanced-networking]: configure-advanced-networking.md
