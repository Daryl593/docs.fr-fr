---
title: "Chargement de donn&#233;es dans un DataSet | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: a53e5dc1-9669-49d4-828d-efa633237066
caps.latest.revision: 2
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 2
---
# Chargement de donn&#233;es dans un DataSet
Un objet <xref:System.Data.DataSet> doit être rempli avant que vous puissiez l'interroger avec [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)].  Il existe plusieurs manières de remplir le <xref:System.Data.DataSet>.  Par exemple, vous pouvez utiliser [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] pour interroger la base de données et charger les résultats dans le <xref:System.Data.DataSet>.  Pour plus d'informations, consultez [LINQ à SQL](../../../../docs/framework/data/adonet/sql/linq/index.md).  
  
 Une autre manière de charger des données dans un <xref:System.Data.DataSet> consiste à utiliser la classe <xref:System.Data.Common.DataAdapter> pour extraire des données de la base de données.  L'exemple suivant illustre ce concept.  
  
## Exemple  
 Cet exemple utilise un <xref:System.Data.Common.DataAdapter> pour interroger la base de données AdventureWorks et en extraire des informations de vente à partir de l'année 2002, et charge les résultats dans un <xref:System.Data.DataSet>.  Une fois que le <xref:System.Data.DataSet> a été rempli, vous pouvez y exécuter des requêtes à l'aide de [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)].  La méthode `FillDataSet` de cet exemple est utilisée dans les exemples de requête de la rubrique [Exemples de LINQ to DataSet](../../../../docs/framework/data/adonet/linq-to-dataset-examples.md).  Pour plus d'informations, consultez [Interrogation de DataSets](../../../../docs/framework/data/adonet/querying-datasets-linq-to-dataset.md).  
  
 [!code-csharp[DP LINQ to DataSet Examples#FillDataSet](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#filldataset)]
 [!code-vb[DP LINQ to DataSet Examples#FillDataSet](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#filldataset)]  
  
## Voir aussi  
 [Vue d'ensemble de LINQ to DataSet](../../../../docs/framework/data/adonet/linq-to-dataset-overview.md)   
 [Interrogation de DataSets](../../../../docs/framework/data/adonet/querying-datasets-linq-to-dataset.md)   
 [Exemples de LINQ to DataSet](../../../../docs/framework/data/adonet/linq-to-dataset-examples.md)