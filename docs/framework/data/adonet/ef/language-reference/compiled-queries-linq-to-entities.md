---
title: "Requ&#234;tes compil&#233;es (LINQ to Entities) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
ms.assetid: 8025ba1d-29c7-4407-841b-d5a3bed40b7a
caps.latest.revision: 5
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 5
---
# Requ&#234;tes compil&#233;es (LINQ to Entities)
Lorsque vous possédez une application qui exécute de nombreuses fois des requêtes similaires d'un point de vue structurel dans Entity Framework, vous pouvez souvent améliorer les performances en compilant la requête une fois et en l'exécutant plusieurs fois avec des paramètres différents.  Par exemple, une application peut avoir besoin de récupérer tous les clients d'une ville spécifique ; la ville est spécifiée à l'exécution par l'utilisateur dans un formulaire.  À cette fin, LINQ to Entities prend en charge l'utilisation des requêtes compilées.  
  
 Depuis le .NET Framework 4.5, les requêtes LINQ sont mises en cache automatiquement.  Cependant, vous pouvez toujours utiliser des requêtes LINQ compilées pour réduire ce coût dans les exécutions ultérieures et les requêtes compilées peuvent être plus efficaces que les requêtes LINQ qui sont automatiquement mises en cache.  Notez que les requêtes LINQ to Entities qui appliquent l'opérateur `Enumerable.Contains` aux collections en mémoire ne sont pas automatiquement mises en cache.  Le paramétrage des collections en mémoire dans les requêtes LINQ compilées n'est pas autorisé.  
  
 La classe <xref:System.Data.Objects.CompiledQuery> permet la compilation et la mise en cache des requêtes en vue de leur réutilisation.  Conceptuellement, cette classe contient une méthode <xref:System.Data.Objects.CompiledQuery>'s `Compile` avec plusieurs surcharges.  Appelez la méthode `Compile` pour créer un nouveau délégué pour représenter la requête compilée.  Les méthodes `Compile`, fournies avec un objet <xref:System.Data.Objects.ObjectContext> et des valeurs de paramètres, retournent un délégué qui produit un résultat \(tel qu'une instance de <xref:System.Linq.IQueryable%601>\).  La requête compile une fois durant la première exécution uniquement.  Les options de fusion définies pour la requête au moment de la compilation ne peuvent pas être modifiées ultérieurement.  Une fois la requête compilée vous pouvez seulement fournir des paramètres de type primitif, mais vous ne pouvez pas remplacer des parties de la requête qui modifieraient le SQL généré.  Pour plus d'informations, consultez [Options de fusion Entity Framework et requêtes compilées](http://go.microsoft.com/fwlink/?LinkId=199591)  
  
 L'expression de requête [!INCLUDE[linq_entities](../../../../../../includes/linq-entities-md.md)] que compile la méthode `Compile` de <xref:System.Data.Objects.CompiledQuery> est représentée par l'un des délégués `Func` génériques, tels que <xref:System.Func%605>.  Au maximum, l'expression de requête peut encapsuler un paramètre `ObjectContext`, un paramètre de retour et seize paramètres de requête.  Si plus de seize paramètres de requête sont requis, vous pouvez créer une structure dont les propriétés représentent des paramètres de requête.  Vous pouvez alors utiliser les propriétés sur la structure dans l'expression de requête une fois les propriétés définies.  
  
## Exemple  
 L'exemple suivant compile puis appelle une requête qui accepte un paramètre d'entrée <xref:System.Decimal> et retourne une séquence de commandes où le montant total dû est supérieur à 200,00 $ :  
  
 [!code-csharp[DP L2E Conceptual Examples#CompiledQuery2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery2)]
 [!code-vb[DP L2E Conceptual Examples#CompiledQuery2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery2)]  
  
## Exemple  
 L'exemple suivant compile puis appelle une requête qui retourne une instance de <xref:System.Data.Objects.ObjectQuery%601> :  
  
 [!code-csharp[DP L2E Conceptual Examples#CompiledQuery1_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery1_mq)]
 [!code-vb[DP L2E Conceptual Examples#CompiledQuery1_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery1_mq)]  
  
## Exemple  
 L'exemple suivant compile puis appelle une requête qui retourne la moyenne des prix courants des produits sous la forme d'une valeur <xref:System.Decimal> :  
  
 [!code-csharp[DP L2E Conceptual Examples#CompiledQuery3_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery3_mq)]
 [!code-vb[DP L2E Conceptual Examples#CompiledQuery3_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery3_mq)]  
  
## Exemple  
 L'exemple suivant compile puis appelle une requête qui accepte un paramètre d'entrée <xref:System.String> puis retourne un `Contact` dont l'adresse de messagerie commence par la chaîne spécifiée :  
  
 [!code-csharp[DP L2E Conceptual Examples#CompiledQuery4_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery4_mq)]
 [!code-vb[DP L2E Conceptual Examples#CompiledQuery4_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery4_mq)]  
  
## Exemple  
 L'exemple suivant compile puis appelle une requête qui accepte les paramètres d'entrée <xref:System.DateTime> et <xref:System.Decimal> et retourne une séquence de commandes où la date de commande est ultérieure au 8 mars 2003 et le montant total dû est inférieur à 300,00 $ :  
  
 [!code-csharp[DP L2E Conceptual Examples#CompiledQuery5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery5)]
 [!code-vb[DP L2E Conceptual Examples#CompiledQuery5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery5)]  
  
## Exemple  
 L'exemple suivant compile puis appelle une requête qui accepte un paramètre d'entrée <xref:System.DateTime> et retourne une séquence de commandes dont la date de commande est postérieure au 8 mars 2004.  Cette requête retourne les informations de commande sous la forme d'une séquence de types anonymes.  Les types anonymes sont déduits par le compilateur, si bien que vous ne pouvez pas spécifier les paramètres de type dans la méthode `Compile` de <xref:System.Data.Objects.CompiledQuery> et le type est défini dans la requête elle\-même.  
  
 [!code-csharp[DP L2E Conceptual Examples#CompiledQuery6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery6)]
 [!code-vb[DP L2E Conceptual Examples#CompiledQuery6](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery6)]  
  
## Exemple  
 L'exemple suivant compile puis appelle une requête qui accepte un paramètre d'entrée de la structure définie par l'utilisateur, puis retourne une séquence de commandes.  La structure définit les paramètres de requête de date de début, de date de fin et de montant total dû, et la requête retourne les commandes expédiées entre le 3 mars et le 8 mars 2003 dont le montant total dû est supérieur à 700,00 $.  
  
 [!code-csharp[DP L2E Conceptual Examples#CompiledQuery7](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery7)]
 [!code-vb[DP L2E Conceptual Examples#CompiledQuery7](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery7)]  
  
 La structure qui définit les paramètres de requête :  
  
 [!code-csharp[DP L2E Conceptual Examples#MyParamsStruct](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#myparamsstruct)]
 [!code-vb[DP L2E Conceptual Examples#MyParamsStruct](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#myparamsstruct)]  
  
## Voir aussi  
 [ADO.NET Entity Framework](../../../../../../docs/framework/data/adonet/ef/index.md)   
 [LINQ to Entities](../../../../../../docs/framework/data/adonet/ef/language-reference/linq-to-entities.md)   
 [Options de fusion Entity Framework et requêtes compilées](http://go.microsoft.com/fwlink/?LinkId=199591)