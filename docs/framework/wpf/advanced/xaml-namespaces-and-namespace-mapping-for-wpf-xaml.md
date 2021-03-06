---
title: "Espaces de noms XAML et mappage d&#39;espace de noms pour XAML WPF | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-wpf"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "assemblys, mapper des espaces de noms à"
  - "classes, mapper des espaces de noms à"
  - "classes personnalisées, mapper des espaces de noms à"
  - "mapper des espaces de noms"
  - "mappage d'espaces de noms"
  - "espaces de noms"
  - "XAML, mappage d'espaces de noms"
  - "XAML, espaces de noms"
ms.assetid: 5c0854e3-7470-435d-9fe2-93eec9d3634e
caps.latest.revision: 23
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 22
---
# Espaces de noms XAML et mappage d&#39;espace de noms pour XAML WPF
Cette rubrique explique plus en détail la présence et la finalité des deux mappages d'espace de noms XAML qui figurent souvent dans la balise racine de chaque fichier XAML WPF.  Elle décrit également comment produire des mappages similaires pour utiliser des éléments définis dans votre propre code et\/ou dans des assemblys distincts.  
  
   
  
## Qu'est\-ce qu'un espace de noms XAML ?  
 Un espace de noms XAML est en réalité une extension du concept d'espace de noms XML.  Les techniques de spécification d'un espace de noms XAML reposent sur la syntaxe d'espace de noms XML, la convention de l'utilisation d'URI comme identificateurs d'espaces de noms, à l'aide de préfixes permettant de fournir un moyen pour référencer plusieurs espaces de noms à partir de la même source de balise, et ainsi de suite.  Le concept principal ajouté à la définition XAML de l'espace de noms XML tient au fait qu'un espace de noms XAML implique à la fois une portée d'unicité pour les utilisations de balisage, et également influence la façon dont les entités de balisage sont potentiellement soutenues par les espaces de noms CLR spécifiques et les assemblys référencés.  Cette dernière considération est également influencée par le concept d'un contexte de schéma XAML.  Mais pour comprendre comment WPF fonctionne avec les espaces de noms XAML, vous pouvez généralement imaginer les espaces de noms XAML en termes d'espaces de noms XAML par défaut, d'espace de noms de langage XAML et tout espace de noms XAML supplémentaire, mappé directement par votre balisage XAML aux espaces de noms CLR de stockage spécifiques et assemblys référencés.  
  
<a name="The_WPF_and_XAML_Namespace_Declarations"></a>   
## Déclarations des espaces de noms XAML et WPF  
 Généralement deux déclarations d'espaces de noms XML se trouvent dans les déclarations d'espace de noms figurant dans la balise racine de nombreux fichiers XAML.  La première déclaration mappe l'espace de noms XAML client WPF \/ infrastructure général comme valeur par défaut :  
  
 `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`  
  
 La deuxième déclaration mappe \(généralement\) un espace de noms XAML distinct au préfixe `x:`.  
  
 `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`  
  
 La relation entre ces déclarations est la suivante : le mappage du préfixe `x:` prend en charge les intrinsèques qui font partie de la définition de langage XAML, et [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] est une implémentation qui utilise XAML comme langage et définit un vocabulaire de ses objets pour XAML.  Étant donné que les utilisations du vocabulaire WPF seront beaucoup plus courantes que les utilisations des intrinsèques XAML, le vocabulaire WPF est mappé comme valeur par défaut.  
  
 La convention de préfixage `x:` pour le mappage de prise en charge des intrinsèques en langage XAML est suivie par des modèles de projet, des exemple de code et la documentation des fonctionnalités de langage dans ce [!INCLUDE[TLA2#tla_sdk](../../../../includes/tla2sharptla-sdk-md.md)].  L'espace de noms XAML définit de nombreuses fonctions courantes qui sont nécessaires y compris pour les applications WPF de base.  Par exemple, pour joindre tout code\-behind à un fichier XAML par le biais d'une classe partielle, vous devez nommer cette classe comme attribut `x:Class` dans l'élément racine du fichier XAML approprié.  Ou bien, tout élément tel qu'il est défini dans une page XAML à laquelle vous souhaitez accéder comme une ressource à clé doit avoir l'attribut `x:Key` défini par l'élément en question.  Pour plus d'informations sur ces questions et d'autres aspects du langage XAML, consultez [Vue d'ensemble du langage XAML \(WPF\)](../../../../docs/framework/wpf/advanced/xaml-overview-wpf.md) ou [Syntaxe XAML en détail](../../../../docs/framework/wpf/advanced/xaml-syntax-in-detail.md).  
  
<a name="Mapping_To_Custom_Classes_and_Assemblies"></a>   
## Mappage aux classes et assemblys personnalisés  
 Vous pouvez mapper des espaces de noms XML à des assemblys à l'aide d'une série de jetons dans une déclaration de préfixe `xmlns`, de la même manière que les espaces de noms XAML WPF et XAML intrinsèques standard sont mappés aux préfixes.  
  
 La syntaxe prend les valeurs et les jetons nommés possibles suivants :  
  
 `clr-namespace:` Espace de noms CLR déclaré dans l'assembly contenant les types publics à exposer comme éléments.  
  
 `assembly=` Assembly contenant une partie ou l'intégralité de l'espace de noms [!INCLUDE[TLA2#tla_clr](../../../../includes/tla2sharptla-clr-md.md)] référencé.  En général, cette valeur est simplement le nom de l'assembly, pas le chemin d'accès, et n'inclut pas l'extension \(telle que .dll ou .exe\).  Le chemin d'accès à cet assembly doit être établi en tant que référence de projet dans le fichier projet qui contient le XAML que vous essayez de mapper.  Pour incorporer le contrôle de version et la signature avec nom fort, la valeur `assembly` peut être une chaîne telle que définie par <xref:System.Reflection.AssemblyName>, plutôt que le simple nom de la chaîne.  
  
 Notez que le jeton `clr-namespace`  est séparé de sa valeur par deux\-points \(:\) alors que le caractère qui sépare le jeton `assembly` de sa valeur est le signe égal \(\=\).  Le caractère à utiliser entre ces deux jetons est un point\-virgule.  N'incluez par ailleurs pas d'espace blanc n'importe où dans la déclaration.  
  
### Exemple de mappage personnalisé de base  
 Le code suivant définit un exemple de classe personnalisée :  
  
```csharp  
namespace SDKSample {  
    public class ExampleClass : ContentControl {  
        public ExampleClass() {  
        ...  
        }  
    }  
}  
```  
  
```vb  
Namespace SDKSample  
    Public Class ExampleClass  
        Inherits ContentControl  
         ...  
        Public Sub New()  
        End Sub  
    End Class  
End Namespace  
```  
  
 Cette classe personnalisée est ensuite compilée dans une bibliothèque qui, conformément aux paramètres du projet \(non affichés\), est nommée `SDKSampleLibrary`.  
  
 Pour référencer cette classe personnalisée, vous devez également l'inclure en tant que référence pour votre projet actuel, comme vous le feriez à l'aide de l'interface utilisateur de l'Explorateur de solutions dans Visual Studio.  
  
 Maintenant que vous avez une bibliothèque qui contient une classe, ainsi qu'une référence à cette bibliothèque dans les paramètres du projet, vous pouvez ajouter le mappage de préfixe suivant en tant qu'élément de votre élément racine dans le code XAML :  
  
 `xmlns:custom="clr-namespace:SDKSample;assembly=SDKSampleLibrary"`  
  
 Pour résumer, le code suivant est du code XAML qui inclut le mappage personnalisé avec la valeur par défaut typique et les mappages x: dans la balise racine, puis utilise une référence préfixée pour instancier `ExampleClass` dans cette interface utilisateur :  
  
```xaml  
<Page x:Class="WPFApplication1.MainPage"  
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"   
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  
    xmlns:custom="clr-namespace:SDKSample;assembly=SDKSampleLibrary">  
  ...  
  <custom:ExampleClass/>  
...  
</Page>  
```  
  
### Mappage aux assemblys actuels  
 `assembly` peut être omis si le `clr-namespace` référencé est défini dans le même assembly que le code d'application qui fait référence aux classes personnalisées.  Une syntaxe équivalente dans ce cas consiste à spécifier `assembly=`, sans jeton de chaîne après le signe égal.  
  
 Les classes personnalisées ne peuvent pas être utilisées comme élément racine d'une page si elles sont définies dans le même assembly.  Les classes partielles ne doivent pas être mappées ; seules les classes qui ne sont pas la classe partielle d'une page dans votre application doivent être mappées si vous prévoyez de les référencer en tant qu'éléments dans le code XAML.  
  
<a name="Mapping_CLR_Namespaces_to_XML_Namespaces_in_an"></a>   
## Mappage d'espaces de noms CLR à des espaces de noms XML dans un assembly  
 WPF définit un attribut CLR qui est consommé par les processeurs XAML pour mapper plusieurs espaces de noms CLR à un espace de noms XAML unique.  Cet attribut, <xref:System.Windows.Markup.XmlnsDefinitionAttribute>, est placé au niveau de l'assembly dans le code source qui produit ce dernier.  Le code source de l'assembly WPF utilise cet attribut pour mapper les divers espaces de noms communs, tels que <xref:System.Windows> et <xref:System.Windows.Controls>, à l'espace de noms [!INCLUDE[TLA#tla_wpfxmlnsv1](../../../../includes/tlasharptla-wpfxmlnsv1-md.md)].  
  
 <xref:System.Windows.Markup.XmlnsDefinitionAttribute> prend deux paramètres : le nom d'espace de noms XML\/XAML, et le nom d'espace de noms CLR.  Il peut exister plusieurs <xref:System.Windows.Markup.XmlnsDefinitionAttribute> pour mapper plusieurs espaces de noms CLR à un même espace de noms XML.  Une fois mappés, les membres de ces espaces de noms peuvent également être référencés sans qualification complète, si vous le souhaitez, en fournissant l'instruction `using` appropriée dans la page code\-behind de la classe partielle.  Pour plus d'informations, consultez <xref:System.Windows.Markup.XmlnsDefinitionAttribute>.  
  
## Espaces de noms concepteurs et autres préfixes de modèles XAML  
 Si vous travaillez avec les environnements de développement et\/ou les outils de conception pour XAML WPF, vous remarquerez peut\-être qu'il existe d'autres espaces de noms \/ préfixes XAML définis dans le balisage XAML.  
  
 [!INCLUDE[wpfdesigner_current_long](../../../../includes/wpfdesigner-current-long-md.md)] utilise un espace de noms concepteur généralement mappé au préfixe `d:`.  Les modèles de projet plus récents pour WPF peuvent pré\-mapper cet espace de noms XAML pour prendre en charge l'échange du XAML entre [!INCLUDE[wpfdesigner_current_long](../../../../includes/wpfdesigner-current-long-md.md)] et d'autres environnements de design.  Cet espace de noms XAML de design est utilisé pour perpétuer l'état de conception pendant les allers\-retours de l'interface utilisateur basée sur XAML dans le concepteur.  Il est également utilisé pour les fonctionnalités telles que `d:IsDataSource`, qui activent des sources de données du runtime dans un concepteur.  
  
 Un autre préfixe que vous pouvez voir mappé est `mc:`.  `mc:` est destiné à la compatibilité du balisage et exploite un modèle de compatibilité du balisage qui n'est pas nécessairement spécifique au XAML.  Dans une certaine mesure, les fonctionnalités de compatibilité du balisage peuvent être utilisées pour échanger XAML entre les infrastructures ou à travers d'autres limites d'implémentation de stockage, pour travailler entre des contextes de schémas XAML, et fournir la compatibilité pour des modes limités dans les concepteurs, et ainsi de suite.  Pour plus d'informations sur les concepts de compatibilité du balisage et leurs rapports à WPF, consultez [Fonctionnalités de langage pour la compatibilité du balisage \(mc:\)](../../../../docs/framework/wpf/advanced/markup-compatibility-mc-language-features.md).  
  
## WPF et chargement des assemblys  
 Le contexte de schéma XAML pour WPF s'intègre avec le modèle d'application WPF, qui à son tour utilise le concept défini par CLR de <xref:System.AppDomain>.  La séquence suivante décrit comment le contexte de schéma XAML interprète la manière de charger les assemblys ou de rechercher les types au moment de l'exécution ou de la conception, selon l'utilisation WPF de <xref:System.AppDomain> et d'autres facteurs.  
  
1.  Itérez par <xref:System.AppDomain>, en recherchant un assembly déjà chargé qui correspond à tous les aspects du nom en commençant par l'assembly le plus récemment chargé.  
  
2.  Si le nom est qualifié, appelez <xref:System.Reflection.Assembly.Load%28System.String%29?displayProperty=fullName> sur le nom qualifié.  
  
3.  Si le nom court \+ le jeton de clé publique d'un nom qualifié correspond à l'assembly que le balisage à partir duquel le balisage a été chargé, retournez cet assembly.  
  
4.  Utilisez la combinaison nom court \+ jeton de clé publique pour appeler <xref:System.Reflection.Assembly.Load%28System.String%29?displayProperty=fullName>.  
  
5.  Si le nom est non qualifié, appelez <xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=fullName>.  
  
 Le XAML libre n'utilise pas l'étape 3 ; il n'y a pas de chargement depuis l'assembly.  
  
 Le XAML compilé pour WPF \(généré par l'intermédiaire de XamlBuildTask\) n'utilise pas les assemblys déjà chargés de <xref:System.AppDomain> \(étape 1\).  En outre, le nom ne doit jamais être non qualifié de la sortie de XamlBuildTask, donc l'étape 5 ne s'applique pas.  
  
 Le BAML compilé \(généré par l'intermédiaire de PresentationBuildTask\) utilise toutes les étapes, bien que le BAML ne doit également pas contenir de noms d'assembly non qualifiés.  
  
## Voir aussi  
 [Présentation des espaces de noms XML \(page éventuellement en anglais\)](http://go.microsoft.com/fwlink/?LinkId=98069)   
 [Vue d'ensemble du langage XAML \(WPF\)](../../../../docs/framework/wpf/advanced/xaml-overview-wpf.md)