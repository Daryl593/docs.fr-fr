---
title: "Modification des propri&#233;t&#233;s de pr&#233;fixe d&#39;espace de noms | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
ms.assetid: d5c87cbe-4d69-429f-aad5-3103c2ca2770
caps.latest.revision: 3
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 3
---
# Modification des propri&#233;t&#233;s de pr&#233;fixe d&#39;espace de noms
La classe **XmlNode** vous permet de modifier le préfixe d'espace de noms associé à un nœud précis.  Par exemple, le code suivant montre la modification du préfixe d'un élément.  
  
```vb  
Dim doc as XmlDocument = new XmlDocument()  
doc.LoadXml("<a:test xmlns:a='123' xmlns:b='456'/>")  
Dim e as XmlElement = doc.DocumentElement  
e.Prefix = "b"  
Console.WriteLine(doc.InnerXml)  
```  
  
```csharp  
XmlDocument doc = new XmlDocument();  
doc.LoadXml("<a:test xmlns:a='123' xmlns:b='456'/>");  
XmlElement e = doc.DocumentElement;         
e.Prefix = "b";  
Console.WriteLine(doc.InnerXml);  
```  
  
 **Sortie**  
  
```  
<b:test xmlns:a="123" xmlns:b="456" />  
```  
  
 La modification du préfixe d'un nœud n'entraîne pas la modification de son espace de noms.  Ce dernier peut être défini uniquement au moment de la création du nœud.  Lorsque vous rendez persistante l'arborescence, de nouveaux attributs d'espace de noms peuvent ne plus être persistants pour satisfaire le préfixe défini.  Si le nouvel espace de noms ne peut pas être créé, le préfixe est modifié de telle sorte que le nœud préserve son nom local et son espace de noms.  L'exemple suivant illustre l'ajout d'un attribut d'espace de noms.  
  
```vb  
Dim doc as XmlDocument = new XmlDocument()  
doc.LoadXml("<test xmlns='123'/>")  
Dim e as XmlElement = doc.DocumentElement  
e.Prefix = "a"  
Console.WriteLine(doc.InnerXml)  
```  
  
```csharp  
XmlDocument doc = new XmlDocument();  
doc.LoadXml("<test xmlns='123'/>");  
XmlElement e = doc.DocumentElement;         
e.Prefix = "a";  
Console.WriteLine(doc.InnerXml);  
```  
  
 **Sortie**  
  
```  
<a:test xmlns="123" xmlns:a="123" />  
```  
  
 Lorsque l'arborescence a été rendue persistante sous la forme d'une chaîne à la suite de l'appel à **doc.InnerXml**, l'attribut `xmlns:a='123'` a été ajouté pour préserver l'espace de noms de l'élément `test`.  Sa valeur était `'123'` et est restée `'123'`.  
  
## Voir aussi  
 [DOM \(Document Object Model\) XML](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)