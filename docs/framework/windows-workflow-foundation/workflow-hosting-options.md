---
title: "Options d&#39;h&#233;bergement de workflow | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 37bcd668-9c5c-4e7c-81da-a1f1b3a16514
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# Options d&#39;h&#233;bergement de workflow
La plupart des exemples [!INCLUDE[wf](../../../includes/wf-md.md)] utilisent des workflows hébergés dans une application console, mais il ne s'agit pas d'un scénario réaliste pour les workflows réels.Les workflows dans les applications d'entreprise réelles sont hébergés dans des processus persistants, un service Windows créé par le développeur ou une application serveur telle que [!INCLUDE[iisver](../../../includes/iisver-md.md)] ou AppFabric.Les différences entre ces approches sont les suivantes.  
  
## Hébergement de workflows dans IIS avec Windows AppFabric  
 IIS avec AppFabric est l'hôte par défaut pour les workflows.L'application hôte de workflows utilisant AppFabric est WAS \(Windows Activation Services\), qui supprime la dépendance de HTTP sur IIS uniquement.  
  
## Hébergement de workflows dans IIS uniquement  
 Il n'est pas recommandé d'utiliser uniquement [!INCLUDE[iisver](../../../includes/iisver-md.md)], car il existe des outils de gestion et d'analyse disponibles avec AppFabric qui facilitent la maintenance des applications en cours d'exécution.Les workflows doivent être hébergés dans [!INCLUDE[iisver](../../../includes/iisver-md.md)] uniquement si vous rencontrez des problèmes d'infrastructure lors de la migration vers AppFabric.  
  
> [!WARNING]
>  [!INCLUDE[iisver](../../../includes/iisver-md.md)] recycle les pools d'applications régulièrement pour différentes raisons.Lorsqu'un pool d'applications est recyclé, IIS cesse de recevoir des messages de l'ancien pool, et instancie un pool d'applications pour recevoir de nouvelles demandes.Si un workflow continue de fonctionner après avoir envoyé une réponse, [!INCLUDE[iisver](../../../includes/iisver-md.md)] ne se rendra pas compte du travail effectué, et peut recycler le pool d'applications d'hébergement.Si cela se produit, le workflow s'interrompra, et les services de suivi enregistreront un message [1004 \- WorkflowInstanceAborted](../../../docs/framework/windows-workflow-foundation//1004-workflowinstanceaborted.md) avec un champ de raison vide.  
>   
>  Si la persistance est utilisée, l'hôte doit explicitement redémarrer les instances interrompues à partir du dernier point de persistance.  
>   
>  Si AppFabric est utilisé, le service de gestion de workflow reprend en fin de compte le workflow à partir du dernier point correct de persistance si la persistance est utilisée.Si aucune persistance n'est utilisée, et le workflow exécute des opérations en dehors d'un modèle de requête\/réponse, des données seront perdues lors de l'interruption du workflow.  
  
## Hébergement d'un workflow dans un service Windows personnalisé  
 Créer un service de workflow personnalisé pour héberger le workflow nécessite que le développeur duplique nombre des fonctionnalités prêtes à l'emploi fournies par AppFabric, mais offre plus de souplesse avec des fonctionnalités personnalisées.Cette option ne doit être prise en compte que si AppFabric s'avère ne pas être une option.