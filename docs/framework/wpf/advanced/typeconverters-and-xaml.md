---
title: "TypeConverters et XAML | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-wpf"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "classes, TypeConverter"
  - "TypeConverter (classe)"
  - "XAML, TypeConverter (classe)"
ms.assetid: f6313e4d-e89d-497d-ac87-b43511a1ae4b
caps.latest.revision: 13
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 12
---
# TypeConverters et XAML
Cette rubrique présente l'objectif de la conversion de types de chaîne comme une fonctionnalité générale du  langage XAML.  Dans le .NET Framework, la classe <xref:System.ComponentModel.TypeConverter> sert à un objectif particulier dans le cadre de l'implémentation pour une classe personnalisée managée qui peut être utilisée comme une valeur de propriété dans l'utilisation d'attribut XAML.  Si vous écrivez une classe personnalisée et voulez que les instances de la classe soient utilisables comme valeurs d'attribut définissables XAML, il se peut que vous deviez appliquer un <xref:System.ComponentModel.TypeConverterAttribute> à votre  classe, écrire une classe personnalisée <xref:System.ComponentModel.TypeConverter>, ou les deux.  
  
   
  
## Concepts de conversion de types  
  
### XAML et valeurs de chaîne  
 Lorsque vous définissez une valeur d'attribut dans un fichier XAML, le type initial de cette valeur est une chaîne en texte pur.  Même d'autres primitives telles que <xref:System.Double> sont initialement des chaînes de texte pour un processeur XAML.  
  
 Un processeur XAML a besoin de deux types d'informations afin de pouvoir traiter une valeur d'attribut.  Le premier élément d'information est le type valeur de la propriété à définir.  N'importe quelle chaîne qui définit une valeur d'attribut et qui est traitée en XAML doit être convertie ou résolue en une valeur de ce type.  Si la valeur est une primitive comprise par l'analyseur XAML \(telle qu'une valeur numérique\), une conversion directe de la chaîne est tentée.  Si la valeur est une énumération, la chaîne est utilisée pour rechercher une correspondance de nom à une constante nommée dans cette énumération.  Si la valeur n'est ni une primitive comprise par l'analyseur, ni une énumération, le type en question doit être en mesure de fournir une instance du type ou une valeur, en fonction d'une chaîne convertie.  Cela est fait en indiquant une classe de convertisseur de type.  Le convertisseur de type est une classe d'assistance pour la fourniture de valeurs d'une autre classe, à la fois pour le scénario XAML et également pour les appels de code dans le code .NET.  
  
### Utilisation du comportement de conversion de types existant en XAML  
 En fonction de votre connaissance des concepts XAML sous\-jacents, il se peut que vous utilisiez déjà un  comportement de conversion de types dans des applications XAML de base sans vous en rendre compte.  Par exemple, WPF définit des centaines de propriétés qui prennent une valeur de type <xref:System.Windows.Point>.  Un <xref:System.Windows.Point> est une valeur qui décrit une coordonnée dans un espace de coordonnées à deux dimensions et il a deux propriétés importantes : <xref:System.Windows.Point.X%2A> et <xref:System.Windows.Point.Y%2A>.  Lorsque vous spécifiez un point en XAML, vous le spécifiez comme une chaîne avec un séparateur \(en général une virgule\) entre les valeurs <xref:System.Windows.Point.X%2A> et <xref:System.Windows.Point.Y%2A> que vous fournissez.  Par exemple : `<LinearGradientBrush StartPoint="0,0" EndPoint="1,1">`.  
  
 Même ce type simple de <xref:System.Windows.Point> et son utilisation simple en XAML implique un convertisseur de type.  Dans ce cas, il s'agit de la classe <xref:System.Windows.PointConverter>.  
  
 Le convertisseur de type pour <xref:System.Windows.Point> défini au niveau de la classe simplifie les utilisations de balises de toutes les propriétés qui prennent <xref:System.Windows.Point>.  Sans un convertisseur de type ici, vous auriez besoin de la balise suivante beaucoup plus détaillée pour l'exemple indiqué précédemment :  
  
 `<LinearGradientBrush>`  
  
 `<LinearGradientBrush.StartPoint>`  
  
 `<Point X="0" Y="0"/>`  
  
 `</LinearGradientBrush.StartPoint>`  
  
 `<LinearGradientBrush.EndPoint>`  
  
 `<Point X="1" Y="1"/>`  
  
 `</LinearGradientBrush.EndPoint>`  
  
 `<LinearGradientBrush>`  
  
 L'utilisation d'une chaîne de conversion de type ou d'une syntaxe équivalente plus détaillée est une question de style de codage.  Votre workflow d'outils XAML peut également influencer la définition des valeurs.  Certains outils XAML ont tendance à émettre la forme la plus détaillée du balisage, car il est plus facile de parcourir les vues du concepteur ou son propre mécanisme de sérialisation.  
  
 Les convertisseurs de types existants peuvent être découverts en général dans les types WPF et .NET Framework en vérifiant la présence d'un <xref:System.ComponentModel.TypeConverterAttribute> appliqué dans une classe \(ou une propriété\).  Cet attribut nommera la classe qui est le convertisseur de type de prise en charge pour les valeurs de ce type, pour des besoins XAML ainsi d'autres.  
  
### Convertisseurs de types et extensions de balisage  
 Les extensions de balisage et les convertisseurs de types remplissent des rôles orthogonaux quant au comportement du processeur XAML et aux scénarios auxquels ils s'appliquent.  Bien que le contexte soit disponible pour les utilisations des extensions de balisage, le comportement de conversion de type des propriétés pour lesquelles une extension de balisage fournit une valeur n'est généralement pas vérifié dans les implémentations des extensions.  En d'autres termes, même si une extension de balisage retourne une chaîne de texte en tant que sortie `ProvideValue`, le comportement de conversion de type associé à cette chaîne tel qu'il est appliqué à une propriété ou un type de valeur de propriété spécifique n'est pas appelé. En général, la fonction d'une extension de balisage consiste à traiter une chaîne et retourner un objet sans faire appel à un convertisseur de type.  
  
 Une situation courante où il est nécessaire d'utiliser une extension de balisage plutôt qu'un convertisseur de type est la création d'une référence à un objet existant.  Au mieux, un convertisseur de type sans état pourrait uniquement générer une nouvelle instance, ce qui n'est peut\-être pas souhaitable.  Pour plus d'informations sur les extensions de balisage, consultez [Extensions de balisage et XAML WPF](../../../../docs/framework/wpf/advanced/markup-extensions-and-wpf-xaml.md).  
  
### Convertisseurs de types natifs  
 Dans une implémentation WPF et .NET Framework de l'analyseur XAML, il existe certains types qui gèrent nativement la conversion de type, cependant ce ne sont pas des types qui peuvent être considérés comme des  primitives.  Un exemple d'un tel type est <xref:System.DateTime>.  La raison de ceci est basée sur le fonctionnement de l'architecture .NET Framework : le type <xref:System.DateTime> est défini dans mscorlib, la bibliothèque de base du .NET.  <xref:System.DateTime> ne peut pas être attribué avec un attribut qui provient d'un autre assembly et qui présente une dépendance \(<xref:System.ComponentModel.TypeConverterAttribute> est de Système\), c'est pourquoi le mécanisme de découverte du convertisseur de type habituel par attribution ne peut pas être pris en charge.  À la place, l'analyseur XAML a une liste des types qui doivent subir ce traitement natif et qui traite ces types de la même façon que les vraies primitives sont traitées.  \(Dans le cas de <xref:System.DateTime>, cela implique un appel à <xref:System.DateTime.Parse%2A>.  
  
<a name="Implementing_a_Type_Converter"></a>   
## Implémentation d'un convertisseur de type  
  
### TypeConverter  
 Dans l'exemple précédent <xref:System.Windows.Point>, la classe <xref:System.Windows.PointConverter> a été indiquée.  Pour les implémentations .NET de XAML, tous les convertisseurs de types utilisés à des fins XAML sont des classes qui dérivent de la classe de base <xref:System.ComponentModel.TypeConverter>.  La classe <xref:System.ComponentModel.TypeConverter> existait dans les versions du .NET Framework qui précèdent l'existence de XAML; l'une de ses utilisations d'origine était fournir la conversion de chaînes pour les boîtes de dialogue de propriétés dans les concepteurs visuels.  Pour XAML, le rôle de <xref:System.ComponentModel.TypeConverter> est développé pour inclure le fait d'être la classe de base pour les conversions en chaînes et de chaînes qui permettent l'analyse d'une valeur d'attribut de chaîne et le traitement d'une valeur à l'exécution d'une propriété spécifique de l'objet en chaîne pour la sérialisation comme un attribut.  
  
 <xref:System.ComponentModel.TypeConverter> définit quatre membres qui sont pertinents pour la conversion en et de chaînes à des fins de traitement XAML :  
  
-   <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A>  
  
-   <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A>  
  
-   <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>  
  
-   <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A>  
  
 La plus importante de ces méthodes est <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A>.  Cette méthode convertit la chaîne d'entrée en type d'objet requis.  Au sens strict, la méthode <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> pourrait être implémentée pour convertir une plage beaucoup plus large de types en  type de destination conçu pour le convertisseur, et donc permettre une extension au delà du XAML, par exemple  la prise en charge de conversions à l'exécution, mais pour XAML, seul le chemin d'accès de code peut traiter une entrée <xref:System.String>.  
  
 La seconde méthode la plus importante est <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>.  Si une application est convertie en une représentation de balise \(par exemple, si elle est enregistrée en XAML en tant que fichier\) <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> doit produire une représentation de balisage.  Dans ce cas, le chemin d'accès de code qui est important pour XAML est le passage d'un  `destinationType` de <xref:System.String>.  
  
 <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> et <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A> sont des méthodes de support utilisées lorsqu'un service interroge les fonctions de l'implémentation <xref:System.ComponentModel.TypeConverter>.  Vous devez implémenter ces méthodes pour retourner `true` pour des cas spécifiques de type pris en charge par les méthodes de conversion correspondantes de votre  convertisseur.  Pour XAML, cela correspond au type <xref:System.String>.  
  
### Informations de culture et convertisseurs de types pour XAML  
 Chaque implémentation <xref:System.ComponentModel.TypeConverter> peut avoir sa propre interprétation de la validité d'une chaîne dans le cadre d'une conversion et peut également utiliser ou ignorer la description de type passée comme paramètres.  Un élément fondamental est à prendre en compte en ce qui concerne la culture et la conversion de type XAML.  L'utilisation de chaînes localisables comme valeurs d'attribut est prise en charge intégralement par XAML.  Mais l'utilisation de cette chaîne localisable comme entrée de convertisseur de type avec des spécifications de culture spécifiques n'est pas prise en charge, car les convertisseurs de types pour les valeurs d'attribut XAML impliquent un comportement d'analyse de langage fixe, à l'aide de la culture `en-US`.  Pour plus d'informations sur les raisons de conception de cette restriction, vous devez consulter la spécification de langage XAML \([\[MS\-XAML\]](http://go.microsoft.com/fwlink/?LinkId=114525)\).  
  
 Par exemple, certaines cultures utilisent une virgule comme séparateur décimal pour les nombres, ce qui peut être un problème.  Cela créera un conflit avec le comportement de nombreux convertisseurs de types XAML WPF, qui consiste à utiliser une virgule comme un séparateur \(pour des raisons historiques : forme X,Y courante ou listes délimitées par des virgules\).  Même le passage d'une culture dans le XAML environnant \(définition de `Language` ou `xml:lang` à la culture `sl-SI`, exemple d'une culture qui utilise une virgule comme séparateur décimal\) ne résout pas le problème.  
  
### Implémentation de ConvertFrom  
 Pour être utilisable en tant qu'implémentation <xref:System.ComponentModel.TypeConverter> qui prend en charge le code XAML, la méthode <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> du convertisseur doit accepter une chaîne comme paramètre `value`.  Si la chaîne a un format valide et peut être convertie par l'implémentation <xref:System.ComponentModel.TypeConverter>, l'objet retourné doit alors pouvoir prendre en charge un cast en type attendu par la propriété.  Sinon, l'implémentation <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> doit retourner `null`.  
  
 Chaque implémentation <xref:System.ComponentModel.TypeConverter> peut avoir sa propre interprétation de la validité d'une chaîne dans le cadre d'une conversion et peut également utiliser ou ignorer la description de type ou les contextes de culture passés comme paramètres.  Cependant, le traitement XAML WPF peut ne pas passer de valeurs au contexte de description de type dans tous les cas, et également ne pas passer la culture en fonction de `xml:lang`.  
  
> [!NOTE]
>  N'utilisez pas les accolades, en particulier {, comme un élément possible de format de chaîne.  Ces caractères sont réservés comme entrée et sortie d'une séquence d'extension de balisage.  
  
### Implémentation de ConvertTo  
 <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> peut être utilisé pour assurer la prise en charge de la sérialisation.  La prise en charge de la sérialisation via <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> pour votre type personnalisé et son convertisseur de type n'est pas une spécification absolue.  Toutefois, si vous implémentez un contrôle ou que vous utilisez la sérialisation dans le cadre des fonctionnalités ou de la conception de la classe, vous devez implémenter <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>.  
  
 Pour être utilisable comme implémentation <xref:System.ComponentModel.TypeConverter> qui prend en charge XAML, la méthode <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> pour ce convertisseur doit accepter une instance du type \(ou une valeur\) prise en charge comme paramètre `value`.  Lorsque le paramètre `destinationType` est le type <xref:System.String>, l'objet retourné doit être en mesure à être casté en <xref:System.String>.  La chaîne retournée doit représenter une valeur sérialisée de `value`.  Idéalement, le format de sérialisation que vous choisissez doit pouvoir générer la même valeur si cette chaîne est passée à l'implémentation <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> du même convertisseur, sans perte significative d'information.  
  
 Si la valeur ne peut pas être sérialisée ou que le convertisseur ne prend pas en charge la sérialisation, l'implémentation <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> doit retourner `null` et elle peut lever une exception dans ce cas.  Mais si vous levez des exceptions, vous devez signaler l'incapacité à utiliser cette conversion dans le cadre de votre implémentation <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> afin que la meilleure pratique qui consiste à vérifier en premier avec <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> pour éviter des exceptions soit prise en charge.  
  
 Si le paramètre `destinationType` n'est pas de type <xref:System.String>, vous pouvez choisir votre propre gestion de convertisseur.  En général, vous rétablirez le traitement de l'implémentation de base, qui avec <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> lève une exception spécifique.  
  
### Implémentation de CanConvertTo  
 Votre implémentation <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> doit retourner la valeur `true` pour `destinationType` de type <xref:System.String> ou déférer à l'implémentation de base.  
  
### Implémentation de CanConvertFrom  
 L'implémentation <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A> doit retourner la valeur `true` pour `sourceType` de type <xref:System.String>, et sinon déférer à l'implémentation de base.  
  
<a name="Applying_the_TypeConverterAttribute"></a>   
## Application de TypeConverterAttribute  
 Pour pouvoir utiliser le convertisseur de type personnalisé comme convertisseur de type pour une classe personnalisée par un processeur XAML, vous devez appliquer [!INCLUDE[TLA#tla_netframewkattr](../../../../includes/tlasharptla-netframewkattr-md.md)]<xref:System.ComponentModel.TypeConverterAttribute> à la définition de classe.  Le <xref:System.ComponentModel.TypeConverterAttribute.ConverterTypeName%2A> que vous spécifiez à travers l'attribut doit être le nom de type du convertisseur de type personnalisé.  Avec cet attribut appliqué, lorsqu'un processeur XAML gère des valeurs où le type de propriété utilise votre type de classe personnalisée, il peut entrer des chaînes et retourner des instances d'objet.  
  
 Vous pouvez également fournir un convertisseur de type en fonction de la propriété.  Au lieu d'appliquer un [!INCLUDE[TLA#tla_netframewkattr](../../../../includes/tlasharptla-netframewkattr-md.md)]<xref:System.ComponentModel.TypeConverterAttribute> à la définition de classe, appliquez\-le à une définition de propriété \(la définition principale et non pas les implémentations `get`\/`set` qui s'y trouvent\).  Le type de la propriété doit correspondre au type traité par le convertisseur de type personnalisé.  Avec cet attribut appliqué, lorsqu'un processeur XAML gère des valeurs de cette propriété, il peut traiter des chaînes d'entrée et retourner des instances d'objet.  La technique de conversion en fonction de la propriété est particulièrement utile si vous choisissez d'utiliser un type de propriété [!INCLUDE[TLA#tla_netframewk](../../../../includes/tlasharptla-netframewk-md.md)] ou d'une autre bibliothèque où vous ne pouvez pas contrôler la définition de classe et ne pouvez pas appliquer un <xref:System.ComponentModel.TypeConverterAttribute> à cet endroit.  
  
## Voir aussi  
 <xref:System.ComponentModel.TypeConverter>   
 [Vue d'ensemble du langage XAML \(WPF\)](../../../../docs/framework/wpf/advanced/xaml-overview-wpf.md)   
 [Extensions de balisage et XAML WPF](../../../../docs/framework/wpf/advanced/markup-extensions-and-wpf-xaml.md)   
 [Syntaxe XAML en détail](../../../../docs/framework/wpf/advanced/xaml-syntax-in-detail.md)