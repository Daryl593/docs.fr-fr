---
title: "Comment&#160;: sp&#233;cifier une liaison de service dans la configuration | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 885037f7-1c2b-4d7a-90d9-06b89be172f2
caps.latest.revision: 17
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 17
---
# Comment&#160;: sp&#233;cifier une liaison de service dans la configuration
Dans cet exemple, un contrat `ICalculator` est défini pour un service de calculatrice de base, le service est implémenté dans la classe `CalculatorService`, puis son point de terminaison est configuré dans le fichier Web.config, où il est spécifié que le service utilise <xref:System.ServiceModel.BasicHttpBinding>.Pour une description de la façon de configurer ce service à l'aide du code au lieu d'une configuration, consultez [Comment : spécifier une liaison de service dans le code](../../../docs/framework/wcf/how-to-specify-a-service-binding-in-code.md).  
  
 Il est généralement conseillé de spécifier de façon déclarative les informations de liaison et d'adresse dans la configuration plutôt que de manière impérative dans le code.La définition de points de terminaison dans le code est généralement peu pratique car les liaisons et les adresses pour un service déployé sont en général différentes de celles utilisées au cours du développement du service.Plus généralement, le fait de laisser les informations de liaison et d'adresse hors du code leur permet de changer sans nécessiter de recompilation ou de redéploiement de l'application.  
  
 Toutes les étapes de configuration suivantes peuvent être entreprises à l'aide du [Outil Éditeur de configuration \(SvcConfigEditor.exe\)](../../../docs/framework/wcf/configuration-editor-tool-svcconfigeditor-exe.md).  
  
 Pour la copie source de cet exemple, consultez [BasicBinding](../../../docs/framework/wcf/samples/basicbinding.md).  
  
### Pour spécifier le BasicHttpBinding à utiliser pour configurer le service  
  
1.  Définissez un contrat de service pour le type de service.  
  
     [!code-csharp[C_HowTo_ConfigureServiceBinding#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureservicebinding/cs/source.cs#1)]
     [!code-vb[C_HowTo_ConfigureServiceBinding#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_configureservicebinding/vb/source.vb#1)]  
  
2.  Implémentez le contrat de service dans une classe de service.  
  
     [!code-csharp[C_HowTo_ConfigureServiceBinding#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureservicebinding/cs/source.cs#2)]
     [!code-vb[C_HowTo_ConfigureServiceBinding#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_configureservicebinding/vb/source.vb#2)]  
  
    > [!NOTE]
    >  Les informations de liaison ou d'adresse ne sont pas spécifiées dans l'implémentation du service.De plus, il n'est pas nécessaire d'écrire du code pour extraire ces informations du fichier de configuration.  
  
3.  Créez un fichier Web.config pour configurer un point de terminaison pour le `CalculatorService` qui utilise le <xref:System.ServiceModel.WSHttpBinding>.  
  
    ```  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.serviceModel>  
        <services>  
          <service name=" CalculatorService" >  
            <endpoint   
            <-- Leave the address blank to be populated by default-->  
            <--from the hosting environment,in this case IIS, so -->  
            <-- the address will just be that of the IIS Virtual -->  
            <--Directory.-->  
                address=""   
            <--Specify the binding type -->  
                binding="wsHttpBinding"  
            <--Specify the binding configuration name for that -->  
            <--binding type. This is optional but useful if you  -->  
            <--want to modify the properties of the binding. -->  
            <--The bindingConfiguration name Binding1 is defined  -->  
            <--below in the bindings element.  -->  
                bindingConfiguration="Binding1"  
                contract="ICalculator" />  
          </service>  
        </services>  
        <bindings>  
          <wsHttpBinding>  
            <binding name="Binding1">  
              <-- Binding property values can be modified here. -->  
              <--See the next procedure. -->  
            </binding>  
          </wsHttpBinding>  
       </bindings>  
      </system.serviceModel>  
    </configuration>  
  
    ```  
  
4.  Créez un fichier Service.svc qui contient la ligne suivante et placez\-le dans votre répertoire virtuel des services Internet \(IIS\).  
  
    ```  
    <%@ServiceHost language=c# Service="CalculatorService" %>   
    ```  
  
### Pour modifier les valeurs par défaut des propriétés de liaison  
  
1.  Pour modifier l'une des valeurs de propriétés par défaut de <xref:System.ServiceModel.WSHttpBinding>, créez un nom de configuration de liaison \- `<binding name="Binding1">` \- dans l'élément [\<wsHttpBinding\>](../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md) et définissez les nouvelles valeurs des attributs de la liaison dans cet élément de liaison.Par exemple, pour modifier les valeurs par défaut de délai d'attente d'ouverture et de fermeture de 1 minute à 2 minutes, ajoutez le code suivant au fichier de configuration.  
  
    ```  
    <wsHttpBinding>  
      <binding name="Binding1"  
               closeTimeout="00:02:00"  
               openTimeout="00:02:00">  
      </binding>  
    </wsHttpBinding>  
    ```  
  
## Voir aussi  
 [Utilisation de liaisons pour configurer des services et des clients](../../../docs/framework/wcf/using-bindings-to-configure-services-and-clients.md)   
 [Spécification d'une adresse de point de terminaison](../../../docs/framework/wcf/specifying-an-endpoint-address.md)