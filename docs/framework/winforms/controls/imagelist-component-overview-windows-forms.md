---
title: "Vue d&#39;ensemble du composant ImageList (Windows Forms) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "ImageList"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "contrôles de collection, images"
  - "icône"
  - "ImageList (composant Windows Forms), à propos du composant ImageList"
ms.assetid: 7e25d89b-5633-40c1-afc3-82e0e301ffa2
caps.latest.revision: 8
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 8
---
# Vue d&#39;ensemble du composant ImageList (Windows Forms)
Le composant Windows Forms <xref:System.Windows.Forms.ImageList> sert à stocker des images, qui peuvent ensuite être affichées par des contrôles.  Une liste d'images vous permet d'écrire du code pour un catalogue d'images unique et cohérent.  Par exemple, vous pouvez faire pivoter les images affichées par un contrôle <xref:System.Windows.Forms.Button> simplement en modifiant la propriété <xref:System.Windows.Forms.ButtonBase.ImageIndex%2A> ou <xref:System.Windows.Forms.ButtonBase.ImageKey%2A> du bouton.  Vous pouvez aussi associer la même liste d'images à plusieurs contrôles.  Par exemple, si vous utilisez à la fois un contrôle <xref:System.Windows.Forms.ListView> et un contrôle <xref:System.Windows.Forms.TreeView> pour afficher la même liste de fichiers, la modification de l'icône d'un fichier dans la liste d'images provoque l'affichage de la nouvelle icône dans les deux vues.  
  
## Utilisation d'ImageList avec des contrôles  
 Vous pouvez utiliser une liste d'images avec n'importe quel contrôle ayant une propriété `ImageList` ou, dans le cas du contrôle <xref:System.Windows.Forms.ListView>, les propriétés <xref:System.Windows.Forms.ListView.SmallImageList%2A> et <xref:System.Windows.Forms.ListView.LargeImageList%2A>.  Les contrôles qui peuvent être associés à une liste d'images sont, entre autres, les contrôles <xref:System.Windows.Forms.ListView>, <xref:System.Windows.Forms.TreeView>, <xref:System.Windows.Forms.ToolBar>, <xref:System.Windows.Forms.TabControl>, <xref:System.Windows.Forms.Button>, <xref:System.Windows.Forms.CheckBox>, <xref:System.Windows.Forms.RadioButton> et <xref:System.Windows.Forms.Label>.  Pour associer la liste d'images à un contrôle, affectez comme valeur de la propriété `ImageList` du contrôle le nom du composant <xref:System.Windows.Forms.ImageList>.  
  
## Principales propriétés  
 La principale propriété du composant <xref:System.Windows.Forms.ImageList> est <xref:System.Windows.Forms.ImageList.Images%2A>, qui contient les images devant être utilisées par le contrôle associé.  Chaque image est accessible par sa valeur d'index ou par sa clé.  La propriété <xref:System.Windows.Forms.ImageList.ColorDepth%2A> détermine le nombre de couleurs avec lesquelles les images sont affichées.  Les images sont toutes affichées à la même taille, définie par la propriété <xref:System.Windows.Forms.ImageList.ImageSize%2A>.  Les images plus grandes seront ajustées.  
  
 Si vous utilisez [!INCLUDE[vsprvslong](../../../../includes/vsprvslong-md.md)], vous avez accès à une grande bibliothèque d'images standard utilisables dans votre application.  
  
## Voir aussi  
 <xref:System.Windows.Forms.ImageList>   
 [Comment : ajouter ou supprimer des images avec le composant ImageList Windows Forms](../../../../docs/framework/winforms/controls/how-to-add-or-remove-images-with-the-windows-forms-imagelist-component.md)