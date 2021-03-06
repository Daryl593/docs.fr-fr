---
title: "UDP Activation | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 4b0ccd10-0dfb-4603-93f9-f0857c581cb7
caps.latest.revision: 15
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 15
---
# UDP Activation
Cet exemple est basé sur l'exemple [Transport: UDP](../../../../docs/framework/wcf/samples/transport-udp.md).Il étend l'exemple [Transport: UDP](../../../../docs/framework/wcf/samples/transport-udp.md) afin de prendre en charge l'activation de processus à l'aide du service d'activation des processus Windows \(WAS , Windows Process Activation Service\).  
  
 L'exemple se compose de trois éléments majeurs :  
  
-   Un activateur de protocole UDP, un processus autonome qui reçoit des messages UDP de la part des applications qui seront activées ;  
  
-   Un client qui utilise le transport personnalisé d'UDP pour envoyer des messages ;  
  
-   Un service \(hébergé dans un processus de travail activé par WAS\) qui reçoit des messages sur le transport UDP personnalisé.  
  
## Activateur de protocole UDP  
 L'activateur de protocole UDP est un pont entre le client [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] et le service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].Il assure la communication des données à travers le protocole UDP au niveau de la couche de transport.Il possède deux fonctionnalités majeures :  
  
-   L'adaptateur d'écouteur \(LA\) WAS, qui collabore avec WAS pour activer des processus en réponse à des messages entrants ;  
  
-   L'écouteur de protocole UDP, qui accepte les messages UDP de la part des applications qui seront activées.  
  
 L'activateur doit s'exécuter comme un programme autonome sur l'ordinateur serveur.Normalement, les adaptateurs d'écouteur WAS \(tels que NetTcpActivator et NetPipeActivator\) sont implémentés dans les services Windows à durée d'exécution longue.Toutefois, pour des questions de simplicité et de clarté, cet exemple implémente l'activateur de protocole en tant qu'application autonome.  
  
### Adaptateur d'écouteur WAS  
 L'adaptateur d'écouteur WAS pour UDP est implémenté dans la classe `UdpListenerAdapter`.C'est le module qui interagit avec WAS pour effectuer l'activation d'application pour le protocole UDP.Cela s'effectue en appelant les API Webhost suivantes :  
  
-   `WebhostRegisterProtocol`  
  
-   `WebhostUnregisterProtocol`  
  
-   `WebhostOpenListenerChannelInstance`  
  
-   `WebhostCloseAllListenerChannelInstances`  
  
 Après avoir appelé `WebhostRegisterProtocol`, l'adaptateur d'écouteur reçoit le rappel `ApplicationCreated` de WAS pour toutes les applications enregistrées dans applicationHost.config \(localisé dans %windir%\\system32\\inetsrv\).Dans cet exemple, nous gérons uniquement les applications avec le protocole UDP \(avec l'identificateur de protocole « net.udp »\) activé.D'autres implémentations peuvent gérer cela différemment si de telles implémentations répondent aux modifications de configuration dynamiques de l'application \(par exemple, le passage d'application de l'état désactivé à l'état activé\).  
  
 Lorsque le rappel `ConfigManagerInitializationCompleted` est reçu, il indique que WAS a terminé toutes les notifications pour l'initialisation du protocole.À ce stade, l'adaptateur d'écouteur est prêt à traiter les demandes d'activation.  
  
 Lorsqu'une nouvelle demande arrive pour la première fois pour une application, l'adaptateur d'écouteur appelle `WebhostOpenListenerChannelInstance` dans WAS, qui démarre le processus de travail s'il n'est pas déjà démarré.Ensuite, les gestionnaires de protocoles sont chargés et la communication entre l'adaptateur d'écouteur et l'application virtuelle peut démarrer.  
  
 L'adaptateur d'écouteur est enregistré dans %SystemRoot%\\System32\\inetsrv\\ApplicationHost.config dans la section \<`listenerAdapters`\> comme suit :  
  
```  
<add name="net.udp" identity="S-1-5-21-2127521184-1604012920-1887927527-387045" />  
```  
  
### Écouteur de protocole  
 L'écouteur de protocole d'UDP est un module interne à l'activateur de protocole qui écoute un point de terminaison UDP au nom de l'application virtuelle.Il est implémenté dans la classe `UdpSocketListener`.Le point de terminaison est représenté comme un `IPEndpoint` pour lequel le numéro de port est extrait de la liaison du protocole pour le site.  
  
### Service de contrôle  
 Dans cet exemple, nous utilisons [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] pour communiquer entre l'activateur et le processus de travail WAS.Le service qui réside dans l'activateur est appelé le service de contrôle.  
  
## Gestionnaires de protocoles  
 Après que l'adaptateur d'écouteur a appelé `WebhostOpenListenerChannelInstance`, le gestionnaire de processus WAS démarre le processus de travail s'il n'est pas démarré.Le gestionnaire d'application à l'intérieur du processus de travail charge ensuite le gestionnaire de protocole du processus \(PPH\) UDP avec la demande pour ce `ListenerChannelId`.Le PPH appelle à son tour `IAdphManager`.`StartAppDomainProtocolListenerChannel` pour démarrer le gestionnaire de protocole AppDomain \(ADPH\) UDP.  
  
## HostedUDPTransportConfiguration  
 Les informations sont enregistrées dans Web.config comme suit :  
  
```  
<serviceHostingEnvironment>  
<add name="net.udp" transportConfigurationType="Microsoft.ServiceModel.Samples.Hosting.HostedUdpTransportConfiguration, UdpActivation, Version=1.0.0.0, Culture=neutral, PublicKeyToken=6fa904d2da1848d6" />  
</serviceHostingEnvironment>  
```  
  
## Installation spéciale pour cet exemple  
 Cet exemple ne peut être généré et exécuté que sur Windows Vista, Windows Server 2008 ou Windows 7.Pour exécuter l'exemple, vous devez d'abord configurer correctement tous les composants.Procédez comme suit pour installer l'exemple.  
  
#### Pour installer cet exemple  
  
1.  Installez [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)] 4.0 à l'aide de la commande suivante.  
  
    ```  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
  
    ```  
  
2.  Générez le projet sur Windows Vista.Après la compilation, il effectue également les opérations suivantes dans la phase de post\-génération :  
  
    -   Installe la liaison UDP avec le site « Site Web par défaut » ;  
  
    -   Crée l'application virtuelle « ServiceModelSamples » qui doit pointer vers le chemin d'accès physique : « %SystemDrive%\\inetpub\\wwwroot\\servicemodelsamples » ;  
  
    -   Il active également le protocole « net.udp » pour cette application virtuelle.  
  
3.  Démarrez l'application d'interface utilisateur « WasNetActivator.exe ».Cliquez sur l'onglet **Installation**, activez les cases à cocher suivantes puis cliquez sur **Installer** pour les installer :  
  
    -   Adaptateur d'écouteur UDP  
  
    -   Gestionnaires de protocoles UDP  
  
4.  Cliquez sur l'onglet **Activation** de l'application d'interface utilisateur « WasNetActivator.exe ».Cliquez sur le bouton **Démarrer** pour démarrer l'adaptateur d'écouteur.Vous êtes maintenant prêt à exécuter le programme.  
  
    > [!NOTE]
    >  Lorsque vous avez terminé avec cet exemple, vous devez exécuter Cleanup.bat pour supprimer la liaison net.udp du « Site Web par défaut ».  
  
## Utilisation de l'exemple  
 Après la compilation, quatre binaires différents sont générés :  
  
-   Client.exe : le code client.App.config est compilé dans le fichier de configuration client Client.exe.config.  
  
-   UDPActivation.dll : la bibliothèque qui contient toutes les implémentations UDP majeures.  
  
-   Service.dll : le code de service.Est copié dans le répertoire \\bin de l'application virtuelle ServiceModelSamples.Le fichier du service est Service.svc et le fichier de configuration est Web.config.Après compilation, ils sont copiés à l'emplacement suivant : %SystemDrive%\\Inetpub\\wwwroot\\ServiceModelSamples.  
  
-   WasNetActivator : le programme de l'activateur UDP.  
  
-   Assurez\-vous que tous les éléments requis sont installés correctement.Les étapes suivantes indiquent comment exécuter l'exemple :  
  
1.  Assurez\-vous que les services Windows suivants ont été démarrés :  
  
    -   service d'activation des processus Windows \(WAS\) ;  
  
    -   Services Internet \(IIS\) : W3SVC.  
  
2.  Puis démarrez l'activateur, WasNetActivator.exe.Sous l'onglet **Activation**, le seul protocole, **UDP**, est sélectionné dans la liste déroulante.Cliquez sur le bouton **Démarrer** pour démarrer l'activateur.  
  
3.  Une fois l'activateur démarré, vous pouvez exécuter le code client en exécutant Client.exe à partir d'une fenêtre de commande.La sortie peut se présenter comme suit :  
  
    ```  
    Testing Udp Activation.  
    Start the status service.  
    Sending UDP datagrams.  
    Type a word that you want to say to the server: Hello, world!  
        Sending datagram: Hello, world![0]  
        Sending datagram: Hello, world![1]  
        Sending datagram: Hello, world![2]  
        Sending datagram: Hello, world![3]  
        Sending datagram: Hello, world![4]  
    Calling UDP duplex contract (ICalculatorContract).  
        0 + 0 = 0  
        1 + 2 = 3  
        2 + 4 = 6  
        3 + 6 = 9  
        4 + 8 = 12  
    Getting status and dump server traces:  
        Operation 'Hello' is called: Hello, world![0]  
        Operation 'Hello' is called: Hello, world![1]  
        Operation 'Hello' is called: Hello, world![2]  
        Operation 'Hello' is called: Hello, world![3]  
        Operation 'Hello' is called: Hello, world![4]  
        Operation 'Add' is called: 0 + 0  
        Operation 'Add' is called: 1 + 2  
        Operation 'Add' is called: 2 + 4  
        Operation 'Add' is called: 3 + 6  
        Operation 'Add' is called: 4 + 8  
    Press <ENTER> to complete test.  
    ```  
  
> [!IMPORTANT]
>  Les exemples peuvent déjà être installés sur votre ordinateur.Recherchez le répertoire \(par défaut\) suivant avant de continuer.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page \(éventuellement en anglais\) des [exemples Windows Communication Foundation \(WCF\) et Windows Workflow Foundation \(WF\) pour .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) pour télécharger tous les exemples [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] et [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Cet exemple se trouve dans le répertoire suivant.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WCF\Extensibility\Transport\UdpActivation`  
  
## Voir aussi