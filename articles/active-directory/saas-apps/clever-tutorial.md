---
title: 'Didacticiel : Intégration d’Azure Active Directory à Clever | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Clever.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: e65f0cb3ef30fb5b001acdb72481c1c3b55ca058
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197311"
---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a>Didacticiel : Intégration d’Azure Active Directory à Clever

Dans ce didacticiel, vous allez apprendre à intégrer Clever dans Azure Active Directory (Azure AD).

L’intégration de Clever dans Azure AD vous offre les avantages suivants :

- Dans Azure AD, vous pouvez contrôler qui a accès à Clever.
- Vous pouvez autoriser les utilisateurs à se connecter automatiquement à Clever (via l’authentification unique) avec leur compte Azure AD.
- Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec Clever, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Un abonnement Clever pour lequel l’authentification unique est activée

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez [obtenir un essai d’un mois](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de Clever depuis la galerie
1. Configuration et test de l’authentification unique Azure AD

## <a name="adding-clever-from-the-gallery"></a>Ajout de Clever depuis la galerie
Pour configurer l’intégration de Clever avec Azure AD, vous devez ajouter Clever, disponible dans la galerie, à votre liste d’applications SaaS gérées.

**Pour ajouter Clever à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**. 

    ![Bouton Azure Active Directory][1]

1. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![Panneau Applications d’entreprise][2]
    
1. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application][3]

1. Dans la zone de recherche, tapez **Clever**, sélectionnez **Clever** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Clever dans la liste des résultats](./media/clever-tutorial/tutorial_clever_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Clever avec un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur Clever correspondant dans Azure AD. En d’autres termes, une relation entre un utilisateur Azure AD et un utilisateur Clever associé doit être établie.

Dans Clever, assignez la valeur de **nom d’utilisateur** dans Azure AD comme valeur de **nom d’utilisateur** pour établir la relation.

Pour configurer et tester l’authentification unique Azure AD avec Clever, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
1. **[Créer utilisateur de test Clever](#create-a-clever-test-user)** pour avoir un équivalent de Britta Simon dans Clever lié à la représentation Azure AD associée.
1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
1. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure et configurez l’authentification unique dans votre application Clever.

**Pour configurer l’authentification unique Azure AD avec Clever, procédez comme suit :**

1. Dans le Portail Azure, sur la page d’intégration de l’application **Clever**, cliquez sur **Authentification unique**.

    ![Lien Configurer l’authentification unique][4]

1. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.

    ![Boîte de dialogue Authentification unique](./media/clever-tutorial/tutorial_clever_samlbase.png)

1. Dans la section **Domaine et URL Clever**, procédez comme suit :

    ![Informations d’authentification unique dans Domaine et URL Clever](./media/clever-tutorial/tutorial_clever_url.png)

    a. Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://clever.com/in/<companyname>`

    b. Dans la zone de texte **Identificateur**, tapez l’URL : `https://clever.com/oauth/saml/metadata.xml`

    > [!NOTE]
    > La valeur de l’URL de connexion n’est pas réelle. Mettez à jour cette valeur avec l’URL d’authentification réelle. Pour obtenir cette valeur, contactez [l’équipe de support technique de Clever](https://clever.com/about/contact/).

1. Dans la section  **Certificat de signature SAML** , cliquez sur le bouton Copier pour copier l’ **URL des métadonnées de fédération de l’application** , puis collez-la dans le Bloc-notes.
    
    ![Configurer l'authentification unique](./media/clever-tutorial/tutorial_metadataurl.png)

1. L’application Clever attend les assertions SAML dans un format spécifique, ce qui vous oblige à ajouter des mappages d’attributs personnalisés à votre configuration **Attributs du jeton SAML**.

    La capture d’écran suivante montre un exemple :

    ![Configure Single Sign-On](./media/clever-tutorial/tutorial_clever_07.png)

1. Dans la section **Attributs utilisateur** de la boîte de dialogue **Authentification unique**, configurez le jeton SAML comme sur l’image ci-dessus et procédez comme suit :
    
    | Nom de l'attribut  | Valeur de l’attribut |
    | --------------- | -------------------- |
    | clever.teacher.credentials.district_username|user.userprincipalname|
    | clever.student.credentials.district_username| user.userprincipalname |
    | Firstname  | user.givenname |
    | Lastname  | user.surname |

    a. Cliquez sur **Ajouter un attribut** pour ouvrir la boîte de dialogue **Ajouter un attribut**.

    ![Configure Single Sign-On](./media/clever-tutorial/tutorial_attribute_04.png)
    
    ![Configure Single Sign-On](./media/clever-tutorial/tutorial_attribute_05.png)
    
    b. Dans la zone de texte **Attribut**, indiquez le nom d’attribut pour cette ligne.

    c. Dans la liste **Valeur** , saisissez la valeur d’attribut affichée pour cette ligne.

    d. Laissez la zone de texte **Espace de noms** vide.
    
    d. Cliquez sur **OK**.
    
1. Cliquez sur le bouton **Enregistrer** .

    ![Bouton Enregistrer de la page Configurer l’authentification unique](./media/clever-tutorial/tutorial_general_400.png)

1. Dans une autre fenêtre de navigateur web, connectez-vous au site de votre entreprise Clever en tant qu’administrateur.

1. Dans la barre d’outils, cliquez sur **Instant Login**.

    ![Instant Login](./media/clever-tutorial/ic798984.png "Instant Login")

    > [!NOTE]
    > Avant de pouvoir tester l’authentification unique, vous devez contacter l[’équipe du support Clever Client](https://clever.com/about/contact/) pour activer l’authentification unique Office 365 dans le back end.

1. Dans la page **Instant Login** , procédez comme suit :
    
      ![Instant Login](./media/clever-tutorial/ic798985.png "Instant Login")
    
      a. Renseignez le champ **Login URL**.
    
      >[!NOTE]
      >**Login URL** est une valeur personnalisée. Pour obtenir cette valeur, contactez [l’équipe de support technique de Clever](https://clever.com/about/contact/).
    
      b. Sous **Identity System**, sélectionnez **ADFS**.

      c. Dans la zone de texte **URL des métadonnées**, collez la valeur **URL des métadonnées de fédération de l’application** que vous avez copiée sur le portail Azure.
    
      d. Cliquez sur **Enregistrer**.

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

   ![Créer un utilisateur de test Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le volet gauche du Portail Azure, cliquez sur le bouton **Azure Active Directory**.

    ![Bouton Azure Active Directory](./media/clever-tutorial/create_aaduser_01.png)

1. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](./media/clever-tutorial/create_aaduser_02.png)

1. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue **Tous les utilisateurs**.

    ![Bouton Ajouter](./media/clever-tutorial/create_aaduser_03.png)

1. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :

    ![Boîte de dialogue Utilisateur](./media/clever-tutorial/create_aaduser_04.png)

    a. Dans la zone **Nom**, tapez **BrittaSimon**.

    b. Dans la zone **Nom d’utilisateur** , tapez l’adresse e-mail de l’utilisateur Britta Simon.

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ **Mot de passe**.

    d. Cliquez sur **Créer**.

### <a name="create-a-clever-test-user"></a>Créer un utilisateur de test Clever

Pour se connecter à Clever, les utilisateurs d’Azure AD doivent être approvisionnés dans Clever.

Si vous optez pour Clever, collaborez avec l’ [équipe du support technique de Clever](https://clever.com/about/contact/)  pour ajouter des utilisateurs à la plateforme Clever. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

>[!NOTE]
>Vous pouvez utiliser n’importe quel autre outil ou API de création de compte d’utilisateur fourni par Clever pour approvisionner des comptes d’utilisateurs Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Clever.

![Attribuer le rôle utilisateur][200]

**Pour affecter Britta Simon à Clever, procédez comme suit :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201]

1. Dans la liste des applications, sélectionnez **Clever**.

    ![Lien Clever dans la liste des applications](./media/clever-tutorial/tutorial_clever_app.png)

1. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »][202]

1. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Volet Ajouter une attribution][203]

1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

1. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

1. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.

### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Lorsque vous cliquez sur la vignette Clever dans le volet d’accès, vous devez être connecté automatiquement à votre application Clever.
Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/clever-tutorial/tutorial_general_01.png
[2]: ./media/clever-tutorial/tutorial_general_02.png
[3]: ./media/clever-tutorial/tutorial_general_03.png
[4]: ./media/clever-tutorial/tutorial_general_04.png

[100]: ./media/clever-tutorial/tutorial_general_100.png

[200]: ./media/clever-tutorial/tutorial_general_200.png
[201]: ./media/clever-tutorial/tutorial_general_201.png
[202]: ./media/clever-tutorial/tutorial_general_202.png
[203]: ./media/clever-tutorial/tutorial_general_203.png

