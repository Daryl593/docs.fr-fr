---
title: "Activation du flux de transaction | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "transactions (WCF), activer le flux"
ms.assetid: a03f5041-5049-43f4-897c-e0292d4718f7
caps.latest.revision: 17
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 17
---
# Activation du flux de transaction
[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] fournit des options très flexibles pour contrôler le flux de transaction.Les paramètres de flux de transaction d'un service peuvent être exprimés à l'aide d'une combinaison d'attributs et de configuration.  
  
## Paramètres de flux de transaction  
 Les paramètres de flux de transaction sont générés pour un point de terminaison de service suite à l'intersection des trois valeurs suivantes :  
  
-   Attribut <xref:System.ServiceModel.TransactionFlowAttribute> spécifié pour chaque méthode dans le contrat de service.  
  
-   Propriété de liaison `TransactionFlow` dans la liaison spécifique.  
  
-   Propriété de liaison `TransactionFlowProtocol` dans la liaison spécifique.La propriété de liaison `TransactionFlowProtocol` vous permet de choisir entre deux protocoles de transaction différents que vous pouvez utiliser pour transmettre une transaction.Les sections ci\-dessous décrivent brièvement ces deux protocoles.  
  
### Protocole WS\-AtomicTransaction  
 Le protocole WS\-AtomicTransaction \(WS\-AT\) est utile pour les scénarios où l'interopérabilité avec des piles de protocoles tiers est requise.  
  
### Protocole OleTransactions  
 Le protocole OleTransactions est utile pour les scénarios où l'interopérabilité avec les piles de protocoles tiers n'est pas requise et où le responsable du déploiement d'un service sait à l'avance que le service de protocole WS\-AT est désactivé localement ou que la topologie du réseau existante ne favorise pas l'utilisation de WS\-AT.  
  
 Le tableau ci\-dessous indique les différents types de flux de transaction qui peuvent être générés à l'aide de ces différentes combinaisons.  
  
|Liaison<br /><br /> TransactionFlow|Propriété de liaison TransactionFlow|Protocole de liaison TransactionFlowProtocol|Type de flux de transaction|  
|---------------------------------|------------------------------------------|--------------------------------------------------|---------------------------------|  
|Mandatory|true|WS\-AT|La transaction doit être transmise dans le format WS\-AT interopérable.|  
|Mandatory|true|OleTransactions|La transaction doit être transmise dans le format OleTransactions [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].|  
|Mandatory|false|Non applicable|Non applicable parce qu'il s'agit d'une configuration non valide.|  
|Allowed|true|WS\-AT|La transaction peut être transmise dans le format WS\-AT interopérable.|  
|Allowed|true|OleTransactions|La transaction peut être transmise dans le format OleTransactions [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].|  
|Allowed|false|Valeur quelconque|Une transaction n'est pas transmise.|  
|NotAllowed|Valeur quelconque|Valeur quelconque|Une transaction n'est pas transmise.|  
  
 Le tableau ci\-dessous résume les résultats de traitement de message.  
  
|Message entrant|Paramètre TransactionFlow|En\-tête de transaction|Résultat de traitement du message|  
|---------------------|-------------------------------|-----------------------------|---------------------------------------|  
|La transaction correspond au format de protocole prévu|Allowed ou Mandatory|`MustUnderstand`  est égal à `true`.|Processus|  
|La transaction ne correspond pas au format de protocole prévu|Mandatory|`MustUnderstand`  est égal à `false`.|Rejet car une transaction est requise|  
|La transaction ne correspond pas au format de protocole prévu|Allowed|`MustUnderstand`  est égal à `false`.|Rejet car l'en\-tête n'est pas compris|  
|Transaction utilisant un format de protocole quelconque|NotAllowed|`MustUnderstand`  est égal à `false`.|Rejet car l'en\-tête n'est pas compris|  
|Aucune transaction|Mandatory|N\/A|Rejet car une transaction est requise|  
|Aucune transaction|Allowed|N\/A|Processus|  
|Aucune transaction|NotAllowed|N\/A|Processus|  
  
 Alors que chaque méthode sur un contrat peut avoir des spécifications de flux de transaction différentes, le paramètre de protocole de flux de transaction est délimité au niveau de la liaison.Cela signifie que toutes les méthodes qui partagent le même point de terminaison \(et par conséquent la même liaison\) partagent également la même stratégie qui autorise ou requiert un flux de transaction, ainsi que le même protocole de transaction, le cas échéant.  
  
## Activation du flux de transaction au niveau de la méthode  
 Les spécifications de flux de transaction ne sont pas toujours les mêmes pour toutes les méthodes dans un contrat de service.Par conséquent, [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] fournit également un mécanisme basé sur les attributs qui permet d'exprimer les préférences de flux de transaction de chaque méthode.Cela est effectué à l'aide de l'attribut <xref:System.ServiceModel.TransactionFlowAttribute> qui spécifie le niveau dans lequel une opération de service accepte un en\-tête de transaction.Vous devez marquer vos méthodes de contrat de service avec cet attribut si vous souhaitez activer le flux de transaction.Cet attribut accepte l'une des valeurs de l'énumération <xref:System.ServiceModel.TransactionFlowOption>, dans laquelle la valeur par défaut est <xref:System.ServiceModel.TransactionFlowOption>.Si une valeur quelconque à l'exception de <xref:System.ServiceModel.TransactionFlowOption> est spécifiée, la méthode ne doit pas être unidirectionnelle.Un développeur peut utiliser cet attribut pour spécifier des spécifications de flux de transaction ou des contraintes au niveau de la méthode au moment du design.  
  
## Activant du flux de transaction au niveau du point de terminaison  
 Outre le paramètre de flux de transaction au niveau de la méthode que l'attribut <xref:System.ServiceModel.TransactionFlowAttribute> fournit, [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] fournit un paramètre au niveau du point de terminaison pour le flux de transaction qui permet aux administrateurs de contrôler le flux de transaction à un niveau supérieur.  
  
 Cela est effectué à l'aide de <xref:System.ServiceModel.Channels.TransactionFlowBindingElement>, qui vous permet d'activer ou de désactiver le flux de transaction entrant dans les paramètres de liaison d'un point de terminaison, ainsi que de spécifier le format de protocole de transaction souhaité pour les transactions entrantes.  
  
 Si le flux de transaction est désactivé pour la liaison, alors qu'une des opérations sur un contrat de service requiert une transaction entrante, une exception de validation est levée au démarrage du service.  
  
 La plupart des liaisons standard que [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] fournit contiennent les attributs `transactionFlow` et `transactionProtocol` qui vous permettent de configurer la liaison spécifique pour accepter les transactions entrantes.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] la définition des éléments de configuration, consultez [\<liaison\>](../../../../docs/framework/misc/binding.md).  
  
 Un administrateur ou un responsable du déploiement peuvent utiliser le flux de transaction au niveau du point de terminaison pour configurer des spécifications ou des contraintes de flux de transaction au moment du déploiement, à l'aide du fichier de configuration.  
  
## Sécurité  
 Pour garantir la sécurité et l'intégrité du système, vous devez sécuriser les échanges de messages lorsque vous transmettez des transactions entre des applications.Vous ne devez pas transmettre ni divulguer les détails d'une transaction à toute application qui n'est pas habilitée à participer à cette même transaction.  
  
 Lorsque vous générez des clients [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] pour des services Web inconnus ou non approuvés par le biais de l'échange de métadonnées, les appels aux opérations sur ces services Web doivent supprimer la transaction actuelle, si possible.L'exemple ci\-dessous montre comment procéder.  
  
```  
//client code which has an ambient transaction  
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Suppress))  
{  
    // No transaction will flow to this operation  
    untrustedProxy.Operation1(...);  
    scope.Complete();  
}  
//remainder of client code  
```  
  
 De plus, les services doivent être configurés pour accepter les transactions entrantes uniquement à partir de clients qu'ils ont authentifiés et autorisés.Les transactions entrantes doivent être acceptées uniquement si elles proviennent de clients hautement approuvés.  
  
## Assertions de stratégie  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] utilise les assertions de stratégie pour contrôler le flux de transaction.Les assertions de stratégie se trouvent dans le document de stratégie d'un service, lequel est généré en regroupant les contrats, la configuration et les attributs.Le client peut obtenir le document de stratégie du service à l'aide d'une commande HTTP GET ou d'une demande\-réponse WS\-MetadataExchange.Les clients peuvent alors traiter le document de stratégie pour déterminer quelles opérations sur un contrat de service peuvent prendre en charge ou requérir le flux de transaction.  
  
 Les assertions de stratégie de flux de transaction affectent le flux de transaction en spécifiant les en\-têtes SOAP qu'un client doit envoyer à un service pour représenter une transaction.Tous les en\-têtes de transaction doivent être marqués avec `MustUnderstand` égal à `true`.Tout message avec un en\-tête marqué autrement est rejeté avec une erreur SOAP.  
  
 Une seule assertion de stratégie liée aux transactions peut être présente sur une opération unique.Les documents de stratégie avec plusieurs assertions de transaction sur une opération sont considérés non valides et sont rejetés par [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].De plus, seul un protocole de transaction unique peut être présent au sein de chaque type de port.Les documents de stratégie avec des opérations faisant référence à plusieurs protocoles de transaction au sein d'un type de port unique sont considérés non valides et sont rejetés par l'[Outil Service Model Metadata Tool \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md).Les documents de stratégie avec des assertions de transaction présentes sur les messages de sortie ou les messages d'entrée unidirectionnels sont également considérés non valides.