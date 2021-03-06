---
title: "Strat&#233;gie de s&#233;curit&#233; de WPF&#160;- ing&#233;nierie de s&#233;curit&#233; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-wpf"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "gestion du code critique"
  - "cycle de vie de la sécurité (SDL - Security Development Lifecycle), gestion du code critique"
  - "cycle de vie de la sécurité (SDL - Security Development Lifecycle), analyse de la sécurité"
  - "cycle de vie de la sécurité (SDL - Security Development Lifecycle), outils d'analyse de la source"
  - "cycle de vie de la sécurité (SDL - Security Development Lifecycle), outils d'édition de la source"
  - "cycle de vie de la sécurité (SDL - Security Development Lifecycle), techniques de test"
  - "cycle de vie de la sécurité (SDL - Security Development Lifecycle), modélisation de menace"
  - "sécurité, techniques de test"
  - "outils d'analyse de la source"
  - "outils d'édition de la source"
  - "tester, sécurité"
  - "modélisation de menace"
ms.assetid: 0fc04394-4e47-49ca-b0cf-8cd1161d95b9
caps.latest.revision: 11
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 9
---
# Strat&#233;gie de s&#233;curit&#233; de WPF&#160;- ing&#233;nierie de s&#233;curit&#233;
Trustworthy Computing \(informatique de confiance\) est une initiative de Microsoft qui vise à garantir la production de code sécurisé.  Un élément clé de l'initiative Trustworthy Computing est [!INCLUDE[TLA#tla_sdl](../../../includes/tlasharptla-sdl-md.md)].  [!INCLUDE[TLA2#tla_sdl](../../../includes/tla2sharptla-sdl-md.md)] est une pratique d'ingénierie utilisée en association avec des processus d'ingénierie standard pour faciliter la livraison de code sécurisé.  [!INCLUDE[TLA2#tla_sdl](../../../includes/tla2sharptla-sdl-md.md)] se compose de dix phases qui associent les meilleures pratiques à la formalisation, à la mesurabilité et à une structure supplémentaire qui comprend les aspects suivants :  
  
-   analyse de la conception de la sécurité ;  
  
-   contrôles de qualité basés sur des outils ;  
  
-   test de pénétration ;  
  
-   réexamen final de la sécurité ;  
  
-   gestion de la sécurité du produit après lancement.  
  
## Spécificités de WPF  
 L'équipe d'ingénierie [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] applique et étend [!INCLUDE[TLA2#tla_sdl](../../../includes/tla2sharptla-sdl-md.md)], dont la combinaison inclut les aspects clés suivants :  
  
 [Modélisation des menaces](#thread_modeling)  
  
 [Analyse de la sécurité et outils de modification](#tools)  
  
 [Techniques de test](#techniques)  
  
 [Gestion du code critique](#critical_code)  
  
<a name="threat_modeling"></a>   
### Modélisation des menaces  
 La modélisation des menaces est une composante essentielle de [!INCLUDE[TLA2#tla_sdl](../../../includes/tla2sharptla-sdl-md.md)], qui vise à profiler un système dans le but d'identifier les failles de sécurité potentielles.  Une fois les failles identifiées, la modélisation des menaces permet aussi de s'assurer que des mesures de prévention appropriées ont été prises.  
  
 D'un point de vue général, la modélisation des menaces comporte les étapes clés suivantes, ici appliquées à l'exemple d'une épicerie :  
  
1.  **Identification des ressources** :  les ressources d'une épicerie peuvent inclure les employés, un coffre\-fort, les caisses et le stock.  
  
2.  **Énumération des points d'entrée** :  les points d'entrée d'une épicerie peuvent inclure les portes de devant et de derrière, les fenêtres, le quai de chargement et les climatiseurs.  
  
3.  **Réflexion sur les attaques possibles à l'encontre des ressources via les points d'entrée** :  dans une épicerie, le *coffre\-fort* peut être la cible d'une attaque via le point d'entrée que constitue le *climatiseur* ; celui\-ci pourrait être démonté et, par le passage ainsi libéré, permettre d'extraire le coffre\-fort hors du magasin.  
  
 La modélisation des menaces s'applique à l'échelle de [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] et comprend les éléments suivants :  
  
-   Comment l'analyseur [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] lit les fichiers, mappe le texte aux classes de modèle objet correspondantes et crée le code réel.  
  
-   Comment un handle de fenêtre \(hWnd\) est créé, envoie des messages et est utilisé pour restituer le contenu d'une fenêtre.  
  
-   Comment la liaison de données obtient les ressources et interagit avec le système.  
  
 Ces modèles de menace sont importants pour identifier les spécifications de conception de sécurité et les mesures de prévention pendant le processus de développement.  
  
<a name="tools"></a>   
### Analyse de la source et outils de modification  
 Outre les éléments manuels de révision du code de sécurité de [!INCLUDE[TLA2#tla_sdl](../../../includes/tla2sharptla-sdl-md.md)], l'équipe [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] utilise plusieurs outils pour analyser la source et apporter les modifications associées pour réduire les vulnérabilités en matière de sécurité.  Une large gamme d'outils source est utilisé et comprend les éléments suivants :  
  
-   **FXCop** : recherche les problèmes de sécurité courants dans le code managé, depuis les règles d'héritage et l'utilisation de la sécurité d'accès du code jusqu'aux méthodes permettant d'interagir sans risque avec le code non managé.  Consultez [FXCop](http://www.gotdotnet.com/team/fxcop/).  
  
-   **Prefix\/Prefast** : recherche les failles de sécurité et les problèmes de sécurité courants dans le code non managé, tels que les dépassements de mémoire tampon, les problèmes de chaîne de format et la vérification d'erreurs.  
  
-   **API interdites** : analyse le code source à la recherche d'une utilisation accidentelle de fonctions connues pour provoquer des problèmes de sécurité, telles que `strcpy`.  Une fois identifiées, ces fonctions sont remplacées par d'autres, plus sûres.  
  
<a name="techniques"></a>   
### Techniques de test  
 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] utilise plusieurs techniques de test de sécurité, à savoir :  
  
-   **Test de la boîte blanche** : les testeurs examinent le code source et élaborent des tests d'attaque.  
  
-   **Test de la boîte noire** : les testeurs tachent de trouver des failles de sécurité en examinant l'API et les fonctionnalités, puis essaient d'attaquer le produit.  
  
-   **Régression des problèmes de sécurité d'autres produits** : le cas échéant, les problèmes de sécurité provenant de produits connexes sont testés.  Ainsi, des variantes appropriées d'une soixantaine de problèmes de sécurité pour [!INCLUDE[TLA2#tla_ie](../../../includes/tla2sharptla-ie-md.md)] ont été identifiées et testées pour vérifier si elles étaient applicables à [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)].  
  
-   **Test de pénétration basé sur des outils et la technique du « fuzzing »** : le fuzzing \(ou tests à données aléatoires\) consiste à exploiter la plage d'entrées d'un lecteur de fichiers à travers diverses entrées.  Par exemple, dans [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)], cette technique est employée pour rechercher des défaillances dans le code de décodage d'image.  
  
<a name="critical_code"></a>   
### Gestion du code critique  
 Pour les [!INCLUDE[TLA#tla_xbap#plural](../../../includes/tlasharptla-xbapsharpplural-md.md)], [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] génère un bac à sable \(sandbox\) de sécurité en s'appuyant sur la prise en charge de [!INCLUDE[TLA2#tla_winfx](../../../includes/tla2sharptla-winfx-md.md)] pour baliser et suivre du code critique de sécurité qui élève les privilèges \(consultez **Méthodologie de sécurité critique** dans [Stratégie de sécurité de WPF \- ingénierie de la plateforme](../../../docs/framework/wpf/wpf-security-strategy-platform-security.md)\).  Compte tenu des hautes exigences de qualité en matière de sécurité sur le code critique de sécurité, un tel code bénéficie d'un niveau de contrôle supplémentaire sur le plan de la gestion de la source et de l'audit de sécurité.  Environ 5 à 10 % de [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] est constitué de code critique de sécurité, qui est examiné par une équipe de révision dédiée.  Le code source et le processus d'archivage est géré par le suivi du code critique de sécurité et le mappage de chaque entité critique \(c'est\-à\-dire,  une méthode qui contient le code critique\) à son état de validation.  L'état de validation s'accompagne des noms d'un ou plusieurs réviseurs.  Chaque build quotidienne de [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] compare le code critique à celui des versions précédentes pour vérifier la présence éventuelle de modifications non approuvées.  Si un ingénieur modifie du code critique sans l'approbation de l'équipe de révision, celui\-ci est identifié et corrigé immédiatement.  Ce processus permet d'appliquer et de maintenir un niveau particulièrement élevé de surveillance sur le code du bac à sable \(sandbox\) [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)].  
  
## Voir aussi  
 [Sécurité](../../../docs/framework/wpf/security-wpf.md)   
 [Sécurité de confiance partielle de WPF](../../../docs/framework/wpf/wpf-partial-trust-security.md)   
 [Stratégie de sécurité de WPF \- ingénierie de la plateforme](../../../docs/framework/wpf/wpf-security-strategy-platform-security.md)   
 [Trustworthy Computing \(informatique de confiance\)](http://www.microsoft.com/mscorp/twc/default.mspx)   
 [Modélisation des menaces des applications](http://msdn.microsoft.com/security/securecode/threatmodeling/acetm/)   
 [Instructions de sécurité : .NET Framework 2.0](http://msdn.microsoft.com/library/default.asp?url=/library/dnpag2/html/PAGGuidelines0003.asp)