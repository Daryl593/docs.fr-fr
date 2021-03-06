---
title: "D&#233;ploiement d&#39;applications WCF &#224; l&#39;aide de ClickOnce | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 1a11feee-2a47-4d3e-a28a-ad69d5ff93e0
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# D&#233;ploiement d&#39;applications WCF &#224; l&#39;aide de ClickOnce
Les applications clientes qui utilisent [!INCLUDE[indigo1](../../../includes/indigo1-md.md)] peuvent être déployées à l'aide de la technologie ClickOnce.Cette technologie leur permet de tirer parti des protections de sécurité d'exécution fournies par la sécurité d'accès du code, sous réserve qu'elles soient signées numériquement avec un certificat approuvé.Le certificat utilisé pour signer l'application ClickOnce doit résider dans le magasin d'éditeurs approuvés, et la stratégie de sécurité locale sur l'ordinateur client doit être configurée pour accorder des autorisations Confiance totale aux applications signées avec le certificat de l'éditeur.  
  
 Pour plus d'informations sur la configuration d'éditeurs approuvés et d'applications ClickOnce, consultez [Configuration d'éditeurs approuvés ClickOnce](http://go.microsoft.com/fwlink/?LinkId=94774) \(page pouvant être en anglais\).  
  
## Voir aussi  
 [Vue d'ensemble du déploiement d'applications approuvées](http://go.microsoft.com/fwlink/?LinkId=94775)   
 [Déploiement de ClickOnce pour les applications Windows Forms](http://go.microsoft.com/fwlink/?LinkId=94776)