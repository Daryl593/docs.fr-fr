---
title: "Effectuer des jointures groupées"
description: "Guide pratique pour effectuer des jointures groupées."
keywords: .NET, .NET Core, C#
author: BillWagner
manager: wpickett
ms.author: wiwagn
ms.date: 12/1/2016
ms.topic: article
ms.prod: .net-core
ms.technology: .net-core-technologies
ms.devlang: dotnet
ms.assetid: 9667daf9-a5fd-4b43-a5c4-a9c2b744000e
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 0410c5f673e61f91c00a69cb1659e0d72852f128
ms.contentlocale: fr-fr
ms.lasthandoff: 07/28/2017

---
# <a name="perform-grouped-joins"></a>Effectuer des jointures groupées

La jointure groupée est utile pour produire des structures de données hiérarchiques. Elle associe chaque élément de la première collection à un jeu d’éléments corrélés de la deuxième collection.  
  
 Par exemple, une classe ou une table de base de données relationnelle nommée `Student` peut contenir deux champs : `Id` et `Name`. Une deuxième classe ou table de base de données relationnelle nommée `Course` peut contenir deux champs : `StudentId` et `CourseTitle`. Une jointure groupée de ces deux sources de données, basée sur les champs `Student.Id` et `Course.StudentId` correspondants, regroupe chaque `Student` avec une collection d’objets `Course` (qui peut être vide).  
  
> [!NOTE]
>  Chaque élément de la première collection apparaît dans le jeu de résultats d’une jointure groupée, même si des éléments corrélés sont trouvés dans la deuxième collection. Si aucun élément corrélé n’est trouvé, la séquence d’éléments corrélés pour cet élément est vide. Le sélecteur de résultats a donc accès à chaque élément de la première collection. Cela n’est pas le cas du sélecteur de résultats dans une jointure non groupée, qui ne peut pas accéder à des éléments de la première collection qui n’ont aucune correspondance dans la deuxième collection.  
  
 Le premier exemple de cette rubrique montre comment effectuer une jointure groupée. Le deuxième exemple montre comment utiliser une jointure groupée pour créer des éléments XML.  
  
## <a name="example"></a>Exemple  
  
### <a name="group-join-example"></a>Exemple de jointure groupée  
 L’exemple suivant effectue une jointure groupée d’objets de types `Person` et `Pet` où `Person` correspond à la propriété `Pet.Owner`. Contrairement à une jointure non groupée qui produirait une paire d’éléments pour chaque correspondance, la jointure groupée produit un seul objet résultant pour chaque élément de la première collection, qui est un objet `Person` dans cet exemple. Les éléments correspondants de la deuxième collection, qui sont des objets `Pet` dans cet exemple, sont regroupés dans une collection. Enfin, le sélecteur de résultats crée un type anonyme pour chaque correspondance qui se compose de `Person.FirstName` et d’une collection d’objets `Pet`.  
  
 [!code-cs[CsLINQProgJoining#5](../../../samples/snippets/csharp/concepts/linq/how-to-perform-grouped-joins_1.cs)]  
  
## <a name="example"></a>Exemple  
  
### <a name="group-join-to-create-xml-example"></a>Exemple de jointure groupée pour créer des éléments XLM  
 Les jointures groupées sont appropriées pour créer des éléments XML à l’aide de LINQ to XML. L’exemple suivant est similaire à l’exemple précédent, sauf qu’au lieu de créer des types anonymes, le sélecteur de résultats crée des éléments XML qui représentent les objets joints.  
  
 [!code-cs[CsLINQProgJoining#6](../../../samples/snippets/csharp/concepts/linq/how-to-perform-grouped-joins_2.cs)]  
 
## <a name="see-also"></a>Voir aussi  
 <xref:System.Linq.Enumerable.Join%2A>   
 <xref:System.Linq.Enumerable.GroupJoin%2A>   
 [Effectuer des jointures internes](perform-inner-joins.md)   
 [Effectuer des jointures externes gauches](perform-left-outer-joins.md)   
 [Types anonymes](../programming-guide/classes-and-structs/anonymous-types.md)   
 

