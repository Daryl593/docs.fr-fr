---
title: "Validation des relations des activit&#233;s | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 6f11a34e-ed67-4bce-88ce-7e96bbb4d052
caps.latest.revision: 8
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 8
---
# Validation des relations des activit&#233;s
Cet exemple se compose de trois activités, `CreateCity`, `CreateState` et `CreateCountry`.`CreateCity` doit être à l'intérieur d'une activité `CreateState`, et `CreateState` à l'intérieur d'une activité `CreateCountry`.Pour les besoins de cet exemple, la logique de validation est dans le code pour l'activité `CreateState`, et dans XAML pour l'activité `CreateCity`.Les deux contraintes ont le même comportement.  
  
 [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] fournit les trois activités d'assistance suivantes qui permettent au développeur de valider les relations entre les activités.  
  
 <xref:System.Activities.Validation.GetParentChain>  
 Fournit la collection de toutes les activités qui appartiennent au parent du nœud actuel  
  
 <xref:System.Activities.Validation.GetChildSubtree>  
 Fournit la collection de toutes les activités qui appartiennent à la sous\-arborescence du nœud actuel, à l'exclusion du nœud actuel  
  
 <xref:System.Activities.Validation.GetWorkflowTree>  
 Fournit la collection de toutes les activités de la même arborescence que le nœud actuel  
  
 Dans l'exemple, une activité <xref:System.Activities.Statements.ForEach%601> est utilisée pour parcourir la collection retournée par <xref:System.Activities.Validation.GetParentChain>Pour chaque élément de la collection, son type est comparé à `CreateCountry` \(ou à `CreateState` si `CreateCity` est validé\). Une fois que qu'une correspondance est trouvée, l'indicateur de résultat a la valeur `true`.Enfin, un <xref:System.Activities.Validation.AssertValidation> est utilisé pour générer une erreur de validation si l'indicateur de résultat a la valeur `false`.  
  
### Pour utiliser cet exemple  
  
1.  Ouvrez l'exemple de solution ContainmentValidation.sln dans [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)].  
  
2.  Générez la solution.  
  
3.  Appuyez sur Ctrl\+F5 pour lancer le programme.  
  
> [!IMPORTANT]
>  Les exemples peuvent déjà être installés sur votre ordinateur.Recherchez le répertoire \(par défaut\) suivant avant de continuer :  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page \(éventuellement en anglais\) des [exemples Windows Communication Foundation \(WCF\) et Windows Workflow Foundation \(WF\) pour .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) pour télécharger tous les exemples [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] et [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Cet exemple se trouve dans le répertoire suivant :  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WF\Basic\Validation\ActivityRelationships`  
  
## Voir aussi