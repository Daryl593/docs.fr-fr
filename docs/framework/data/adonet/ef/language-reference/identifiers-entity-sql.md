---
title: "Identificateurs (Entity SQL) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
ms.assetid: d58a5edd-7b5c-48e1-b5d7-a326ff426aa4
caps.latest.revision: 2
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 2
---
# Identificateurs (Entity SQL)
Les identificateurs sont utilisés dans [!INCLUDE[esql](../../../../../../includes/esql-md.md)] pour représenter des alias d'expression de requête, des références de variables, des propriétés d'objets, des fonctions, etc.  [!INCLUDE[esql](../../../../../../includes/esql-md.md)] fournit deux types d'identificateurs : les identificateurs simples et les identificateurs entre guillemets.  
  
## Identificateurs simples  
 Un identificateur simple dans [!INCLUDE[esql](../../../../../../includes/esql-md.md)] est une séquence de caractères alphanumériques et de traits de soulignement.  Le premier caractère de l'identificateur doit être un caractère alphabétique \(a\-z ou A\-Z\).  
  
## Identificateurs entre guillemets  
 Un identificateur entre guillemets est une séquence quelconque de caractères entre crochets \(\[\]\).  Les identificateurs entre guillemets vous permettent de spécifier des identificateurs avec des caractères qui ne sont pas valides dans des identificateurs.  Tous les caractères entre les crochets sont inclus dans l'identificateur, y compris les espaces.  
  
 Un identificateur entre guillemets ne peut pas inclure les caractères suivants :  
  
-   Saut de ligne  
  
-   Retours chariot  
  
-   Tabulations  
  
-   Retour arrière  
  
-   Crochets supplémentaires \(c'est\-à\-dire des crochets situés entre les crochets qui délimitent l'identificateur\)  
  
 Un identificateur entre guillemets peut inclure des caractères Unicode.  
  
 Les identificateurs entre guillemets vous permettent de créer des caractères de nom de propriété qui ne sont pas valides dans les identificateurs, comme l'illustre l'exemple suivant :  
  
 `SELECT c.ContactName AS [Contact Name] FROM customers AS c`  
  
 Vous pouvez également utiliser des identificateurs entre guillemets pour spécifier un identificateur qui est un mot clé réservé [!INCLUDE[esql](../../../../../../includes/esql-md.md)].  Par exemple, si le type `Email` a une propriété nommée "From", vous pouvez éliminer l'ambiguïté liée au mot clé réservé FROM en utilisant des crochets, comme suit :  
  
 `SELECT e.[From] FROM emails AS e`  
  
 Vous pouvez utiliser un identificateur entre guillemets à droite d'un opérateur point \(.\).  
  
 `SELECT t FROM ts as t WHERE t.[property] == 2`  
  
 Pour utiliser le crochet dans un identificateur, ajoutez un crochet supplémentaire.  Dans l'exemple suivant, « `abc]` » est l'identificateur :  
  
 `SELECT t from ts as t WHERE t.[abc]]] == 2`  
  
 Pour connaître la sémantique de comparaison des identificateurs, voir [Jeu de caractères en entrée](../../../../../../docs/framework/data/adonet/ef/language-reference/input-character-set-entity-sql.md).  
  
## Règles d'alias  
 Nous recommandons de spécifier des alias dans les requêtes [!INCLUDE[esql](../../../../../../includes/esql-md.md)] chaque fois que cela est nécessaire, y compris les constructions [!INCLUDE[esql](../../../../../../includes/esql-md.md)] suivantes :  
  
-   Champs d'un constructeur de ligne  
  
-   Éléments dans la clause FROM d'une expression de requête  
  
-   Éléments dans la clause SELECT d'une expression de requête  
  
-   Éléments dans la clause GROUP BY d'une expression de requête  
  
### Alias valides  
 Tous les identificateurs simples et les identificateurs entre guillemets sont des alias valides dans [!INCLUDE[esql](../../../../../../includes/esql-md.md)].  
  
### Génération d'alias  
 Si aucun alias n'est spécifié dans une expression de requête [!INCLUDE[esql](../../../../../../includes/esql-md.md)], [!INCLUDE[esql](../../../../../../includes/esql-md.md)] essaie de générer un alias en fonction des règles simples suivantes :  
  
-   Si l'expression de requête \(pour laquelle l'alias n'est pas spécifié\) est un identificateur simple ou entre guillemets, cet identificateur est utilisé comme alias.  Par exemple,  `ROW(a, [b])`  devient  `ROW(a AS a, [b] AS [b])` ;  
  
-   Si l'expression de requête est une expression plus complexe, mais que le dernier composant de cette expression de requête est un identificateur simple, cet identificateur est utilisé comme alias.  Par exemple,  `ROW(a.a1, b.[b1])`  devient  `ROW(a.a1 AS a1, b.[b1] AS [b1])` ;  
  
 Nous vous recommandons de ne pas utiliser d'alias implicite si vous souhaitez utiliser le nom d'alias ultérieurement.  Chaque fois que des alias \(implicites ou explicites\) sont en conflit ou sont répétés dans la même étendue, il se produit une erreur de compilation.  Un alias implicite passera la compilation même s'il existe un alias explicite ou implicite du même nom.  
  
 Des alias implicites sont générés automatiquement en fonction de l'entrée d'utilisateur.  Par exemple, la ligne de code ci\-dessous générera NAME comme alias pour les deux colonnes et générera ainsi un conflit.  
  
```  
SELECT product.NAME, person.NAME  
```  
  
 La ligne de code ci\-dessous, qui utilise des alias explicites, échouera également.  Toutefois, l'échec sera plus apparent en lisant le code.  
  
```  
SELECT 1 AS X, 2 AS X …  
```  
  
## Règles de portée  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] définit les règles de portée qui déterminent quand des variables particulières sont visibles dans le langage de requête.  Certaines expressions ou instructions introduisent de nouveaux noms.  Les règles de portée déterminent où ces noms peuvent être utilisés, ainsi que quand et où une nouvelle déclaration du même nom qu'une autre peut masquer son prédécesseur.  
  
 Lorsque des noms sont définis dans une requête [!INCLUDE[esql](../../../../../../includes/esql-md.md)], ils sont considérés comme définis dans une étendue.  Une étendue couvre une région entière de la requête.  Toutes les expressions ou les références de nom dans une certaine étendue peuvent voir les noms définis dans cette étendue.  Avant le début d'une étendue et après sa fin, il n'est pas possible de référencer les noms définis dans l'étendue.  
  
 Les étendues peuvent être imbriquées.  Les parties d'[!INCLUDE[esql](../../../../../../includes/esql-md.md)] introduisent de nouvelles étendues qui couvrent des régions entières, et ces régions peuvent contenir d'autres expressions [!INCLUDE[esql](../../../../../../includes/esql-md.md)] qui introduisent également des étendues.  Lorsque des étendues sont imbriquées, des références peuvent être faites aux noms définis dans l'étendue la plus intérieure, qui contient la référence.  Des références peuvent également être faites à tous les noms définis dans toutes les étendues extérieures.  Deux étendues quelconques définies dans une même étendue sont considérées comme des étendues sœurs.  Il n'est pas possible d'effectuer des références à des noms définis dans des étendues sœurs.  
  
 Si un nom déclaré dans une étendue intérieure correspond à un nom déclaré dans une étendue extérieure, les références dans l'étendue intérieure ou dans les étendues déclarées dans cette étendue font référence uniquement au nom nouvellement déclaré.  Le nom dans l'étendue extérieure est masqué.  
  
 Même dans une même étendue, les noms ne peuvent pas être référencés avant d'être définis.  
  
 Des noms globaux peuvent exister dans le cadre de l'environnement d'exécution.  Cela peut inclure les noms de collections ou de variables d'environnement persistantes.  Pour qu'un nom soit global, il doit être déclaré dans l'étendue la plus extérieure.  
  
 Les paramètres ne sont pas inclus dans une étendue.  Comme les références aux paramètres incluent une syntaxe spéciale, les noms des paramètres n'entrent jamais en collision avec d'autres noms dans la requête.  
  
### Expressions de requête  
 Une expression de requête [!INCLUDE[esql](../../../../../../includes/esql-md.md)] introduit une nouvelle étendue.  Les noms définis dans la clause FROM sont introduits dans l'étendue from dans l'ordre de leur apparition, de gauche à droite.  Dans la liste de jointure, les expressions peuvent faire référence aux noms définis précédemment dans la liste.  Les propriétés publiques \(champs, etc.\) des éléments identifiés dans la clause FROM ne sont pas ajoutées à l'étendue from.  Elles doivent toujours être référencées par le nom qualifié par un alias.  En général, toutes les parties de l'expression SELECT sont considérées dans l'étendue from.  
  
 La clause GROUP BY introduit également une nouvelle étendue sœur.  Chaque groupe peut avoir un nom de groupe qui fait référence à la collection d'éléments dans le groupe.  Chaque expression de regroupement introduira également un nouveau nom dans l'étendue de groupe.  De plus, l'agrégat d'imbrication \(ou le groupe nommé\) est également ajouté à l'étendue.  Les expressions de regroupement elles\-mêmes sont incluses dans l'étendue from.  Toutefois, lorsqu'une clause GROUP BY est utilisée, la liste de sélection \(projection\), la clause HAVING et la clause ORDER BY sont considérées incluses dans l'étendue de groupe et non dans l'étendue from.  Les agrégats reçoivent un traitement spécial, tel que cela est décrit dans la liste à puce ci\-dessous.  
  
 Remarques supplémentaires sur les étendues :  
  
-   La liste de sélection peut introduire de nouveaux noms dans l'étendue, dans l'ordre.  Les expressions de projection de droite peuvent faire référence aux noms projetés de gauche.  
  
-   La clause ORDER BY peut faire référence aux noms \(alias\) spécifiés dans la liste de sélection.  
  
-   L'ordre d'évaluation des clauses au sein de l'expression SELECT détermine l'ordre dans lequel les noms sont introduits dans l'étendue.  La clause FROM est évaluée en premier, suivie de la clause WHERE, de la clause GROUP BY, de la clause HAVING, de la clause SELECT et enfin de la clause ORDER BY.  
  
### Gestion des agrégats  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] prend en charge deux formes d'agrégats : les agrégats basés sur les collections et les agrégats basés sur les groupes.  Les agrégats basés sur les collections correspondent à la construction privilégiée dans [!INCLUDE[esql](../../../../../../includes/esql-md.md)], alors que les agrégats basés sur les groupes sont pris en charge pour la compatibilité avec SQL.  
  
 Lors de la résolution d'un agrégat, [!INCLUDE[esql](../../../../../../includes/esql-md.md)] essaie en premier lieu de le traiter comme un agrégat basé sur les collections.  En cas d'échec, [!INCLUDE[esql](../../../../../../includes/esql-md.md)] transforme l'entrée de l'agrégat en une référence à l'agrégat d'imbrication et essaie de résoudre cette nouvelle expression, comme l'illustre l'exemple ci\-dessous.  
  
 `AVG(t.c) becomes AVG(group..(t.c))`  
  
## Voir aussi  
 [Référence Entity SQL](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-reference.md)   
 [Vue d'ensemble d'Entity SQL](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-overview.md)   
 [Jeu de caractères en entrée](../../../../../../docs/framework/data/adonet/ef/language-reference/input-character-set-entity-sql.md)