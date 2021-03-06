---
title: "Proc&#233;dure&#160;: configurer le suivi avec WorkflowServiceHost | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: ed1485fe-7529-4351-bca3-8bb915260b17
caps.latest.revision: 14
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 14
---
# Proc&#233;dure&#160;: configurer le suivi avec WorkflowServiceHost
Cette rubrique explique comment configurer le suivi pour un workflow [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] hébergé dans <xref:System.ServiceModel.Activities.WorkflowServiceHost>.  Il est configuré via un fichier Web.config en spécifiant un comportement de service.  
  
### Configurer le suivi dans le fichier de configuration  
  
1.  Ajoutez <xref:System.Activities.Tracking.EtwTrackingParticipant> à l'aide de l'élément \<`behavior`\> dans un fichier de configuration, comme indiqué dans l'exemple suivant.  
  
    ```  
    <behaviors>  
       <serviceBehaviors>  
         <behavior>  
           <etwTracking profileName="Sample Tracking Profile" />  
         </behavior>              
       </serviceBehaviors>  
    <behaviors>  
  
    ```  
  
    > [!NOTE]
    >  L'exemple de configuration précédent utilise la configuration simplifiée.  [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Configuration simplifiée](../../../../docs/framework/wcf/simplified-configuration.md).  
  
     L'exemple de configuration précédent ajoute un objet <xref:System.Activities.Tracking.EtwTrackingParticipant> et spécifie un nom de modèle de suivi.  Les modèles de suivi sont créés dans un élément \<`trackingProfile`\> au sein d'un élément \<`tracking`\>.  Le modèle de suivi contient des requêtes de suivi qui permettent à un participant de suivi de s'abonner à des événements de workflow émis lorsque l'état d'une instance de workflow change au moment de l'exécution.  L'exemple suivant montre comment créer un modèle de suivi.  
  
    ```xml  
    <system.serviceModel>  
        <tracking>   
         <trackingProfile name="Sample Tracking Profile">  
            <workflow activityDefinitionId="*">  
               <workflowInstanceQueries>  
                 <workflowInstanceQuery>  
                    <states>  
                       <state name="Started"/>  
                       <state name="Completed"/>  
                    </states>  
                </workflowInstanceQuery>  
             </workflowInstanceQueries>  
           </workflow>  
         </trackingProfile>   
       </tracking>  
    </system.serviceModel>  
  
    ```  
  
     [!INCLUDE[crabout](../../../../includes/crabout-md.md)] les modèles de suivi, consultez [Modèles de suivi](../../../../docs/framework/windows-workflow-foundation//tracking-profiles.md).  
  
     [!INCLUDE[crabout](../../../../includes/crabout-md.md)] le suivi en général, consultez [Suivi et traçage de workflow](../../../../docs/framework/windows-workflow-foundation//workflow-tracking-and-tracing.md).  
  
### Configurer le suivi dans le code  
  
1.  Ajoutez le <xref:System.Activities.Tracking.EtwTrackingParticipant> à l'aide du comportement <xref:System.ServiceModel.Activities.Description.EtwTrackingBehavior> dans le code, comme indiqué dans l'exemple suivant.  
  
    ```csharp  
    host.Description.Behaviors.Add(new EtwTrackingBehavior { ProfileName = "Sample Tracking Profile" });  
    ```  
  
     L'exemple de code précédent ajoute un objet <xref:System.Activities.Tracking.EtwTrackingParticipant> et spécifie un nom de modèle de suivi.  Les modèles de suivi sont créés dans un élément \<`trackingProfile`\> au sein d'un élément \<`tracking`\> comme indiqué sans la section précédente.  
  
     [!INCLUDE[crabout](../../../../includes/crabout-md.md)] les modèles de suivi, consultez [Modèles de suivi](../../../../docs/framework/windows-workflow-foundation//tracking-profiles.md).  
  
     [!INCLUDE[crabout](../../../../includes/crabout-md.md)] le suivi en général, consultez [Suivi et traçage de workflow](../../../../docs/framework/windows-workflow-foundation//workflow-tracking-and-tracing.md).  Pour obtenir un exemple de configuration du suivi par programme, consultez [Configuration du suivi d'un workflow](../../../../docs/framework/windows-workflow-foundation//configuring-tracking-for-a-workflow.md).  
  
## Voir aussi  
 [Configuration simplifiée pour WCF Services](../../../../docs/framework/wcf/samples/simplified-configuration-for-wcf-services.md)   
 [Services de workflow](../../../../docs/framework/wcf/feature-details/workflow-services.md)   
 [Modèles de suivi](../../../../docs/framework/windows-workflow-foundation//tracking-profiles.md)