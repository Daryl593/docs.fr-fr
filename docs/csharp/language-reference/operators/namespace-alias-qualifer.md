---
title: "::, opérateur (référence C#)"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- ::_CSharpKeyword
dev_langs:
- CSharp
helpviewer_keywords:
- ':: operator [C#]'
- 'namespaces [C#], :: operator'
- namespace alias qualifier operator (::) [C#]
ms.assetid: 698b5a73-85cf-4e0e-9e8e-6496887f8527
caps.latest.revision: 21
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 97b9fa706669244ac579602a107c092e8593567d
ms.contentlocale: fr-fr
ms.lasthandoff: 07/28/2017

---
# <a name="-operator-c-reference"></a>::, opérateur (référence C#)
Le qualificateur d’alias d’espace de noms (`::`) s’utilise pour rechercher les identificateurs. Il se place toujours entre deux identificateurs, comme dans cet exemple :  
  
 [!code-cs[csRefOperators#27](../../../csharp/language-reference/operators/codesnippet/CSharp/namespace-alias-qualifer_1.cs)]  
  
## <a name="remarks"></a>Notes  
 Le qualificateur d’alias d’espace de noms peut être `global`. Il appelle alors une recherche dans l’espace de noms global, plutôt que dans un espace de noms avec alias.  
  
## <a name="for-more-information"></a>Pour plus d'informations  
 Pour obtenir un exemple d’utilisation de l’opérateur `::`, consultez la section suivante :  
  
-   [Guide pratique pour utiliser l’alias d’espace de noms global](../../../csharp/programming-guide/namespaces/how-to-use-the-global-namespace-alias.md)  
  
## <a name="c-language-specification"></a>Spécification du langage C#  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur C#](../../../csharp/language-reference/index.md)   
 [Guide de programmation C#](../../../csharp/programming-guide/index.md)   
 [Opérateurs C#](../../../csharp/language-reference/operators/index.md)   
 [Mots clés d’espaces de noms](../../../csharp/language-reference/keywords/namespace-keywords.md)   
 [. , opérateur](../../../csharp/language-reference/operators/member-access-operator.md)   
 [extern alias](../../../csharp/language-reference/keywords/extern-alias.md)

