---
title: "Vue d&#39;ensemble de l&#39;int&#233;gration &#224; des applications COM+ | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
helpviewer_keywords: 
  - "WCF, intégration COM+"
  - "Windows Communication Foundation, intégration COM+"
ms.assetid: e481e48f-7096-40eb-9f20-7f0098412941
caps.latest.revision: 29
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 29
---
# Vue d&#39;ensemble de l&#39;int&#233;gration &#224; des applications COM+
[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] fournit un environnement riche permettant de créer des applications distribuées.  Si vous vous servez déjà d'une logique d'application à base de composants hébergée dans COM\+, vous pouvez utiliser [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] pour étendre votre logique existante plutôt que devoir la réécrire.  Un scénario courant consiste à exposer une logique métier COM\+ ou Enterprise Services existante par le biais de services Web.  
  
 Lorsqu'une interface sur un composant COM\+ est exposée comme service Web, la spécification et le contrat de ces services sont déterminés par un mappage automatique exécuté au moment de l'initialisation de l'application.  La liste suivante présente le modèle conceptuel de ce mappage :  
  
-   Un service est défini pour chaque classe COM exposée.  
  
-   Le contrat du service est directement dérivé de la définition d'interface du composant sélectionné avec la possibilité d'exclusion de méthode définie dans la configuration.  
  
-   Les opérations figurant dans ce contrat sont directement dérivées des méthodes sur la définition d'interface du composant.  
  
-   Les paramètres de ces opérations sont directement dérivés du type d'interopérabilité COM qui correspond aux paramètres de méthode du composant.  
  
 Les adresses et liaisons de transport par défaut du service sont fournies dans un fichier de configuration de service, mais elles peuvent être reconfigurées si nécessaire.  
  
> [!NOTE]
>  Les contrats des services Web exposés restent constants tant que les interfaces et la configuration COM\+ restent inchangées.  La modification de plusieurs interfaces ne met pas automatiquement à jour les services disponibles et requiert de réexécuter l'outil ComSvcConfig.exe \(COM\+ Service Model Configuration\).  
  
 Les spécifications d'authentification et d'autorisation de l'application COM\+ et de ses composants continuent d'être appliquées lorsqu'elle sert de service Web.  
  
 Si l'appelant initie une transaction de service Web, les composants marqués comme transactionnels s'inscrivent dans l'étendue de cette transaction.  
  
 Les étapes suivantes sont requises pour exposer l'interface d'un composant COM\+ comme service Web sans modifier le composant :  
  
1.  Déterminez si l'interface du composant COM\+ peut être exposée comme service Web.  
  
2.  Sélectionnez un mode d'hébergement approprié.  
  
3.  Utilisez l'outil ComSvcConfig.exe \(COM\+ Service Model Configuration\) pour ajouter un service Web pour l'interface.  [!INCLUDE[crabout](../../../../includes/crabout-md.md)] l'utilisation de ComSvcConfig.exe, consultez [Comment : utiliser l'outil de configuration de modèle de service COM\+](../../../../docs/framework/wcf/feature-details/how-to-use-the-com-service-model-configuration-tool.md).  
  
4.  Configurez les paramètres de service supplémentaires dans le fichier de configuration de l'application.  [!INCLUDE[crabout](../../../../includes/crabout-md.md)] la configuration d'un composant, consultez [Comment : configurer des paramètres de service COM\+](../../../../docs/framework/wcf/feature-details/how-to-configure-com-service-settings.md).  
  
## Interfaces prises en charge  
 Certaines restrictions s'appliquent aux types d'interfaces qui peuvent être exposés comme service Web.  Les types d'interfaces suivants ne sont pas pris en charge :  
  
-   Les interfaces qui passent des références d'objet comme paramètres \- voir la section concernant la prise en charge limitée des références d'objet.  
  
-   Les interfaces qui passent des types incompatibles avec les conversions d'interopérabilité COM [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)].  
  
-   Les interfaces des applications dont la fonction de regroupement d'applications est activée lorsqu'elles sont hébergées par COM\+.  
  
-   Les interfaces des composants marqués comme privés pour l'application.  
  
-   Les interfaces d'infrastructure COM\+.  
  
-   Les interfaces issues de l'application système.  
  
-   Les interfaces issues des composants Enterprise Services qui n'ont pas été ajoutés au cache GAC \(Global Assembly Cache\).  
  
### Prise en charge limitée des références d'objet  
 Comme un certain nombre de composants COM\+ déployés n'utilisent pas de paramètres de référence d'objet, pour retourner un objet ADO Recordset par exemple, l'intégration COM\+ inclut une prise en charge limitée des paramètres de référence d'objet.  Cette prise en charge est limitée aux objets qui implémentent l'interface COM `IPersistStream`.  Elle inclut les objets ADO Recordset et peut être implémentée pour des objets COM propres à l'application.  
  
 Pour activer cette prise en charge, l'outil ComSvcConfig.exe fournit le commutateur **allowreferences** qui désactive le paramètre de signature de méthode normal et vérifie que l'outil s'exécute pour garantir que les paramètres de référence d'objet ne sont pas utilisés.  De plus, les types d'objet que vous passez comme paramètres doivent être nommés et identifiés dans l'élément de configuration \<`persistableTypes`\> qui est un enfant de l'élément \<`comContract`\>.  
  
 Lorsque cette fonctionnalité est activée, le service d'intégration COM\+ utilise l'interface `IPersistStream` pour sérialiser ou désérialiser l'instance d'objet.  Si l'instance d'objet ne prend pas en charge `IPersistStream`, une exception est levée.  
  
 Dans une application cliente, les méthodes sur l'objet <xref:System.ServiceModel.ComIntegration.PersistStreamTypeWrapper> peuvent être utilisées pour passer un objet dans un service, et pour récupérer un objet de la même façon.  
  
> [!NOTE]
>  En raison de la nature personnalisée et propre à la plateforme de la méthode de sérialisation, il est préférable de l'utiliser entre des clients [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] et des services [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].  
  
## Sélection du mode d'hébergement  
 COM\+ expose des services Web dans l'un des modes d'hébergement suivants :  
  
-   Hébergé par COM\+  
  
     Le service Web est hébergé dans le processus serveur COM\+ dédié de l'application \(Dllhost.exe\).  Ce mode requiert le démarrage explicite de l'application pour qu'elle puisse recevoir des demandes de service Web.  Les options COM\+ « Exécuter en tant que service NT » ou « Ne pas arrêter l'exécution lors de l'inactivité » peuvent être utilisées pour empêcher l'arrêt de l'application et de ses services suite à une période d'inactivité.  Ce mode fournit à la fois un service Web et un accès DCOM à l'application serveur.  
  
-   Hébergé sur le Web  
  
     Le service Web est hébergé dans un processus de travail de serveur Web.  Ce mode ne requiert pas l'activation de COM\+ lorsque la demande initiale est reçue.  Si l'application n'est pas active lorsque cette demande est reçue, elle est activée automatiquement avant le traitement de la demande.  Ce mode fournit également un service Web et un accès DCOM à l'application serveur, mais provoque un saut de processus pour les demandes de service Web.   En général, il requiert l'activation de l'emprunt d'identité par le client.  Dans [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], cette opération peut être réalisée avec la propriété <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> de la classe <xref:System.ServiceModel.Security.WindowsClientCredential>, accessible en tant que propriété de la classe générique <xref:System.ServiceModel.ChannelFactory%601>, ainsi que la valeur d'énumération <xref:System.Security.Principal.TokenImpersonationLevel>.  
  
-   Hébergé sur le Web dans un processus  
  
     Le service Web et la logique d'application COM\+ sont hébergés dans le processus de travail de serveur Web.  Cela permet l'activation automatique du mode Hébergé sur le Web sans provoquer de saut de processus pour les demandes de service Web.  L'inconvénient est que l'application serveur n'est pas accessible par le biais de DCOM.  
  
### Considérations relatives à la sécurité  
 Comme pour d'autres services [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], les paramètres de sécurité du service exposé sont administrés à l'aide des paramètres de configuration du canal [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].  D'ordinaire, les paramètres de sécurité DCOM, tels que les paramètres des autorisations DCOM à l'échelle de l'ordinateur, ne sont pas appliqués.  Pour appliquer des rôles d'application COM\+, l'autorisation « Appliquer les vérifications d'accès au niveau du composant » doit être activée pour le composant.  
  
 L'utilisation d'une liaison non sécurisée peut exposer la communication à des risques de falsification et de divulgation d'informations.  Pour éviter ce problème, il est recommandé de recourir à une liaison sécurisée.  
  
 Pour les modes Hébergé par COM\+ et Hébergé sur le Web, les applications clientes doivent autoriser le processus serveur à emprunter l'identité de l'utilisateur client.  Cette procédure peut s'effectuer sur les clients [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] en affectant au niveau d'emprunt d'identité la valeur <xref:System.Security.Principal.TokenImpersonationLevel>.  
  
 Lorsque les services IIS \(Internet Information Services\) ou le service d'activation de processus de Windows \(WAS, Windows Process Activation Service\) utilisent le transport HTTP, l'outil Httpcfg.exe peut être exécuté pour réserver une adresse de point de terminaison de transport.  Dans d'autres configurations, il est important de se protéger contre les services non autorisés qui se comportent comme le service prévu.  Pour empêcher un service non autorisé de démarrer au point de terminaison souhaité, le service légitime peut être configuré pour s'exécuter comme un service NT.  Cette méthode permet au service légitime de revendiquer l'adresse du point de terminaison avant un service non autorisé.  
  
 Lors de l'exposition d'une application COM\+ avec des rôles COM\+ configurés comme un service hébergé sur le Web, le paramètre « Démarrer le compte de l'invité Internet » doit être ajouté à l'un des rôles de l'application.  Ce compte, généralement appelé IWAM\_nom\_ordinateur, doit être ajouté pour activer l'arrêt normal des objets après leur utilisation.  Aucune autre autorisation ne doit être accordée au compte.  
  
 Les fonctionnalités de recyclage de processus COM\+ ne peuvent pas être utilisées sur des applications intégrées.  Si l'application est configurée pour utiliser le recyclage de processus et que les composants s'exécutent dans un processus hébergé par COM\+, le service ne démarre pas.  Cette exigence ne concerne pas les services utilisant le mode Hébergé sur le Web dans un processus car les paramètres de recyclage de processus ne sont pas appliqués.  
  
## Voir aussi  
 [Vue d'ensemble de l'intégration à des applications COM](../../../../docs/framework/wcf/feature-details/integrating-with-com-applications-overview.md)