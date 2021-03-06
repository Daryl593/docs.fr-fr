---
title: "Proc&#233;dure&#160;: d&#233;terminer la version de d&#233;couverte d&#39;une demande Probe | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: b3c4e2e2-2957-4074-ae6a-776a5ca84278
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# Proc&#233;dure&#160;: d&#233;terminer la version de d&#233;couverte d&#39;une demande Probe
Un proxy de découverte peut exposer plusieurs points de terminaison de découverte à l'aide de différentes versions de découverte.  Lorsqu'une demande UDP multicast Probe arrive au proxy, celui\-ci doit répondre avec un message de suppression de multidiffusion.  Pour cela, il doit connaître la version de découverte de la demande.  
  
### Pour déterminer la version de découverte d'une demande Probe  
  
1.  Dans la méthode qui répond à une demande Probe \(par exemple <xref:System.ServiceModel.Discovery.DiscoveryProxy.OnBeginFind%2A>\) utilisez la propriété <xref:System.ServiceModel.OperationContext.Current%2A> statique pour rechercher un <xref:System.ServiceModel.Discovery.DiscoveryOperationContextExtension> comme indiqué dans le code suivant.  
  
    ```  
    DiscoveryOperationContextExtension doce = OperationContext.Current.Extensions.Find<DiscoveryOperationContextExtension>();  
    // Access the discovery version from the DiscoveryOperationContextExtension  
    doce.DiscoveryVersion;  
  
    ```  
  
## Voir aussi  
 <xref:System.ServiceModel.Discovery.Configuration.AnnouncementEndpointElement.DiscoveryVersion%2A>   
 [Implémentation d'un proxy de découverte](../../../../docs/framework/wcf/feature-details/implementing-a-discovery-proxy.md)   
 [Exemple Discovery Proxy](../../../../docs/framework/wcf/samples/discovery-proxy-sample.md)