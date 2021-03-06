---
title: "Comment&#160;: restituer manuellement des graphiques mis en m&#233;moire tampon | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "BufferedGraphics (classe)"
  - "scintillement, réduire en restituant manuellement les graphiques"
  - "graphiques (Windows Forms), rendu"
ms.assetid: 5192295e-bd8e-45f7-8bd6-5c4f6bd21e61
caps.latest.revision: 10
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 10
---
# Comment&#160;: restituer manuellement des graphiques mis en m&#233;moire tampon
Si vous gérez vos propres graphismes mis en mémoire tampon, vous devez pouvoir créer et restituer des mémoires tampon de graphiques.  Vous pouvez créer des instances de la classe <xref:System.Drawing.BufferedGraphics> associée aux surfaces de dessin sur votre écran en appelant la méthode <xref:System.Drawing.BufferedGraphicsContext.Allocate%2A>.  Cette méthode crée une instance de <xref:System.Drawing.BufferedGraphics> associée à une surface de rendu particulière, comme un formulaire ou un contrôle.  Après avoir créé une instance de <xref:System.Drawing.BufferedGraphics>, vous pouvez dessiner des graphismes dans la mémoire tampon qu'elle représente par le biais de la propriété <xref:System.Drawing.BufferedGraphics.Graphics%2A>.  Après avoir effectué toutes les opérations graphiques, vous pouvez copier le contenu de la mémoire tampon à l'écran en appelant la méthode <xref:System.Drawing.BufferedGraphics.Render%2A>.  
  
> [!NOTE]
>  Si vous effectuez votre propre rendu, la consommation de mémoire augmentera, bien que cette augmentation puisse être très légère.  
  
### Pour afficher manuellement des graphismes mis en mémoire tampon  
  
1.  Obtenez une référence à une instance de la classe <xref:System.Drawing.BufferedGraphicsContext>.  Pour plus d'informations, consultez [Comment : gérer manuellement des graphiques mis en mémoire tampon](../../../../docs/framework/winforms/advanced/how-to-manually-manage-buffered-graphics.md).  
  
2.  Créez une instance de la classe <xref:System.Drawing.BufferedGraphics> en appelant la méthode <xref:System.Drawing.BufferedGraphicsContext.Allocate%2A>, comme indiqué dans l'exemple de code suivant.  
  
     [!code-csharp[System.Windows.Forms.LegacyBufferedGraphics#21](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/CS/Class1.cs#21)]
     [!code-vb[System.Windows.Forms.LegacyBufferedGraphics#21](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/VB/Class1.vb#21)]  
  
3.  Dessinez des graphismes dans la mémoire tampon de graphiques en définissant la propriété <xref:System.Drawing.BufferedGraphics.Graphics%2A>.  Par exemple :  
  
     [!code-csharp[System.Windows.Forms.LegacyBufferedGraphics#22](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/CS/Class1.cs#22)]
     [!code-vb[System.Windows.Forms.LegacyBufferedGraphics#22](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/VB/Class1.vb#22)]  
  
4.  Quand vous avez terminé toutes vos opérations de dessin dans la mémoire tampon de graphiques, appelez la méthode <xref:System.Drawing.BufferedGraphics.Render%2A> pour restituer la mémoire tampon, soit sur la surface de dessin associée à cette mémoire tampon, soit sur une surface de dessin spécifiée, comme illustré dans l'exemple de code suivant.  
  
     [!code-csharp[System.Windows.Forms.LegacyBufferedGraphics#23](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/CS/Class1.cs#23)]
     [!code-vb[System.Windows.Forms.LegacyBufferedGraphics#23](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/VB/Class1.vb#23)]  
  
5.  Quand vous avez terminé d'afficher les graphismes, appelez la méthode `Dispose` sur l'instance de <xref:System.Drawing.BufferedGraphics> pour libérer les ressources système.  
  
     [!code-csharp[System.Windows.Forms.LegacyBufferedGraphics#24](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/CS/Class1.cs#24)]
     [!code-vb[System.Windows.Forms.LegacyBufferedGraphics#24](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/VB/Class1.vb#24)]  
  
## Voir aussi  
 <xref:System.Drawing.BufferedGraphicsContext>   
 <xref:System.Drawing.BufferedGraphics>   
 [Graphiques mis deux fois en mémoire tampon](../../../../docs/framework/winforms/advanced/double-buffered-graphics.md)   
 [Comment : gérer manuellement des graphiques mis en mémoire tampon](../../../../docs/framework/winforms/advanced/how-to-manually-manage-buffered-graphics.md)