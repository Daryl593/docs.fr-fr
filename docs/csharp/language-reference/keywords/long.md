---
title: "long (référence C#)"
ms.date: 2017-03-14
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- long_CSharpKeyword
- long
dev_langs:
- CSharp
helpviewer_keywords:
- long keyword [C#]
ms.assetid: f9b24319-1f39-48be-a42b-d528ee28a7fd
caps.latest.revision: 17
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
ms.openlocfilehash: 5f7d2d6a3d5781b4e120b8399c7206d4429dd98e
ms.contentlocale: fr-fr
ms.lasthandoff: 07/28/2017

---
# <a name="long-c-reference"></a>long (référence C#)

`long` désigne un type intégral qui stocke des valeurs en fonction de la taille et de la plage indiquées dans le tableau suivant.  
  
|Type|Plage|Taille|Type .NET Framework|  
|----------|-----------|----------|-------------------------|  
|`long`|-9 223 372 036 854 775 808 à 9 223 372 036 854 775 807|Entier 64 bits signé|<xref:System.Int64?displayProperty=fullName>|  
  
## <a name="literals"></a>Littéraux 

Vous pouvez déclarer et initialiser une variable `long` en lui assignant un littéral décimal, un littéral hexadécimal ou un littéral binaire (à compter de C# 7). 

Dans l’exemple suivant, les entiers égaux à 4 294 967 296 représentés comme des littéraux décimaux, hexadécimaux et binaires sont assignés aux valeurs `long`.  
  
[!code-cs[long](../../../../samples/snippets/csharp/language-reference/keywords/numeric-literals.cs#Long)]  

> [!NOTE] 
> Vous utilisez le préfixe `0x` ou `0X` pour désigner un littéral hexadécimal, et le préfixe `0b` ou `0B` pour désigner un littéral binaire. Les littéraux décimaux n’ont pas de préfixe. 

À compter de C# 7, vous pouvez également utiliser le caractère de soulignement, `_`, comme séparateur numérique pour améliorer la lisibilité, comme dans l’exemple suivant.

[!code-cs[long](../../../../samples/snippets/csharp/language-reference/keywords/numeric-literals.cs#LongS)]  
 
 Les littéraux entiers peuvent également inclure un suffixe qui désigne le type. Le suffixe `L` désigne un type `long`. L’exemple suivant utilise le suffixe `L` pour désigner un entier long :
 
```csharp
long value = 4294967296L;  
```  

> [!NOTE]
>  Vous pouvez aussi utiliser la lettre minuscule « l » comme suffixe. Ceci génère cependant un avertissement du compilateur, car la lettre « l » peut être facilement confondue avec le chiffre « 1 ». Utilisez « L » pour plus de clarté.  
  
 Quand vous utilisez le suffixe `L`, le type du littéral entier est déterminé comme étant `long` ou [ulong](../../../csharp/language-reference/keywords/ulong.md), en fonction de sa taille. Dans ce cas, le type est `long`, car le littéral est inférieur à la plage de [ulong](../../../csharp/language-reference/keywords/ulong.md).  
  
 Le suffixe est souvent utilisé pour appeler des méthodes surchargées. Par exemple, les méthodes surchargées suivantes ont des paramètres de type `long` et [int](../../../csharp/language-reference/keywords/int.md) :  
  
```csharp
public static void SampleMethod(int i) {}  
public static void SampleMethod(long l) {}  
```  
  
 Le suffixe `L` garantit que la surcharge appropriée est appelée :  
  
```csharp  
SampleMethod(5);    // Calls the method with the int parameter  
SampleMethod(5L);   // Calls the method with the long parameter  
```  
Quand un littéral entier n’a pas de suffixe, son type est le premier des types suivants dans lesquels sa valeur peut être représentée : 

1. [int](int.md)
2. [uint](../../../csharp/language-reference/keywords/uint.md)
3. `long`
4. [ulong](../../../csharp/language-reference/keywords/ulong.md) 

Dans l’exemple précédent, le littéral 4294967296 est de type `long`, car il dépasse la plage de [uint](../../../csharp/language-reference/keywords/uint.md) (consultez le [Tableau des types intégraux](../../../csharp/language-reference/keywords/integral-types-table.md) pour les tailles de stockage des types intégraux).  
  
 Vous pouvez utiliser le type `long` avec d’autres types intégraux dans la même expression, l’expression est évaluée comme `long` (ou [bool](../../../csharp/language-reference/keywords/bool.md) dans le cas d’expressions relationnelles ou booléennes). Par exemple, l’expression suivante s’évalue en `long` :  
  
```csharp  
898L + 88  
```  
  
 Pour plus d’informations sur les expressions arithmétiques combinant des types virgule flottante et des types intégraux, consultez [float](../../../csharp/language-reference/keywords/float.md) et [double](../../../csharp/language-reference/keywords/double.md).  
  
## <a name="conversions"></a>Conversions  
 Il existe une conversion implicite prédéfinie de `long` en [float](../../../csharp/language-reference/keywords/float.md), [double](../../../csharp/language-reference/keywords/double.md) ou [decimal](../../../csharp/language-reference/keywords/decimal.md). Dans le cas contraire, vous devez utiliser un cast. Par exemple, l’instruction suivante génère une erreur de compilation sans un cast explicite :  
  
```csharp  
int x = 8L;        // Error: no implicit conversion from long to int  
int x = (int)8L;   // OK: explicit conversion to int  
```  
  
 Il existe une conversion implicite prédéfinie de [sbyte](../../../csharp/language-reference/keywords/sbyte.md), [byte](../../../csharp/language-reference/keywords/byte.md), [short](../../../csharp/language-reference/keywords/short.md), [ushort](../../../csharp/language-reference/keywords/ushort.md), [int](../../../csharp/language-reference/keywords/int.md), [uint](../../../csharp/language-reference/keywords/uint.md) ou [char](../../../csharp/language-reference/keywords/char.md) vers `long`.  
  
 Notez aussi qu’il n’existe pas de conversion implicite des types virgule flottante vers `long`. Par exemple, l’instruction suivante génère une erreur du compilateur, sauf si vous utilisez un cast explicite :  
  
```csharp  
long x = 3.0;         // Error: no implicit conversion from double  
long y = (long)3.0;   // OK: explicit conversion  
```  
  
## <a name="c-language-specification"></a>Spécification du langage C#  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 <xref:System.Int64>   
 [Informations de référence sur C#](../../../csharp/language-reference/index.md)   
 [Guide de programmation C#](../../../csharp/programming-guide/index.md)   
 [Mots clés C#](../../../csharp/language-reference/keywords/index.md)   
 [Tableau des types intégraux](../../../csharp/language-reference/keywords/integral-types-table.md)   
 [Tableau des types intégrés](../../../csharp/language-reference/keywords/built-in-types-table.md)   
 [Tableau des conversions numériques implicites](../../../csharp/language-reference/keywords/implicit-numeric-conversions-table.md)   
 [Tableau des conversions numériques explicites](../../../csharp/language-reference/keywords/explicit-numeric-conversions-table.md)

