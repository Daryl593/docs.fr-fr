---
title: "Options et directives concernant la technologie ADO.NET | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: c8577281-38e6-4ce5-b036-572039a4c3d8
caps.latest.revision: 6
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 6
---
# Options et directives concernant la technologie ADO.NET
La plateforme de données ADO.NET est une stratégie multiversion visant à limiter le codage et la maintenance requis des développeurs, en permettant à ces derniers de concevoir des programmes à partir de modèles de données d'entité conceptuels.  Cette plateforme inclut ADO.NET Entity Framework et les technologies associées.  
  
## Entity Framework  
 ADO.NET Entity Framework est conçu pour permettre aux développeurs de créer des applications d'accès aux données en programmant à partir d'un modèle d'application conceptuel plutôt que directement sur un schéma de stockage relationnel.  L'objectif est de limiter les opérations de codage et de maintenance requises par les applications orientées données.  Pour plus d'informations, consultez [ADO.NET Entity Framework](../../../../docs/framework/data/adonet/ef/index.md).  
  
### EDM \(Entity Data Model\)  
 Un modèle de données d'entité \(EDM\) est une spécification de conception qui définit les données d'application comme des ensembles d'entités et de relations.  Les données de ce modèle prennent en charge le mappage relationnel objet et la programmabilité des données à travers l'ensemble des applications.  
  
### Object Services  
 Object Services permet aux programmeurs d'interagir avec le modèle conceptuel par le biais de classes CLR \(Common Language Runtime\).  Ces classes peuvent être générées automatiquement à partir du modèle conceptuel ou développées indépendamment pour refléter la structure du modèle conceptuel.  Object Services prend également en charge l'infrastructure Entity Framework, notamment des services tels que la gestion d'état, le suivi des modifications, la résolution d'identité, le chargement et l'exploration des relations, la propagation des modifications des objets en modifications de base de données et la prise en charge de la génération de requêtes pour Entity SQL.  Pour plus d'informations, consultez [Object Services Overview \(Entity Framework\)](http://msdn.microsoft.com/fr-fr/43014cf9-c9cb-4538-bfbb-197820b60038).  
  
### LINQ to Entities  
 LINQ to Entities est une implémentation LINQ \(Language\-Integrated Query\) qui permet aux développeurs de créer des requêtes fortement typées par rapport au contexte de l'objet Entity Framework, à l'aide d'expressions LINQ et d'opérateurs de requête LINQ standard.  LINQ to Entities permet aux développeurs de travailler sur un modèle conceptuel doté d'un mappage relationnel objet très souple sur l'ensemble des bases de données Microsoft SQL Server et tierces.  Pour plus d'informations, consultez [LINQ to Entities](../../../../docs/framework/data/adonet/ef/language-reference/linq-to-entities.md).  
  
### Entity SQL  
 Entity SQL est un langage de requête textuel conçu pour interagir avec un modèle EDM.  Entity SQL est un dialecte SQL qui contient des constructions pour l'interrogation en termes de concepts de modélisation de niveau supérieur, comme l'héritage, les types complexes et les relations explicites.  Les développeurs peuvent également utiliser Entity SQL directement avec Object Services.  Pour plus d'informations, consultez [Langage Entity SQL](../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-language.md).  
  
### EntityClient  
 EntityClient est un nouveau fournisseur de données .NET Framework qui permet d'interagir avec un modèle EDM.  EntityClient suit le modèle du fournisseur de données .NET Framework pour l'exposition d'objets <xref:System.Data.EntityClient.EntityConnection> et <xref:System.Data.EntityClient.EntityCommand> qui retournent un <xref:System.Data.EntityClient.EntityDataReader>.  EntityClient fonctionne avec le langage Entity SQL, fournissant ainsi un mappage souple aux fournisseurs de données spécifiques au stockage.  Pour plus d'informations, consultez [EntityClient and Entity SQL](http://msdn.microsoft.com/fr-fr/49202ab9-ac98-4b4b-a05c-140e422bf527).  
  
### Entity Data Model Tools  
 Entity Framework fournit des outils en ligne de commande, des Assistants et des concepteurs permettant de faciliter la création d'applications EDM.  Le contrôle EntityDataSource prend en charge les scénarios de liaison de données basés sur le modèle EDM.  La surface de programmation du contrôle EntityDataSource est similaire à celle des autres contrôles de source de données dans Visual Studio.  Pour plus d'informations, consultez [ADO.NET Entity Data Model  Tools](http://msdn.microsoft.com/fr-fr/91076853-0881-421b-837a-f582f36be527).  
  
## LINQ to SQL  
 LINQ to SQL est une implémentation de mappage relationnel objet qui vous permet de modéliser une base de données SQL Server à l'aide de classes .NET Framework.  LINQ to SQL vous permet d'interroger la base de données en utilisant LINQ ainsi que de mettre à jour, d'insérer et de supprimer des données dans cette base de données.  LINQ to SQL prend en charge les transactions, les vues et les procédures stockées, en fournissant un moyen facile d'intégrer la validation des données et des règles de logique métier dans le modèle de données.  L'outil Concepteur Objet\/Relationnel \(Concepteur O\/R\) vous permet de modéliser les classes d'entité et les associations basées sur les objets dans une base de données.  Pour plus d'informations, consultez [LINQ to SQL Tools dans Visual Studio](../Topic/LINQ%20to%20SQL%20Tools%20in%20Visual%20Studio2.md).  
  
## Services de données WCF  
 [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] déploie des services de données sur le Web ou sur un intranet.  Les données sont structurées sous la forme d'entités et de relations conformément aux spécifications du modèle EDM.  Les données déployées sur ce modèle sont adressables par le protocole HTTP standard.  Pour plus d'informations, consultez [WCF Data Services 4.5](../../../../docs/framework/data/wcf/index.md).  
  
## Voir aussi  
 [Vue d'ensemble d'ADO.NET](../../../../docs/framework/data/adonet/ado-net-overview.md)   
 [Nouveautés dans ADO.NET](../../../../docs/framework/data/adonet/whats-new.md)   
 [Fournisseurs managés ADO.NET et Centre de développement de DataSet](http://go.microsoft.com/fwlink/?LinkId=217917)