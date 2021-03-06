---
title: "Configuration du service d&#39;activation de processus de Windows pour son utilisation dans Windows Communication Foundation | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 1d50712e-53cd-4773-b8bc-a1e1aad66b78
caps.latest.revision: 12
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 12
---
# Configuration du service d&#39;activation de processus de Windows pour son utilisation dans Windows Communication Foundation
Cette rubrique décrit les étapes requises pour configurer le service d'activation de processus de Windows \(également appelé service WAS\) dans [!INCLUDE[wv](../../../../includes/wv-md.md)] pour héberger des services [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] qui ne communiquent pas sur des protocoles réseau HTTP.Les sections suivantes définissent les étapes pour cette configuration :  
  
-   Installez \(ou vérifiez l'installation\) les composants d'activation [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] requis.  
  
-   Créez un site WAS avec les liaisons de protocole réseau que vous souhaitez utiliser ou ajoutez un nouveau protocole qui crée une liaison vers un site existant.  
  
-   Créez une application pour héberger vos services et permettre à cette application d'utiliser les protocoles réseau requis.  
  
-   Générez un service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] qui expose un point de terminaison non http.  
  
## Configuration d'un site avec des liaisons non HTTP  
 Pour utiliser une liaison non HTTP avec le service WAS, la liaison de site doit être ajoutée à la configuration WAS.Le magasin de configurations pour le service WAS est le fichier applicationHost.config situé dans le répertoire %windir%\\system32\\inetsrv\\config.Ce magasin de configurations est partagé à la fois par le service WAS et le service IIS 7.0.  
  
 applicationHost.config est un fichier texte XML qui peut être ouvert avec un éditeur de texte standard \(tel que le Bloc\-notes\).Toutefois, l'outil de configuration de la ligne de commande [!INCLUDE[iisver](../../../../includes/iisver-md.md)] \(appcmd.exe\) est la meilleure méthode pour ajouter des liaisons de site non HTTP.  
  
 La commande suivante ajoute une liaison de site net.tcp au site Web par défaut à l'aide de appcmd.exe \(cette commande est entrée comme une ligne unique\).  
  
```  
appcmd.exe set site "Default Web Site" -+bindings.[protocol='net.tcp',bindingInformation='808:*']  
```  
  
 Cette commande ajoute la nouvelle liaison net.tcp au site Web par défaut en ajoutant la ligne indiquée ci\-dessous au fichier applicationHost.config.  
  
```  
<sites>  
    <site name="Default Web Site" id="1">  
        <bindings>  
            <binding protocol="HTTP" bindingInformation="*:80:" />  
            //The following line is added by the command.  
            <binding protocol="net.tcp" bindingInformation="808:*" />  
        </bindings>  
    </site>  
</sites>  
```  
  
## Activation de l'utilisation des protocoles non HTTP par une application  
 Vous pouvez activer ou désactiver des protocoles réseau individuels au niveau de l'application.La commande suivante montre comment activer à la fois les protocoles HTTP et net.tcp pour une application qui s'exécute dans le `Default Web Site`.  
  
```  
appcmd.exe set app "Default Web Site/appOne" /enabledProtocols:net.tcp  
```  
  
 La liste des protocoles actifs peut être aussi  définie dans l'élément \<applicationDefaults\> de la configuration XML du site stocké dans ApplicationHost.config.  
  
 Le code XML suivant issu du fichier applicationHost.config illustre un site lié à la fois à des protocoles HTTP et non HTTP.La configuration supplémentaire requise pour la prise en charge de protocoles non HTTP est appelée avec des commentaires.  
  
```  
<sites>  
    <site name="Default Web Site" id="1">  
    <application path="/">  
        <virtualDirectory path="/" physicalPath="D:\inetpub\wwwroot" />  
    </application>  
       <bindings>  
            //The following two lines are added by the command.  
            <binding protocol="HTTP" bindingInformation="*:80:" />  
            <binding protocol="net.tcp" bindingInformation="808:*" />  
       </bindings>  
    </site>  
    <siteDefaults>  
        <logFile   
        customLogPluginClsid="{FF160663-DE82-11CF-BC0A-00AA006111E0}"  
          directory="D:\inetpub\logs\LogFiles" />  
        <traceFailedRequestsLogging   
          directory="D:\inetpub\logs\FailedReqLogFiles" />  
    </siteDefaults>  
    <applicationDefaults   
      applicationPool="DefaultAppPool"   
      //The following line is inserted by the command.  
      enabledProtocols="http, net.tcp" />  
    <virtualDirectoryDefaults allowSubDirConfig="true" />  
</sites>  
```  
  
 Si vous tentez d'activer un service en utilisant WAS pour l'activation non\-HTTP et vous n'avez pas installé et configuré WAS, l'erreur suivante s'affiche :  
  
```Output  
[InvalidOperationException : le protocole « net.tcp » n'a pas d'implémentation de type HostedTransportConfiguration enregistrée.]   System.ServiceModel.AsyncResult.End(IAsyncResult result) +15778592   System.ServiceModel.Activation.HostedHttpRequestAsyncResult.End(IAsyncResult result) +15698937   System.ServiceModel.Activation.HostedHttpRequestAsyncResult.ExecuteSynchronous(HttpApplication context, Boolean flowContext) +265   System.ServiceModel.Activation.HttpModule.ProcessRequest(Object sender, EventArgs e) +227   System.Web.SyncEventExecutionStep.System.Web.HttpApplication.IExecutionStep.Execute() +80   System.Web.HttpApplication.ExecuteStep(IExecutionStep step, Boolean& completedSynchronously) +171  
```  
  
 Si cette erreur s'affiche, assurez\-vous que WAS pour l'activation non HTTP est installé et configuré correctement.[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][Comment : installer et configurer des composants d'activation WCF](../../../../docs/framework/wcf/feature-details/how-to-install-and-configure-wcf-activation-components.md).  
  
## Génération d'un service WCF qui utilise le service WAS pour l'activation non HTTP  
 Une fois que vous effectuez les étapes pour installer et configurer le service WAS \(voir [Comment : installer et configurer des composants d'activation WCF](../../../../docs/framework/wcf/feature-details/how-to-install-and-configure-wcf-activation-components.md)\), la configuration d'un service pour utiliser le service WAS pour l'activation est semblable à la configuration d'un service hébergé dans IIS.  
  
 Pour des instructions détaillées sur la génération d'un service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] activé WAS, consultez [Comment : héberger un service WCF dans WAS](../../../../docs/framework/wcf/feature-details/how-to-host-a-wcf-service-in-was.md).  
  
## Voir aussi  
 [Hébergement dans le service d'activation de processus de Windows \(WAS, Windows Process Activation Service\)](../../../../docs/framework/wcf/feature-details/hosting-in-windows-process-activation-service.md)   
 [Fonctionnalités d'hébergement de Windows Server AppFabric](http://go.microsoft.com/fwlink/?LinkId=201276)