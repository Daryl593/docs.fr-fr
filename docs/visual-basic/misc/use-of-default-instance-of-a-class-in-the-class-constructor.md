---
title: "L’utilisation de l’instance par d&#233;faut d’une classe dans le constructeur de classe pourrait entra&#238;ner des appels r&#233;currents infinis | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.technology: 
  - "devlang-visual-basic"
ms.topic: "article"
ms.assetid: 9645b47f-7de5-46d0-bb45-d5fdaa8aaa2a
caps.latest.revision: 13
author: "stevehoag"
ms.author: "shoag"
caps.handback.revision: 13
---
# L’utilisation de l’instance par d&#233;faut d’une classe dans le constructeur de classe pourrait entra&#238;ner des appels r&#233;currents infinis
Une instance par défaut d’une classe a été utilisée dans le constructeur de la classe. Cela peut entraîner un appel récurrent infini, également appelé « boucle infinie ».  
  
### Pour corriger cette erreur  
  
-   Supprimez l’instance par défaut du constructeur de la classe.  
  
## Voir aussi  
 [NOT IN BUILD : Utilisation des constructeurs et des destructeurs](http://msdn.microsoft.com/fr-fr/548eebe1-86c4-4377-b2f5-447cb8be3d90)