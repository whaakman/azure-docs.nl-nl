---
title: Vinden van de volgende hop met Azure-netwerk-Watcher volgende Hop - Azure-portal | Microsoft Docs
description: In dit artikel wordt beschreven hoe u kunt vinden wat het volgende hoptype is en IP-adres met de volgende Hop met de Azure portal
services: network-watcher
documentationcenter: na
author: georgewallace
manager: jimdial
editor: ''
ms.assetid: 7b459dcf-4077-424e-a774-f7bfa34c5975
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 445ec8c7eeb8dd715d3778b44372d16666da7fb8
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2018
ms.locfileid: "31809971"
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-the-portal"></a>Uitzoeken wat het volgende hoptype gebruikt de mogelijkheid van de volgende Hop in Azure met behulp van de portal netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST-API](network-watcher-check-next-hop-rest.md)

De volgende hop is een functie van netwerk-Watcher die de mogelijkheid get biedt de volgende hoptype en IP-adres op basis van een opgegeven virtuele machine. Deze functie is handig bij het bepalen of een virtuele machine uitgaand verkeer van een gateway, internet of virtuele netwerken om te gaan naar de bestemming passeert.

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher. Het scenario wordt ervan uitgegaan dat er een resourcegroep met een geldige virtuele machine bestaat om te worden gebruikt.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel maakt gebruik van volgende hop voor informatie over het volgende hoptype en IP-adres voor een resource. Voor meer informatie over de volgende Hop, gaat u naar [volgende Hop overzicht](network-watcher-next-hop-overview.md).

U wordt in dit scenario:

* De volgende hop ophalen van een virtuele machine.

## <a name="get-next-hop"></a>Ophalen van de volgende Hop

### <a name="step-1"></a>Stap 1

Navigeer naar uw netwerk-Watcher-resource in de Azure-portal.

### <a name="step-2"></a>Stap 2

Klik op **volgende hop** in het navigatiedeelvenster, selecteer de virtuele machine en de netwerkinterface, invullen van de bron en doel-IP en op de **volgende hop** knop.

> [!NOTE]
> Volgende hop is vereist dat de VM-resource wordt toegewezen om te worden uitgevoerd.

![volgende hop-overzicht][1]

### <a name="step-3"></a>Stap 3

Zodra de taak voltooid is, worden de resultaten worden geleverd. Het IP-adres en het type apparaat dat de volgende hop is, worden weergegeven. De volgende tabel bevat de beschikbare geretourneerde waarden in de portal.

**Volgend Hoptype**

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Geen

Als een aangepaste route is gebruikt voor dit verkeer routeren, wordt de gebruiker gedefinieerde route (UDR) ook met de resultaten weergegeven.

![ophalen van de volgende hop-resultaten][2]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het bekijken van de groep beveiligingsinstellingen van uw netwerk via een programma te bezoeken [NSG controleren met de netwerk-Watcher](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-check-next-hop-portal/figure1.png
[2]: ./media/network-watcher-check-next-hop-portal/figure2.png














