---
title: Inleiding tot de weergave van de beveiligingsgroep in Azure Network Watcher | Microsoft Docs
description: Deze pagina biedt een overzicht van de weergave van Network Watcher beveiligingsmogelijkheid
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: jdial
ms.openlocfilehash: 15f6bd0d7da63924e52db8ec7e2cbb0ee7483f82
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391535"
---
# <a name="introduction-to-network-security-group-view-in-azure-network-watcher"></a>Inleiding tot netwerkbeveiligingsgroep weergeven in Azure Network Watcher

Netwerkbeveiligingsgroepen worden gekoppeld op het subnetniveau van een of op een NIC-niveau. Als dat is gekoppeld op het subnetniveau van een, geldt dit voor alle VM-exemplaren in het subnet. Netwerkbeveiligingsgroep weergeven retourneert de geconfigureerde Netwerkbeveiligingsgroepen en regels die zijn gekoppeld op het niveau van een NIC en het subnet voor een virtuele machine biedt inzicht in de configuratie. Bovendien worden de effectieve beveiligingsregels voor elk van de NIC's in een virtuele machine geretourneerd. Met behulp van Network Security Group weergeven, kunt u een virtuele machine voor netwerk-beveiligingsproblemen, zoals poorten openen beoordelen. U kunt ook controleren of uw Netwerkbeveiligingsgroep werkt zoals verwacht op basis van een [vergelijking tussen de geconfigureerde en de goedgekeurde beveiligingsregels](network-watcher-nsg-auditing-powershell.md).

Er is een meer uitgebreide use-case in beveiliging en controle. U kunt een uitgebreide set beveiligingsregels definiëren als een model voor het beheer van beveiliging in uw organisatie. Een periodieke nalevingscontroles kan worden geïmplementeerd op een programmatische manier door het vergelijken van de prescriptieve regels aan de effectieve regels voor elk van de virtuele machines in uw netwerk.

In de portal voor regels worden gedeeld door effectief, Subnet, netwerkinterface en standaard. Dit biedt een eenvoudige weergave in de regels toegepast op een virtuele machine. Een downloadknop wordt geboden voor het downloaden van de beveiligingsregels, ongeacht het tabblad eenvoudig in een CSV-bestand.

![weergave van de beveiligingsgroep][1]

Regels kunnen worden geselecteerd en een nieuwe blade geopend van de Netwerkbeveiligingsgroep en de bron- en voorvoegsels weer te geven. Vanaf deze blade kunt u rechtstreeks naar de resource Network Security Group navigeren.

![Inzoomen][2]

### <a name="next-steps"></a>Volgende stappen

Meer informatie over het controleren van uw instellingen voor Network Security Group recentst [Netwerkbeveiligingsgroep controle-instellingen met PowerShell](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









