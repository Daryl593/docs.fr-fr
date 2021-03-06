---
title: "D&#233;bogage d&#39;erreurs d&#39;authentification Windows | Microsoft Docs"
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
  - "WCF, authentification"
  - "WCF, authentification Windows"
ms.assetid: 181be4bd-79b1-4a66-aee2-931887a6d7cc
caps.latest.revision: 21
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 21
---
# D&#233;bogage d&#39;erreurs d&#39;authentification Windows
Lorsque vous utilisez l'authentification Windows comme un mécanisme de sécurité, l'interface SSPI \(Security Support Provider Interface\) gère les processus de sécurité.Lorsque des erreurs de sécurité se produisent au niveau de la couche SSPI, elles sont signalées par [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)].Cette rubrique fournit une infrastructure et un ensemble de questions permettant de diagnostiquer les erreurs.  
  
 Pour obtenir une vue d'ensemble du protocole Kerberos, consultez [Explication du protocole Kerberos](http://go.microsoft.com/fwlink/?LinkID=86946)\(page éventuellement en anglais\) ; pour une vue d'ensemble de SSPI, consultez [SSPI](http://go.microsoft.com/fwlink/?LinkId=88941) \(page éventuellement en anglais\).  
  
 Pour l'authentification Windows, [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] utilise généralement le fournisseur SSP \(Security Support Provider\) *Negotiate*, qui exécute l'authentification mutuelle Kerberos entre le client et le service.Si le protocole Kerberos est non disponible, [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] revient par défaut à NTLM \(NT LAN Manager\).Toutefois, vous pouvez configurer [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] pour utiliser uniquement le protocole Kerberos \(et lever une exception si Kerberos n'est pas disponible\).Vous pouvez également configurer [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] pour utiliser des formulaires restreints du protocole Kerberos.  
  
## Méthodologie de débogage  
 La méthode de base est la suivante :  
  
1.  Déterminez si vous utilisez l'authentification Windows.Si vous utilisez un autre schéma, cette rubrique ne s'applique pas.  
  
2.  Si vous êtes certain d'utiliser l'authentification Windows, déterminez si votre configuration [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] utilise Kerberos direct ou Negotiate.  
  
3.  Une fois que vous avez déterminé si votre configuration utilise le protocole Kerberos ou NTLM, vous pouvez comprendre les messages d'erreur dans le contexte correct.  
  
### Disponibilité du protocole Kerberos et de NTLM  
 Le fournisseur SSP Kerberos requiert qu'un contrôleur de domaine se comporte comme un centre de distribution de clés \(KDC, Key Distribution Center\).Le protocole Kerberos est disponible uniquement lorsque le client et le service utilisent des identités de domaine.NTLM est utilisé dans d'autres combinaisons de comptes, tel qu'indiqué dans le tableau suivant.  
  
 Les en\-têtes du tableau présentent les types de compte possibles utilisés par le serveur.La colonne de gauche présente les types de compte possibles utilisés par le client.  
  
||Utilisateur local|Système local|Utilisateur de domaine|Ordinateur de domaine|  
|-|-----------------------|-------------------|----------------------------|---------------------------|  
|Utilisateur local|NTLM|NTLM|NTLM|NTLM|  
|Système local|NTLM anonyme|NTLM anonyme|NTLM anonyme|NTLM anonyme|  
|Utilisateur de domaine|NTLM|NTLM|Kerberos|Kerberos|  
|Ordinateur de domaine|NTLM|NTLM|Kerberos|Kerberos|  
  
 Plus précisément, les quatre types de comptes incluent :  
  
-   Utilisateur local : profil utilisateur, ordinateur uniquement.Par exemple : `MachineName\Administrator` ou `MachineName\ProfileName`.  
  
-   Système local : compte intégré SYSTEM sur un ordinateur qui n'est pas joint à un domaine.  
  
-   Utilisateur de domaine : compte d'utilisateur sur un domaine Windows.Par exemple : `DomainName\ProfileName`.  
  
-   Ordinateur de domaine : processus avec identité d'ordinateur s'exécutant sur un ordinateur joint à un domaine Windows.Par exemple : `MachineName\Network Service`.  
  
> [!NOTE]
>  Les informations d'identification du service sont capturées lorsque la méthode <xref:System.ServiceModel.ICommunicationObject.Open%2A> de la classe <xref:System.ServiceModel.ServiceHost> est appelée.Les informations d'identification du client sont lues chaque fois que le client envoie un message.  
  
## Problèmes courants d'authentification Windows  
 Cette section présente certains problèmes d'authentification Windows courants et les solutions possibles.  
  
### Protocole Kerberos  
  
#### Problèmes de SPN\/UPN rencontrés avec le protocole Kerberos  
 Si vous utilisez l'authentification Windows, et que le protocole Kerberos est utilisé ou négocié par SSPI, l'URL utilisée par le point de terminaison client doit inclure le nom de domaine complet de l'hôte du service dans l'URL de service.Cela suppose que le compte sous lequel le service s'exécute a accès à la clé SPN \(Service Principal Name\) par défaut de l'ordinateur créée lorsque celui\-ci est ajouté au domaine Active Directory, ce qui s'effectue le plus souvent en exécutant le service sous le compte Service Réseau.Si le service n'a pas accès à la clé SPN de l'ordinateur, vous devez fournir le SPN correct ou un nom d'utilisateur principal \(UPN, User Principal Name\) du compte sous lequel le service s'exécute dans l'identité de point de terminaison du client.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] le fonctionnement de [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] avec SPN et UPN, consultez [Identité du service et authentification](../../../../docs/framework/wcf/feature-details/service-identity-and-authentication.md).  
  
 Dans les scénarios d'équilibrage de charge, tels que les batteries de serveurs Web ou les jardins Web, une pratique courante consiste à définir un compte unique pour chaque application, assigner un SPN à ce compte et veiller à ce que tous les services de l'application s'exécutent sous ce compte.  
  
 Pour obtenir un SPN pour le compte de votre service, vous devez être administrateur de domaine Active Directory.[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][Complément technique pour Windows](http://go.microsoft.com/fwlink/?LinkID=88330).  
  
#### Le protocole Kerberos direct requiert l'exécution du service sous un compte d'ordinateur de domaine  
 Cela se produit lorsque la propriété `ClientCredentialType` a la valeur `Windows` et que la propriété <xref:System.ServiceModel.MessageSecurityOverHttp.NegotiateServiceCredential%2A> a la valeur `false`, tel qu'indiqué dans le code suivant.  
  
 [!code-csharp[C_DebuggingWindowsAuth#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#1)]
 [!code-vb[C_DebuggingWindowsAuth#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#1)]  
  
 Pour y remédier, exécutez le service à l'aide d'un compte d'ordinateur de domaine, tel que Service réseau, sur un ordinateur joint au domaine.  
  
### La délégation requiert la négociation d'informations d'identification  
 Pour utiliser le protocole d'authentification Kerberos avec délégation, vous devez implémenter le protocole Kerberos avec la négociation d'informations d'identification \(parfois appelé Kerberos « multi\-leg » ou Kerberos « multi\-step »\).Si vous implémentez l'authentification Kerberos sans négociation d'informations d'identification \(parfois appelée Kerberos « one\-shot » ou Kerberos « single\-leg »\), une exception sera levée.  
  
 Pour implémenter Kerberos avec la négociation d'information d'identification, procédez comme suit :  
  
1.  Implémentez la délégation en affectant <xref:System.Security.Principal.TokenImpersonationLevel> à <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A>.  
  
2.  Requérez la négociation SSPI :  
  
    1.  Si vous utilisez des liaisons standard, affectez `true` à la propriété `NegotiateServiceCredential`.  
  
    2.  Si vous utilisez des liaisons personnalisées, affectez `SspiNegotiated` à l'attribut `AuthenticationMode` de l'élément `Security`.  
  
3.  Requérez que la négociation SSPI utilise Kerberos en interdisant l'utilisation de NTLM :  
  
    1.  Effectuez cette opération dans le code, à l'aide de l'instruction suivante : `ChannelFactory.Credentials.Windows.AllowNtlm = false`  
  
    2.  Vous pouvez également effectuer cette opération dans le fichier de configuration en affectant `false` à l'attribut `allowNtlm`.Cet attribut est contenu dans [\<fenêtres\>](../../../../docs/framework/configure-apps/file-schema/wcf/windows-of-clientcredentials-element.md).  
  
### Protocole NTLM  
  
#### Négociation du retour de SSP à NTLM, mais NTLM est désactivé  
 La propriété <xref:System.ServiceModel.Security.WindowsClientCredential.AllowNtlm%2A> a la valeur `false`, ce qui conduit [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] à déployer le meilleur effort pour lever une exception si NTLM est utilisé.Notez que l'affectation de la valeur `false` à cette propriété peut ne pas empêcher la transmission des informations d'identification NTLM.  
  
 La section suivante indique comment désactiver le retour à NTLM.  
  
 [!code-csharp[C_DebuggingWindowsAuth#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#4)]
 [!code-vb[C_DebuggingWindowsAuth#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#4)]  
  
#### Échec d'ouverture de session NTLM  
 Les informations d'identification du client ne sont pas valides sur le service.Vérifiez que le nom d'utilisateur et le mot de passe sont correctement définis et correspondent à un compte connu sur l'ordinateur sur lequel le service s'exécute.NTLM utilise les informations d'identification spécifiées pour se connecter à l'ordinateur du service.Même si les informations d'identification sont valides sur l'ordinateur sur lequel le client s'exécute, cette ouverture de session échouera si elles ne sont pas valides sur l'ordinateur du service.  
  
#### Une ouverture de session NTLM anonyme se produit, mais les ouvertures de session anonymes sont désactivées par défaut  
 Lors de la création d'un client, la propriété <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> a la valeur <xref:System.Security.Principal.TokenImpersonationLevel>, tel qu'indiqué dans l'exemple suivant, mais par défaut, le serveur interdit les ouvertures de session anonymes.En effet, la valeur par défaut de la propriété <xref:System.ServiceModel.Security.WindowsServiceCredential.AllowAnonymousLogons%2A> de la classe <xref:System.ServiceModel.Security.WindowsServiceCredential> est `false`.  
  
 Le code client suivant tente d'activer des ouvertures de session anonymes \(notez que la propriété par défaut est `Identification`\).  
  
 [!code-csharp[C_DebuggingWindowsAuth#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#5)]
 [!code-vb[C_DebuggingWindowsAuth#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#5)]  
  
 Le code de service suivant modifie la valeur par défaut afin d'activer des ouvertures de session anonymes par le serveur.  
  
 [!code-csharp[C_DebuggingWindowsAuth#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#6)]
 [!code-vb[C_DebuggingWindowsAuth#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#6)]  
  
 [!INCLUDE[crabout](../../../../includes/crabout-md.md)] l'emprunt d'identité, consultez [Délégation et emprunt d'identité](../../../../docs/framework/wcf/feature-details/delegation-and-impersonation-with-wcf.md).  
  
 Le client s'exécute également en tant que service Windows, à l'aide du compte intégré SYSTEM.  
  
### Autres problèmes  
  
#### Les informations d'identification du client ne sont pas correctement définies  
 L'authentification Windows utilise l'instance <xref:System.ServiceModel.Security.WindowsClientCredential> retournée par la propriété <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> de la classe <xref:System.ServiceModel.ClientBase%601>, et non pas <xref:System.ServiceModel.Security.UserNamePasswordClientCredential>.Le code suivant présente un exemple incorrect.  
  
 [!code-csharp[C_DebuggingWindowsAuth#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#2)]
 [!code-vb[C_DebuggingWindowsAuth#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#2)]  
  
 Le code suivant présente l'exemple correct.  
  
 [!code-csharp[C_DebuggingWindowsAuth#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#3)]
 [!code-vb[C_DebuggingWindowsAuth#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#3)]  
  
#### SSPI n'est pas disponible  
 Les systèmes d'exploitation suivants ne prennent pas en charge l'authentification Windows lorsqu'elle est utilisée en tant que serveur : [!INCLUDE[wxp](../../../../includes/wxp-md.md)] Édition familiale, [!INCLUDE[wxp](../../../../includes/wxp-md.md)] Media Center Edition et [!INCLUDE[wv](../../../../includes/wv-md.md)] Édition familiale.  
  
#### Développement et déploiement avec des identités différentes  
 Si vous développez votre application sur un ordinateur, la déployez sur un autre et utilisez différents types de compte pour vous authentifier sur chaque ordinateur, vous pouvez observer un comportement différent.Par exemple, supposons que vous développiez votre application sur un ordinateur Windows XP Professionnel à l'aide du mode d'authentification `SSPI Negotiated`.Si vous utilisez un compte d'utilisateur local pour vous authentifier, le protocole NTLM sera alors utilisé.Une fois que l'application est développée, vous déployez le service sur un ordinateur Windows Server 2003 où il s'exécute sous un compte de domaine.À ce stade, le client ne sera pas en mesure d'authentifier le service car il utilisera Kerberos et un contrôleur de domaine.  
  
## Voir aussi  
 <xref:System.ServiceModel.Security.WindowsClientCredential>   
 <xref:System.ServiceModel.Security.WindowsServiceCredential>   
 <xref:System.ServiceModel.Security.WindowsClientCredential>   
 <xref:System.ServiceModel.ClientBase%601>   
 [Délégation et emprunt d'identité](../../../../docs/framework/wcf/feature-details/delegation-and-impersonation-with-wcf.md)   
 [Scénarios non pris en charge](../../../../docs/framework/wcf/feature-details/unsupported-scenarios.md)