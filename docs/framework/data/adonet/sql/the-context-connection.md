---
title: "Connexion de contexte | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: e443ca86-9243-4234-a822-ed10a53a9de0
caps.latest.revision: 4
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 4
---
# Connexion de contexte
Le problème de l'accès aux données interne est un scénario assez courant.  Vous souhaitez accéder au même serveur, celui sur lequel s'exécute votre procédure stockée Common Language Runtime \(CLR\) ou fonction.  Une option consiste à créer une connexion à l'aide de <xref:System.Data.SqlClient.SqlConnection>, à spécifier une chaîne de connexion pointant vers le serveur local, puis à ouvrir la connexion.  Cela nécessite la spécification d'informations d'identification pour la connexion.  La connexion s'opère dans une session de base de données différente de la procédure stockée ou de la fonction, elle peut avoir des options `SET` différentes, elle est dans une transaction séparée, elle ne voit pas vos tables temporaires, etc.  Si votre code de fonction ou de procédure stockée managée s'exécute dans le processus SQL Server, c'est parce que quelqu'un s'est connecté à ce serveur et a exécuté une instruction SQL pour l'invoquer.  Vous souhaitez probablement que la fonction ou la procédure stockée s'exécute dans le contexte de cette connexion, ainsi qu'avec sa transaction, les options `SET`, etc.  C'est ce que l'on appelle une connexion de contexte.  
  
 La connexion de contexte vous permet d'exécuter des instructions Transact\-SQL dans le même contexte que celui dans lequel votre code a été invoqué en premier lieu.  Pour obtenir des informations plus détaillées, voir la documentation en ligne de SQL Server pour la version de SQL Server que vous utilisez.  
  
 **Documentation en ligne de SQL Server**  
  
1.  [La connexion de contexte](http://go.microsoft.com/fwlink/?LinkId=115395)  
  
## Voir aussi  
 [Creating SQL Server 2005 Objects In Managed Code](http://msdn.microsoft.com/fr-fr/5358a825-e19b-49aa-8214-674ce5fed1da)   
 [Fournisseurs managés ADO.NET et Centre de développement de DataSet](http://go.microsoft.com/fwlink/?LinkId=217917)