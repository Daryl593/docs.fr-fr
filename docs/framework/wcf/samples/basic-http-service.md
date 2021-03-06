---
title: "Service HTTP de base | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 27048b43-8a54-4f2a-9952-594bbfab10ad
caps.latest.revision: 9
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 9
---
# Service HTTP de base
Cet exemple montre comment implémenter un service RPC HTTP, communément appelé service « POX » \(Plain Old XML\), à l'aide du modèle de programmation REST [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)].Cet exemple est constitué de deux composants : un service HTTP [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] auto\-hébergé \(Service.cs\) et une application console \(Program.cs\) qui crée le service et lui passe des appels.  
  
## Détails de l'exemple  
 Le service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] expose 2 opérations, `EchoWithGet` et `EchoWithPost`, qui retournent la chaîne passée en entrée.  
  
 L'opération `EchoWithGet` est annotée avec <xref:System.ServiceModel.Web.WebGetAttribute>, ce qui indique que l'opération traite des requêtes HTTP `GET`.Comme le <xref:System.ServiceModel.Web.WebGetAttribute> ne spécifie pas explicitement d'<xref:System.UriTemplate>, l'opération attend le passage de la chaîne d'entrée à l'aide d'un paramètre de chaîne de requête mentionnant le nom `s`.Notez que le format de l'URI attendu par le service peut être personnalisé à l'aide de la propriété <xref:System.ServiceModel.Web.WebGetAttribute.UriTemplate%2A>.  
  
 L'opération `EchoWithPost` est annotée avec <xref:System.ServiceModel.Web.WebInvokeAttribute>, ce qui indique qu'il ne s'agit pas d'une opération `GET` \(elle a des effets secondaires\).Comme le <xref:System.ServiceModel.Web.WebInvokeAttribute> ne spécifie pas explicitement de `Method`, l'opération traite les requêtes HTTP `POST` dont le corps contient la chaîne \(au format XML, par exemple\).Notez que la méthode HTTP et le format de l'URI de la requête peuvent être personnalisés à l'aide des propriétés <xref:System.ServiceModel.Web.WebInvokeAttribute.Method%2A> et <xref:System.ServiceModel.Web.WebInvokeAttribute.UriTemplate>, respectivement.  
  
 Le fichier App.config configure le service WCF avec un <xref:System.ServiceModel.Description.WebHttpEndpoint> par défaut dont la propriété <xref:System.ServiceModel.Description.WebHttpEndpoint.HelpEnabled%2A> a la valeur `true`.Par conséquent, l'infrastructure [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] crée une page d'aide HTML automatique sous `http://localhost:8000/Customers/help` qui fournit des informations sur la façon de construire des requêtes HTTP adressées au service et de consommer la réponse HTTP du service.  
  
 Le fichier Program.cs montre comment une fabrique de canaux [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] peut être utilisée pour passer des appels au service et traiter les réponses.Notez qu'il ne s'agit là que de l'un des moyens d'accéder à un service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].Il est également possible d'accéder au service à l'aide d'autres classes .NET Framework, comme <xref:System.Net.HttpWebRequest> et <xref:System.Net.WebClient>.D'autres exemples du Kit de développement logiciel \(SDK\) \(comme les exemples [Sélection automatique du format](../../../../docs/framework/wcf/samples/automatic-format-selection.md) et [Basic Resource Service](../../../../docs/framework/wcf/samples/basic-resource-service.md)\) montrent comment utiliser ces classes pour communiquer avec un service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].  
  
 L'exemple est constitué d'un service auto\-hébergé et d'un client qui s'exécutent tous deux dans une application console.Lorsque l'application console s'exécute, le client adresse des requêtes au service et affiche les informations pertinentes des réponses dans la fenêtre de console.  
  
#### Pour utiliser cet exemple  
  
1.  Ouvrez la solution de l'exemple Basic HTTP Service.Pour que l'exemple fonctionne correctement, vous devez exécuter [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] en tant qu'administrateur.Pour ce faire, cliquez avec le bouton droit sur l'icône [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] et sélectionnez **Exécuter en tant qu'administrateur** dans le menu contextuel.  
  
2.  Appuyez sur CTRL\+MAJ\+B pour générer la solution, puis appuyez sur Ctrl\+F5 pour exécuter l'application console sans débogage.La fenêtre de console apparaît et fournit l'URI du service en cours d'exécution, ainsi que l'URI de sa page d'aide HTML.Vous pouvez à tout moment consulter la page d'aide HTML en tapant son URI dans un navigateur.Lorsque l'exemple s'exécute, le client écrit l'état de l'activité actuelle.  
  
3.  Appuyez sur une touche quelconque pour arrêter l'exemple.  
  
> [!IMPORTANT]
>  Les exemples peuvent déjà être installés sur votre ordinateur.Recherchez le répertoire \(par défaut\) suivant avant de continuer.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page \(éventuellement en anglais\) des [exemples Windows Communication Foundation \(WCF\) et Windows Workflow Foundation \(WF\) pour .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) pour télécharger tous les exemples [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] et [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Cet exemple se trouve dans le répertoire suivant.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WCF\Basic\Web\BasicHttpService`  
  
## Voir aussi  
 [Sélection automatique du format](../../../../docs/framework/wcf/samples/automatic-format-selection.md)   
 [Basic Resource Service](../../../../docs/framework/wcf/samples/basic-resource-service.md)