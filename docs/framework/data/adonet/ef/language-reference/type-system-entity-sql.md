---
title: "System, type (Entity SQL) | Microsoft Docs"
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
  - "ESQL"
ms.assetid: 818a505b-a196-41dd-aaac-2ccd5f7a2f1a
caps.latest.revision: 3
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 3
---
# System, type (Entity SQL)
[!INCLUDE[esql](../../../../../../includes/esql-md.md)] prend en charge plusieurs types :  
  
-   Les types primitifs \(simples\), tels que `Int32` et `String.`  
  
-   Les types nominaux qui sont définis dans le schéma, tels que <xref:System.Data.Metadata.Edm.EntityType>, <xref:System.Data.Metadata.Edm.ComplexType> et <xref:System.Data.Metadata.Edm.RelationshipType>.  
  
-   Les types anonymes qui ne sont pas définis explicitement dans le schéma : <xref:System.Data.Metadata.Edm.CollectionType>, <xref:System.Data.Metadata.Edm.RowType> et <xref:System.Data.Metadata.Edm.RefType>.  
  
 Cette section décrit les types anonymes qui ne sont pas définis explicitement dans le schéma, mais qui sont prises en charge par [!INCLUDE[esql](../../../../../../includes/esql-md.md)].  Pour plus d'informations sur les types primitifs et nominaux, voir [Conceptual Model Types \(CSDL\)](http://msdn.microsoft.com/fr-fr/987b995f-e429-4569-9559-b4146744def4).  
  
## Lignes  
 La structure d'une ligne dépend de la séquence de membres typés et nommés que la ligne contient.  Un type de ligne n'a aucune identité et ne peut pas faire l'objet d'un héritage.  Les instances du même type de ligne sont équivalentes si les membres sont respectivement équivalents.  Les lignes n'ont aucun comportement au\-delà de leur équivalence structurelle et n'ont aucun équivalent dans le Common Language Runtime.  Les requêtes peuvent donner des structures qui contiennent des lignes ou des collections de lignes.  La liaison d'API entre les requêtes [!INCLUDE[esql](../../../../../../includes/esql-md.md)] et le langage hôte définit la façon dont les lignes sont réalisées dans la requête qui a produit le résultat.  Pour plus d'informations sur la façon de construire une instance de ligne, voir [Construction de types](../../../../../../docs/framework/data/adonet/ef/language-reference/constructing-types-entity-sql.md).  
  
## Collections  
 Les types de collections représentent zéro instance ou plus d'autres objets.  Pour plus d'informations sur la façon de construire une collection, voir [Construction de types](../../../../../../docs/framework/data/adonet/ef/language-reference/constructing-types-entity-sql.md).  
  
## Références  
 Une référence est un pointeur logique vers une entité spécifique dans un jeu d'entités spécifique.  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] prend en charge les opérateurs suivants pour construire, déconstruire et explorer les références :  
  
-   [REF](../../../../../../docs/framework/data/adonet/ef/language-reference/ref-entity-sql.md)  
  
-   [CREATEREF](../../../../../../docs/framework/data/adonet/ef/language-reference/createref-entity-sql.md)  
  
-   [KEY](../../../../../../docs/framework/data/adonet/ef/language-reference/key-entity-sql.md)  
  
-   [DEREF](../../../../../../docs/framework/data/adonet/ef/language-reference/deref-entity-sql.md)  
  
 Vous pouvez explorer une référence en utilisant l'opérateur `.` \(point\) d'accès au membre.  L'extrait de code suivant extrait la propriété ID \(de Order\) en explorant la propriété r \(référence\).  
  
```  
select o2.r.Id   
from (select ref(o) as r from LOB.Orders as o) as o2   
```  
  
 Si la valeur de la référence est Null, si la cible de la référence n'existe pas, le résultat est null.  
  
## Voir aussi  
 [Vue d'ensemble d'Entity SQL](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-overview.md)   
 [Référence Entity SQL](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-reference.md)   
 [CAST](../../../../../../docs/framework/data/adonet/ef/language-reference/cast-entity-sql.md)   
 [Spécifications CSDL, SSDL et MSL](../../../../../../docs/framework/data/adonet/ef/language-reference/csdl-ssdl-and-msl-specifications.md)