---
title: Inleiding tot topologie in Azure-netwerk-Watcher | Microsoft Docs
description: Deze pagina bevat een overzicht van de mogelijkheden van netwerk-Watcher-topologie
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: e753a435-38e0-482b-846b-121cb547555c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: d5cb5ba431eeae1956a9dbf1d48561c66faef9a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-topology-in-azure-network-watcher"></a>Inleiding tot topologie in Azure-netwerk-Watcher

Topologie retourneert een grafiek van netwerkbronnen in een virtueel netwerk. De grafiek wordt de onderlinge verbinding tussen de bronnen voor de end-to-end-netwerkverbinding.

![overzicht van de topologie][1]

In de portal topologie retourneert de resourceobjecten op een per per virtueel netwerk. De relaties zijn afgebeeld met lijnen tussen de bronnen bronnen buiten het gebied van netwerk-Watcher, zelfs als in de groep niet weergegeven. De resources die worden geretourneerd in de portal weergave zijn een subset van de netwerkonderdelen die diagram worden weergegeven aan. Voor een volledig overzicht van netwerkresources kunt u [PowerShell](network-watcher-topology-powershell.md) of [REST](network-watcher-topology-rest.md)

> [!NOTE]
> Een exemplaar van netwerk-Watcher is vereist in elke regio waarin u uitvoeren van de topologie wilt op.

Resources worden geretourneerd van de verbinding tussen deze twee worden gemodelleerd onder twee-relaties.

- **Containment** -voorbeeld: virtueel netwerk bevat een Subnet met een NIC
- **Gekoppeld** -voorbeeld: een NIC is gekoppeld aan een virtuele machine

### <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik van PowerShell voor het ophalen van de topologie-weergave in via [netwerk-Watcher-topologie met PowerShell](network-watcher-topology-powershell.md)

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
