---
title: "Compteurs de performance d&#39;op&#233;ration | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 333a51e0-f56e-4e1a-b359-5c91ff390568
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# Compteurs de performance d&#39;op&#233;ration
Les compteurs de performance d'opération se trouvent sous l'objet de performance `ServiceModelOperation 4.0.0.0` lors de l'affichage avec l'analyseur de performances \(Perfmon.exe\).Chaque opération a une instance individuelle.En d'autres termes, si un contrat donné a 10 opérations, 10 instances du compteur d'opération sont associées à ce contrat.Les instances d'objet sont nommées à l'aide du modèle suivant :  
  
```  
(ServiceName).(ContractName).(OperationName)@(first endpoint listener address)  
```  
  
 Ce compteur vous permet de mesurer la manière dont l'appel est utilisé et comment l'opération s'exécute.  
  
> [!CAUTION]
>  La longueur du nom d'une instance de compteur de performance est limitée.Lorsqu'un nom d'instance de compteur [!INCLUDE[indigo1](../../../../../includes/indigo1-md.md)] dépasse la longueur maximale, [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] remplace une partie du nom par une valeur de hachage.  
  
## Voir aussi  
 [Compteurs de performance](../../../../../docs/framework/wcf/diagnostics/performance-counters/index.md)