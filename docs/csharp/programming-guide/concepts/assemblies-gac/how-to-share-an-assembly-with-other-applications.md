---
title: "Guide pratique pour partager un assembly avec d’autres applications (C#)"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: c30e972b-1693-4e05-b115-c31831fdf9f2
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 85117cfdc9b12a93891e89727412a03acc83289b
ms.contentlocale: fr-fr
ms.lasthandoff: 07/28/2017

---
# <a name="how-to-share-an-assembly-with-other-applications-c"></a>Guide pratique pour partager un assembly avec d’autres applications (C#)
Les assemblys peuvent être privés ou partagés. Par défaut, la plupart des programmes simples se composent d’un assembly privé, car ils ne sont pas destinés à être utilisés par d’autres applications.  
  
 Pour partager un assembly avec d’autres applications, celui-ci doit être placé dans le [Global Assembly Cache](../../../../framework/app-domains/gac.md) (GAC).  
  
### <a name="sharing-an-assembly"></a>Partage d’un assembly  
  
1.  Créez votre assembly. Pour plus d’informations, consultez [Création d’assemblys](../../../../framework/app-domains/create-assemblies.md).  
  
2.  Attribuez un nom fort à votre assembly. Pour plus d’informations, consultez [Guide pratique pour signer un assembly avec un nom fort](../../../../framework/app-domains/how-to-sign-an-assembly-with-a-strong-name.md).  
  
3.  Assignez des informations de version à votre assembly. Pour plus d’informations, consultez [Versioning des assemblys](https://msdn.microsoft.com/library/51ket42z).  
  
4.  Ajoutez votre assembly au Global Assembly Cache. Pour plus d’informations, consultez [Guide pratique pour installer un assembly dans le Global Assembly Cache](../../../../framework/app-domains/how-to-install-an-assembly-into-the-gac.md).  
  
5.  Accédez aux types contenus dans l’assembly à partir d’autres applications. Pour plus d’informations, consultez [Guide pratique pour référencer un assembly avec un nom fort](http://msdn.microsoft.com/library/4c6a406a-b5eb-44fa-b4ed-4e95bb95a813).  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de programmation C#](../../../../csharp/programming-guide/index.md)   
 [Programmation à l’aide d’assemblys](../../../../framework/app-domains/programming-with-assemblies.md)

