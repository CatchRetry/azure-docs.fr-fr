---
title: "Tutoriel : Intégration d'Azure Active Directory à Wdesk | Microsoft Docs"
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Wdesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 0894b1ccf976cfa1a932b76f6ed06e1bd45ee1b7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180906"
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a>Tutoriel : Intégration d'Azure Active Directory à Wdesk

Ce didacticiel vous montrera comment intégrer Wdesk à Azure Active Directory (Azure AD).

L’intégration de Wdesk dans Azure AD vous offre les avantages suivants :

- Dans Azure AD, vous pouvez contrôler qui a accès à Wdesk
- Vous pouvez autoriser vos utilisateurs à se connecter automatiquement à Wdesk (via l’authentification unique) avec leur compte Azure AD
- Vous pouvez gérer vos comptes à partir d’un emplacement central : le portail Azure.

Pour en savoir plus sur l’intégration de l’application SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec Wdesk, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Un abonnement Wdesk avec authentification unique activée

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de Wdesk depuis la galerie
1. Configuration et test de l’authentification unique Azure AD

## <a name="adding-wdesk-from-the-gallery"></a>Ajout de Wdesk depuis la galerie
Pour configurer l’intégration de Wdesk avec Azure AD, vous devez ajouter Wdesk à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Wdesk à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**. 

    ![Active Directory][1]

1. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![APPLICATIONS][2]
    
1. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![APPLICATIONS][3]

1. Dans la zone de recherche, tapez **Wdesk**.

    ![Création d’un utilisateur de test Azure AD](./media/wdesk-tutorial/tutorial_wdesk_search.png)

1. Dans le volet de résultats, sélectionnez **Wdesk**, puis cliquez sur **Ajouter** pour ajouter l’application.

    ![Création d’un utilisateur de test Azure AD](./media/wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuration et test de l’authentification unique Azure AD
Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Wdesk, avec un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur Wdesk correspondant dans Azure AD. En d’autres termes, une relation entre un utilisateur Azure AD et un utilisateur Wdesk associé doit être établie.

Pour cela, affectez la valeur de **nom d’utilisateur** dans Azure AD comme valeur de **nom d’utilisateur** dans Wdesk.

Pour configurer et tester l’authentification unique Azure AD avec Wdesk, vous devez suivre les indications des sections suivantes :

1. **[Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
1. **[Création d’un utilisateur de test Azure AD](#creating-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
1. **[Création d’un utilisateur de test Wdesk](#creating-a-wdesk-test-user)** pour avoir un équivalent de Britta Simon dans Wdesk lié à la représentation Azure AD de l’utilisateur.
1. **[Affectation de l’utilisateur de test Azure AD](#assigning-the-azure-ad-test-user)** : permet à Britta Simon d’utiliser l’authentification unique Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** pour vérifier si la configuration fonctionne.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuration de l’authentification unique Azure AD

Dans cette section, vous allez activer l’authentification unique Azure AD dans le portail Azure et configurer l’authentification unique dans votre application Wdesk.

**Pour configurer l’authentification unique Azure AD avec Wdesk, procédez comme suit :**

1. Dans le portail Azure, sur la page d’intégration de l’application **Wdesk**, cliquez sur **Authentification unique**.

    ![Configurer l'authentification unique][4]

1. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.
 
    ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_wdesk_samlbase.png)

1. Dans la section **Domaines et URL Wdesk**, si vous souhaitez configurer l’application en Mode initié par **IDP**, procédez comme suit :

    ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_wdesk_url.png)

    a. Dans la zone de texte **Identificateur**, tapez une URL au format suivant : `https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. Dans la zone de texte **URL de réponse** , tapez une URL au format suivant : `https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

1. Cliquez sur **Afficher les paramètres d’URL avancés**. Si vous souhaitez configurer l’application en mode initié par le **fournisseur de service**, procédez comme suit :

    ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_wdesk_url1.png)

    Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`
     
    > [!NOTE] 
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’identificateur, l’URL de réponse et l’URL de connexion réels. Vous obtenez ces valeurs depuis le portail WDesk lorsque vous configurez l’authentification unique. 
  
1. Dans la section **Certificat de signature SAML**, cliquez sur **Métadonnées XML** puis enregistrez le fichier de métadonnées sur votre ordinateur.

    ![Configure Single Sign-On](./media/wdesk-tutorial/tutorial_wdesk_certificate.png) 

1. Cliquez sur le bouton **Enregistrer** .

    ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_general_400.png)
    
1. Ouvrez une autre fenêtre de navigateur web, puis connectez-vous à Wdesk en tant qu’administrateur de la sécurité.

1. En bas à gauche, cliquez sur **administrateur** et sélectionnez **Administrateur de compte** :
 
     ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

1. Dans Administrateur Wdesk, accédez à **Sécurité**, puis **SAML** > **Paramètres SAML** :

    ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

1. Dans **Paramètres généraux**, cochez l’option **Activer l’authentification unique SAML** :

    ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

1. Dans **Détails sur le fournisseur de services**, procédez comme suit :

    ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      a. Copiez l’**URL de connexion** et collez-la dans la zone de texte **URL d’authentification** sur le portail Azure.
   
      b. Copiez l’**URL de métadonnée** et collez-la dans la zone de texte **Identificateur** sur le portail Azure.
       
      c. Copiez l’**URL de consommateur** et collez-la dans la zone de texte **URL de réponse** sur le portail Azure.
   
      d. Cliquez sur **Enregistrer** sur le portail Azure pour enregistrer les modifications.      

1. Cliquez sur **Configurer les paramètres du fournisseur d’identité** pour ouvrir la boîte de dialogue **Modifier les paramètres du fournisseur d’identité**. Cliquez sur **Choisir un fichier** pour trouver le fichier **Metadata.xml** que vous avez enregistré depuis le portail, puis chargez-le.
    
    ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
1. Cliquez sur **Save changes**.

    ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> Vous pouvez maintenant lire une version concise de ces instructions dans le [portail Azure](https://portal.azure.com), pendant que vous configurez l’application.  Après avoir ajouté cette application à partir de la section **Active Directory > Applications d’entreprise**, cliquez simplement sur l’onglet **Authentification unique** et accédez à la documentation incorporée par le biais de la section **Configuration** en bas. Pour en savoir plus sur la fonctionnalité de documentation incorporée, accédez à : [Documentation incorporée Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Création d’un utilisateur de test Azure AD
L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

![Créer un utilisateur Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le panneau de navigation gauche du **portail Azure**, cliquez sur l’icône **Azure Active Directory**.

    ![Création d’un utilisateur de test Azure AD](./media/wdesk-tutorial/create_aaduser_01.png) 

1. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.
    
    ![Création d’un utilisateur de test Azure AD](./media/wdesk-tutorial/create_aaduser_02.png) 

1. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue.
 
    ![Création d’un utilisateur de test Azure AD](./media/wdesk-tutorial/create_aaduser_03.png) 

1. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :
 
    ![Création d’un utilisateur de test Azure AD](./media/wdesk-tutorial/create_aaduser_04.png) 

    a. Dans la zone de texte **Nom**, entrez **BrittaSimon**.

    b. Dans la zone de texte **Nom d’utilisateur**, tapez **l’adresse e-mail** de Britta Simon.

    c. Sélectionnez **Afficher le mot de passe** et notez la valeur du **mot de passe**.

    d. Cliquez sur **Créer**.
 
### <a name="creating-a-wdesk-test-user"></a>Création d’un utilisateur de test Wdesk

Pour se connecter à Wdesk, les utilisateurs d’Azure AD doivent être approvisionnés dans Wdesk. Dans Wdesk, l’approvisionnement est une tâche manuelle.

**Pour approvisionner un compte d’utilisateur, procédez comme suit :**

1. Connectez-vous à Wdesk en tant qu’administrateur de la sécurité.
1. Accédez à **Administrateur** > **Compte Administrateur**.

     ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

1. Dans **Personnes**, cliquez sur **Membres**.

1. Cliquez maintenant sur **Ajouter membre** pour ouvrir la boîte de dialogue **Ajouter membre**. 
   
    ![Création d’un utilisateur de test Azure AD](./media/wdesk-tutorial/createuser1.png)  

1. Dans la zone de texte **Utilisateur**, entrez un nom d’utilisateur, par exemple **brittasimon@contoso.com**, puis cliquez sur **Continuer**.

    ![Création d’un utilisateur de test Azure AD](./media/wdesk-tutorial/createuser3.png)

1.  Renseignez les détails comme indiqué ci-dessous :
  
    ![Création d’un utilisateur de test Azure AD](./media/wdesk-tutorial/createuser4.png)
 
    a. Dans la zone de texte **E-mail**, entrez une adresse e-mail d’utilisateur, par exemple **brittasimon@contoso.com**.

    b. Dans la zone de texte **Prénom**, entrez le prénom de l’utilisateur, par exemple **Britta**.

    c. Dans la zone de texte **Nom**, entrez le nom de l’utilisateur, par exemple **Simon**.

1. Cliquez sur **Enregistrer membre**.  

    ![Création d’un utilisateur de test Azure AD](./media/wdesk-tutorial/createuser5.png)

### <a name="assigning-the-azure-ad-test-user"></a>Affectation de l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Wdesk.

![Affecter des utilisateurs][200] 

**Pour affecter Britta Simon à Wdesk, procédez comme suit :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201] 

1. Dans la liste des applications, sélectionnez **Wdesk**.

    ![Configurer l'authentification unique](./media/wdesk-tutorial/tutorial_wdesk_app.png) 

1. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Affecter des utilisateurs][202] 

1. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Affecter des utilisateurs][203]

1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

1. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

1. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.
    
### <a name="testing-single-sign-on"></a>Test de l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Lorsque vous cliquez sur la vignette Wdesk dans le volet d’accès, vous êtes connecté automatiquement à votre application Wdesk.
Pour plus d'informations sur le volet d'accès, consultez [Présentation du volet d'accès](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/wdesk-tutorial/tutorial_general_01.png
[2]: ./media/wdesk-tutorial/tutorial_general_02.png
[3]: ./media/wdesk-tutorial/tutorial_general_03.png
[4]: ./media/wdesk-tutorial/tutorial_general_04.png

[100]: ./media/wdesk-tutorial/tutorial_general_100.png

[200]: ./media/wdesk-tutorial/tutorial_general_200.png
[201]: ./media/wdesk-tutorial/tutorial_general_201.png
[202]: ./media/wdesk-tutorial/tutorial_general_202.png
[203]: ./media/wdesk-tutorial/tutorial_general_203.png

