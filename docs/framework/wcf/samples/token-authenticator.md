---
title: "Token Authenticator | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 84382f2c-f6b1-4c32-82fa-aebc8f6064db
caps.latest.revision: 22
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 22
---
# Token Authenticator
Cet exemple montre comment implémenter un authentificateur de jetons personnalisé.Dans [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)], un authentificateur de jetons permet de valider le jeton utilisé avec le message, en vérifiant qu'il est cohérent, et en authentifiant l'identité associée au jeton.  
  
 Les authentificateurs de jetons personnalisés sont utiles dans un grand nombre de cas, dont les suivants :  
  
-   Lorsque vous souhaitez substituer le mécanisme d'authentification par défaut associé à un jeton.  
  
-   Lorsque vous générez un jeton personnalisé.  
  
 Cet exemple indique :  
  
-   Comment un client peut s'authentifier à l'aide d'une paire nom d'utilisateur\/mot de passe.  
  
-   Comment le serveur peut valider les informations d'identification du client à l'aide d'un authentificateur de jetons personnalisé.  
  
-   Comment le code de service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] s'intègre à l'authentificateur de jetons personnalisé.  
  
-   Comment le serveur peut être authentifié à l'aide de son certificat X.509.  
  
 Cet exemple montre également comment l'identité de l'appelant est accessible à partir de [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] après le processus d'authentification du jeton personnalisé.  
  
 Le service expose un point de terminaison unique permettant de communiquer avec le service, défini à l'aide du fichier de configuration App.config.Le point de terminaison se compose d'une adresse, d'une liaison et d'un contrat.La liaison est configurée avec un `wsHttpBinding` standard, avec le mode de sécurité du message \(mode par défaut de `wsHttpBinding`\).Cet exemple définit le `wsHttpBinding` standard pour qu'il utilise l'authentification par nom d'utilisateur du client.Le service configure également le certificat de service à l'aide du comportement `serviceCredentials`.Le comportement `securityCredentials` vous permet de spécifier un certificat de service.Un certificat de service permet à un client d'authentifier le service et d'assurer la protection des messages.La configuration suivante référence le certificat localhost installé pendant l'installation de l'exemple, tel que décrit dans les instructions d'installation suivantes.  
  
```  
<system.serviceModel>  
    <services>  
      <service   
          name="Microsoft.ServiceModel.Samples.CalculatorService"  
          behaviorConfiguration="CalculatorServiceBehavior">  
        <host>  
          <baseAddresses>  
            <!-- configure base address provided by host -->  
            <add baseAddress ="http://localhost:8000/servicemodelsamples/service" />  
          </baseAddresses>  
        </host>  
        <!-- use base address provided by host -->  
        <endpoint address=""  
                  binding="wsHttpBinding"  
                  bindingConfiguration="Binding1"   
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
      </service>  
    </services>  
  
    <bindings>  
      <wsHttpBinding>  
        <binding name="Binding1">  
          <security mode="Message">  
            <message clientCredentialType="UserName" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="CalculatorServiceBehavior">  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <!--   
          The serviceCredentials behavior allows one to define a service certificate.  
          A service certificate is used by a client to authenticate the service and provide message protection.  
          This configuration references the "localhost" certificate installed during the setup instructions.  
....        -->  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
  </system.serviceModel>  
  
```  
  
 La configuration de point de terminaison de client se compose d'un nom de configuration, d'une adresse absolue pour le point de terminaison de service, de la liaison et du contrat.La liaison de client est configurée avec le `Mode` et le `clientCredentialType` appropriés.  
  
```  
<system.serviceModel>  
    <client>  
      <endpoint name=""  
                address="http://localhost:8000/servicemodelsamples/service"   
                binding="wsHttpBinding"   
                bindingConfiguration="Binding1"   
                contract="Microsoft.ServiceModel.Samples.ICalculator">  
      </endpoint>  
    </client>  
  
    <bindings>  
      <wsHttpBinding>  
        <binding name="Binding1">  
          <security mode="Message">  
            <message clientCredentialType="UserName" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
  </system.serviceModel>  
```  
  
 L'implémentation cliente définit le nom d'utilisateur et le mot de passe à utiliser.  
  
```  
static void Main()  
{  
     ...  
     client.ClientCredentials.UserNamePassword.UserName = username;  
     client.ClientCredentials.UserNamePassword.Password = password;  
     ...  
}  
```  
  
## Authentificateur de jetons personnalisé  
 Suivez les étapes suivantes pour créer un authentificateur de jetons personnalisé :  
  
1.  Écrivez un authentificateur de jetons personnalisé.  
  
     L'exemple implémente un authentificateur de jetons personnalisé qui valide que le nom d'utilisateur a un format d'adresse de messagerie valide.Il dérive <xref:System.IdentityModel.Selectors.UserNameSecurityTokenAuthenticator>.La méthode la plus importante de cette classe est <xref:System.IdentityModel.Selectors.UserNameSecurityTokenAuthenticator.ValidateUserNamePasswordCore%28System.String%2CSystem.String%29>.Dans cette méthode, l'authentificateur valide le format du nom d'utilisateur et également que le nom d'hôte ne provient pas d'un domaine non autorisé.Si ces deux conditions sont vérifiées, il retourne une collection d'instances <xref:System.IdentityModel.Policy.IAuthorizationPolicy> en lecture seule qui est par la suite utilisée pour fournir des revendications représentant les informations stockée à l'intérieur du jeton de nom d'utilisateur.  
  
    ```  
    protected override ReadOnlyCollection<IAuthorizationPolicy> ValidateUserNamePasswordCore(string userName, string password)  
    {  
        if (!ValidateUserNameFormat(userName))  
            throw new SecurityTokenValidationException("Incorrect UserName format");  
  
        ClaimSet claimSet = new DefaultClaimSet(ClaimSet.System, new Claim(ClaimTypes.Name, userName, Rights.PossessProperty));  
        List<IIdentity> identities = new List<IIdentity>(1);  
        identities.Add(new GenericIdentity(userName));  
        List<IAuthorizationPolicy> policies = new List<IAuthorizationPolicy>(1);  
        policies.Add(new UnconditionalPolicy(ClaimSet.System, claimSet, DateTime.MaxValue.ToUniversalTime(), identities));  
        return policies.AsReadOnly();  
    }  
    ```  
  
2.  Fournissez une stratégie d'autorisation retournée par l'authentificateur de jetons personnalisé.  
  
     Cet exemple fournit sa propre implémentation de <xref:System.IdentityModel.Policy.IAuthorizationPolicy> appelé `UnconditionalPolicy` qui retourne l'ensemble de revendications et d'identités qui lui ont été passées dans son constructeur.  
  
    ```  
    class UnconditionalPolicy : IAuthorizationPolicy  
    {  
        String id = Guid.NewGuid().ToString();  
        ClaimSet issuer;  
        ClaimSet issuance;  
        DateTime expirationTime;  
        IList<IIdentity> identities;  
  
        public UnconditionalPolicy(ClaimSet issuer, ClaimSet issuance, DateTime expirationTime, IList<IIdentity> identities)  
        {  
            if (issuer == null)  
                throw new ArgumentNullException("issuer");  
            if (issuance == null)  
                throw new ArgumentNullException("issuance");  
  
            this.issuer = issuer;  
            this.issuance = issuance;  
            this.identities = identities;  
            this.expirationTime = expirationTime;  
        }  
  
        public string Id  
        {  
            get { return this.id; }  
        }  
  
        public ClaimSet Issuer  
        {  
            get { return this.issuer; }  
        }  
  
        public DateTime ExpirationTime  
        {  
            get { return this.expirationTime; }  
        }  
  
        public bool Evaluate(EvaluationContext evaluationContext, ref object state)  
        {  
            evaluationContext.AddToTarget(this, this.issuance);  
  
            if (this.identities != null)  
            {  
                object value;  
                IList<IIdentity> contextIdentities;  
                if (!evaluationContext.Properties.TryGetValue("Identities", out value))  
                {  
                    contextIdentities = new List<IIdentity>(this.identities.Count);  
                    evaluationContext.Properties.Add("Identities", contextIdentities);  
                }  
                else  
                {  
                    contextIdentities = value as IList<IIdentity>;  
                }  
                foreach (IIdentity identity in this.identities)  
                {  
                    contextIdentities.Add(identity);  
                }  
            }  
  
            evaluationContext.RecordExpirationTime(this.expirationTime);  
            return true;  
        }  
    }  
    ```  
  
3.  Écrivez un gestionnaire de jetons de sécurité personnalisé.  
  
     <xref:System.IdentityModel.Selectors.SecurityTokenManager> permet de créer <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator> pour des objets <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> spécifiques qui lui sont passés dans la méthode `CreateSecurityTokenAuthenticator`.Le gestionnaire de jetons de sécurité permet également de créer des fournisseurs et des sérialiseurs de jeton, mais ceux\-ci ne sont pas traités dans cet exemple.Dans cet exemple, le gestionnaire de jetons de sécurité personnalisé hérite de classe <xref:System.ServiceModel.Security.ServiceCredentialsSecurityTokenManager> et substitue la méthode `CreateSecurityTokenAuthenticator` pour retourner l'authentificateur de jeton de nom d'utilisateur personnalisé lorsque les spécifications de jeton passées indiquent que l'authentificateur de nom d'utilisateur est demandé.  
  
    ```  
  
    public class MySecurityTokenManager : ServiceCredentialsSecurityTokenManager  
    {  
        MyUserNameCredential myUserNameCredential;  
  
        public MySecurityTokenManager(MyUserNameCredential myUserNameCredential)  
            : base(myUserNameCredential)  
        {  
            this.myUserNameCredential = myUserNameCredential;  
        }  
  
        public override SecurityTokenAuthenticator CreateSecurityTokenAuthenticator(SecurityTokenRequirement tokenRequirement, out SecurityTokenResolver outOfBandTokenResolver)  
        {  
            if (tokenRequirement.TokenType ==  SecurityTokenTypes.UserName)  
            {  
                outOfBandTokenResolver = null;  
                return new MyTokenAuthenticator();  
            }  
            else  
            {  
                return base.CreateSecurityTokenAuthenticator(tokenRequirement, out outOfBandTokenResolver);  
            }  
        }  
    }  
    ```  
  
4.  Écrivez une information d'identification de service personnalisée.  
  
     La classe d'informations d'identification de service permet de représenter les informations d'identification qui sont configurées pour le service et crée un gestionnaire de jetons de sécurité utilisé pour obtenir des authentificateurs, des fournisseurs et des sérialiseurs de jeton.  
  
    ```  
    public class MyUserNameCredential : ServiceCredentials  
    {  
  
        public MyUserNameCredential()  
            : base()  
        {  
        }  
  
        protected override ServiceCredentials CloneCore()  
        {  
            return new MyUserNameCredential();  
        }  
  
        public override SecurityTokenManager CreateSecurityTokenManager()  
        {  
            return new MySecurityTokenManager(this);  
        }  
  
    }  
    ```  
  
5.  Configurez le service pour qu'il utilise l'information d'identification de service personnalisée.  
  
     Pour que le service utilise l'information d'identification de service personnalisée, nous supprimons la classe d'informations d'identification de service par défaut après avoir capturé le certificat de service qui est déjà préconfiguré dans les informations d'identification de service par défaut, puis nous configurons la nouvelle instance d'informations d'identification de service afin qu'elle utilise les certificats de service préconfigurés et ajoutons cette nouvelle instance aux comportements de service.  
  
    ```  
    ServiceCredentials sc = serviceHost.Credentials;  
    X509Certificate2 cert = sc.ServiceCertificate.Certificate;  
    MyUserNameCredential serviceCredential = new MyUserNameCredential();  
    serviceCredential.ServiceCertificate.Certificate = cert;  
    serviceHost.Description.Behaviors.Remove((typeof(ServiceCredentials)));  
    serviceHost.Description.Behaviors.Add(serviceCredential);  
    ```  
  
 Pour afficher les informations sur l'appelant, vous pouvez utiliser <xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A>, tel qu'indiqué dans le code suivant.<xref:System.ServiceModel.ServiceSecurityContext.Current%2A> contient des informations sur les revendications relatives à l'appelant actuel.  
  
```  
static void DisplayIdentityInformation()  
{  
    Console.WriteLine("\t\tSecurity context identity  :  {0}",   
            ServiceSecurityContext.Current.PrimaryIdentity.Name);  
     return;  
}  
  
```  
  
 Lorsque vous exécutez l'exemple, les demandes et réponses d'opération s'affichent dans la fenêtre de console cliente.Appuyez sur ENTER dans la fenêtre du client pour arrêter le client.  
  
## Fichier de commandes d'installation  
 Le fichier de commandes Setup.bat inclus avec cet exemple permet de configurer le serveur avec les certificats appropriés pour exécuter une application auto\-hébergée qui requiert une sécurité basée sur le certificat du serveur.Ce fichier de commandes doit être modifié pour fonctionner sur plusieurs ordinateurs ou sans hébergement.  
  
 Les éléments suivants fournissent une vue d'ensemble des différentes sections des fichiers de commandes afin qu'ils puissent être modifiés pour s'exécuter dans la configuration appropriée.  
  
-   Création du certificat de serveur  
  
     Les lignes suivantes du fichier de commandes Setup.bat créent le certificat de serveur à utiliser.La variable `%SERVER_NAME%` représente le nom du serveur.Modifiez\-la pour spécifier votre propre nom de serveur.La valeur par défaut dans ce fichier de commandes est localhost.  
  
    ```  
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
  
    ```  
  
-   Installation du certificat de serveur dans le magasin de certificats approuvé du client.  
  
     Les lignes suivantes du fichier de commandes Setup.bat copient le certificat de serveur dans le magasin de personnes de confiance du client.Cette étape est obligatoire parce que les certificats générés par Makecert.exe ne sont pas implicitement approuvés par le système client.Si vous disposez déjà d'un certificat associé à un certificat racine approuvé du client, par exemple un certificat émis par Microsoft, cette étape de remplissage du magasin de certificats client avec le certificat de serveur n'est pas requise.  
  
    ```  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
    > [!NOTE]
    >  Le fichier de commandes d'installation est conçu pour être exécuté à partir d'une invite de commandes du Kit de développement Windows SDK.La variable d'environnement du Kit de développement MS SDK doit pointer vers le répertoire d'installation du Kit de développement SDK.Cette variable est définie automatiquement dans une invite de commandes du Kit de développement Windows SDK.  
  
#### Pour configurer et générer l'exemple  
  
1.  Assurez\-vous d'avoir effectué la procédure figurant à la section [Procédure d'installation unique pour les exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2.  Pour générer la solution, suivez les instructions indiquées dans [Génération des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
#### Pour exécuter l'exemple sur le même ordinateur  
  
1.  Ouvrez une fenêtre d'invite de commandes de [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] avec des privilèges d'administrateur et exécutez Setup.bat à partir du dossier d'installation de l'exemple.Tous les certificats requis pour l'exécution de l'exemple sont ainsi installés.  
  
    > [!NOTE]
    >  Le fichier de commandes Setup.bat est conçu pour être exécuté à partir d'une invite de commandes de [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)].La variable d'environnement PATH définie dans l'invite de commandes [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] pointe vers le répertoire qui contient les exécutables requis par le script Setup.bat.  
  
2.  Lancez service.exe à partir de service\\bin.  
  
3.  Lancez client.exe à partir de \\client\\bin.L'activité du client s'affiche sur son application de console.  
  
4.  Si le client et le service ne parviennent pas à communiquer, consultez la rubrique [Troubleshooting Tips](http://msdn.microsoft.com/fr-fr/8787c877-5e96-42da-8214-fa737a38f10b).  
  
#### Pour exécuter l'exemple sur plusieurs ordinateurs  
  
1.  Créez un répertoire sur l'ordinateur de service pour les fichiers binaires du service.  
  
2.  Copiez les fichiers programme du service dans le répertoire de service sur l'ordinateur de service.Copiez également les fichiers Setup.bat et Cleanup.bat sur l'ordinateur de service.  
  
3.  Le nom de sujet de votre certificat de serveur doit contenir le nom de domaine complet de l'ordinateur.Le fichier App.config du service doit être mis à jour afin de prendre en compte ce nouveau nom de certificat.Vous pouvez en créer un en utilisant Setup.bat si vous affectez à la variable `%SERVER_NAME%` le nom d'hôte complet de l'ordinateur sur lequel le service s'exécutera.Notez que vous devez exécuter le fichier Setup.bat à partir d'une fenêtre d'invite de commandes de Visual Studio ouverte avec des privilèges d'administrateur.  
  
4.  Copiez le certificat de serveur dans le magasin CurrentUser\-TrustedPeople du client.Cette opération n'est pas nécessaire sauf lorsque le certificat de serveur est émis par un émetteur approuvé du client.  
  
5.  Dans le fichier App.config sur l'ordinateur de service, modifiez la valeur de l'adresse de base pour spécifier le nom de l'ordinateur complet au lieu de localhost.  
  
6.  Sur l'ordinateur de service, exécutez Service.exe à partir d'une invite de commandes.  
  
7.  Copiez les fichiers programme du client du dossier \\client\\bin\\ \(situé dans le dossier correspondant à votre langue\) sur l'ordinateur client.  
  
8.  Dans le fichier Client.exe.config de l'ordinateur client, modifiez la valeur d'adresse du point de terminaison afin qu'elle corresponde à la nouvelle adresse de votre service.  
  
9. Sur l'ordinateur client, lancez Client.exe à partir d'une invite de commandes.  
  
10. Si le client et le service ne parviennent pas à communiquer, consultez la rubrique [Troubleshooting Tips](http://msdn.microsoft.com/fr-fr/8787c877-5e96-42da-8214-fa737a38f10b).  
  
#### Pour procéder au nettoyage après exécution de l'exemple  
  
1.  Exécutez Cleanup.bat dans le dossier exemples une fois que vous avez terminé d'exécuter l'exemple.  
  
## Voir aussi