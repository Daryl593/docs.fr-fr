---
title: "Utilisation du suivi pour r&#233;soudre les probl&#232;mes pos&#233;s par les applications | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 8851adde-c3c2-4391-9523-d8eb831490af
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# Utilisation du suivi pour r&#233;soudre les probl&#232;mes pos&#233;s par les applications
[!INCLUDE[wf](../../../includes/wf-md.md)] vous permet de suivre les informations associées au workflow pour donner des détails concernant l'exécution d'une application de [!INCLUDE[wf2](../../../includes/wf2-md.md)] ou d'un service.Les hôtes [!INCLUDE[wf2](../../../includes/wf2-md.md)] sont en mesure de capturer des événements du workflow pendant l'exécution d'une instance de workflow.Si votre workflow génère des erreurs ou des exceptions, vous pouvez utiliser les détails du suivi [!INCLUDE[wf2](../../../includes/wf2-md.md)] pour résoudre les problèmes liés à son traitement.  
  
## Dépannage d'un WF à l'aide du suivi WF  
 Pour détecter des erreurs dans le traitement d'une activité [!INCLUDE[wf2](../../../includes/wf2-md.md)], vous pouvez activer le suivi avec un modèle de suivi chargé de rechercher un <xref:System.Activities.Tracking.ActivityStateRecord> dont l'état est Faulted.La requête correspondante est spécifiée dans le code suivant.  
  
```  
<activityStateQueries>  
              <activityStateQuery activityName="*">  
                <states>  
                  <state name="Faulted" />  
                </states>  
              </activityStateQuery>  
 </activityStateQueries>  
```  
  
 Si une erreur est propagée et gérée dans un gestionnaire d'erreur \(tel qu'une activité <xref:System.Activities.Statements.TryCatch>\), cette erreur peut être détectée à l'aide d'un <xref:System.Activities.Tracking.FaultPropagationRecord>.<xref:System.Activities.Tracking.FaultPropagationRecord> indique l'activité source de l'erreur et le nom du gestionnaire d'erreur.<xref:System.Activities.Tracking.FaultPropagationRecord> contient des détails sur l'erreur sous la forme d'une pile d'exception. La requête permettant de s'abonner à un <xref:System.Activities.Tracking.FaultPropagationRecord> est illustrée dans l'exemple suivant.  
  
```  
<faultPropagationQueries>  
              <faultPropagationQuery faultSourceActivityName ="*" faultHandlerActivityName="*"/>  
 </faultPropagationQueries>  
  
```  
  
 Si une erreur n'est pas gérée dans le workflow, cela génère une exception non prise en charge au niveau de l'instance du workflow et l'instance du workflow est abandonnée.Pour comprendre les détails de l'exception non prise en charge, le modèle de suivi doit interroger l'enregistrement de l'instance du workflow dont l'état est `state name=”UnhandledException”`, comme spécifié dans l'exemple suivant.  
  
```  
<workflowInstanceQueries>  
              <workflowInstanceQuery>  
                <states>  
                  <state name="UnhandledException" />  
                </states>  
              </workflowInstanceQuery>  
</workflowInstanceQueries>  
```  
  
 Lorsqu'une instance de workflow rencontre une exception non prise en charge, un objet <xref:System.Activities.Tracking.WorkflowInstanceUnhandledExceptionRecord> est émis si le suivi [!INCLUDE[wf2](../../../includes/wf2-md.md)] a été activé.  
  
 Cet enregistrement de suivi contient les détails de l'erreur sous la forme d'une pile d'exception.Il comprend les détails de la source de l'erreur \(par exemple, l'activité\) qui a échoué et généré une exception non prise en charge. Pour s'abonner aux les événements d'erreur issus de [!INCLUDE[wf2](../../../includes/wf2-md.md)], activez le suivi en ajoutant un participant de suivi.Configurez ce participant avec un modèle de suivi qui recherche `ActivityStateQuery (state=”Faulted”)`, <xref:System.Activities.Tracking.FaultPropagationRecord> et `WorkflowInstanceQuery (state=”UnhandledException”)`.  
  
 Si le suivi est activé à l'aide du participant de suivi ETW, les événements d'erreur sont émis sur une session ETW.Les événements peuvent être visualisés à l'aide de l'Observateur d'événements.Cela se trouve sous le nœud **Observateur d'événements\-\>Journaux des applications et des services\-\>Microsoft\-\>Windows\-\>Serveur d'applications\-Applications** dans le canal analytique.  
  
## Voir aussi  
 [Analyse Windows Server AppFabric](http://go.microsoft.com/fwlink/?LinkId=201273)   
 [Analyse d'applications avec AppFabric](http://go.microsoft.com/fwlink/?LinkId=201275)