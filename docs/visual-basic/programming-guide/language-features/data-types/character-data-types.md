---
title: "Types de données (Visual Basic) caractère | Documents Microsoft"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
dev_langs:
- VB
helpviewer_keywords:
- data types [Visual Basic], character
- String data type, character data types
- character data types [Visual Basic]
- Char data type, character data types
- data types [Visual Basic], choosing
ms.assetid: 902479ef-1679-47fc-9911-0c1c5008226c
caps.latest.revision: 23
author: dotnet-bot
ms.author: dotnetcontent
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
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 7ce600fe188c94593e4c07e37883ca11f90d9ae5
ms.lasthandoff: 03/13/2017

---
# <a name="character-data-types-visual-basic"></a>Types de données caractères (Visual Basic)
[!INCLUDE[vbprvb](../../../../csharp/programming-guide/concepts/linq/includes/vbprvb_md.md)]Fournit des *types de données caractère* afin de traiter les caractères affichables et imprimables. Bien que les deux types reconnaissent les caractères Unicode, `Char` contient un caractère, tandis que `String` contient un nombre indéfini de caractères.  
  
 Pour un tableau présentant une comparaison côte-à-côte de la [!INCLUDE[vbprvb](../../../../csharp/programming-guide/concepts/linq/includes/vbprvb_md.md)] des types de données, consultez [des Types de données](../../../../visual-basic/language-reference/data-types/data-type-summary.md).  
  
## <a name="char-type"></a>Char (Type)  
 Le `Char` type de données est un caractère de Unicode sur deux octets (16 bits) unique. Si une variable stocke toujours exactement un caractère, déclarez-le en tant que `Char`. Exemple :  
  
 [!code-vb[VbVbalrCharTypes n °&1;](../../../../visual-basic/programming-guide/language-features/data-types/codesnippet/VisualBasic/character-data-types_1.vb)]  
  
 Chaque valeur possible dans une `Char` ou `String` variable est une *point de code*, ou code de caractère dans le jeu de caractères Unicode. Les caractères Unicode incluent le jeu de caractères ASCII base, divers autres lettres de l’alphabet, les accents, les symboles monétaires, fractions, les signes diacritiques et symboles mathématiques et techniques.  
  
> [!NOTE]
>  Jeu de caractères Unicode réserve les points de code D800 à DFFF (55 296 à 55 551 décimal) pour *paires de substitution*, qui requièrent deux valeurs de 16 bits pour représenter un seul point de code. A `Char` variable ne peut pas contenir une paire de substitution et un `String` utilise deux positions pour contenir une telle paire.  
  
 Pour plus d’informations, consultez [Type de données Char](../../../../visual-basic/language-reference/data-types/char-data-type.md).  
  
## <a name="string-type"></a>Type de chaîne  
 Le `String` type de données est une séquence de zéro ou plusieurs caractères Unicode à deux octets (16 bits). Si une variable peut contenir un nombre indéfini de caractères, déclarez-le en tant que `String`. Exemple :  
  
 [!code-vb[VbVbalrCharTypes n °&2;](../../../../visual-basic/programming-guide/language-features/data-types/codesnippet/VisualBasic/character-data-types_2.vb)]  
  
 Pour plus d’informations, consultez [Type de données String](../../../../visual-basic/language-reference/data-types/string-data-type.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Types de données élémentaires](../../../../visual-basic/programming-guide/language-features/data-types/elementary-data-types.md)   
 [Types de données composites](../../../../visual-basic/programming-guide/language-features/data-types/composite-data-types.md)   
 [Types génériques dans Visual Basic](../../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)   
 [Types valeur et Types référence](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)   
 [Conversions de type dans Visual Basic](../../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)   
 [Dépannage des Types de données](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)   
 [Caractères de type](../../../../visual-basic/programming-guide/language-features/data-types/type-characters.md)
