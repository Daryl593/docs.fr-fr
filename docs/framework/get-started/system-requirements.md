---
title: "Configuration requise pour le .NET Framework"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- software requirements
- .NET Framework, system requirements
- system requirements
- operating systems supported
- hardware requirements
ms.assetid: 298275e2-da1d-4618-9f74-6a3567832350
caps.latest.revision: 95
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 21c6a1485f3d0c38bde065d6ecc7b07d5e424c1d
ms.openlocfilehash: 18875105836ee4be0a3d3fb61a83ef6cd29b8c58
ms.contentlocale: fr-fr
ms.lasthandoff: 08/05/2017

---

# <a name="net-framework-system-requirements"></a>Configuration requise pour le .NET Framework

Les tableaux de cette rubrique indiquent la configuration matérielle et logicielle requise, ainsi que la configuration du système d’exploitation, pour le .NET Framework 4.5 et ses versions intermédiaires (4.5.1 et 4.5.2), le [!INCLUDE[net_v46](../../../includes/net-v46-md.md)] et ses versions intermédiaires (4.6.1 et 4.6.2) ainsi que pour le .NET Framework 4.7. Les environnements de développement qui vous permettent de développer des applications pour le .NET Framework ont des exigences différentes.

Pour obtenir des informations et des liens pour le téléchargement, consultez [Installer le .NET Framework pour les développeurs](../../../docs/framework/install/guide-for-developers.md).

Pour plus d’informations sur le cycle de vie du support des versions du .NET Framework, consultez [Cycle de vie de support des produits Microsoft](https://support.microsoft.com/en-us/lifecycle/search?sort=PN&alpha=Microsoft%20.NET%20Framework&Filter=FilterNO).

## <a name="hardware-requirements"></a>Configuration matérielle requise

|                          |        |
| ------------------------ | ------ |
| **Processeur**            | 1 GHz  |
| **Mémoire vive**                  | 512 Mo |
| **Espace disque (minimum)** |        |
| 32 bits                   | 4,5 Go |
| 64 bits                   | 4,5 Go |

## <a name="installation-requirements"></a>Configuration requise pour l’installation

L’installation du .NET Framework nécessite des privilèges d’administrateur. Si vous n’avez pas de droits d’administrateur sur l’ordinateur sur lequel vous voulez installer le .NET Framework, contactez votre administrateur réseau.

## <a name="supported-client-operating-systems"></a>Systèmes d'exploitation clients pris en charge

| Système d'exploitation | Éditions prises en charge | Préinstallé avec le système d'exploitation | Installable séparément |
| ---------------- | ------------------ | ------------------------ | ---------------------- |
| Windows 10 Creators Update | 32 bits et 64 bits | .NET Framework 4.7 | | 
| Mise à jour anniversaire Windows 10 | 32 bits et 64 bits | [!INCLUDE[net_v462](../../../includes/net-v462-md.md)]|.NET Framework 4.7 |
| Mise à jour de novembre de Windows 10 | 32 bits et 64 bits | [!INCLUDE[net_v461](../../../includes/net-v461-md.md)] | |
| Windows 10 | 32 bits et 64 bits | [!INCLUDE[net_v46](../../../includes/net-v46-md.md)] | [!INCLUDE[net_v461](../../../includes/net-v461-md.md)] |
| [!INCLUDE[win81](../../../includes/win81-md.md)] | 32 bits, 64 bits et ARM | [!INCLUDE[net_v451](../../../includes/net-v451-md.md)] | [!INCLUDE[net_v452](../../../includes/net-v452-md.md)]<br /><br /> [!INCLUDE[net_v46](../../../includes/net-v46-md.md)]<br /><br /> [!INCLUDE[net_v461](../../../includes/net-v461-md.md)]<br /><br /> [!INCLUDE[net_v462](../../../includes/net-v462-md.md)]<br /><br />.NET Framework 4.7 |
| [!INCLUDE[win8](../../../includes/win8-md.md)] | 32 bits, 64 bits et ARM | [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] | [!INCLUDE[net_v451](../../../includes/net-v451-md.md)]<br /><br /> [!INCLUDE[net_v452](../../../includes/net-v452-md.md)]<br /><br /> [!INCLUDE[net_v46](../../../includes/net-v46-md.md)]<br /><br /> [!INCLUDE[net_v461](../../../includes/net-v461-md.md)] |
| Windows 7 SP1|32 bits et 64 bits | -- | .NET Framework 4<br /><br /> [!INCLUDE[net_v45](../../../includes/net-v45-md.md)]<br /><br /> [!INCLUDE[net_v451](../../../includes/net-v451-md.md)]<br /><br /> [!INCLUDE[net_v452](../../../includes/net-v452-md.md)]<br /><br /> [!INCLUDE[net_v46](../../../includes/net-v46-md.md)]<br /><br /> [!INCLUDE[net_v461](../../../includes/net-v461-md.md)]<br /><br /> [!INCLUDE[net_v462](../../../includes/net-v462-md.md)]<br /><br />.NET Framework 4.7 |
| Windows Vista SP2|32 bits et 64 bits | -- | .NET Framework 4<br /><br /> [!INCLUDE[net_v45](../../../includes/net-v45-md.md)]<br /><br /> [!INCLUDE[net_v451](../../../includes/net-v451-md.md)]<br /><br /> [!INCLUDE[net_v452](../../../includes/net-v452-md.md)]<br /><br /> [!INCLUDE[net_v46](../../../includes/net-v46-md.md)] |
| Windows XP |32 bits et 64 bits | -- | .NET Framework 4 |

 **Remarques :**

- Sur les systèmes Windows 7, le .NET Framework nécessite Windows 7 SP1. Si vous êtes sur Windows 7 et que vous n’avez pas encore installé le Service Pack 1, vous devez le faire avant d’installer le .NET Framework.

- Le [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] est pris en charge sur l'environnement Windows PE (Windows Preinstallation Environment). Les fonctionnalités ne sont pas toutes prises en charge dans Windows PE.

- .NET Framework 4 prend également en charge la plateforme IA64.

- Pour toutes les plateformes, nous recommandons d’effectuer une mise à niveau vers le dernier Service Pack Windows et d’installer les mises à jour critiques disponibles à partir du [site web Windows Update](http://go.microsoft.com/fwlink/?LinkId=168461) afin de garantir une compatibilité et une sécurité optimales.

- Sur les systèmes d'exploitation 64 bits, le .NET Framework prend en charge WOW64 (processus 32 bits sur un ordinateur 64 bits) et le processus 64 bits natif.

## <a name="supported-server-operating-systems"></a>Systèmes d'exploitation serveurs pris en charge

| Système d'exploitation | Éditions prises en charge | Préinstallé avec le système d'exploitation | Installable séparément |
| ---------------- | ------------------ | ------------------------ | ---------------------- |
| Windows Server 2016 | 64 bits | [!INCLUDE[net_v462](../../../includes/net-v462-md.md)] | .NET Framework 4.7 |
| [!INCLUDE[winblue_server_2](../../../includes/winblue-server-2-md.md)] | 64 bits | [!INCLUDE[net_v451](../../../includes/net-v451-md.md)] | [!INCLUDE[net_v452](../../../includes/net-v452-md.md)]<br /><br /> [!INCLUDE[net_v46](../../../includes/net-v46-md.md)]<br /><br /> [!INCLUDE[net_v461](../../../includes/net-v461-md.md)]<br /><br /> [!INCLUDE[net_v462](../../../includes/net-v462-md.md)]<br /><br />.NET Framework 4.7 |
| [!INCLUDE[winserver8](../../../includes/winserver8-md.md)] (édition 64 bits) | 64 bits| [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] | [!INCLUDE[net_v451](../../../includes/net-v451-md.md)]<br /><br /> [!INCLUDE[net_v452](../../../includes/net-v452-md.md)]<br /><br /> [!INCLUDE[net_v46](../../../includes/net-v46-md.md)]<br /><br /> [!INCLUDE[net_v461](../../../includes/net-v461-md.md)]<br /><br /> [!INCLUDE[net_v462](../../../includes/net-v462-md.md)]<br /><br />.NET Framework 4.7 |
| Windows Server 2008 R2 SP1|64 bits | -- | .NET Framework 4<br /><br /> [!INCLUDE[net_v45](../../../includes/net-v45-md.md)]<br /><br /> [!INCLUDE[net_v451](../../../includes/net-v451-md.md)]<br /><br /> [!INCLUDE[net_v452](../../../includes/net-v452-md.md)]<br /><br /> [!INCLUDE[net_v46](../../../includes/net-v46-md.md)]<br /><br /> [!INCLUDE[net_v461](../../../includes/net-v461-md.md)]<br /><br /> [!INCLUDE[net_v462](../../../includes/net-v462-md.md)]<br /><br />.NET Framework 4.7 |
| Windows Server 2008 SP2|32 bits et 64 bits | -- | .NET Framework 4<br /><br /> [!INCLUDE[net_v45](../../../includes/net-v45-md.md)]<br /><br /> [!INCLUDE[net_v451](../../../includes/net-v451-md.md)]<br /><br /> [!INCLUDE[net_v452](../../../includes/net-v452-md.md)]<br /><br /> [!INCLUDE[net_v46](../../../includes/net-v46-md.md)] |

 **Remarques :**

- [!INCLUDE[winserver8](../../../includes/winserver8-md.md)] inclut le [!INCLUDE[net_v45](../../../includes/net-v45-md.md)], il n'est donc pas nécessaire de l'installer séparément. De même, [!INCLUDE[winblue_server_2](../../../includes/winblue-server-2-md.md)] inclut le [!INCLUDE[net_v451](../../../includes/net-v451-md.md)].

- Le .NET Framework est pris en charge dans le rôle principal du serveur avec Windows Server 2008 R2 SP1 ou version ultérieure, mais n’est pas pris en charge sur Windows Server 2008 R2 pour les systèmes Itanium.

- Sur Windows Server 2008 SP2, le .NET Framework n’est pas pris en charge dans le rôle principal du serveur.

- Pour toutes les plateformes, nous recommandons une mise à niveau vers le dernier Service Pack Windows et le téléchargement des mises à jour critiques disponibles depuis le [site web Windows Update](http://go.microsoft.com/fwlink/?LinkId=168461) afin de garantir une compatibilité et une sécurité optimales. L'installation du dernier Service Pack Windows peut être obligatoire sur certains systèmes d'exploitation.

- Sur les systèmes d'exploitation 64 bits, le .NET Framework prend en charge WOW64 (processus 32 bits sur un ordinateur 64 bits) et le processus 64 bits natif.

## <a name="see-also"></a>Voir aussi

[Guide d’installation](../../../docs/framework/install/index.md)   
[Bien démarrer](../../../docs/framework/get-started/index.md)   
[Résolution des problèmes liés aux installations et désinstallations bloquées du .NET Framework](../../../docs/framework/install/troubleshoot-blocked-installations-and-uninstallations.md)

