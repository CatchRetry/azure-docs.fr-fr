---
title: Didacticiel - Autoriser une application web à effectuer l’authentification avec des comptes à l’aide d’Azure Active Directory B2C | Microsoft Docs
description: Didacticiel sur l’utilisation d’Azure Active Directory B2C pour fournir une connexion utilisateur pour une application web ASP.NET.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 714d733e765f28d1244f6ee1c7b1cb237c0c4b1f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55198348"
---
# <a name="tutorial-enable-a-web-application-to-authenticate-with-accounts-using-azure-active-directory-b2c"></a>Tutoriel : Autoriser une application web à effectuer l’authentification avec des comptes à l’aide d’Azure Active Directory B2C

Ce didacticiel vous montre comment utiliser Azure Active Directory B2C pour connecter et inscrire des utilisateurs dans une application web ASP.NET. Azure AD B2C permet à vos applications de s’authentifier auprès de comptes des réseaux sociaux, de comptes d’entreprise et de comptes Azure Active Directory à l’aide de protocoles standards ouverts.

Ce tutoriel vous montre comment effectuer les opérations suivantes :

> [!div class="checklist"]
> * Inscrire un exemple d’application web ASP.NET dans votre locataire Azure AD B2C.
> * Créer des flux d’utilisateurs pour l’inscription et la connexion des utilisateurs, la modification d’un profil et la réinitialisation d’un mot de passe.
> * Configurer l’exemple d’application web pour utiliser votre locataire Azure AD B2C. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prérequis

* Utiliser votre propre [locataire Azure AD B2C](active-directory-b2c-get-started.md)
* Installer [Visual Studio 2017](https://www.visualstudio.com/downloads/) avec la charge de travail **Développement ASP.NET et web**.

## <a name="register-web-app"></a>Inscrire une API web

Les applications doivent être [inscrites](../active-directory/develop/developer-glossary.md#application-registration) dans votre locataire avant qu’elles ne puissent recevoir des [jetons d’accès](../active-directory/develop/developer-glossary.md#access-token) de la part de Azure Active Directory. L’inscription d’une application crée un [id d’application](../active-directory/develop/developer-glossary.md#application-id-client-id) pour celle-ci dans votre client. 

Connectez-vous au [portail Azure](https://portal.azure.com/) en tant qu’administrateur général de votre locataire Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Choisissez **Tous les services** dans le coin supérieur gauche du Portail Azure, recherchez et sélectionnez **Azure Active Directory B2C**. Vous devriez désormais utiliser le locataire que vous avez créé dans le tutoriel précédent. 

2. Dans les paramètres B2C, cliquez sur **Applications**, puis sur **Ajouter**. 

    Pour inscrire l’exemple d’application web dans votre locataire, utilisez les paramètres suivants :

    ![Ajouter une nouvelle application](media/active-directory-b2c-tutorials-web-app/web-app-registration.png)
    
    | Paramètre      | Valeur suggérée  | Description                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nom** | Mon exemple d’application web | Entrez un **nom** décrivant votre application aux consommateurs. | 
    | **Inclure une application/API web** | Oui | Sélectionnez **Oui** pour une application web. |
    | **Autoriser le flux implicite** | Oui | Sélectionnez **Oui** puisque l’application utilise la [connexion OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **URL de réponse** | `https://localhost:44316` | Les URL de réponse sont des points de terminaison auxquels Azure AD B2C retourne les jetons demandés par votre application. Dans ce didacticiel, l’exemple s’exécute localement (localhost) et écoute sur le port 44316. |
    | **Inclure le client natif** | Non  | Dans la mesure où il s’agit d’une application web et pas d’un client natif, sélectionnez Non. |
    
3. Cliquez sur **Créer** pour inscrire votre application.

Les applications inscrites sont indiquées dans la liste des applications du client Azure AD B2C. Sélectionnez votre application web dans la liste. Le volet de propriétés de l’application web s’affiche.

![Propriétés de l’application web](./media/active-directory-b2c-tutorials-web-app/b2c-web-app-properties.png)

Notez l’**ID d’application**. Cet ID identifie l’application de manière unique. Il est nécessaire pour configurer l’application ultérieurement dans le didacticiel.

### <a name="create-a-client-password"></a>Créer un mot de passe client

Azure AD B2C utilise l’autorisation OAuth2 pour [les applications clientes](../active-directory/develop/developer-glossary.md#client-application). Les applications web sont des [clients confidentiels](../active-directory/develop/developer-glossary.md#web-client) et nécessitent un ID client ou ID d’application et une clé secrète client, un mot de passe client ou une clé d’application.

1. Sélectionnez la page Clés de l’application web inscrit et cliquez sur **Générer une clé**.

2. Cliquez sur **Enregistrer** pour afficher la clé d’application.

    ![page générale des clés de l’application](media/active-directory-b2c-tutorials-web-app/app-general-keys-page.png)

La clé s’affiche une seule fois dans le portail. Il est important de la copier et d’enregistrer sa valeur. Vous avez besoin de cette valeur pour configurer votre application. Gardez la clé en sécurité. Ne la partagez pas publiquement.

## <a name="create-user-flows"></a>Créer des flux d’utilisateur

Un flux d’utilisateur Azure AD B2C définit l’expérience utilisateur pour une tâche d’identité. Par exemple, la connexion, l’inscription, le changement de mot de passe et la modification de profil sont des flux d’utilisateur courants.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Créer un flux d’utilisateur d’inscription ou de connexion

Pour inscrire des utilisateurs, puis les connecter à l’application web, créez un **flux d’utilisateur d’inscription ou de connexion**.

1. Dans la page du portail Azure AD B2C, sélectionnez **flux d’utilisateur** et cliquez sur **Nouveau flux d’utilisateur**.
2. Sous l’onglet **Recommandé**, cliquez sur **Inscription et connexion**.

    Pour configurer votre flux d’utilisateur, utilisez les paramètres suivants :

    ![Ajouter un flux d’utilisateur d’inscription ou de connexion](media/active-directory-b2c-tutorials-web-app/add-susi-user-flow.png)

    | Paramètre      | Valeur suggérée  | Description                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nom** | SiUpIn | Entrez un **Nom** pour le flux d’utilisateur. Le nom du flux d’utilisateur est préfixé avec **b2c_1_**. Vous utilisez le nom complet du flux d’utilisateur **b2c_1_SiUpIn** dans l’exemple de code. | 
    | **Fournisseurs d’identité** | Inscription par e-mail | Le fournisseur d’identité utilisé pour identifier l’utilisateur. |

3. Sous **Attributs utilisateur et revendications**, cliquez sur **Afficher plus** et sélectionnez les paramètres suivants :

    ![Ajouter un flux d’utilisateur d’inscription ou de connexion](media/active-directory-b2c-tutorials-web-app/add-attributes-and-claims.png)

    | Colonne      | Valeurs suggérées  | Description                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Collecter l’attribut** | Nom d’affichage et Code Postal | Sélectionnez les attributs à collecter auprès de l'utilisateur pendant l'inscription. |
    | **Revendication de retour** | Nom d’affichage, Code Postal, L’utilisateur est nouveau, ID d’objet de l’utilisateur | Sélectionnez les [revendications](../active-directory/develop/developer-glossary.md#claim) que vous souhaitez inclure dans le [jeton d’accès](../active-directory/develop/developer-glossary.md#access-token). |

4. Cliquez sur **OK**.
5. Cliquez sur **Créer** pour créer votre flux d’utilisateur. 

### <a name="create-a-profile-editing-user-flow"></a>Créer un flux d’utilisateur de modification de profil

Pour permettre aux utilisateurs de réinitialiser eux-mêmes les informations de leur profil utilisateur, créez un **flux d’utilisateur de modification de profil**.

1. Dans la page du portail Azure AD B2C, sélectionnez **flux d’utilisateur** et cliquez sur **Nouveau flux d’utilisateur**.
2. Sous l’onglet **Recommandé**, cliquez sur **Modification de profil**.

    Pour configurer votre flux d’utilisateur, utilisez les paramètres suivants :

    | Paramètre      | Valeur suggérée  | Description                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nom** | SiPe | Entrez un **Nom** pour le flux d’utilisateur. Le nom du flux d’utilisateur est préfixé avec **b2c_1_**. Vous utilisez le nom complet du flux d’utilisateur **b2c_1_SiPe** dans l’exemple de code. | 
    | **Fournisseurs d’identité** | Local Account SignIn | Le fournisseur d’identité utilisé pour identifier l’utilisateur. |

3. Sous **Attributs utilisateur**, cliquez sur **Afficher plus** et sélectionnez les paramètres suivants :

    | Colonne      | Valeurs suggérées  | Description                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Collecter l’attribut** | Nom d’affichage et Code Postal | Sélectionnez les attributs que les utilisateurs peuvent modifier durant la modification du profil. |
    | **Revendication de retour** | Nom d’affichage, Code postal, ID d’objet de l’utilisateur | Sélectionnez les [revendications](../active-directory/develop/developer-glossary.md#claim) que vous souhaitez inclure dans le [jeton d’accès](../active-directory/develop/developer-glossary.md#access-token) après une modification de profil réussie. |

4. Cliquez sur **OK**.
5. Cliquez sur **Créer** pour créer votre flux d’utilisateur. 

### <a name="create-a-password-reset-user-flow"></a>Créer un flux d’utilisateur de réinitialisation du mot de passe

Pour activer la réinitialisation du mot de passe sur votre application, vous devez créer un **flux d’utilisateur de réinitialisation de mot de passe**. Ce flux d’utilisateur décrit les expériences des clients lors de la réinitialisation du mot de passe, et le contenu des jetons que l’application reçoit en cas d’opération réussie.

1. Dans la page du portail Azure AD B2C, sélectionnez **Stratégies de réinitialisation de mot de passe** et cliquez sur **Ajouter**.
2. Sous l’onglet **Recommandé**, cliquez sur **Réinitialisation du mot de passe**.

    Pour configurer votre flux d’utilisateur, utilisez les paramètres suivants :

    | Paramètre      | Valeur suggérée  | Description                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nom** | SSPR | Entrez un **Nom** pour le flux d’utilisateur. Le nom du flux d’utilisateur est préfixé avec **b2c_1_**. Vous utilisez le nom complet du flux d’utilisateur **b2c_1_SSPR** dans l’exemple de code. | 
    | **Fournisseurs d’identité** | Réinitialiser le mot de passe à l’aide d’une adresse e-mail | Il s’agit du fournisseur d’identité utilisé pour identifier l’utilisateur. |

3. Sous **Revendications d’application**, cliquez sur **Afficher plus** et sélectionnez les paramètres suivants :
    | Colonne      | Valeur suggérée  | Description                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Revendication de retour** | ID d’objet de l’utilisateur | Sélectionnez les [revendications](../active-directory/develop/developer-glossary.md#claim) que vous souhaitez inclure dans le [jeton d’accès](../active-directory/develop/developer-glossary.md#access-token) après une réinitialisation réussie du mot de passe. |

4. Cliquez sur **OK**.
5. Cliquez sur **Créer** pour créer votre flux d’utilisateur. 

## <a name="update-web-app-code"></a>Mettre à jour le code d’application web

Maintenant que vous disposez d’une application web inscrite et de flux d’utilisateur créés, vous devez configurer votre application pour utiliser votre locataire Azure AD B2C. Dans ce didacticiel, vous configurez un exemple d’application web que vous pouvez télécharger à partir de GitHub. 

[Téléchargez un fichier zip ](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) ou clonez l’exemple d’application web à partir de GitHub. Assurez-vous que vous extrayez l’exemple de fichier dans un dossier où la longueur totale du chemin d’accès est inférieure à 260 caractères.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

L’exemple d’application web ASP.NET est une simple application de liste de tâches pour la création et la mise à jour d’une liste de tâches. L’application utilise [les composants d’intergiciel (middleware) Microsoft OWIN](https://docs.microsoft.com/aspnet/aspnet/overview/owin-and-katana/) pour permettre aux utilisateurs de s’inscrire afin d’utiliser l’application dans votre locataire Azure AD B2C. En créant un flux d’utilisateur Azure AD B2C, les utilisateurs peuvent se servir d’un compte de réseau social ou créer un compte à utiliser comme identité pour accéder à l’application. 

L’exemple de solution contient deux projets :

**Exemple d’application web (TaskWebApp) :** application web permettant de créer et de modifier une liste des tâches. L’application web utilise le flux d’utilisateur d’**inscription ou de connexion** pour inscrire ou connecter des utilisateurs.

**Exemple d’application d’API web (TaskService) :** API web qui prend en charge les fonctionnalités de création, de lecture, de mise à jour et de suppression des listes de tâches. L’API web est protégée par Azure AD B2C et appelée par l’application web.

Vous devez modifier l’application pour utiliser l’inscription d’application à votre locataire, ce qui inclut l’ID d’application et la clé enregistrés précédemment. Vous devez également configurer les flux d’utilisateur que vous avez créés. L’exemple d’application web définit les valeurs de configuration en tant que paramètres d’application dans le fichier Web.config. Pour modifier les paramètres d’application :

1. Ouvrez la solution **B2C-WebAPI-DotNet** dans Visual Studio.

2. Dans le projet d’application web **TaskWebApp**, ouvrez le fichier **Web.config**. Remplacez la valeur pour `ida:Tenant` avec le nom du locataire que vous avez créé. Remplacez la valeur pour `ida:ClientId` avec l’ID d’application que vous avez enregistré. Remplacez la valeur de `ida:ClientSecret` avec la clé que vous avez enregistrée.

3. Dans le fichier **Web.config**, remplacez la valeur pour `ida:SignUpSignInPolicyId` avec `b2c_1_SiUpIn`. Remplacez la valeur pour `ida:EditProfilePolicyId` avec `b2c_1_SiPe`. Remplacez la valeur pour `ida:ResetPasswordPolicyId` avec `b2c_1_SSPR`.

## <a name="run-the-sample-web-app"></a>Exécuter l’exemple d’application web

Dans l’Explorateur de solutions, faites un clic droit sur le projet **TaskWebApp** puis cliquez sur **Définir comme projet de démarrage**

Appuyez sur **F5** pour lancer l’application web. Le navigateur par défaut se lance à l’adresse du site web local `https://localhost:44316/`. 

L’exemple d’application prend en charge l’inscription et la connexion des utilisateurs, la modification d’un profil, et la réinitialisation d’un mot de passe. Ce didacticiel met en évidence la manière dont un utilisateur s’inscrit pour utiliser l’application à l’aide d’une adresse e-mail. Vous pouvez étudier les autres scénarios vous-même.

### <a name="sign-up-using-an-email-address"></a>S’inscrire au moyen d’une adresse e-mail

1. Cliquez sur le lien **S’inscrire / se connecter** dans la bannière supérieure pour vous inscrire en tant qu’utilisateur de l’application web. Cette méthode utilise le flux d’utilisateur **b2c_1_SiUpIn** que vous avez défini à l’étape précédente.

2. Azure AD B2C présente une page de connexion avec un lien pour l’abonnement. Si vous ne possédez pas encore de compte, cliquez sur le lien **Inscrivez-vous maintenant**. 

3. Le flux de travail d’abonnement présente une page pour collecter et vérifier l’identité de l’utilisateur à l’aide d’une adresse e-mail. Le flux de travail d’inscription collecte également le mot de passe et les attributs demandés, qui sont définis dans le flux d’utilisateur.

    Utilisez une adresse e-mail valide et validez à l’aide d’un code de vérification. Définissez un mot de passe. Entrez des valeurs pour les attributs requis. 

    ![Flux de travail d’abonnement](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.png)

4. Cliquez sur **Créer** pour créer un compte local dans le locataire Azure AD B2C.

Maintenant l’utilisateur peut utiliser son adresse e-mail pour vous connecter et utiliser l’application web.

## <a name="clean-up-resources"></a>Supprimer des ressources

Vous pouvez utiliser votre client Azure AD B2C si vous envisagez d’effectuer d’autres didacticiels Azure AD B2C. Si vous n’en avez plus besoin, vous pouvez [supprimer votre client Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous avez découvert comment créer un locataire Azure AD B2C, créer des flux d’utilisateur et mettre à jour l’exemple d’application web pour utiliser votre locataire Azure AD B2C. Passez au prochain didacticiel pour apprendre à inscrire, configurer et appeler une API web ASP.NET protégée par votre client Azure AD B2C.

> [!div class="nextstepaction"]
> [Tutoriel : Utiliser Azure Active Directory B2C pour protéger une API Web ASP.NET](active-directory-b2c-tutorials-web-api.md)
