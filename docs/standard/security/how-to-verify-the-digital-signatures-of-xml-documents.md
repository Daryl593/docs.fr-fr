---
title: "Comment&#160;: v&#233;rifier les signatures num&#233;riques de documents XML | Microsoft Docs"
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
helpviewer_keywords: 
  - "vérifier les signatures"
  - "signatures numériques, vérifier"
  - "signatures, de chiffrement"
  - "System.Security.Cryptography.RSACryptoServiceProvider (classe)"
  - "System.Security.Cryptography.SignedXml (classe)"
  - "vérifier les signatures"
  - "signatures numériques XML"
ms.assetid: a4d5ceb1-b9f5-47e8-9e4a-a2b39110002f
caps.latest.revision: 8
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 8
---
# Comment&#160;: v&#233;rifier les signatures num&#233;riques de documents XML
Vous pouvez utiliser les classes de l'espace de noms <xref:System.Security.Cryptography.Xml> pour vérifier les données XML signées avec une signature numérique.  Les signatures numériques XML \(XMLDSIG\) vous permettent de vérifier que les données n'ont pas été modifiées après leur signature.  Pour plus d'informations sur la norme XMLDSIG, consultez la spécification du World Wide Web Consortium \(W3C\) à l'adresse suivante : http:\/\/www.w3.org\/TR\/xmldsig\-core\/.  
  
 L'exemple de code de cette procédure montre comment vérifier une signature numérique XML contenue dans un élément \<`Signature`\>.  L'exemple récupère une clé publique RSA d'un conteneur de clé, puis utilise cette clé pour vérifier la signature.  
  
 Pour plus d'informations sur la création d'une signature numérique qui peut être vérifiée à l'aide de cette technique, consultez [Comment : signer des documents XML avec des signatures numériques](../../../docs/standard/security/how-to-sign-xml-documents-with-digital-signatures.md).  
  
### Pour vérifier la signature numérique d'un document XML  
  
1.  Pour vérifier le document, vous devez utiliser la même clé asymétrique que celle utilisée pour la signature.  Créez un objet <xref:System.Security.Cryptography.CspParameters> et spécifiez le nom du conteneur de clé qui a été utilisé pour la signature.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#2)]
     [!code-vb[HowToVerifyXMLDocumentRSA#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#2)]  
  
2.  Récupérez la clé publique à l'aide de la classe <xref:System.Security.Cryptography.RSACryptoServiceProvider>.  La clé est automatiquement chargée depuis le conteneur de clé quand vous passez l'objet <xref:System.Security.Cryptography.CspParameters> au constructeur de la classe <xref:System.Security.Cryptography.RSACryptoServiceProvider>.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#3)]
     [!code-vb[HowToVerifyXMLDocumentRSA#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#3)]  
  
3.  Créez un objet <xref:System.Xml.XmlDocument> en chargeant un fichier XML à partir du disque.  L'objet <xref:System.Xml.XmlDocument> contient le document XML signé à vérifier.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#4)]
     [!code-vb[HowToVerifyXMLDocumentRSA#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#4)]  
  
4.  Créez un objet <xref:System.Security.Cryptography.Xml.SignedXml> et passez\-lui l'objet <xref:System.Xml.XmlDocument>.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#5)]
     [!code-vb[HowToVerifyXMLDocumentRSA#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#5)]  
  
5.  Recherchez l'élément \<`signature`\>, puis créez un objet <xref:System.Xml.XmlNodeList>.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#6)]
     [!code-vb[HowToVerifyXMLDocumentRSA#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#6)]  
  
6.  Chargez le code XML du premier élément \<`signature`\> dans l'objet <xref:System.Security.Cryptography.Xml.SignedXml>.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#7)]
     [!code-vb[HowToVerifyXMLDocumentRSA#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#7)]  
  
7.  Vérifiez la signature à l'aide de la méthode <xref:System.Security.Cryptography.Xml.SignedXml.CheckSignature%2A> et de la clé publique RSA.  Cette méthode retourne une valeur booléenne qui indique la réussite ou l'échec.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#8)]
     [!code-vb[HowToVerifyXMLDocumentRSA#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#8)]  
  
## Exemple  
 Cet exemple suppose qu'un fichier nommé `"test.xml"` se trouve dans le même répertoire que le programme compilé.  Le fichier `"test.xml"` doit être signé à l'aide des techniques décrites dans [Comment : signer des documents XML avec des signatures numériques](../../../docs/standard/security/how-to-sign-xml-documents-with-digital-signatures.md).  
  
 [!code-csharp[HowToVerifyXMLDocumentRSA#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#1)]
 [!code-vb[HowToVerifyXMLDocumentRSA#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#1)]  
  
## Compilation du code  
  
-   Pour compiler cet exemple, vous devez inclure une référence à `System.Security.dll`.  
  
-   Incluez les espaces de noms suivants : <xref:System.Xml>, <xref:System.Security.Cryptography> et <xref:System.Security.Cryptography.Xml>.  
  
## Sécurité .NET Framework  
 La clé privée d'une paire de clés asymétriques ne doit jamais être stockée ni transférée en texte brut.  Pour plus d'informations sur les clés de chiffrement symétrique et asymétrique, consultez [Génération de clés pour le chiffrement et le déchiffrement](../../../docs/standard/security/generating-keys-for-encryption-and-decryption.md).  
  
 N'incorporez jamais une clé privée directement dans votre code source.  Les clés incorporées peuvent être lues facilement depuis un assembly à l'aide du [Ildasm.exe \(IL Disassembler\)](../../../docs/framework/tools/ildasm-exe-il-disassembler.md) ou en ouvrant l'assembly dans un éditeur de texte tel que le Bloc\-notes.  
  
## Voir aussi  
 <xref:System.Security.Cryptography.Xml>   
 [Comment : signer des documents XML avec des signatures numériques](../../../docs/standard/security/how-to-sign-xml-documents-with-digital-signatures.md)