---
title: "Attached and Detached Child Tasks | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "tasks, child tasks"
ms.assetid: c95788bf-90a6-4e96-b7bc-58e36a228cc5
caps.latest.revision: 21
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 21
---
# Attached and Detached Child Tasks
Une *tâche enfant* \(ou *tâche imbriquée*\) est une instance <xref:System.Threading.Tasks.Task?displayProperty=fullName> créée dans le délégué utilisateur d'une autre tâche, appelée *tâche parente*.  Une tâche enfant peut être détachée ou attachée.  Une *tâche enfant détachée* est une tâche qui s'exécute indépendamment de son parent.  Une *tâche enfant attachée* est une tâche imbriquée créée avec l'option <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=fullName> dont le parent ne l'empêche pas explicitement ou par défaut d'être attachée.  Une tâche peut créer autant de tâches enfants attachées et détachées que le permettent les ressources système.  
  
 Le tableau suivant répertorie les principales différences entre les deux types de tâches enfants.  
  
|Catégorie|Tâches enfants détachées|Tâches enfants attachées|  
|---------------|------------------------------|------------------------------|  
|Le parent attend que les tâches enfants soient terminées.|Non|Oui|  
|Le parent propage les exceptions levées par les tâches enfants.|Non|Oui|  
|Le statut du parent dépend du statut de l'enfant.|Non|Oui|  
  
 Dans la plupart des scénarios, nous vous recommandons d'utiliser des tâches enfants détachées, car leurs relations avec les autres tâches sont moins complexes.  C'est pourquoi les tâches créées à l'intérieur de tâches parentes sont détachées par défaut et vous devez spécifier explicitement l'option <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=fullName> pour créer une tâche enfant attachée.  
  
## Tâches enfants détachées  
 Même si une tâche enfant est créée par une tâche parente, par défaut, elle est indépendante de celle\-ci.  Dans l'exemple suivant, une tâche parente crée une tâche enfant simple.  Si vous exécutez l'exemple de code plusieurs fois, vous pouvez remarquer que la sortie de l'exemple diffère de celle indiquée et éventuellement d'une exécution à l'autre.  En effet, la tâche parente et les tâches enfants s'exécutent indépendamment les unes des autres ; l'enfant est une tâche détachée.  L'exemple attend seulement que la tâche parente se termine, et la tâche enfant ne peut pas s'exécuter ou s'achever avant la fin de l'application console.  
  
 [!code-csharp[TPL_ChildTasks#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_childtasks/cs/nested1.cs#1)]
 [!code-vb[TPL_ChildTasks#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_childtasks/vb/nested1.vb#1)]  
  
 Si la tâche enfant est représentée par un objet <xref:System.Threading.Tasks.Task%601> plutôt que par un objet <xref:System.Threading.Tasks.Task>, vous pouvez vous assurer que la tâche parente attend la fin de la tâche enfant en accédant à la propriété <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=fullName> de celle\-ci, même s'il s'agit d'une tâche enfant détachée.  La propriété <xref:System.Threading.Tasks.Task%601.Result%2A> bloque jusqu'à ce que sa tâche se termine, comme le montre l'exemple suivant.  
  
 [!code-csharp[TPL_ChildTasks#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_childtasks/cs/childtasks.cs#4)]
 [!code-vb[TPL_ChildTasks#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_childtasks/vb/tpl_childtasks.vb#4)]  
  
## Tâches enfants attachées  
 Contrairement aux tâches enfants détachées, les tâches enfants attachées sont étroitement synchronisées avec le parent.  Vous pouvez convertir la tâche enfant détachée dans l'exemple précédent en tâche enfant attachée à l'aide de l'option <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=fullName> dans l'instruction de création de tâche, comme illustré dans l'exemple suivant.  Dans ce code, la tâche enfant attachée se termine avant son parent.  La sortie de l'exemple est donc la même chaque fois que vous exécutez le code.  
  
 [!code-csharp[TPL_ChildTasks#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_childtasks/cs/child1.cs#2)]
 [!code-vb[TPL_ChildTasks#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_childtasks/vb/child1.vb#2)]  
  
 Vous pouvez utiliser des tâches enfants attachées pour créer des graphiques étroitement synchronisés d'opérations asynchrones.  
  
 Toutefois, une tâche enfant ne peut s'attacher à son parent que si celui\-ci n'interdit pas les tâches enfants attachées.  Pour empêcher explicitement l'attachement de tâches enfants à une tâche parente, vous devez spécifier l'option <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=fullName> dans le constructeur de classe de la tâche parente ou la méthode <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=fullName>.  Pour une interdiction implicite, vous devez créer les tâches parentes en appelant la méthode <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=fullName>.  L'exemple suivant illustre ce comportement.  Il est identique à l'exemple précédent, même si la tâche parente est créée en appelant la méthode <xref:System.Threading.Tasks.Task.Run%28System.Action%29?displayProperty=fullName> plutôt que la méthode <xref:System.Threading.Tasks.TaskFactory.StartNew%28System.Action%29?displayProperty=fullName>.  Étant donné que la tâche enfant n'est pas en mesure de s'attacher à son parent, la sortie de l'exemple est imprévisible.  Étant donné que les options de création de tâches par défaut pour les surcharges <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=fullName> incluent <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=fullName>, cet exemple est fonctionnellement équivalent au premier exemple dans la section « Tâches enfants détachées ».  
  
 [!code-csharp[TPL_ChildTasks#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_childtasks/cs/child1a.cs#3)]
 [!code-vb[TPL_ChildTasks#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_childtasks/vb/child1a.vb#3)]  
  
## Exceptions dans les tâches enfants  
 Si une tâche enfant détachée lève une exception, celle\-ci doit être observée ou gérée directement dans la tâche parente, comme dans le cas de n'importe quelle tâche non imbriquée.  Si une tâche enfant attachée lève une exception, celle\-ci est automatiquement propagée vers la tâche parente, puis vers le thread qui attend ou essaie d'accéder à la propriété <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=fullName> de la tâche.  Ainsi, en utilisant des tâches enfants attachées, vous pouvez gérer toutes les exceptions en un seul point dans l'appel à <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=fullName> sur le thread appelant.  Pour plus d'informations, consultez [Gestion des exceptions](../../../docs/standard/parallel-programming/exception-handling-task-parallel-library.md).  
  
## Annulation et tâches enfants  
 L'annulation de tâche est coopérative.  Autrement dit, pour être annulable, chaque tâche enfant attachée ou détachée doit surveiller l'état du jeton d'annulation.  Pour annuler un parent et tous ses enfants à l'aide d'une demande d'annulation, vous passez le même jeton en tant qu'argument à toutes les tâches et fournissez dans chaque tâche la logique pour répondre à la demande.  Pour plus d'informations, consultez [Task Cancellation](../../../docs/standard/parallel-programming/task-cancellation.md) et [How to: Cancel a Task and Its Children](../../../docs/standard/parallel-programming/how-to-cancel-a-task-and-its-children.md).  
  
### Annulation d'un parent  
 Si un parent s'annule avant le démarrage de sa tâche enfant, celle\-ci ne démarre jamais.  Si un parent s'annule une fois que sa tâche enfant a démarré, celle\-ci s'exécute jusqu'à son terme sauf si elle possède sa propre logique d'annulation.  Pour plus d'informations, consultez [Task Cancellation](../../../docs/standard/parallel-programming/task-cancellation.md).  
  
### Annulation d'une tâche enfant détachée  
 Si une tâche enfant détachée s'annule à l'aide du jeton passé au parent et que celui\-ci n'attend pas la tâche enfant, aucune exception n'est propagée, car l'exception est traitée comme une annulation de coopération bénigne.  Ce comportement est identique à celui de toute tâche de niveau supérieur.  
  
### Annulation d'une tâche enfant attachée  
 Quand une tâche enfant attachée s'annule à l'aide du jeton passé à sa tâche parente, une <xref:System.Threading.Tasks.TaskCanceledException> est propagée vers le thread intermédiaire à l'intérieur d'une <xref:System.AggregateException>.  Vous devez attendre la tâche parente pour pouvoir gérer toutes les exceptions bénignes, en plus de toute exception d'erreur propagée vers un graphique de tâches enfants attachées.  
  
 Pour plus d'informations, consultez [Gestion des exceptions](../../../docs/standard/parallel-programming/exception-handling-task-parallel-library.md).  
  
## Empêcher qu'une tâche enfant ne s'attache à son parent  
 Une exception non gérée levée par une tâche enfant est propagée vers la tâche parente.  Vous pouvez vous baser sur ce comportement pour observer toutes les exceptions de tâche enfant à partir d'une seule tâche racine au lieu de parcourir une arborescence de tâches.  Toutefois, la propagation d'exception peut être problématique quand une tâche parente n'attend pas d'attachement de la part d'un autre code.  Par exemple, imaginez une application qui appelle un composant de bibliothèque tierce à partir d'un objet <xref:System.Threading.Tasks.Task>.  Si ce composant crée également un objet <xref:System.Threading.Tasks.Task> et spécifie <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=fullName> pour l'attacher à la tâche parente, les exceptions non gérées qui se produisent dans la tâche enfant se propagent vers le parent.  Cela peut entraîner un comportement inattendu dans l'application principale.  
  
 Pour empêcher une tâche enfant de s'attacher à sa tâche parente, spécifiez l'option <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=fullName> quand vous créez l'objet <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601> parent.  Si une tâche enfant tente de s'attacher à son parent alors que celui\-ci spécifie l'option <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=fullName>, elle échoue et s'exécute comme si l'option <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=fullName> n'était pas spécifiée.  
  
 Vous pourriez également empêcher une tâche enfant de s'attacher à son parent quand la tâche enfant ne se termine pas en temps voulu.  Étant donné qu'une tâche parente ne se termine pas tant que toutes les tâches enfants ne sont pas achevées, une tâche enfant à exécution longue peut entraîner des performances médiocres de la part de l'application globale.  Pour obtenir un exemple qui montre comment améliorer les performances de l'application en empêchant une tâche de s'attacher à sa tâche parente, consultez [How to: Prevent a Child Task from Attaching to its Parent](../../../docs/standard/parallel-programming/how-to-prevent-a-child-task-from-attaching-to-its-parent.md).  
  
## Voir aussi  
 [Parallel Programming](../../../docs/standard/parallel-programming/index.md)   
 [Data Parallelism](../../../docs/standard/parallel-programming/data-parallelism-task-parallel-library.md)