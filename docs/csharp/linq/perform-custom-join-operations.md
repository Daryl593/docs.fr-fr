---
title: "Effectuer des opérations de jointure personnalisées"
description: "Guide pratique pour effectuer des opérations de jointure personnalisées."
keywords: .NET, .NET Core, C#
author: BillWagner
manager: wpickett
ms.author: wiwagn
ms.date: 12/1/2016
ms.topic: article
ms.prod: .net-core
ms.technology: .net-core-technologies
ms.devlang: dotnet
ms.assetid: 56a2a4a5-7299-497d-b3c3-23c848678911
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 51bdae75346022a7564fdb50e582c143e7762a1f
ms.contentlocale: fr-fr
ms.lasthandoff: 07/28/2017

---
# <a name="perform-custom-join-operations"></a>Effectuer des opérations de jointure personnalisées

Cet exemple montre comment effectuer des opérations de jointure qui ne sont pas possibles avec la clause `join`. Dans une expression de requête, la clause `join` est limitée à et optimisée pour les équijointures, qui sont de loin le type le plus courant d’opération de jointure. Quand vous effectuez une équijointure, l’utilisation de la clause `join` offre généralement les meilleures performances.  
  
 Toutefois, la clause `join` ne peut pas être utilisée dans les cas suivants :  
  
-   Quand la jointure est basée sur une expression de prédicat d’inégalité (une non-équijointure).  
  
-   Quand la jointure est basée sur plusieurs expressions de prédicat d’égalité ou d’inégalité.  
  
-   Quand vous devez introduire une variable de portée temporaire pour la séquence de droite (interne) avant l’opération de jointure.  
  
 Pour effectuer des jointures qui ne sont pas des équijointures, vous pouvez utiliser plusieurs clauses `from` pour introduire chaque source de données indépendamment. Vous appliquez ensuite une expression de prédicat dans une clause `where` à la variable de portée pour chaque source. L’expression peut également prendre la forme d’un appel de méthode.  
  
> [!NOTE]
>  Ne confondez pas ce type d’opération de jointure personnalisée avec l’utilisation de plusieurs clauses `from` permettant d’accéder aux collections internes. Pour plus d’informations, consultez [join, clause](../language-reference/keywords/join-clause.md).  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, la première méthode présente une jointure croisée simple. Les jointures croisées doivent être utilisées avec précaution, car elles peuvent produire des jeux de résultats très volumineux. Toutefois, elles peuvent être utiles dans certains scénarios pour créer des séquences sources sur lesquelles exécuter des requêtes supplémentaires.  
  
 La deuxième méthode produit une séquence de tous les produits dont l’ID de catégorie est répertorié dans la liste des catégories à gauche. Notez l’utilisation de la clause `let` et de la méthode `Contains` pour créer un tableau temporaire. Il est également possible de créer le tableau avant la requête et de supprimer la première clause `from`.  
  
 [!code-cs[csProgGuideLINQ#64](../../../samples/snippets/csharp/concepts/linq/how-to-perform-custom-join-operations_1.cs)]  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, la requête doit joindre deux séquences sur la base des clés correspondantes qui, pour la séquence interne (à droite), ne peuvent pas être obtenues avant la clause de jointure elle-même. Si cette jointure a été effectuée avec une clause `join`, la méthode `Split` doit être appelée pour chaque élément. L’utilisation de plusieurs clauses `from` permet d’éviter à la requête la surcharge inhérente à la répétition de l’appel de méthode. Toutefois, comme la clause `join` est optimisée, dans ce cas particulier, son utilisation peut s’avérer plus rapide que d’utiliser plusieurs clauses `from`. Les résultats dépendront principalement de la surcharge liée à l’appel de la méthode.  
  
 [!code-cs[csProgGuideLINQ#13](../../../samples/snippets/csharp/concepts/linq/how-to-perform-custom-join-operations_2.cs)]  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions de requête LINQ](index.md)   
 [join, clause](../language-reference/keywords/join-clause.md)   
 [Classer les résultats d’une clause Join](order-the-results-of-a-join-clause.md)

