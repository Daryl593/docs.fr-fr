---
title: "Activit&#233;s d&#39;acc&#232;s aux bases de donn&#233;es | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 174a381e-1343-46a8-a62c-7c2ae2c4f0b2
caps.latest.revision: 8
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 8
---
# Activit&#233;s d&#39;acc&#232;s aux bases de donn&#233;es
Les activités d'accès aux bases de données vous permettent d'accéder à une base de données dans un workflow.Ces activités permettent d'utiliser [ADO.NET](http://go.microsoft.com/fwlink/?LinkId=166081) pour accéder à des bases de données afin de récupérer ou de modifier des informations.  
  
> [!IMPORTANT]
>  Les exemples peuvent déjà être installés sur votre ordinateur.Recherchez le répertoire \(par défaut\) suivant avant de continuer.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page de téléchargement pour télécharger tous les exemples [!INCLUDE[wf1](../../../../includes/wf1-md.md)] et [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)].Cet exemple se trouve dans le répertoire suivant.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WF\Scenario\ActivityLibrary\DbActivities`  
  
## Activités de base de données  
 Les sections suivantes détaillent la liste des activités incluses dans cet exemple.  
  
## DbUpdate  
 Exécute une requête SQL qui produit une modification dans la base de données \(insertion, mise à jour, suppression et autres modifications\).  
  
 Cette classe effectue son travail de façon asynchrone \(elle dérive d'<xref:System.Activities.AsyncCodeActivity> et utilise ses fonctionnalités asynchrones\).  
  
 Les informations de connexion peuvent être configurées en définissant un nom invariant de fournisseur \(`ProviderName`\) et la chaîne de connexion \(`ConnectionString`\), ou simplement à l'aide d'un nom de configuration de chaîne de connexion \(`ConfigFileSectionName`\) provenant du fichier de configuration de l'application.  
  
 La requête à exécuter est configurée dans sa propriété `Sql` et les paramètres sont passés via la collection `Parameters`.  
  
 Une fois `DbUpdate` exécuté, le nombre des enregistrements affectés est retourné dans la propriété `AffectedRecords`.  
  
```  
Public class DbUpdate: AsyncCodeActivity  
{  
    [RequiredArgument]  
    [OverloadGroup("ConnectionString")]  
    [DefaultValue(null)]  
    public InArgument<string> ProviderName { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("ConnectionString")]  
    [DependsOn("ProviderName")]  
    [DefaultValue(null)]  
    public InArgument<string> ConnectionString { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("ConfigFileSectionName")]  
    [DefaultValue(null)]  
    public InArgument<string> ConfigName { get; set; }  
  
    [DefaultValue(null)]  
    public CommandType CommandType { get; set; }  
  
    [RequiredArgument]  
    public InArgument<string> Sql { get; set; }  
  
    [DependsOn("Sql")]  
    [DefaultValue(null)]  
    public IDictionary<string, Argument> Parameters { get; }  
  
    [DependsOn("Parameters")]  
    public OutArgument<int> AffectedRecords { get; set; }       
}  
```  
  
|||  
|-|-|  
|Argument|Description|  
|ProviderName|Nom invariant du fournisseur ADO.NET.Si cet argument est défini, `ConnectionString` doit également être défini.|  
|ConnectionString|Chaîne de connexion utilisée pour se connecter à la base de données.Si cet argument est défini, `ProviderName` doit également être défini.|  
|ConfigName|Nom de la section du fichier de configuration où les informations de connexion sont stockées.Lorsque cet argument est défini, `ProviderName` et `ConnectionString` ne sont pas requis.|  
|CommandType|Type du <xref:System.Data.Common.DbCommand> à exécuter.|  
|Sql|Commande SQL à exécuter.|  
|Parameters|Collection des paramètres de la requête SQL.|  
|AffectedRecords|Nombre d'enregistrements affectés par la dernière opération.|  
  
## DbQueryScalar  
 Exécute une requête qui récupère une valeur unique à partir de la base de données.  
  
 Cette classe effectue son travail de façon asynchrone \(elle dérive d'<xref:System.Activities.AsyncCodeActivity%601> et utilise ses fonctionnalités asynchrones\).  
  
 Les informations de connexion peuvent être configurées en définissant un nom invariant de fournisseur \(`ProviderName`\) et la chaîne de connexion \(`ConnectionString`\), ou simplement à l'aide d'un nom de configuration de chaîne de connexion \(`ConfigFileSectionName`\) provenant du fichier de configuration de l'application.  
  
 La requête à exécuter est configurée dans sa propriété `Sql` et les paramètres sont passés via la collection `Parameters`.  
  
 Une fois `DbQueryScalar` exécuté, le scalaire est retourné dans l'argument `Result``out` \(de type `TResult`, défini dans la classe de base <xref:System.Activities.AsyncCodeActivity%601>\).  
  
```  
public class DbQueryScalar<TResult> : AsyncCodeActivity<TResult>  
{  
    // public arguments  
    [RequiredArgument]  
    [OverloadGroup("ConnectionString")]  
    [DefaultValue(null)]  
    public InArgument<string> ProviderName { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("ConnectionString")]  
    [DependsOn("ProviderName")]  
    [DefaultValue(null)]  
    public InArgument<string> ConnectionString { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("ConfigFileSectionName")]  
    [DefaultValue(null)]  
    public InArgument<string> ConfigName { get; set; }  
  
    [DefaultValue(null)]  
    public CommandType CommandType { get; set; }  
  
    [RequiredArgument]  
    public InArgument<string> Sql { get; set; }  
  
    [DependsOn("Sql")]  
    [DefaultValue(null)]  
    public IDictionary<string, Argument> Parameters { get; }  
}  
```  
  
|||  
|-|-|  
|Argument|Description|  
|ProviderName|Nom invariant du fournisseur ADO.NET.Si cet argument est défini, `ConnectionString` doit également être défini.|  
|ConnectionString|Chaîne de connexion utilisée pour se connecter à la base de données.Si cet argument est défini, `ProviderName` doit également être défini.|  
|ConfigName|Nom de la section du fichier de configuration où les informations de connexion sont stockées.Lorsque cet argument est défini, `ProviderName` et `ConnectionString` ne sont pas requis.|  
|CommandType|Type du <xref:System.Data.Common.DbCommand> à exécuter.|  
|Sql|Commande SQL à exécuter.|  
|Parameters|Collection des paramètres de la requête SQL.|  
|Result|Scalaire obtenu une fois la requête exécutée.Cet argument est de type `TResult`.|  
  
## DbQuery  
 Exécute une requête qui récupère une liste d'objets.Une fois la requête exécutée, une fonction de mappage est exécutée \(ce peut être <xref:System.Func%601>\<`DbDataReader`, `TResult`\> ou un <xref:System.Activities.ActivityFunc%601>\<`DbDataReader`, `TResult`\>\).Cette fonction de mappage obtient un enregistrement dans un `DbDataReader` et le mappe à l'objet à retourner.  
  
 Les informations de connexion peuvent être configurées en définissant un nom invariant de fournisseur \(`ProviderName`\) et la chaîne de connexion \(`ConnectionString`\), ou simplement à l'aide d'un nom de configuration de chaîne de connexion \(`ConfigFileSectionName`\) provenant du fichier de configuration de l'application.  
  
 La requête à exécuter est configurée dans sa propriété `Sql` et les paramètres sont passés via la collection `Parameters`.  
  
 Les résultats de la requête SQL sont récupérés à l'aide d'un `DbDataReader`.L'activité itère au sein de `DbDataReader` et mappe les lignes de `DbDataReader` à une instance de `TResult`.L'utilisateur de `DbQuery` doit fournir le code de mappage. Cette opération peut être effectuée de deux façons : à l'aide d'un <xref:System.Func%601>\<`DbDataReader`, `TResult`\> ou d'un <xref:System.Activities.ActivityFunc%601>\<`DbDataReader`, `TResult`\>.Dans le premier cas, le mappage est effectué en une seule impulsion d'exécution.Par conséquent, il est plus rapide, mais ne peut pas être sérialisé en XAML.Dans le dernier cas, le mappage est effectué en plusieurs impulsions.Par conséquent, il peut être plus lent mais peut être sérialisé en XAML et créé de façon déclarative \(toute activité existante peut participer au mappage\).  
  
```  
public class DbQuery<TResult> : AsyncCodeActivity<IList<TResult>> where TResult : class  
{  
    // public arguments  
    [RequiredArgument]  
    [OverloadGroup("ConnectionString")]  
    [DefaultValue(null)]  
    public InArgument<string> ProviderName { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("ConnectionString")]  
    [DependsOn("ProviderName")]  
    [DefaultValue(null)]  
    public InArgument<string> ConnectionString { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("ConfigFileSectionName")]  
    [DefaultValue(null)]  
    public InArgument<string> ConfigName { get; set; }  
  
    [DefaultValue(null)]  
    public CommandType CommandType { get; set; }  
  
    [RequiredArgument]  
    public InArgument<string> Sql { get; set; }  
  
    [DependsOn("Sql")]  
    [DefaultValue(null)]  
    public IDictionary<string, Argument> Parameters { get; }  
  
    [OverloadGroup("DirectMapping")]  
    [DefaultValue(null)]  
    public Func<DbDataReader, TResult> Mapper { get; set; }  
  
    [OverloadGroup("MultiplePulseMapping")]  
    [DefaultValue(null)]  
    public ActivityFunc<DbDataReader, TResult> MapperFunc { get; set; }  
}  
```  
  
|||  
|-|-|  
|Argument|Description|  
|ProviderName|Nom invariant du fournisseur ADO.NET.Si cet argument est défini, `ConnectionString` doit également être défini.|  
|ConnectionString|Chaîne de connexion utilisée pour se connecter à la base de données.Si cet argument est défini, `ProviderName` doit également être défini.|  
|ConfigName|Nom de la section du fichier de configuration où les informations de connexion sont stockées.Lorsque cet argument est défini, `ProviderName` et `ConnectionString` ne sont pas requis.|  
|CommandType|Type du <xref:System.Data.Common.DbCommand> à exécuter.|  
|Sql|Commande SQL à exécuter.|  
|Parameters|Collection des paramètres de la requête SQL.|  
|Mapper|La fonction de mappage \(<xref:System.Func%601>\<`DbDataReader`, `TResult`\>\) qui prend un enregistrement dans le `DataReader` obtenu en résultat de l'exécution de la requête et retourne une instance d'un objet de type `TResult` à ajouter à la collection `Result`.<br /><br /> Dans ce cas, le mappage est effectué en seule impulsion d'exécution, mais il ne peut pas être créé de façon déclarative à l'aide du concepteur.|  
|MapperFunc|La fonction de mappage \(<xref:System.Activities.ActivityFunc%601>\<`DbDataReader`, `TResult`\>\) qui prend un enregistrement dans le `DataReader` obtenu en résultat de l'exécution de la requête et retourne une instance d'un objet de type `TResult` à ajouter à la collection `Result`.<br /><br /> Dans ce cas, le mappage est effectué en plusieurs impulsions d'exécution.Cette fonction peut être sérialisée en XAML et créée de façon déclarative \(toute activité existante peut participer au mappage\).|  
|Result|Liste des objets obtenus en résultat de l'exécution de la requête et de l'exécution de la fonction de mappage pour chaque d'enregistrement de `DataReader`.|  
  
## DbQueryDataSet  
 Exécute une requête qui retourne un <xref:System.Data.DataSet>.Cette classe effectue son travail de façon asynchrone.Elle dérive d'<xref:System.Activities.AsyncCodeActivity>\<`TResult`\> et utilise ses fonctionnalités asynchrones.  
  
 Les informations de connexion peuvent être configurées en définissant un nom invariant de fournisseur \(`ProviderName`\) et la chaîne de connexion \(`ConnectionString`\), ou simplement à l'aide d'un nom de configuration de chaîne de connexion \(`ConfigFileSectionName`\) provenant du fichier de configuration de l'application.  
  
 La requête à exécuter est configurée dans sa propriété `Sql` et les paramètres sont passés via la collection `Parameters`.  
  
 Une fois `DbQueryDataSet` exécuté, `DataSet` est retourné dans l'argument `Result``out` \(de type `TResult`, défini dans la classe de base <xref:System.Activities.AsyncCodeActivity%601>\).  
  
```  
public class DbQueryDataSet : AsyncCodeActivity<DataSet>  
{  
    // public arguments  
    [RequiredArgument]  
    [OverloadGroup("ConnectionString")]  
    [DefaultValue(null)]  
    public InArgument<string> ProviderName { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("ConnectionString")]  
    [DependsOn("ProviderName")]  
    [DefaultValue(null)]  
    public InArgument<string> ConnectionString { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("ConfigFileSectionName")]  
    [DefaultValue(null)]  
    public InArgument<string> ConfigName { get; set; }  
  
    [DefaultValue(null)]  
    public CommandType CommandType { get; set; }  
  
    [RequiredArgument]  
    public InArgument<string> Sql { get; set; }  
  
    [DependsOn("Sql")]  
    [DefaultValue(null)]  
    public IDictionary<string, Argument> Parameters { get; }  
}  
```  
  
|||  
|-|-|  
|Argument|Description|  
|ProviderName|Nom invariant du fournisseur ADO.NET.Si cet argument est défini, `ConnectionString` doit également être défini.|  
|ConnectionString|Chaîne de connexion utilisée pour se connecter à la base de données.Si cet argument est défini, `ProviderName` doit également être défini.|  
|ConfigName|Nom de la section du fichier de configuration où les informations de connexion sont stockées.Lorsque cet argument est défini, `ProviderName` et `ConnectionString` ne sont pas requis.|  
|CommandType|Type du <xref:System.Data.Common.DbCommand> à exécuter.|  
|Sql|Commande SQL à exécuter.|  
|Parameters|Collection des paramètres de la requête SQL.|  
|Result|<xref:System.Data.DataSet> obtenu une fois la requête exécutée.|  
  
## Configuration des informations de connexion  
 Tous les DbActivities partagent les mêmes paramètres de configuration.Ils peuvent être configurés de deux façons :  
  
-   `ConnectionString + InvariantName` : définit le nom invariant du fournisseur ADO.NET et la chaîne de connexion.  
  
    ```  
    Activity dbSelectCount = new DbQueryScalar<DateTime>()  
    {  
        ProviderName = "System.Data.SqlClient",  
        ConnectionString = @"Data Source=.\SQLExpress;  
                             Initial Catalog=DbActivitiesSample;  
                             Integrated Security=True",  
        Sql = "SELECT GetDate()"  
    };  
    ```  
  
-   `ConfigName` : définit le nom de la section de configuration qui contient les informations de connexion.  
  
    ```  
    <connectionStrings>      
        <add name="DbActivitiesSample"  
             providerName="System.Data.SqlClient"  
             connectionString="Data Source=.\SQLExpress;Initial Catalog=DbActivitiesSample;Integrated Security=true"/>  
      </connectionStrings>  
    ```  
  
-   Dans l'activité :  
  
    ```  
    Activity dbSelectCount = new DbQueryScalar<int>()  
    {                  
        ConfigName = "DbActivitiesSample",  
        Sql = "SELECT COUNT(*) FROM Roles"  
    };  
    ```  
  
## Exécution de cet exemple  
  
### Instructions de configuration  
 Cet exemple utilise une base de données.Un script d'installation et de chargement \(Setup.cmd\) est fourni avec l'exemple.Vous devez exécuter ce fichier à l'aide de l'invite de commandes.  
  
 Le script Setup.cmd appelle le fichier de script CreateDb.sql, lequel contient des commandes SQL qui effectuent les opérations suivantes :  
  
-   Crée une base de données appelée DbActivitiesSample.  
  
-   Crée la table Roles.  
  
-   Crée la table Employees.  
  
-   Insère trois enregistrements dans la table Roles.  
  
-   Insère douze enregistrements dans la table Employees.  
  
##### Pour exécuter Setup.cmd  
  
1.  Ouvrez une invite de commandes.  
  
2.  Accédez au dossier d'exemples DbActivities.  
  
3.  Tapez "setup.cmd", puis appuyez sur ENTRÉE.  
  
    > [!NOTE]
    >  Setup.cmd tente d'installer l'exemple dans l'instance SqlExpress de votre ordinateur local.Si vous voulez l'installer dans une autre instance SQL Server, modifiez Setup.cmd avec le nom de la nouvelle instance.  
  
##### Pour désinstaller l'exemple de base de données  
  
1.  Exécutez le fichier Cleanup.cmd du dossier d'exemples dans une invite de commandes.  
  
##### Pour exécuter l'exemple  
  
1.  Ouvrez la solution dans [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)].  
  
2.  Pour compiler la solution, appuyez sur Ctrl\+Maj\+B.  
  
3.  Pour exécuter l'exemple sans débogage, appuyez sur CTRL\+F5.  
  
> [!IMPORTANT]
>  Les exemples peuvent déjà être installés sur votre ordinateur.Recherchez le répertoire \(par défaut\) suivant avant de continuer.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page \(éventuellement en anglais\) des [exemples Windows Communication Foundation \(WCF\) et Windows Workflow Foundation \(WF\) pour .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) pour télécharger tous les exemples [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] et [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Cet exemple se trouve dans le répertoire suivant.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WF\Scenario\ActivityLibrary\DbActivities`