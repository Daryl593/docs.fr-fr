---
title: "Mesure de l&#39;am&#233;lioration du d&#233;marrage avec .NET Native | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: c4d25b24-9c1a-4b3e-9705-97ba0d6c0289
caps.latest.revision: 7
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 7
---
# Mesure de l&#39;am&#233;lioration du d&#233;marrage avec .NET Native
[!INCLUDE[net_native](../../../includes/net-native-md.md)] améliore de façon significative le temps de lancement des applications.  Cette amélioration est particulièrement visible sur les appareils portables à basse consommation d'énergie hébergeant des applications complexes.  Cette rubrique facilite la prise en main de l'instrumentation de base servant à mesurer cette amélioration du démarrage.  
  
 Pour faciliter les investigations des performances, le .NET Framework et Windows utilisent une infrastructure d'événements appelée Suivi d'événements pour Windows \(ETW\) qui permet à votre application de notifier les outils quand des événements se produisent.  Vous pouvez ensuite utiliser un outil appelé PerfView pour afficher et analyser les événements ETW facilement.  Cette rubrique explique comment :  
  
-   utiliser la classe <xref:System.Diagnostics.Tracing.EventSource> pour émettre des événements ;  
  
-   utiliser PerfView pour collecter ces événements ;  
  
-   utiliser PerfView pour afficher ces événements.  
  
## Utilisation de la classe EventSource pour émettre des événements  
 <xref:System.Diagnostics.Tracing.EventSource> fournit une classe de base à partir de laquelle vous pouvez créer un fournisseur d'événements personnalisé.  En règle générale, vous créez une sous\-classe de <xref:System.Diagnostics.Tracing.EventSource> et encapsulez les méthodes `Write*` avec vos méthodes d'événement.  Un modèle de singleton est généralement utilisé pour chaque <xref:System.Diagnostics.Tracing.EventSource>.  
  
 Par exemple, la classe dans l'exemple suivant permet de mesurer deux caractéristiques de performance :  
  
-   La durée écoulée jusqu'à l'appel du constructeur de classe `App`  
  
-   La durée écoulée jusqu'à l'appel du constructeur `MainPage`  
  
 [!code-csharp[ProjectN_ETW#1](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_etw/cs/etw1.cs#1)]  
  
 Quelques éléments méritent votre attention.  Tout d'abord, un singleton est créé dans `AppEventSource.Log`.  Cette instance sera utilisée pour toutes les informations consignées.  Ensuite, chaque méthode d'événement a un <xref:System.Diagnostics.Tracing.EventAttribute>.  Cela permet aux outils d'associer l'index de la méthode <xref:System.Diagnostics.Tracing.EventSource.WriteEvent%2A> à la méthode qui a été appelée sur `AppEventSource`.  
  
 Notez que ces événements sont purement indicatifs.  La majeure partie du code d'application s'exécute après ces événements.  Vous devez comprendre quels sont les événements dans le code qui correspondent aux interactions de l'utilisateur, les mesurer et améliorer ces tests d'évaluation.  En outre, les événements eux\-mêmes ne consignent qu'une seule instance dans le temps.  Il est souvent utile d'avoir des paires d'événements de démarrage et d'arrêt pour chaque opération.  Pendant l'examen du démarrage de l'application, l'événement de démarrage est généralement l'événement « Process\/Start » émis par le système d'exploitation.  
  
 Par exemple, supposons que vous créez un lecteur RSS.  Voici certains moments propices à l'enregistrement d'un événement :  
  
-   La première fois que la page principale est restituée  
  
-   La désérialisation des anciens articles RSS à partir du stockage local  
  
-   Quand votre application commence à synchroniser de nouveaux articles  
  
-   Quand votre application a terminé la synchronisation des nouveaux articles  
  
 L'instrumentation d'une application est simple : il vous suffit d'appeler la méthode appropriée sur la classe dérivée.  À l'aide de la syntaxe `AppEventSource` utilisée dans l'exemple précédent, vous pouvez instrumenter une application comme suit :  
  
 [!code-csharp[ProjectN_ETW#2](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_etw/cs/etw2.cs#2)]  
  
 Quand l'application est instrumentée, vous êtes prêt à collecter des événements.  
  
## Collecte d'événements avec PerfView  
 PerfView utilise des événements ETW pour vous aider à effectuer toutes sortes d'investigations de performances sur votre application.  Il inclut également une interface utilisateur graphique de configuration qui vous permet d'activer ou de désactiver la journalisation de différents types d'événement.  PerfView est un outil gratuit et peut être téléchargé à partir du [Centre de téléchargement Microsoft](http://www.microsoft.com/download/details.aspx?id=28567).  Pour plus d'informations, regardez les [vidéos des didacticiels PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial).  
  
> [!NOTE]
>  PerfView ne peut pas être utilisé pour collecter des événements sur les systèmes ARM.  Pour collecter des événements sur les systèmes ARM, utilisez l'Enregistreur de performance Windows \(WPR\).  Pour plus d'informations, consultez le [billet de blog de Vance Morrison](http://blogs.msdn.com/b/vancem/archive/2012/12/19/collecting-etw-perfview-data-on-an-windows-rt-winrt-arm-surface-device.aspx).  
  
 Vous pouvez également appeler PerfView à partir de la ligne de commande.  Pour consigner uniquement les événements à partir de votre fournisseur, ouvrez la fenêtre d'invite de commandes et entrez la commande suivante :  
  
```  
perfview -KernelEvents:Process -OnlyProviders:*MyCompany-MyApp collect outputFile   
```  
  
 où :  
  
 `-KernelEvents:Process`  
 Indique que vous voulez savoir quand le processus démarre et s'arrête.  Vous avez besoin de pouvoir soustraire l'événement « Process\/Start » pour votre application des autres moments d'événement.  
  
 `-OnlyProviders:*MyCompany-MyApp`  
 Désactive les autres fournisseurs que PerfView active par défaut et active le fournisseur MyCompany\-MyApp.  \(L'astérisque indique que c'est un <xref:System.Diagnostics.Tracing.EventSource>.\)  
  
 `collect outputFile`  
 Indique que vous souhaitez démarrer la collecte et enregistrer les données dans outputFile.etl.zip.  
  
 Exécutez votre application après avoir démarré PerfView.  Vous devez retenir quelques points quand vous exécutez votre application :  
  
-   Utilisez une version Release, pas une version Debug.  Les versions Debug contiennent souvent du code supplémentaire de vérification et de gestion des erreurs qui risque de ralentir l'exécution de votre application.  
  
-   L'exécution de votre application avec un débogueur attaché a une incidence sur les performances de votre application.  
  
-   Windows utilise plusieurs stratégies de mise en cache pour accélérer les temps de lancement d'application.  Si votre application est actuellement mise en cache en mémoire et qu'elle ne doit pas être chargée à partir du disque, elle démarrera plus rapidement.  Pour garantir la cohérence, démarrez et fermez votre application plusieurs fois avant de la mesurer.  
  
 Une fois que vous avez exécuté votre application pour permettre à PerfView de collecter les événements émis, cliquez sur le bouton **Arrêter la collecte**.  En règle générale, vous devez arrêter la collecte avant de fermer votre application afin de ne pas obtenir d'événements superflus.  Toutefois, la mesure de performances d'arrêt ou de suspension peut vous amener à continuer la collecte.  
  
## Affichage des événements  
 Pour afficher les événements qui ont déjà été collectés, utilisez PerfView pour ouvrir le fichier .etl ou. etl.zip que vous avez créé et choisissez **Événements**.  ETW aura recueilli des informations sur un grand nombre d'événements, y compris les événements issus d'autres processus.  Pour concentrer vos recherches, complétez les zones de texte suivantes dans l'affichage des événements :  
  
-   Dans la zone **Filtre des processus**, spécifiez le nom de votre application \(sans « .exe »\).  
  
-   Dans la zone **Filtre des types d'événement**, spécifiez `Process/Start | MyCompany-MyApp`.  Cette opération définit un filtre pour les événements de MyCompany\-MyApp et pour l'événement Windows Kernel\/Process\/Start.  
  
 Sélectionnez tous les événements répertoriés dans le volet de gauche \(Ctrl\+A\) et appuyez sur la touche **Entrée**.  À présent, l'horodatage de chaque événement doit apparaître.  Ces horodatages étant exprimés par rapport au début de la trace, vous devez soustraire l'heure de chaque événement de l'heure de début du processus pour identifier le temps écoulé depuis le démarrage.  Si vous utilisez Ctrl\+clic pour sélectionner deux horodatages, vous verrez la différence qui les sépare dans la barre d'état en bas de la page.  Cela permet de voir facilement le temps écoulé entre deux événements dans l'affichage \(y compris le début des processus\).  Vous pouvez ouvrir le menu contextuel de l'affichage pour accéder à de nombreuses options utiles, telles que l'exportation vers des fichiers CSV ou l'ouverture de Microsoft Excel pour enregistrer ou traiter les données.  
  
 En répétant la procédure pour votre application d'origine et la version que vous avez créée à l'aide de la chaîne d'outils [!INCLUDE[net_native](../../../includes/net-native-md.md)], vous pouvez comparer les performances.  En règle générale, les applications [!INCLUDE[net_native](../../../includes/net-native-md.md)] démarrent plus rapidement que les applications non\-[!INCLUDE[net_native](../../../includes/net-native-md.md)].  Si vous souhaitez en savoir plus, PerfView peut également identifier les parties de votre code qui prennent le plus de temps.  Pour plus d'informations, regardez les [didacticiels PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) ou consultez le [billet de blog de Vance Morrison](http://blogs.msdn.com/b/vancem/archive/2011/12/28/publication-of-the-perfview-performance-analysis-tool.aspx).  
  
## Voir aussi  
 <xref:System.Diagnostics.Tracing.EventSource>