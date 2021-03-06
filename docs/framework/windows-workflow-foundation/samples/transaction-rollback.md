---
title: "Restauration de transaction | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 7f377147-7529-4689-a588-608cee87fdf8
caps.latest.revision: 9
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 9
---
# Restauration de transaction
Cet exemple montre comment créer un <xref:System.Activities.NativeActivity> personnalisé qui accède au <xref:System.Activities.RuntimeTransactionHandle> ambiant pour obtenir la transaction ambiante et la restaurer explicitement.  
  
## Détails de l'exemple  
 Dans le workflow, une transaction est effectuée automatiquement lorsque le <xref:System.Activities.Statements.TransactionScope> ou le <xref:System.ServiceModel.Activities.TransactedReceiveScope> le plus à l'extérieur se termine.Une transaction est implicitement restaurée lorsqu'une exception non prise en charge se propage à travers la limite d'étendue.Dans certains cas, toutefois, il est logique de restaurer explicitement une transaction sans avoir à lever une exception.Dans ce cas, vous pouvez utiliser l'activité de restauration personnalisée comme celle de cet exemple pour abandonner explicitement la transaction ambiante et fournir un motif d'exception facultative.  
  
 `RollbackActivity` est un <xref:System.Activities.NativeActivity> car il nécessite l'accès aux propriétés d'exécution pour obtenir le <xref:System.Activities.RuntimeTransactionHandle> ambiant.Dans la méthode `Execute`, il obtient le <xref:System.Activities.RuntimeTransactionHandle> et vérifie s'il est `null`, ce qui indique que l'activité a été utilisée sans transaction runtime ambiante.Il obtient ensuite la transaction, en vérifiant à nouveau si la valeur `null` est présente.Il est possible d'avoir un <xref:System.Activities.RuntimeTransactionHandle> ambiant sans jamais initialiser une transaction runtime.Enfin, il abandonne la transaction en appelant <xref:System.Transactions.Transaction.Rollback%2A> et en spécifiant une exception fournie par l'utilisateur ou une exception générique qui stipule que l'activité a restauré la transaction.  
  
 Le workflow de la démonstration est composé d'un <xref:System.Activities.Statements.TransactionScope> dont le corps imprime l'état de la transaction avant et après l'exécution de `RollbackActivity`.Notez que, même si la transaction a été restaurée, <xref:System.Activities.Statements.TransactionScope> s'exécute jusqu'à son achèvement et n'abandonne pas le workflow tant que le corps n'a pas terminé son exécution.Le workflow est abandonné car la propriété <xref:System.Activities.Statements.TransactionScope.AbortInstanceOnTransactionFailure%2A> a `true` comme valeur par défaut.  
  
#### Pour utiliser cet exemple  
  
1.  Chargez la solution TransactionRollback.sln dans [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)].  
  
2.  Appuyez sur Ctrl\+Maj\+B pour générer la solution.  
  
3.  Appuyez sur CTRL\+F5 pour exécuter l'application.  
  
> [!IMPORTANT]
>  Les exemples peuvent déjà être installés sur votre ordinateur.Recherchez le répertoire \(par défaut\) suivant avant de continuer.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page \(éventuellement en anglais\) des [exemples Windows Communication Foundation \(WCF\) et Windows Workflow Foundation \(WF\) pour .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) pour télécharger tous les exemples [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] et [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Cet exemple se trouve dans le répertoire suivant.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WF\Scenario\Transactions\TransactionRollback`  
  
## Voir aussi  
 [Transactions](../../../../docs/framework/windows-workflow-foundation//workflow-transactions.md)