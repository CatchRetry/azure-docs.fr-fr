---
title: Inscription Azure pour systèmes intégrés Azure Stack | Microsoft Docs
description: Décrit le processus d’inscription Azure pour les déploiements à plusieurs nœuds d’Azure Stack connectés à Azure.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2019
ms.author: jeffgilb
ms.reviewer: brbartle
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: d9ab89afba2b83f99bfbf432d033cd0546a25a9d
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247389"
---
# <a name="register-azure-stack-with-azure"></a>Inscrire Azure Stack auprès d’Azure

L’inscription d’Azure Stack auprès d’Azure permet de télécharger des éléments de la Place de marché à partir d’Azure et de configurer la génération de rapports de données commerciales envoyés à Microsoft. Après avoir enregistré Azure Stack, l’utilisation est signalée au commerce Azure et vous pouvez le voir dans l’abonnement utilisé pour l’inscription.

Les informations contenues dans cet article décrivent l’inscription de systèmes intégrés Azure Stack auprès d’Azure. Pour plus d’informations sur l’inscription du kit ASDK auprès d’Azure, consultez [Inscription d’Azure Stack](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-register) dans la documentation ASDK.

> [!IMPORTANT]  
> L’inscription est obligatoire pour prendre en charge les fonctionnalités complètes d’Azure Stack, notamment pour proposer les éléments sur la Place de marché. En outre, vous enfreindrez les conditions du contrat de licence d’Azure Stack si vous ne vous inscrivez pas lorsque vous utilisez le modèle de facturation à l’utilisation. Pour en savoir plus sur les modèles de licence d’Azure Stack, consultez la [page Comment acheter](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Prérequis

Vous devez mettre en place les éléments suivants avant l’inscription :

 - Vérifier vos informations d’identification
 - Définir le mode de langage PowerShell
 - Installer PowerShell pour Azure Stack
 - Télécharger les outils Azure Stack
 - Déterminer votre scénario d’inscription

### <a name="verify-your-credentials"></a>Vérifier vos informations d’identification

Avant d’inscrire Azure Stack auprès d’Azure, vous devez disposer des éléments suivants :

- L’ID d’abonnement d’un abonnement Azure. L’inscription est prise en charge uniquement pour les abonnements aux services partagés EA, CSP et CSP. Les CSP ont le choix d’[utiliser un abonnement CSP ou APSS](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription).<br><br>Pour obtenir l’ID, connectez-vous à Azure et cliquez sur **Tous les services**. Ensuite, sous la catégorie **GÉNÉRAL**, sélectionnez **Abonnements** et cliquez sur l’abonnement que vous voulez utiliser. Sous **Éléments principaux**, vous trouverez alors l’ID d’abonnement.

  > [!Note]  
  > Les abonnements au cloud en Allemagne ne sont actuellement pas pris en charge.

- Le nom d’utilisateur et le mot de passe d’un compte propriétaire de l’abonnement.

- Le compte d’utilisateur doit avoir accès à l’abonnement Azure, et avoir les autorisations nécessaires pour créer des applications avec une identité et des principaux de service dans le répertoire associé à cet abonnement. Nous vous recommandons d’inscrire Azure Stack auprès d’Azure avec une administration selon le principe des privilèges minimum en [créant un compte de service à utiliser pour l’inscription](azure-stack-registration-role.md), au lieu d’utiliser les informations d’identification de l’administrateur général.

- Le fournisseur de ressources Azure Stack inscrit (pour plus d’informations, consultez la section Inscrire le fournisseur de ressources Azure Stack ci-dessous).

Après l’inscription, l’autorisation d’administrateur général Azure Active Directory n’est pas nécessaire. Toutefois, certaines opérations peuvent demander des informations d’identification d’administrateur général. Par exemple, un script d’installation d’un fournisseur de ressources ou une nouvelle fonctionnalité peut avoir besoin d’une autorisation spécifique. Vous pouvez temporairement réactiver les autorisations d’administrateur général du compte ou utiliser un compte d’administrateur général distinct qui est propriétaire de *l’abonnement fournisseur par défaut*.

Si vous n’avez pas d’abonnement Azure répondant à ces exigences, vous pouvez [créer un compte Azure gratuit ici](https://azure.microsoft.com/free/?b=17.06). L’inscription d’Azure Stack n’entraîne aucun frais sur votre abonnement Azure.

### <a name="powershell-language-mode"></a>Mode de langage PowerShell

Pour inscrire correctement Azure Stack, le mode de langage PowerShell doit être défini sur **FullLanguageMode**.  Pour vérifier que le mode de langage actuel est défini sur full, ouvrez une fenêtre PowerShell avec élévation de privilèges et exécutez les applets de commande PowerShell suivantes :

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Vérifiez que la sortie retourne **FullLanguageMode**. Si tout autre mode de langage est retourné, vous devez effectuer l’inscription sur une autre machine ou bien définir le mode de langage sur **FullLanguageMode** avant de continuer.

### <a name="install-powershell-for-azure-stack"></a>Installer PowerShell pour Azure Stack

Utilisez la dernière version de PowerShell pour Azure Stack pour effectuer l’inscription auprès d’Azure.

Si la dernière version n’est pas déjà installée, voir [Installer PowerShell pour Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).

### <a name="download-the-azure-stack-tools"></a>Télécharger les outils Azure Stack

Le référentiel GitHub d’outils Azure Stack contient des modules PowerShell qui prennent en charge les fonctionnalités Azure Stack, notamment des fonctionnalités d’inscription. Pendant le processus d’inscription, vous devez importer et utiliser le module PowerShell **RegisterWithAzure.psm1**, qui se trouve dans le référentiel d’outils Azure Stack, pour inscrire votre instance Azure Stack auprès d’Azure.

Pour garantir que vous utilisez la version la plus récente, vous devez supprimer toutes les versions existantes des outils Azure Stack et [télécharger la dernière version à partir de GitHub](azure-stack-powershell-download.md) avant l’inscription auprès de Azure.

### <a name="determine-your-registration-scenario"></a>Déterminer votre scénario d’inscription

Votre déploiement Azure Stack peut être *connecté* ou *déconnecté*.

 - **Connecté**  
 Connecté signifie que vous avez déployé Azure Stack afin qu’il puisse se connecter à Internet et à Azure. Vous disposez soit d’Azure Active Directory (Azure AD) ou d’Active Directory Federation Services (AD FS) pour votre magasin d’identités. Avec un déploiement connecté, vous pouvez choisir entre deux modèles de facturation : paiement à l’utilisation ou selon la capacité.
    - [Inscrire un déploiement Azure Stack connecté auprès d’Azure à l’aide du modèle de facturation de **paiement à l’utilisation**](#register-connected-with-pay-as-you-go-billing)
    - [Inscrire un déploiement Azure Stack connecté auprès d’Azure à l’aide du modèle de facturation de **capacité**](#register-connected-with-capacity-billing)

 - **Déconnecté**  
 Avec l’option de déploiement déconnecté de Azure, vous pouvez déployer et utiliser Azure Stack sans connexion à internet. Toutefois, avec un déploiement déconnecté, vous êtes limité à un magasin d’identités AD FS et au modèle de facturation basée sur la capacité.
    - [Inscrire un déploiement Azure Stack déconnecté à l’aide du modèle de facturation de **capacité**](#register-disconnected-with-capacity-billing)

### <a name="determine-a-unique-registration-name-to-use"></a>Déterminer le nom d’inscription unique à utiliser 
Quand vous inscrivez Azure Stack sur Azure, vous devez fournir un nom d’inscription unique. Un moyen simple d’associer votre abonnement Azure Stack avec une inscription Azure est d’utiliser votre **ID cloud** Azure Stack. 

> [!NOTE]
> Pour les inscriptions Azure Stack basées sur le modèle de facturation par capacité, vous devez changer le nom unique lors de la réinscription après l’expiration des abonnements annuels, sauf si vous [supprimez l’inscription expirée](azure-stack-registration.md#change-the-subscription-you-use) et que vous vous réinscrivez à Azure.

Pour déterminer l’ID de cloud associé à votre déploiement Azure Stack, ouvrez PowerShell en tant qu’administrateur sur un ordinateur qui a accès au point de terminaison privilégié, exécutez les commandes suivantes et notez la valeur de **CloudID** : 

```powershell
Run: Enter-PSSession -ComputerName <privileged endpoint computer name> -ConfigurationName PrivilegedEndpoint
Run: get-azurestackstampinformation 
```

## <a name="register-connected-with-pay-as-you-go-billing"></a>Inscrire un déploiement connecté avec facturation de paiement à l’utilisation

Utilisez ces étapes pour inscrire Azure Stack auprès d’Azure à l’aide du modèle de facturation de paiement à l’utilisation.

> [!Note]  
> Toutes ces étapes doivent être exécutées à partir d’un ordinateur qui a accès au point de terminaison privilégié (PEP). Pour plus de détails sur le PEP, voir [Utilisation du point de terminaison privilégié dans Azure Stack](azure-stack-privileged-endpoint.md).

Les environnements connectés peuvent accéder à Internet et à Azure. Pour ces environnements, vous devez inscrire le fournisseur de ressources Azure Stack auprès d’Azure, puis configurer votre modèle de facturation.

1. Pour inscrire le fournisseur de ressources Azure Stack auprès d’Azure, démarrez PowerShell ISE en tant qu’administrateur et utilisez les applets de commande PowerShell suivantes avec le paramètre **EnvironmentName** défini sur le type d’abonnement Azure approprié (voir les paramètres ci-dessous).

2. Ajoutez le compte Azure que vous utilisez pour inscrire Azure Stack. Pour ajouter le compte, exécutez la cmdlet **Add-AzureRmAccount**. Vous êtes invité à entrer vos informations d’identification de compte Azure et vous devrez peut-être utiliser l’authentification à 2 facteurs en fonction de la configuration de votre compte.

   ```PowerShell
      Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Paramètre | Description |  
   |-----|-----|
   | EnvironmentName | Le nom de l’environnement d’abonnement cloud Azure. Les noms d’environnement pris en charge sont **AzureCloud**, **AzureUSGovernment**, ou, si vous utilisez un abonnement Azure en Chine, **AzureChinaCloud**.  |

3. Si vous avez plusieurs abonnements, exécutez la commande suivante pour sélectionner celui que vous souhaitez utiliser :  

   ```PowerShell  
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Exécutez la commande suivante pour inscrire le fournisseur de ressources Azure Stack dans votre abonnement Azure :

   ```PowerShell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Démarrez PowerShell ISE en tant qu’administrateur et accédez au dossier **Registration** dans le répertoire **AzureStack-Tools-master** créé lorsque vous avez [téléchargé les outils Azure Stack](#bkmk_tools). Importez le module **RegisterWithAzure.psm1** à l’aide de PowerShell :

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

6. Ensuite, dans la même session PowerShell, vérifiez que vous êtes connecté au contexte Azure PowerShell approprié. Il s’agit du compte Azure qui a été utilisé précédemment pour inscrire le fournisseur de ressources Azure Stack. PowerShell pour :

   ```PowerShell  
      Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Paramètre | Description |  
   |-----|-----|
   | EnvironmentName | Le nom de l’environnement d’abonnement cloud Azure. Les noms d’environnement pris en charge sont **AzureCloud**, **AzureUSGovernment**, ou, si vous utilisez un abonnement Azure en Chine, **AzureChinaCloud**.  |

7. Dans la même session PowerShell, exécutez l’applet de commande **Set-AzsRegistration**. PowerShell pour :  

   ```PowerShell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse `
      -RegistrationName $RegistrationName
   ```
   Pour plus d’informations sur l’applet de commande Set-AzsRegistration, voir [Référence de l’inscription](#registration-reference).

  Le processus prend entre 10 et 15 minutes. Quand la commande est terminée, le message **« Votre environnement est à présent inscrit et activé avec les paramètres fournis. »** s’affiche.

## <a name="register-connected-with-capacity-billing"></a>Inscrire un déploiement connecté avec facturation selon la capacité

Utilisez ces étapes pour inscrire Azure Stack auprès d’Azure à l’aide du modèle de facturation de paiement à l’utilisation.

> [!Note]  
> Toutes ces étapes doivent être exécutées à partir d’un ordinateur qui a accès au point de terminaison privilégié (PEP). Pour plus de détails sur le PEP, voir [Utilisation du point de terminaison privilégié dans Azure Stack](azure-stack-privileged-endpoint.md).

Les environnements connectés peuvent accéder à Internet et à Azure. Pour ces environnements, vous devez inscrire le fournisseur de ressources Azure Stack auprès d’Azure, puis configurer votre modèle de facturation.

1. Pour inscrire le fournisseur de ressources Azure Stack auprès d’Azure, démarrez PowerShell ISE en tant qu’administrateur et utilisez les applets de commande PowerShell suivantes avec le paramètre **EnvironmentName** défini sur le type d’abonnement Azure approprié (voir les paramètres ci-dessous).

2. Ajoutez le compte Azure que vous utilisez pour inscrire Azure Stack. Pour ajouter le compte, exécutez la cmdlet **Add-AzureRmAccount**. Vous êtes invité à entrer vos informations d’identification de compte Azure et vous devrez peut-être utiliser l’authentification à 2 facteurs en fonction de la configuration de votre compte.

   ```PowerShell  
      Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Paramètre | Description |  
   |-----|-----|
   | EnvironmentName | Le nom de l’environnement d’abonnement cloud Azure. Les noms d’environnement pris en charge sont **AzureCloud**, **AzureUSGovernment**, ou, si vous utilisez un abonnement Azure en Chine, **AzureChinaCloud**.  |

3. Si vous avez plusieurs abonnements, exécutez la commande suivante pour sélectionner celui que vous souhaitez utiliser :  

   ```PowerShell  
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Exécutez la commande suivante pour inscrire le fournisseur de ressources Azure Stack dans votre abonnement Azure :

   ```PowerShell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Démarrez PowerShell ISE en tant qu’administrateur et accédez au dossier **Registration** dans le répertoire **AzureStack-Tools-master** créé lorsque vous avez [téléchargé les outils Azure Stack](#bkmk_tools). Importez le module **RegisterWithAzure.psm1** à l’aide de PowerShell :

  ```PowerShell  
  $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
  $RegistrationName = "<unique-registration-name>"
  Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -AgreementNumber <EA agreement number> `
      -BillingModel Capacity `
      -RegistrationName $RegistrationName
  ```
   > [!Note]  
   > Vous pouvez désactiver la création de rapports avec le paramètre UsageReportingEnabled pour l’applet de commande **Set-AzsRegistration** en définissant le paramètre avec la valeur false. 
   
  Pour plus d’informations sur l’applet de commande Set-AzsRegistration, voir [Référence de l’inscription](#registration-reference).

## <a name="register-disconnected-with-capacity-billing"></a>Inscrire un déploiement déconnecté avec facturation selon la capacité

Si vous inscrivez Azure Stack dans un environnement déconnecté (sans connectivité Internet), vous devez obtenir un jeton d’inscription à partir de l’environnement d’Azure Stack, puis l’utiliser sur un ordinateur qui peut se connecter à Azure et sur lequel [PowerShell pour Azure Stack est installé](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Obtenir un jeton d’inscription à partir de l’environnement d’Azure Stack

1. Démarrez PowerShell ISE en tant qu’administrateur et accédez au dossier **Registration** dans le répertoire **AzureStack-Tools-master** créé lorsque vous avez [téléchargé les outils Azure Stack](#bkmk_tools). Importez le module **RegisterWithAzure.psm1** :  

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Pour obtenir le jeton d’inscription, exécutez les applets de commande PowerShell suivantes :  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -UsageReportingEnabled:$False -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```
   Pour plus d’informations sur l’applet de commande Get-AzsRegistrationToken, voir [Référence de l’inscription](#registration-reference).

   > [!Tip]  
   > Le jeton d’inscription est enregistré dans le fichier spécifié pour *$FilePathForRegistrationToken*. Vous pouvez modifier à votre convenance le nom ou le chemin d’accès au fichier.

3. Enregistrez ce jeton d’inscription pour l’utiliser sur la machine connectée à Azure. Vous pouvez copier le fichier ou le texte à partir de $FilePathForRegistrationToken.

### <a name="connect-to-azure-and-register"></a>Se connecter à Azure et s’inscrire

Sur l’ordinateur qui est connecté à Internet, effectuez les mêmes étapes pour importer le module RegisterWithAzure.psm1 et vous connecter au bon contexte Azure Powershell. Appelez ensuite Register-AzsEnvironment. Spécifiez le jeton d’inscription pour vous inscrire auprès d’Azure. Si vous inscrivez plusieurs instances Azure Stack en utilisant le même ID d’abonnement Azure, spécifiez un nom d’inscription unique. Exécutez l’applet de commande suivante :

  ```PowerShell  
  $registrationToken = "<Your Registration Token>"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $registrationToken  -RegistrationName $RegistrationName
  ```

Si vous le souhaitez, vous pouvez utiliser l’applet de commande Get-Content pour pointer vers un fichier contenant votre jeton d’inscription :

  ```PowerShell  
  $registrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $registrationToken -RegistrationName $RegistrationName
  ```

  > [!Note]  
  > Enregistrez le nom de la ressource d’inscription et le jeton d’inscription pour une référence ultérieure.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Récupérer une clé d’activation auprès d’Azure Registration Resource

Vous devez ensuite extraire une clé d’activation auprès de la ressource d’inscription créée dans Azure pendant Register-AzsEnvironment.

Pour obtenir la clé d’activation, exécutez les applets de commande PowerShell suivantes :  

  ```Powershell
  $RegistrationResourceName = "<unique-registration-name>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!Tip]  
  > La clé d’activation est enregistrée dans le fichier spécifié pour *$KeyOutputFilePath*. Vous pouvez modifier à votre convenance le nom ou le chemin d’accès au fichier.

### <a name="create-an-activation-resource-in-azure-stack"></a>Créer une ressource d’activation dans Azure Stack

Revenez à l’environnement Azure Stack avec le fichier ou le texte obtenu à partir de la clé d’activation créée depuis Get-AzsActivationKey. Vous créez ensuite une ressource d’activation dans Azure Stack en utilisant cette clé d’activation. Pour créer une ressource d’activation, exécutez les applets de commande PowerShell suivantes :  

  ```Powershell
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

Si vous le souhaitez, vous pouvez utiliser l’applet de commande Get-Content pour pointer vers un fichier contenant votre jeton d’inscription :

  ```Powershell
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>'
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

## <a name="verify-azure-stack-registration"></a>Vérifier l’inscription Azure Stack

Vous pouvez utiliser la vignette **Gestion des régions** pour vérifier que l’inscription Azure Stack a réussi. Cette vignette est disponible sur le tableau de bord par défaut du portail d’administration. L’état peut être inscrit ou non. S’il est inscrit, il montre également l’ID d’abonnement Azure que vous avez utilisé pour inscrire votre instance Azure Stack ainsi que le groupe de ressources et le nom de l’inscription.

1. Connectez-vous au [portail d’administration Azure Stack](https://adminportal.local.azurestack.external).

2. Dans le tableau de bord, sélectionnez **Gestion des régions**.

3. Sélectionner **Propriétés**. Ce panneau affiche l’état et les détails de votre environnement. L’état peut être **Inscrit** ou **Non inscrit**.

    [ ![Vignette Gestion des régions](media/azure-stack-registration/admin1sm.png "Vignette Gestion des régions") ](media/azure-stack-registration/admin1.png#lightbox)

    Si l’état est Inscrit, les propriétés sont les suivantes :
    
    - **ID d’abonnement de l’inscription** : ID d’abonnement Azure inscrit et associé à Azure Stack
    - **Groupe de ressources de l’inscription** : Groupe de ressources Azure dans l’abonnement associé contenant les ressources Azure Stack.

4. Utilisez le portail Azure pour voir les inscriptions d’application Azure Stack. Connectez-vous au portail Azure avec un compte associé à l’abonnement que vous avez utilisé pour inscrire Azure Stack. Passez au locataire associé à Azure Stack.
5. Accédez à **Azure Active Directory > Inscriptions des applications > Afficher toutes les applications**.

    ![Inscriptions des applications](media/azure-stack-registration/app-registrations.png)

    Les inscriptions des applications Azure Stack sont préfixées de **Azure Stack**.

Vous pouvez également vérifier si votre inscription a réussi à l’aide de la fonctionnalité Gestion de la Place de marché. Si vous voyez une liste d’éléments de la Place de marché dans le panneau Gestion de la Place de marché, votre inscription a réussi. Toutefois, dans les environnements déconnectés, les éléments de la Place de marché n’apparaissent pas dans Gestion de la Place de marché. Dans ce cas, vous pouvez utiliser l’outil en mode hors connexion pour vérifier l’inscription.

> [!NOTE]
> Une fois l’inscription terminée, l’avertissement relatif à la non-inscription n’apparaît plus. Dans les scénarios déconnectés, vous recevez un message dans Gestion de la Place de marché vous invitant à inscrire et à activer votre instance Azure Stack, même si l’inscription a déjà été effectuée.

## <a name="renew-or-change-registration"></a>Renouveler ou modifier l’inscription

### <a name="renew-or-change-registration-in-connected-environments"></a>Renouveler ou de modifier l’inscription dans des environnements connectés

Vous devez mettre à jour ou renouveler votre inscription dans les cas suivants :

- Après le renouvellement de votre abonnement annuel basé sur la capacité.
- Lorsque vous changez de modèle de facturation.
- Lorsque vous mettez à l’échelle des modifications (ajout/suppression de nœuds) pour la facturation basée sur la capacité.

#### <a name="change-the-subscription-you-use"></a>Modifier l’abonnement que vous utilisez

Si vous souhaitez changer l’abonnement que vous utilisez, vous devez d’abord exécuter l’applet de commande **Remove-AzsRegistration**, vous assurer d’être connecté au contexte Azure PowerShell correct, puis exécuter **Set-AzsRegistration** avec les paramètres modifiés, y compris le \<modèle de facturation\> :

  ```PowerShell  
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
  ```

#### <a name="change-the-billing-model-or-how-to-offer-features"></a>Modifier le modèle de facturation ou la façon dont les fonctionnalités sont offertes

Si vous souhaitez modifier le modèle de facturation ou la façon dont les fonctionnalités sont offertes pour votre installation, vous pouvez appeler la fonction d’inscription pour définir les nouvelles valeurs. Vous n’avez pas besoin de supprimer l’inscription actuelle d’abord :

  ```PowerShell  
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Renouveler ou de modifier l’inscription dans des environnements déconnectés

Vous devez mettre à jour ou renouveler votre inscription dans les cas suivants :

- Après le renouvellement de votre abonnement annuel basé sur la capacité.
- Lorsque vous changez de modèle de facturation.
- Lorsque vous mettez à l’échelle des modifications (ajout/suppression de nœuds) pour la facturation basée sur la capacité.

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Supprimer la ressource d’activation d’Azure Stack

Vous devez d’abord supprimer la ressource d’activation d’Azure Stack, puis la ressource d’inscription dans Azure.  

Pour supprimer la ressource d’activation d’Azure Stack, exécutez les applets de commande PowerShell suivantes dans votre environnement Azure Stack :  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

Ensuite, pour supprimer la ressource d’inscription d’Azure, vérifiez que vous êtes sur un ordinateur connecté à Azure, connectez-vous au contexte Azure PowerShell correct et exécutez les applets de commande PowerShell appropriées, comme décrit ci-dessous.

Vous pouvez utiliser le jeton d’inscription utilisé pour créer la ressource :  

  ```Powershell
  $registrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $registrationToken
  ```

Vous pouvez également utiliser le nom d’inscription :

  ```Powershell
  $registrationName = "AzureStack-<unique-registration-name>"
  Unregister-AzsEnvironment -RegistrationName $registrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Effectuer une réinscription à l’aide des étapes déconnectées

Vous avez maintenant complètement annulé l’inscription dans un scénario déconnecté et devez répéter les étapes pour inscrire un environnement Azure Stack dans un scénario déconnecté.

### <a name="disable-or-enable-usage-reporting"></a>Désactiver ou activer les rapports d’utilisation

Pour les environnements Azure Stack qui utilisent un modèle de facturation selon la capacité, désactivez la création de rapports d’utilisation avec le paramètre **UsageReportingEnabled** en utilisant l’applet de commande **Set-AzsRegistration** ou  **Get-AzsRegistrationToken**. Azure Stack crée par défaut des rapports sur les métriques d’utilisation. Les opérateurs qui se basent sur la capacité ou qui gèrent un environnement déconnecté doivent désactiver la génération de rapports d’utilisation.

#### <a name="with-a-connected-azure-stack"></a>Avec un déploiement Azure Stack connecté

   ```PowerShell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel Capacity
      -RegistrationName $RegistrationName
   ```

#### <a name="with-a-disconnected-azure-stack"></a>Avec un déploiement Azure Stack déconnecté

1. Pour changer le jeton d’inscription, exécutez les applets de commande PowerShell suivantes :  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential -UsageReportingEnabled:$False
   $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!Tip]  
   > Le jeton d’inscription est enregistré dans le fichier spécifié pour *$FilePathForRegistrationToken*. Vous pouvez modifier à votre convenance le nom ou le chemin d’accès au fichier.

2. Enregistrez ce jeton d’inscription pour l’utiliser sur la machine connectée à Azure. Vous pouvez copier le fichier ou le texte à partir de $FilePathForRegistrationToken.

## <a name="move-a-registration-resource"></a>Déplacer une ressource d’inscription
Le déplacement d’une ressource d’inscription entre des groupes de ressources sous le même abonnement **est** pris en charge pour tous les environnements. Toutefois, le déplacement d’une ressource d’inscription entre abonnements est uniquement pris en charge pour les fournisseurs de services cloud quand les deux abonnements correspondent au même ID partenaire. Pour plus d’informations sur le déplacement de ressources vers un nouveau groupe de ressources, voir [Déplacer des ressources vers un nouveau groupe de ressource ou un nouvel abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).

## <a name="registration-reference"></a>Référence de l’inscription

### <a name="set-azsregistration"></a>Set-AzsRegistration

Vous pouvez utiliser Set-AzsRegistration pour inscrire Azure Stack auprès d’Azure et activer ou désactiver l’offre d’éléments sur la Place de marché et la création de rapports d’utilisation.

Pour exécuter l’applet de commande, vous avez besoin des éléments suivants :
- Un abonnement Azure global de n’importe quel type.
- Vous devez également être connecté à Azure PowerShell avec un compte propriétaire ou collaborateur de cet abonnement.

```PowerShell
    Set-AzsRegistration [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String> [[-AzureContext]
    <PSObject>] [[-ResourceGroupName] <String>] [[-ResourceGroupLocation] <String>] [[-BillingModel] <String>]
    [-MarketplaceSyndicationEnabled] [-UsageReportingEnabled] [[-AgreementNumber] <String>] [[-RegistrationName]
    <String>] [<CommonParameters>]
   ```

| Paramètre | Type | Description |
|-------------------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PrivilegedEndpointCredential | PSCredential | Les informations d’identification qui sont utilisées pour [accéder au point de terminaison privilégié](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). Le nom d’utilisateur est au format **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | Chaîne | Une console PowerShell distante préconfigurée qui vous fournit des fonctionnalités telles que la collecte de journaux et d’autres tâches de post-déploiement. Pour en savoir plus, reportez-vous à l’article relatif à l’[utilisation du point de terminaison privilégié](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). |
| AzureContext | PSObject |  |
| ResourceGroupName | Chaîne |  |
| ResourceGroupLocation | Chaîne |  |
| BillingModel | Chaîne | Le modèle de facturation utilisé par votre abonnement. Les valeurs valides pour ce paramètre sont : Capacity, PayAsYouUse et Development. |
| MarketplaceSyndicationEnabled | True/False | Détermine si la fonctionnalité de gestion de la Place de Marché est, ou non, disponible dans le portail. Définissez la valeur true en cas d’inscription avec une connectivité Internet. Définissez la valeur false en cas d’inscription dans des environnements déconnectés. Pour les inscriptions déconnectées, l’[outil de syndication hors connexion](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario) peut être utilisé pour le téléchargement d’éléments de la Place de Marché. |
| UsageReportingEnabled | True/False | Azure Stack crée par défaut des rapports sur les métriques d’utilisation. Les opérateurs qui se basent sur la capacité ou qui gèrent un environnement déconnecté doivent désactiver la génération de rapports d’utilisation. Les valeurs valides pour ce paramètre sont : True, False. |
| AgreementNumber | Chaîne |  |
| RegistrationName | Chaîne | Définissez un nom unique pour l’inscription si vous exécutez le script d’inscription dans plusieurs instances Azure Stack en utilisant le même ID d’abonnement Azure. Par défaut, le paramètre a la valeur **AzureStackRegistration**. Cependant, si vous utilisez le même nom sur plusieurs instances d’Azure Stack, le script échoue. |

### <a name="get-azsregistrationtoken"></a>Get-AzsRegistrationToken

Get-AzsRegistrationToken génère un jeton d’inscription à partir des paramètres d’entrée.

```PowerShell  
    Get-AzsRegistrationToken [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String>
    [-BillingModel] <String> [[-TokenOutputFilePath] <String>] [-UsageReportingEnabled] [[-AgreementNumber] <String>]
    [<CommonParameters>]
```
| Paramètre | Type | Description |
|-------------------------------|--------------|-------------|
| PrivilegedEndpointCredential | PSCredential | Les informations d’identification qui sont utilisées pour [accéder au point de terminaison privilégié](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). Le nom d’utilisateur est au format **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | Chaîne |  Une console PowerShell distante préconfigurée qui vous fournit des fonctionnalités telles que la collecte de journaux et d’autres tâches de post-déploiement. Pour en savoir plus, reportez-vous à l’article relatif à l’[utilisation du point de terminaison privilégié](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). |
| AzureContext | PSObject |  |
| ResourceGroupName | Chaîne |  |
| ResourceGroupLocation | Chaîne |  |
| BillingModel | Chaîne | Le modèle de facturation utilisé par votre abonnement. Les valeurs valides pour ce paramètre sont : Capacity, PayAsYouUse et Development. |
| MarketplaceSyndicationEnabled | True/False |  |
| UsageReportingEnabled | True/False | Azure Stack crée par défaut des rapports sur les métriques d’utilisation. Les opérateurs qui se basent sur la capacité ou qui gèrent un environnement déconnecté doivent désactiver la génération de rapports d’utilisation. Les valeurs valides pour ce paramètre sont : True, False. |
| AgreementNumber | Chaîne |  |


## <a name="next-steps"></a>Étapes suivantes

[Télécharger des éléments de la Place de marché à partir d’Azure](azure-stack-download-azure-marketplace-item.md)
