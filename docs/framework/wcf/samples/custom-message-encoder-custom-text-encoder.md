---
title: "Encodeur de message personnalis&#233; : Encodeur de texte personnalis&#233; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 68ff5c74-3d33-4b44-bcae-e1d2f5dea0de
caps.latest.revision: 28
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 28
---
# Encodeur de message personnalis&#233; : Encodeur de texte personnalis&#233;
Cet exemple montre comment implémenter un encodeur de message texte personnalisé à l'aide de [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)].  
  
> [!WARNING]
>  Les exemples peuvent déjà être installés sur votre ordinateur.Recherchez le répertoire \(par défaut\) suivant avant de continuer.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page \(éventuellement en anglais\) des [exemples Windows Communication Foundation \(WCF\) et Windows Workflow Foundation \(WF\) pour .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) pour télécharger tous les exemples [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] et [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Cet exemple se trouve dans le répertoire suivant.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WCF\Extensibility\MessageEncoder\Text`  
  
 Le <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> de [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] prend uniquement en charge les encodages UTF\-8, UTF\-16 et Unicode Big\-Endian.L'encodeur de message texte personnalisé de cet exemple prend en charge l'ensemble des encodages de caractères pris en charge par la plateforme qui peuvent être nécessaires pour l'interopérabilité.L'exemple se compose d'un programme de console client \(.exe\), d'une bibliothèque de service \(.dll\) hébergée par les services IIS \(Internet Information Services\) et d'une bibliothèque d'encodeurs de message texte \(.dll\).Le service implémente un contrat qui définit un modèle de communication demande\-réponse.Le contrat est défini par l'interface `ICalculator`, laquelle expose les opérations mathématiques suivantes : addition, soustraction, multiplication et division.Le client adresse des demandes synchrones à une opération mathématique donnée et le service répond avec le résultat.Le client et le service utilisent `CustomTextMessageEncoder` au lieu du <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> par défaut.  
  
 L'implémentation d'encodeur personnalisée se compose d'une fabrique d'encodeur de message, d'un encodeur de message, d'un élément de liaison d'encodage de message et d'un gestionnaire de configuration, et présente les éléments suivants :  
  
-   Création d'un encodeur personnalisé et d'une fabrique d'encodeur.  
  
-   Création d'un élément de liaison pour un encodeur personnalisé.  
  
-   Utilisation de la configuration de liaison personnalisée permettant d'intégrer des éléments de liaison personnalisés.  
  
-   Développement d'un gestionnaire de configuration personnalisé afin de permettre la configuration de fichier d'un élément de liaison personnalisé.  
  
### Pour configurer, générer et exécuter l'exemple  
  
1.  Installez [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)] 4.0 à l'aide de la commande suivante.  
  
    ```  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
  
    ```  
  
2.  Assurez\-vous d'avoir effectué la [Procédure d'installation unique pour les exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
3.  Pour générer la solution, suivez les instructions indiquées dans la rubrique [Génération des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
4.  Pour exécuter l'exemple dans une configuration à un ou plusieurs ordinateurs, suivez les instructions indiquées dans [Exécution des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
## Fabrique d'encodeur de message et encodeur de message  
 Lorsque <xref:System.ServiceModel.ServiceHost> ou le canal client est ouvert, le composant au moment du design `CustomTextMessageBindingElement` crée `CustomTextMessageEncoderFactory`.La fabrique crée `CustomTextMessageEncoder`.L'encodeur de message fonctionne à la fois en mode de diffusion en continu et en mode de mise en mémoire tampon.Il utilise <xref:System.Xml.XmlReader> et <xref:System.Xml.XmlWriter> pour respectivement lire et écrire les messages.Contrairement aux enregistreurs et lecteurs XML optimisés de [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] qui prennent uniquement en charge UTF\-8, UTF\-16 et Unicode Big\-Endian, ces lecteurs et enregistreurs prennent en charge l'ensemble des encodages de caractères pris en charge par la plateforme.  
  
 L'exemple de code suivant présente CustomTextMessageEncoder.  
  
```csharp  
  
public class CustomTextMessageEncoder : MessageEncoder  
{  
    private CustomTextMessageEncoderFactory factory;  
    private XmlWriterSettings writerSettings;  
    private string contentType;  
  
    public CustomTextMessageEncoder(CustomTextMessageEncoderFactory factory)  
    {  
        this.factory = factory;  
  
        this.writerSettings = new XmlWriterSettings();  
        this.writerSettings.Encoding = Encoding.GetEncoding(factory.CharSet);  
        this.contentType = string.Format("{0}; charset={1}",   
            this.factory.MediaType, this.writerSettings.Encoding.HeaderName);  
    }  
  
    public override string ContentType  
    {  
        get  
        {  
            return this.contentType;  
        }  
    }  
  
    public override string MediaType  
    {  
        get   
        {  
            return factory.MediaType;  
        }  
    }  
  
    public override MessageVersion MessageVersion  
    {  
        get   
        {  
            return this.factory.MessageVersion;  
        }  
    }  
  
    public override Message ReadMessage(ArraySegment<byte> buffer, BufferManager bufferManager, string contentType)  
    {     
        byte[] msgContents = new byte[buffer.Count];  
        Array.Copy(buffer.Array, buffer.Offset, msgContents, 0, msgContents.Length);  
        bufferManager.ReturnBuffer(buffer.Array);  
  
        MemoryStream stream = new MemoryStream(msgContents);  
        return ReadMessage(stream, int.MaxValue);  
    }  
  
    public override Message ReadMessage(Stream stream, int maxSizeOfHeaders, string contentType)  
    {  
        XmlReader reader = XmlReader.Create(stream);  
        return Message.CreateMessage(reader, maxSizeOfHeaders, this.MessageVersion);  
    }  
  
    public override ArraySegment<byte> WriteMessage(Message message, int maxMessageSize, BufferManager bufferManager, int messageOffset)  
    {  
        MemoryStream stream = new MemoryStream();  
        XmlWriter writer = XmlWriter.Create(stream, this.writerSettings);  
        message.WriteMessage(writer);  
        writer.Close();  
  
        byte[] messageBytes = stream.GetBuffer();  
        int messageLength = (int)stream.Position;  
        stream.Close();  
  
        int totalLength = messageLength + messageOffset;  
        byte[] totalBytes = bufferManager.TakeBuffer(totalLength);  
        Array.Copy(messageBytes, 0, totalBytes, messageOffset, messageLength);  
  
        ArraySegment<byte> byteArray = new ArraySegment<byte>(totalBytes, messageOffset, messageLength);  
        return byteArray;  
    }  
  
    public override void WriteMessage(Message message, Stream stream)  
    {  
        XmlWriter writer = XmlWriter.Create(stream, this.writerSettings);  
        message.WriteMessage(writer);  
        writer.Close();  
    }  
}  
```  
  
 L'exemple de code suivant montre comment créer la fabrique d'encodeur de message.  
  
```csharp  
public class CustomTextMessageEncoderFactory : MessageEncoderFactory  
{  
    private MessageEncoder encoder;  
    private MessageVersion version;  
    private string mediaType;  
    private string charSet;  
  
    internal CustomTextMessageEncoderFactory(string mediaType, string charSet,  
        MessageVersion version)  
    {  
        this.version = version;  
        this.mediaType = mediaType;  
        this.charSet = charSet;  
        this.encoder = new CustomTextMessageEncoder(this);  
    }  
  
    public override MessageEncoder Encoder  
    {  
        get   
        {   
            return this.encoder;  
        }  
    }  
  
    public override MessageVersion MessageVersion  
    {  
        get   
        {   
            return this.version;  
        }  
    }  
  
    internal string MediaType  
    {  
        get  
        {  
            return this.mediaType;  
        }  
    }  
  
    internal string CharSet  
    {  
        get  
        {  
            return this.charSet;  
        }  
    }  
}  
```  
  
## Élément de liaison d'encodage de message  
 Les éléments de liaison permettent de configurer la pile d'exécution [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].Pour utiliser l'encodeur de message personnalisé dans une application [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], un élément de liaison est requis et permet de créer la fabrique d'encodeur de message avec les paramètres appropriés au niveau correct dans la pile d'exécution.  
  
 `CustomTextMessageBindingElement` dérive de la classe de base <xref:System.ServiceModel.Channels.BindingElement> et hérite de la classe <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>.Cela permet à d'autres composants [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] de reconnaître cet élément de liaison comme étant un élément de liaison d'encodage de message.L'implémentation de <xref:System.ServiceModel.Channels.MessageEncodingBindingElement.CreateMessageEncoderFactory%2A> retourne une instance de la fabrique d'encodeur de message correspondante avec les paramètres appropriés.  
  
 `CustomTextMessageBindingElement` expose des paramètres pour `MessageVersion`, `ContentType` et `Encoding` via des propriétés.L'encodeur prend en charge les versions Soap11Addressing et Soap12Addressing1.La valeur par défaut est Soap11Addressing1.La valeur par défaut de `ContentType` est "text\/xml".La propriété `Encoding` vous permet de définir la valeur de l'encodage de caractères souhaité.L'exemple de client et de service utilise l'encodage de caractères ISO\-8859\-1 \(Latin1\), qui n'est pas pris en charge par le <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> de [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].  
  
 Le code suivant montre comment créer la liaison par programme à l'aide de l'encodeur de message texte personnalisé.  
  
```  
ICollection<BindingElement> bindingElements = new List<BindingElement>();  
HttpTransportBindingElement httpBindingElement = new HttpTransportBindingElement();  
CustomTextMessageBindingElement textBindingElement = new CustomTextMessageBindingElement();  
bindingElements.Add(textBindingElement);  
bindingElements.Add(httpBindingElement);  
CustomBinding binding = new CustomBinding(bindingElements);  
```  
  
## Ajout de la prise en charge des métadonnées à l'élément de liaison d'encodage de message  
 Les types qui dérivent de <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> sont chargés de mettre à jour la version de la liaison SOAP dans le document WSDL généré pour le service.Pour ce faire, implémentez la méthode `ExportEndpoint` sur l'interface <xref:System.ServiceModel.Description.IWsdlExportExtension>, puis modifiez le document WSDL généré.Dans cet exemple, `CustomTextMessageBindingElement` utilise la logique d'exportation WSDL de `TextMessageEncodingBinidngElement`.  
  
 Pour cet exemple, la configuration du client est créée manuellement.Vous ne pouvez pas utiliser Svcutil.exe pour générer la configuration du client car `CustomTextMessageBindingElement` n'exporte pas d'assertion de stratégie pour décrire son comportement.Il est en général préférable d'implémenter l'interface <xref:System.ServiceModel.Description.IPolicyExportExtension> sur un élément de liaison personnalisé pour exporter une assertion de stratégie personnalisée qui décrit le comportement ou la fonctionnalité implémentée par l'élément de liaison.Pour obtenir un exemple d'exportation d'assertion de stratégie pour un élément de liaison personnalisé, consultez l'exemple [Transport: UDP](../../../../docs/framework/wcf/samples/transport-udp.md).  
  
## Gestionnaire de configuration de liaison d'encodage de message  
 La section précédente montre comment utiliser l'encodeur de message texte personnalisé par programme.`CustomTextMessageEncodingBindingSection` implémente un gestionnaire de configuration qui vous permet de spécifier l'utilisation d'un encodeur de message texte personnalisé dans un fichier de configuration.La classe `CustomTextMessageEncodingBindingSection` dérive de la classe <xref:System.ServiceModel.Configuration.BindingElementExtensionElement>.La propriété `BindingElementType` indique au système de configuration le type d'élément de liaison à créer pour cette section.  
  
 Tous les paramètres définis par `CustomTextMessageBindingElement` sont exposés en tant que propriétés dans `CustomTextMessageEncodingBindingSection`.<xref:System.Configuration.ConfigurationPropertyAttribute> permet de mapper les attributs d'élément de configuration aux propriétés et d'affecter des valeurs par défaut si les attributs ne sont pas définis.Après avoir chargé et appliqué les valeurs de la configuration aux propriétés du type, la méthode <xref:System.ServiceModel.Configuration.BindingElementExtensionElement.CreateBindingElement%2A> est appelée et convertit les propriétés en instance concrète d'un élément de liaison.  
  
 Ce gestionnaire de configuration mappe à la représentation suivante dans le fichier App.config ou Web.config du service ou du client.  
  
```  
<customTextMessageEncoding encoding="utf-8" contentType="text/xml" messageVersion="Soap11Addressing1" />  
  
```  
  
 L'exemple utilise l'encodage ISO\-8859\-1.  
  
 Pour utiliser ce gestionnaire de configuration, il doit être enregistré à l'aide de l'élément de configuration suivant.  
  
```  
<extensions>  
    <bindingElementExtensions>  
        <add name="customTextMessageEncoding" type="   
Microsoft.ServiceModel.Samples.CustomTextMessageEncodingBindingSection,   
                  CustomTextMessageEncoder" />  
    </bindingElementExtensions>  
</extensions>  
  
```  
  
## Voir aussi