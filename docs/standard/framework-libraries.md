---
title: "Bibliothèques de framework"
description: "Découvrez la façon dont les bibliothèques fournissent des implémentations pour de nombreux types généraux et spécifiques d’une application, algorithmes et fonctionnalités d’utilitaire."
keywords: .NET, .NET Core
author: richlander
ms.author: ronpet
ms.date: 06/20/2016
ms.topic: article
ms.prod: .net
ms.technology: dotnet-standard
ms.devlang: dotnet
ms.assetid: 7b77b6c1-8367-4602-bff3-91e4c05ac643
ms.translationtype: HT
ms.sourcegitcommit: 934373d61407c8cc19b7d6424898a582880f9c21
ms.openlocfilehash: 9c38237a8e729155c443298814f4c98bec61016b
ms.contentlocale: fr-fr
ms.lasthandoff: 08/21/2017

---

# <a name="framework-libraries"></a>Bibliothèques de framework

.NET offre un grand ensemble standard de bibliothèques de classes, appelées bibliothèques de classes de base (ensemble principal) ou bibliothèques de classes de framework (ensemble complet). Ces bibliothèques fournissent des implémentations pour de nombreux types généraux et spécifiques d’une application, algorithmes et fonctionnalités d’utilitaire. Les bibliothèques commerciales et de la Communauté étant créées à partir des bibliothèques de classes de framework, elles sont tout de suite faciles à utiliser pour des tâches de calcul très diverses.

Une partie de ces bibliothèques est fournie avec chaque implémentation .NET. Les API de bibliothèque de classes de base doivent être incluses dans toutes les implémentations .NET parce que les développeurs le souhaitent et parce que les bibliothèques courantes en ont besoin pour s’exécuter. Les bibliothèques spécifiques aux applications en plus de la bibliothèque de classes de base, comme ASP.NET, ne sont pas disponibles dans toutes les implémentations .NET.

## <a name="base-class-libraries"></a>Bibliothèques de classes de base

Les bibliothèques de classes de base fournissent les types les plus fondamentaux ainsi que les fonctionnalités d’utilitaire et constituent la base de toutes les autres bibliothèques de classes .NET. Elles ont pour objectif de fournir des implémentations très générales sans préférer une charge de travail plutôt qu’une autre. Les performances sont toujours un facteur important, car les applications peuvent préférer une stratégie en particulier, comme la faible latence par rapport au haut débit ou une mémoire insuffisante par rapport à une faible utilisation du processeur. Ces bibliothèques sont généralement destinées à être très performantes et adoptent une approche intermédiaire en fonction des différents besoins en performances. Pour la plupart des applications, cette approche fonctionne plutôt bien.

## <a name="primitive-types"></a>Types primitifs

.NET inclut un ensemble de types primitifs qui sont utilisés (à des degrés divers) dans tous les programmes. Ces types contiennent des données, comme des nombres, des chaînes, des octets et des objets arbitraires. Le langage C# comprend des mots clés pour ces types. Un exemple d’ensemble de ces types est indiqué ci-dessous, avec les mots clés C# correspondants.

* <xref:System.Object?displayProperty=fullName> ([object](../csharp/language-reference/keywords/object.md)) : Classe de base par excellence dans le système de type CRL. Elle constitue la racine de la hiérarchie des types.
* <xref:System.Int16?displayProperty=fullName> ([short](../csharp/language-reference/keywords/short.md)) : Type entier signé 16 bits. L’entier non signé <xref:System.UInt16> existe également.
* <xref:System.Int32?displayProperty=fullName> ([int](../csharp/language-reference/keywords/int.md)) : Type entier signée 32 bits. L’entier non signé [UInt32](../csharp/language-reference/keywords/uint.md) existe également.
* <xref:System.Single?displayProperty=fullName> ([float](../csharp/language-reference/keywords/float.md)) : Type virgule flottante 32 bits.
* <xref:System.Decimal?displayProperty=fullName> ([decimal](../csharp/language-reference/keywords/decimal.md)) : Type décimal 128 bits.
* <xref:System.Byte?displayProperty=fullName> ([byte](../csharp/language-reference/keywords/byte.md)) : Entier non signé 8 bits qui représente un octet de mémoire.
* <xref:System.Boolean?displayProperty=fullName>([bool](../csharp/language-reference/keywords/bool.md)) : Type booléen qui représente `true` ou `false`.
* <xref:System.Char?displayProperty=fullName> ([char](../csharp/language-reference/keywords/char.md)) : Type numérique 16 bits qui représente un caractère Unicode.
* <xref:System.String?displayProperty=fullName> ([string](../csharp/language-reference/keywords/string.md)) : Représente une série de caractères. Différent de `char[]`, mais permet l’indexation dans chaque `char` individuel de `string`.

## <a name="data-structures"></a>Structures de données

.NET inclut un ensemble de structures de données qui sont les bêtes de somme de presque toutes les applications .NET. Il s’agit essentiellement de collections, mais on retrouve également d’autres types.

*   <xref:System.Array> : Représente un tableau d’objets fortement typés accessibles par index. Sa taille est fixe, par sa construction.
*   <xref:System.Collections.Generic.List%601> : Représente une liste fortement typée d’objets accessibles par index. Est automatiquement redimensionnée en fonction des besoins.
*   <xref:System.Collections.Generic.Dictionary%602> : Représente une collection de valeurs indexées par une clé. Les valeurs sont accessibles par une clé. Est automatiquement redimensionnée en fonction des besoins.
*   <xref:System.Uri> : Fournit une représentation objet d’un URI (Uniform Resource Identifier) et un accès simplifié aux parties de l’identificateur.
*   <xref:System.DateTime> : Représente un instant, généralement exprimé sous la forme d’une date et d’une heure.

## <a name="utility-apis"></a>API d’utilitaire

.NET inclut un ensemble d’API d’utilitaire qui fournissent des fonctionnalités pour de nombreuses tâches importantes.

*   <xref:System.Net.Http.HttpClient> : API qui permet d’envoyer des requêtes HTTP et de recevoir des réponses HTTP d’une ressource identifiée par un URI.
*   <xref:System.Xml.Linq.XDocument> : API qui permet de charger et d’interroger des documents XML avec LINQ.
*   <xref:System.IO.StreamReader> : API qui permet de lire des fichiers (<xref:System.IO.StringWriter>). Peut être utilisée pour écrire des fichiers.

## <a name="app-model-apis"></a>API de modèle d’application

Il existe de nombreux modèles d’application qui peuvent être utilisés avec .NET, fournis par plusieurs sociétés.

*   [ASP.NET](http://asp.net) : Fournit un framework web pour créer des sites et des services web. Pris en charge sur Windows, Linux et Mac OS (dépend de la version d’ASP.NET).

