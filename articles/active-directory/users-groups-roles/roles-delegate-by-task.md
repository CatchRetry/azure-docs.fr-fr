---
title: Déléguer des rôles moins privilégiés par tâche dans Azure Active Directory | Microsoft Docs
description: Rôles à déléguer pour des tâches d’identité dans Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 3b6c5b08fa3f915c541837abe5f52c7ec3d9b87e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55185201"
---
# <a name="administrator-roles-by-identity-task-in-azure-active-directory"></a>Rôles d’administrateur par tâche d’identité dans Azure Active Directory

Dans cet article, vous trouverez les informations nécessaires pour restreindre les autorisations d’administrateur d’un utilisateur en attribuant des rôles moins privilégiés dans Azure Active Directory (Azure AD). Il contient des tâches d’administrateur organisées par domaine de fonctionnalités et le rôle moins privilégié nécessaire pour effectuer chaque tâche, ainsi que d’autres rôles non-administrateur général qui peuvent effectuer la tâche.

## <a name="application-proxy"></a>Proxy d’application

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Configurer l’application de proxy d’application | Administrateur d’application | 
Configurer les propriétés du groupe de connecteurs | Administrateur d’application | 
Créer l’inscription de l’application quand la capacité est désactivée pour tous les utilisateurs | Développeur d’applications | Administrateur d’application cloud, administrateur d’application
Créer un groupe de connecteurs | Administrateur d’application | 
Supprimer un groupe de connecteurs | Administrateur d’application | 
Désactiver le proxy d’application | Administrateur d’application | 
Télécharger le service de connecteur | Administrateur d’application | 
Lire toute la configuration | Administrateur d’application | 

## <a name="b2c"></a>B2C

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Créer des annuaires Azure AD B2C | Tous les utilisateurs non invités ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Créer des applications B2C | Administrateur général | 
Créer des applications d’entreprise | Administrateur d'applications cloud | Administrateur d’application
Créer, lire, mettre à jour et supprimer des stratégies B2C | Administrateur général | 
Créer, lire, mettre à jour et supprimer des fournisseurs d’identité | Administrateur général | 
Créer, lire, mettre à jour et supprimer des flux utilisateur de réinitialisation de mot de passe | Administrateur général | 
Créer, lire, mettre à jour et supprimer des flux utilisateur de modification de profil | Administrateur général | 
Créer, lire, mettre à jour et supprimer des flux utilisateur de connexion | Administrateur général | 
Créer, lire, mettre à jour et supprimer des flux utilisateur d’inscription |Administrateur général | 
Créer, lire, mettre à jour et supprimer des attributs utilisateur | Administrateur général | 
Créer, lire, mettre à jour et supprimer des utilisateurs | Administrateur général ([consultez la documentation](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs))
Lire toute la configuration | Administrateur général | 
Lire les journaux d’audit B2C | Administrateur général ([consultez la documentation](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs)) | 

## <a name="company-branding"></a>Marque de société

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Configurer la marque de la société | Administrateur général | 
Lire toute la configuration | Lecteurs d’annuaires | Rôle d’utilisateur par défaut ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="company-properties"></a>Propriétés de l’entreprise

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Configurer les propriétés de l’entreprise | Administrateur général | 

## <a name="connect"></a>Connecter

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Authentification directe | Administrateur général | 
Lire toute la configuration | Administrateur général | 
Authentification unique homogène | Administrateur général | 

## <a name="connect-health"></a>Connect Health

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Ajouter ou supprimer des services | Propriétaire ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Appliquer des correctifs à une erreur de synchronisation | Contributeur ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Propriétaire
Configurer les notifications | Contributeur ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Propriétaire
Configurer les paramètres | Propriétaire ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Configurer les notifications de synchronisation | Contributeur ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Propriétaire
Lire les rapports de sécurité ADFS | Lecteur de sécurité | Contributeur, propriétaire
Lire toute la configuration | Lecteur ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Contributeur, propriétaire
Lire les erreurs de synchronisation | Lecteur ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Contributeur, propriétaire
Lire les services de synchronisation | Lecteur ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Contributeur, propriétaire
Afficher les métriques et les alertes | Lecteur ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Contributeur, propriétaire
Afficher les métriques et les alertes | Lecteur ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Contributeur, propriétaire
Afficher les métriques et les alertes de service de synchronisation | Lecteur ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Contributeur, propriétaire


## <a name="custom-domain-names"></a>Noms de domaine personnalisés

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Gérer des domaines | Administrateur général | 
Lire toute la configuration | Lecteurs d’annuaires | Rôle d’utilisateur par défaut ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="domain-services"></a>Services de domaine

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Créer une instance Azure AD Domain Services | Administrateur général | 
Effectuer toutes les tâches Azure AD Domain Services | Groupe Administrateurs de contrôleur de domaine Azure AD ([consultez la documentation](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-administer-domain#administrative-tasks-you-can-perform-on-a-managed-domain)) | 
Lire toute la configuration | Lecteur sur l’abonnement Azure contenant le service AD DS | 

## <a name="devices"></a>Appareils

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Désactiver un appareil | Administrateur d’appareil cloud | 
Activer un appareil | Administrateur d’appareil cloud | 
Lire la configuration de base | Rôle d’utilisateur par défaut ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Lire les clés BitLocker | Lecteur de sécurité | Administrateur de mot de passe, administrateur de la sécurité

## <a name="enterprise-applications"></a>Applications d’entreprise

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Donner son consentement pour les autorisations déléguées | Administrateur d’application cloud | Administrateur d’application
Donner son consentement pour les autorisations d’application, hors Microsoft Graph ou Azure AD Graph | Administrateur d’application cloud | Administrateur d’application
Donner son consentement pour les autorisations d’application à Microsoft Graph ou Azure AD Graph | Administrateur général | 
Donner son consentement pour les applications propriétaires de données | Rôle d’utilisateur par défaut ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Créer une application d’entreprise | Administrateur d’application cloud | Administrateur d’application
Gérer le proxy d’application | Administrateur d’application | 
Gérer les paramètres utilisateur | Administrateur général | 
Lire la révision d’accès d’un groupe ou d’une application | Lecteur de sécurité | Administrateur de la sécurité, administrateur d’utilisateurs
Lire toute la configuration | Rôle d’utilisateur par défaut ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Mettre à jour les attributions d’application d’entreprise | Propriétaire d’une application d’entreprise ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrateur d’application cloud, administrateur d’application
Mettre à jour les propriétaires d’application d’entreprise | Propriétaire d’une application d’entreprise ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrateur d’application cloud, administrateur d’application
Mettre à jour les propriétés d’une application d’entreprise | Propriétaire d’une application d’entreprise ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrateur d’application cloud, administrateur d’application
Mettre à jour le provisionnement d’une application d’entreprise | Propriétaire d’une application d’entreprise ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrateur d’application cloud, administrateur d’application
Mettre à jour le libre-service d’une application d’entreprise | Propriétaire d’une application d’entreprise ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrateur d’application cloud, administrateur d’application
Mettre à jour les propriétés de l’authentification unique | Propriétaire d’une application d’entreprise ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrateur d’application cloud, administrateur d’application

## <a name="groups"></a>Groupes

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Affecter une licence | Administrateur de compte d’utilisateur | 
Créer un groupe | Administrateur de compte d’utilisateur | 
Créer, mettre à jour ou supprimer la révision d’accès d’un groupe ou d’une application | Administrateur de compte d’utilisateur | 
Gérer l’expiration des groupes | Administrateur de compte d’utilisateur | 
Gérer les paramètres de groupe | Administrateur général | 
Lire toutes les configurations (à l’exception de l’appartenance masquée) | Lecteurs d’annuaires | Rôle d’utilisateur par défaut ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Lire l’appartenance masquée | Membre de groupe | Propriétaire de groupe, administrateur de mot de passe, administrateur Exchange, administrateur SharePoint, administrateur Teams, administrateur de compte d’utilisateur
Lire l’appartenance des groupes avec une appartenance masquée | Administrateur du support technique | Administrateur de compte d’utilisateur, administrateur Teams
Révoquer une licence | Administrateur de licence | Administrateur de compte d’utilisateur
Mettre à jour l’appartenance à un groupe | Propriétaire de groupe ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrateur de compte d’utilisateur
Mettre à jour les propriétaires de groupe | Propriétaire de groupe ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrateur de compte d’utilisateur
Mettre à jour les propriétés de groupe | Propriétaire de groupe ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrateur de compte d’utilisateur

## <a name="identity-protection"></a>Identity Protection

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Configurer des notifications d’alerte| Security Administrator | 
Configurer et activer ou désactiver la stratégie MFA| Security Administrator | 
Configurer et activer ou désactiver la stratégie de connexion à risque| Security Administrator | 
Configurer et activer ou désactiver la stratégie d’utilisateur à risque | Security Administrator | 
Configurer des synthèses hebdomadaires | Security Administrator| 
Ignorance de tous les événements à risque | Security Administrator | 
Corriger ou ignorer des vulnérabilités | Security Administrator | 
Lire toute la configuration | Lecteur de sécurité | 
Lire tous les événements à risque | Lecteur de sécurité | 
Lire les vulnérabilités | Lecteur de sécurité | 

## <a name="licenses"></a>Licences

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Affecter une licence | Administrateur de licence | Administrateur de compte d’utilisateur
Lire toute la configuration | Lecteurs d’annuaires | Rôle d’utilisateur par défaut ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Révoquer une licence | Administrateur de licence | Administrateur de compte d’utilisateur
Tester ou acheter un abonnement | Administrateur de facturation | 


## <a name="monitoring---audit-logs"></a>Surveillance - Journaux d’audit

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Lire les journaux d’audit | Lecteur de rapports | Lecteur Sécurité, administrateur de la sécurité

## <a name="monitoring---sign-ins"></a>Surveillance - Connexions

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Lire les journaux de connexion | Lecteur de rapports | Lecteur Sécurité, administrateur de la sécurité

## <a name="multi-factor-authentication"></a>Authentification multifacteur

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Supprimer tous les mots de passe d’application existants qui ont été générés par les utilisateurs sélectionnés | Administrateur général | 
Désactiver MFA | Administrateur général | 
Activer la MFA | Administrateur général | 
Gérer les paramètres du service MFA | Administrateur général | 
Demander aux utilisateurs sélectionnés de fournir à nouveau des méthodes de contact | Administrateur général | 
Restaurer l’authentification multifacteur pour tous les appareils mémorisés  | Administrateur général | 

## <a name="mfa-server"></a>Serveur MFA

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Blocage/déblocage des utilisateurs | Administrateur général | 
Configurer le verrouillage de compte | Administrateur général | 
Configurer les règles de mise en cache | Administrateur général | 
Configurer l’alerte de fraude | Administrateur général
Configurer les notifications | Administrateur général | 
Configurer un contournement à usage unique | Administrateur général | 
Configurer les paramètres d’appel téléphonique | Administrateur général | 
Configurer des fournisseurs | Administrateur général | 
Configurez les paramètres du serveur | Administrateur général | 
Lire un rapport d’activité | Administrateur général | 
Lire toute la configuration | Administrateur général | 
Lire l’état du serveur | Administrateur général |  

## <a name="organizational-relationships"></a>Relations organisationnelles

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Gérer les fournisseurs d’identité | Administrateur général | 
Gérer les paramètres | Administrateur général | 
Gérer les conditions d’utilisation | Administrateur général | 
Lire toute la configuration | Administrateur général | 

## <a name="password-reset"></a>Réinitialisation de mot de passe

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Configurer les méthodes d’authentification | Administrateur général | 
Configurer la personnalisation | Administrateur général | 
Configurer la notification | Administrateur général | 
Configurer l’intégration locale | Administrateur général | 
Configurer les propriétés de la réinitialisation de mot de passe | Administrateur général | 
Configurer l’inscription | Administrateur général | 
Lire toute la configuration | Administrateur de la sécurité, administrateur d’utilisateurs | 

## <a name="privileged-identity-management"></a>Privileged Identity Management

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Affecter des utilisateurs aux rôles | Administrateur de rôle privilégié | 
Configurer les paramètres de rôle | Administrateur de rôle privilégié | 
Afficher l’activité d’audit | Lecteur de sécurité | 
Afficher les appartenances aux rôles | Lecteur de sécurité | 

## <a name="roles-and-administrators"></a>Rôles et administrateurs

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Gérer les attributions de rôles | Administrateur de rôle privilégié | 
Lire la révision d’accès d’un rôle d’Azure AD  | Lecteur de sécurité | Administrateur de la sécurité, administrateur de rôle privilégié
Lire toute la configuration | Rôle d’utilisateur par défaut ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 

## <a name="security---authentication-methods"></a>Sécurité - Méthodes d’authentification

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Configurer les méthodes d’authentification | Administrateur général | 
Lire toute la configuration | Administrateur général | 

## <a name="security---conditional-access"></a>Sécurité - Accès conditionnel

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Configurer des adresses IP approuvées MFA | Administrateur de l’accès conditionnel | 
Créer des contrôles personnalisés | Administrateur de l’accès conditionnel | Administrateur de sécurité
Créer des emplacements nommés | Administrateur de l’accès conditionnel | Administrateur de sécurité
Création des stratégies | Administrateur de l’accès conditionnel | Administrateur de sécurité
Créer des conditions d’utilisation | Administrateur de l’accès conditionnel | Administrateur de sécurité
Créer un certificat de connectivité VPN | Administrateur de l’accès conditionnel | Administrateur de sécurité
Supprimer une stratégie classique | Administrateur de l’accès conditionnel | Administrateur de sécurité
Supprimer des conditions d’utilisation | Administrateur de l’accès conditionnel | Administrateur de sécurité
Supprimer un certificat de connectivité VPN | Administrateur de l’accès conditionnel | Administrateur de sécurité
Désactiver une stratégie classique | Administrateur de l’accès conditionnel | Administrateur de sécurité
Gérer des contrôles personnalisés | Administrateur de l’accès conditionnel | Administrateur de sécurité
Gérer des emplacements nommés | Administrateur de l’accès conditionnel | Administrateur de sécurité
Gérer les conditions d’utilisation | Administrateur de l’accès conditionnel | Administrateur de sécurité
Lire toute la configuration | Lecteur de sécurité | Administrateur de sécurité
Lire des emplacements nommés | Lecteur de sécurité | Administrateur de l’accès conditionnel, administrateur de la sécurité

## <a name="security---identity-security-score"></a>Sécurité - Score de sécurité d’identité

Tâche | Rôle moins privilégié | Autres rôles | 
---- | --------------------- | ----------------
Lire toute la configuration | Lecteur de sécurité | Administrateur de sécurité
Lire un score de sécurité | Lecteur de sécurité | Administrateur de sécurité
Mettre à jour l’état d’un événement | Administrateur de sécurité | 

## <a name="security---risky-sign-ins"></a>Sécurité - Connexions à risque

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Lire toute la configuration | Lecteur de sécurité | 
Lire les connexions à risque | Lecteur de sécurité | 

## <a name="security---users-flagged-for-risk"></a>Sécurité - Utilisateurs avec indicateur de risque

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Ignorer tous les événements | Security Administrator | 
Lire toute la configuration | Lecteur de sécurité | 
Lire les utilisateurs avec indicateur de risque | Lecteur de sécurité | 

## <a name="users"></a>Utilisateurs

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Ajouter un utilisateur à un rôle d’annuaire | Administrateur de rôle privilégié | 
Ajouter un utilisateur à un groupe | Administrateur de compte d’utilisateur | 
Affecter une licence | Administrateur de licence | Administrateur de compte d’utilisateur
Créer un utilisateur invité | Inviteur d’invités | Administrateur de compte d’utilisateur
Créer un utilisateur | Administrateur de compte d’utilisateur | 
Suppression d’utilisateurs | Administrateur de compte d’utilisateur | 
Invalider les jetons d’actualisation des administrateurs limités (consultez la documentation) | Administrateur de compte d’utilisateur | 
Invalider les jetons d’actualisation des non-administrateurs (consultez la documentation) | Administrateur de mots de passe | Administrateur de compte d’utilisateur
Invalider les jetons d’actualisation des administrateurs privilégiés (consultez la documentation) | Administrateur général | 
Lire la configuration de base | Rôle d’utilisateur par défaut ([consultez la documentation](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions) | 
Réinitialiser le mot de passe pour les administrateurs limités (consultez la documentation) | Administrateur de compte d’utilisateur | 
Réinitialiser le mot de passe des non-administrateurs (consultez la documentation) | Administrateur de mots de passe | Administrateur de compte d’utilisateur
Réinitialiser le mot de passe des administrateurs privilégiés | Administrateur général | 
Révoquer une licence | Administrateur de licence | Administrateur de compte d’utilisateur
Mettre à jour toutes les propriétés, sauf le nom d’utilisateur principal | Administrateur de compte d’utilisateur | 
Mettre à jour le nom d’utilisateur principal pour les administrateurs limités (consultez la documentation) | Administrateur de compte d’utilisateur | 
Mettre à jour la propriété Nom d’utilisateur principal sur les administrateurs privilégiés (consultez la documentation) | Administrateur général | 
Mettre à jour les paramètres utilisateur | Administrateur général | 


## <a name="support"></a>Support

Tâche | Rôle moins privilégié | Autres rôles
---- | --------------------- | ----------------
Envoyer un ticket de support | Administrateur de services | Administrateur d’application, administrateur de facturation, administrateur d’application cloud, administrateur de conformité, administrateur Dynamics 365, administrateur Desktop Analytics, administrateur Exchange, administrateur de mot de passe, administrateur Information Protection, administrateur Intune, administrateur Skype Entreprise, administrateur Power BI, administrateur d’authentification privilégié, administrateur SharePoint, administrateur des communications Teams, administrateur Teams, administrateur d’utilisateur, administrateur Analyse du temps de travail

## <a name="next-steps"></a>Étapes suivantes

* [Guide pratique pour attribuer ou supprimer des rôles d’administrateur Azure AD](directory-manage-roles-portal.md)
* [Informations de référence sur les rôles d’administrateur Azure AD](directory-assign-admin-roles.md)
