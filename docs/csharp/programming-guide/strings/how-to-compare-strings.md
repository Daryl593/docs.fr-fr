---
title: "Guide pratique pour comparer des chaînes (Guide de programmation C#)"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- strings [C#], comparison
- comparing strings [C#]
ms.assetid: e1268e28-ee98-4695-98e9-92280f1c33c0
caps.latest.revision: 23
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
ms.openlocfilehash: 494b9ef1a1e6c8958dd3df2edb44debf32690eeb
ms.contentlocale: fr-fr
ms.lasthandoff: 07/28/2017

---
# <a name="how-to-compare-strings-c-programming-guide"></a>Guide pratique pour comparer des chaînes (Guide de programmation C#)
Lorsque vous comparez des chaînes, vous produisez un résultat indiquant qu’une chaîne est supérieure ou inférieure à l’autre, ou que les deux chaînes sont égales. Les règles qui déterminent le résultat sont différentes selon que vous effectuez une *comparaison ordinale* ou une *comparaison dépendante de la culture*. Il est important d’utiliser le bon type de comparaison pour chaque tâche.  
  
 Utilisez les comparaisons ordinales de base lorsque vous devez comparer ou trier les valeurs de deux chaînes sans tenir compte des conventions linguistiques. Une comparaison ordinale de base (`System.StringComparison.Ordinal`) respecte la casse, ce qui signifie que les deux chaînes doivent correspondre, caractère pour caractère : « et » n’est pas égal à « Et » ni à « ET ». Une variation fréquemment utilisée est `System.StringComparison.OrdinalIgnoreCase`, qui permet une correspondance pour « et », « Et » et « ET ». `StringComparison.OrdinalIgnoreCase` est souvent utilisé pour comparer des noms de fichiers, des noms de chemins, des chemins réseau et toute autre chaîne dont la valeur ne change pas avec les paramètres régionaux de l’ordinateur. Pour plus d'informations, consultez <xref:System.StringComparison?displayProperty=fullName>.  
  
 Les comparaisons dépendantes de la culture sont généralement utilisées pour comparer et trier des chaînes entrées par les utilisateurs finaux, car les caractères et les conventions de tri de ces chaînes peuvent varier selon les paramètres régionaux de l’ordinateur. Même les chaînes qui contiennent des caractères identiques peuvent être triées différemment selon la culture du thread actuel.  
  
> [!NOTE]
>  Lorsque vous comparez des chaînes, vous devez utiliser les méthodes qui spécifient explicitement le type de comparaison que vous souhaitez effectuer. Cela rend votre code plus lisible et plus facile à gérer. Si possible, utilisez les surcharges des méthodes des classes <xref:System.String?displayProperty=fullName> et <xref:System.Array?displayProperty=fullName> qui prennent un paramètre d’énumération <xref:System.StringComparison>, afin de pouvoir spécifier le type de comparaison à effectuer. Il est préférable d’éviter d’utiliser les opérateurs `==` et `!=` lorsque vous comparez des chaînes. Évitez également d’utiliser les méthodes d’instance <xref:System.String.CompareTo%2A?displayProperty=fullName>, car aucune des surcharges ne prend un <xref:System.StringComparison>.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment comparer correctement des chaînes dont les valeurs ne changeront pas avec les paramètres régionaux de l’ordinateur. De plus, il montre également la fonctionnalité de *centralisation des chaînes* du langage C#. Lorsqu’un programme déclare plusieurs variables de chaînes identiques, le compilateur les stocke au même emplacement. En appelant la méthode <xref:System.Object.ReferenceEquals%2A>, vous pouvez voir que les deux chaînes référencent le même objet en mémoire. Utilisez la méthode <xref:System.String.Copy%2A?displayProperty=fullName> pour éviter la centralisation, comme illustré dans l’exemple.  
  
 [!code-cs[csProgGuideStrings#11](../../../csharp/programming-guide/strings/codesnippet/CSharp/how-to-compare-strings_1.cs)]  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment appliquer la méthode recommandée pour comparer des chaînes en utilisant les méthodes <xref:System.String?displayProperty=fullName> qui prennent une énumération <xref:System.StringComparison>. Notez que les méthodes d’instance <xref:System.String.CompareTo%2A?displayProperty=fullName> ne sont pas utilisées ici, car aucune des surcharges ne prend un <xref:System.StringComparison>.  
  
 [!code-cs[csProgGuideStrings#31](../../../csharp/programming-guide/strings/codesnippet/CSharp/how-to-compare-strings_2.cs)]  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment trier et rechercher des chaînes dans un tableau en fonction de la culture, à l’aide des méthodes statiques <xref:System.Array> qui prennent un paramètre <xref:System.StringComparer?displayProperty=fullName>.  
  
 [!code-cs[csProgGuideStrings#32](../../../csharp/programming-guide/strings/codesnippet/CSharp/how-to-compare-strings_3.cs)]  
  
 Les classes de collection telles que <xref:System.Collections.Hashtable?displayProperty=fullName>, <xref:System.Collections.Generic.Dictionary%602?displayProperty=fullName> et <xref:System.Collections.Generic.List%601?displayProperty=fullName> ont des constructeurs qui prennent un paramètre <xref:System.StringComparer?displayProperty=fullName> quand le type des éléments ou des clés est `string`. En général, vous devez utiliser ces constructeurs chaque fois que cela vous est possible, et spécifier `Ordinal` ou `OrdinalIgnoreCase`.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:System.Globalization.CultureInfo?displayProperty=fullName>   
 <xref:System.StringComparer?displayProperty=fullName>   
 [Chaînes](../../../csharp/programming-guide/strings/index.md)   
 [Comparaison de chaînes](../../../standard/base-types/comparing.md)   
 [Globalisation et localisation d’applications](/visualstudio/ide/globalizing-and-localizing-applications)

