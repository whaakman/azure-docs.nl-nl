---
title: Azure Containerexemplaren regio en resource beschikbaarheid | Azure Docs
description: Ontdek welke Azure-regio's ondersteuning voor de implementatie van de containerexemplaren en de CPU en geheugen limieten voor ALE exemplaren.
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 2b9b1b864bbfd73383759212dd7d91f8e4941544
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Beschikbaarheid in regio's voor exemplaren van Azure-Container

Tijdens de preview zijn exemplaren van Azure-Container beschikbaar in de volgende regio's met de opgegeven CPU en geheugenlimieten.

| Locatie | OS | CPU | Geheugen (GB) |
| -------- | -- | :---: | :-----------: |
| West-Europa, VS-West, VS-Oost | Linux | 2 | 7 |
| West-Europa, VS-West, VS-Oost | Windows | 2 | 3,5 |

## <a name="resource-availability"></a>Beschikbaarheid van resources

Containerexemplaren gemaakt binnen de grenzen van deze resource zijn onderworpen aan de beschikbaarheid binnen de regio voor de implementatie. Wanneer een regio zwaar wordt belast is, treedt een fout opgetreden bij het implementeren van exemplaren.

Om te beperken die een implementatie is mislukt, probeer exemplaren met lagere CPU en geheugeninstellingen implementeren, of uw implementatie op een later tijdstip.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het oplossen van container exemplaar implementatie [implementatieproblemen oplossen met Azure Containerexemplaren](container-instances-troubleshooting.md).