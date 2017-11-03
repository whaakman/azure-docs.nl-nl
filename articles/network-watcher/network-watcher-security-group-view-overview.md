---
title: Inleiding tot security groep weergeven in Azure-netwerk-Watcher | Microsoft Docs
description: Deze pagina bevat een overzicht van de mogelijkheid tot de weergave van netwerk-Watcher
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: jdial
ms.openlocfilehash: f4175875b68c52e68588b8d0debd003ab73427ec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-network-security-group-view-in-azure-network-watcher"></a>Inleiding tot het netwerk beveiliging groep weergeven in Azure-netwerk-Watcher

Netwerkbeveiligingsgroepen zijn gekoppeld op het subnetniveau van een of een NIC-niveau. Als gekoppeld op het subnetniveau van een, geldt dit voor alle VM-instanties in het subnet. Weergave van de Netwerkbeveiligingsgroep retourneert de geconfigureerde nsg's en regels die zijn gekoppeld aan een NIC en het subnetmasker niveau hebben voor een virtuele machine biedt meer informatie over de configuratie. Bovendien worden de regels voor een effectieve beveiligingsmethode voor elk van de NIC's in een virtuele machine geretourneerd. Met behulp van de Netwerkbeveiligingsgroep weergeven, kunt u een virtuele machine op netwerk beveiligingsproblemen zoals open poorten beoordelen. U kunt ook valideren als de Netwerkbeveiligingsgroep werkt zoals verwacht op basis van een [vergelijking tussen de geconfigureerde en de regels voor een effectieve beveiligingsmethode](network-watcher-nsg-auditing-powershell.md).

Er is een meer uitgebreide gebruiksvoorbeeld in security compatibiliteit en controle. U kunt een uitgebreide set beveiligingsregels als model voor beveiliging toezicht definiëren in uw organisatie. Een audit periodieke naleving kan worden geïmplementeerd op een programmatische manier door het vergelijken van de richtlijnen regels met de effectieve regels voor elk van de virtuele machines in uw netwerk.

In de portal regels worden gedeeld door effectieve, Subnet, netwerkinterface en standaard. Dit biedt een eenvoudige weergave in de regels die zijn toegepast op een virtuele machine. Downloadknop is om te gemakkelijk alle beveiligingsregels ongeacht het tabblad downloaden naar een CSV-bestand opgegeven.

![beveiliging groep weergeven][1]

Regels kunnen worden geselecteerd en een nieuwe blade wordt geopend om weer te geven van de Netwerkbeveiligingsgroep en de bron- en doelserver voorvoegsels. U kunt via deze blade navigeren rechtstreeks aan de Netwerkbeveiligingsgroep-resource.

![DrillDown][2]

### <a name="next-steps"></a>Volgende stappen

Meer informatie over het controleren van de instellingen van de Netwerkbeveiligingsgroep in via [Netwerkbeveiligingsgroep controle-instellingen met PowerShell](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









