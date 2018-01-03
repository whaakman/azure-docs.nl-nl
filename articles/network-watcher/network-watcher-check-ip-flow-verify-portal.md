---
title: Controleer of verkeer met Azure-netwerk-Watcher IP-stroom controleren of - Azure-portal | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt controleren als verkeer naar of van een virtuele machine wordt toegestaan of geweigerd
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: e0e3e9a8-70eb-409a-a744-0ce9deb27148
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 8333ccfd1e4cc917c8af4b3006292e43b77ecc7f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Controleer als verkeer wordt toegestaan of geweigerd naar of van een virtuele machine met het IP-stromen controleren of een onderdeel van Azure-netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST-API](network-watcher-check-ip-flow-verify-rest.md)


IP-stroom controleren is een functie van netwerk-Watcher die u controleren kunt of er verkeer is toegestaan naar of van een virtuele machine. De validatie kan worden uitgevoerd voor binnenkomend of uitgaand verkeer. Dit scenario is nuttig voor het ophalen van de huidige status of een virtuele machine contact met een externe resource of een andere bron opnemen kunt. IP-stroom controleren om te controleren of als uw regels Netwerkbeveiligingsgroep (NSG) juist zijn geconfigureerd en problemen oplossen stromen die worden geblokkeerd door het NSG-regels kunnen worden gebruikt. Een andere reden voor het gebruik van IP-stroom controleren om ervoor te zorgen verkeer dat u blokkeren wilt is goed door het NSG worden geblokkeerd.

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher of een bestaand exemplaar van netwerk-Watcher hebben. Het scenario wordt ervan uitgegaan dat er een resourcegroep met een geldige virtuele machine bestaat om te worden gebruikt.

## <a name="scenario"></a>Scenario

Dit scenario maakt gebruik van IP-stromen controleren om te controleren of als een virtuele machine naar een andere computer via poort 443 communiceren kan. Als het verkeer wordt geweigerd, wordt de beveiligingsregel die dat verkeer wordt geweigerd. Voor meer informatie over IP-stromen controleren, gaat u naar [overzicht van IP-stromen controleren](network-watcher-ip-flow-verify-overview.md)

### <a name="run-ip-flow-verify"></a>Voer IP-stroom controleren

Navigeer naar uw netwerk-Watcher en klikt u op **IP-stroom controleren**. Selecteer de virtuele machine en u wilt controleren of het verkeer van netwerkinterface. Voer eventueel aanvullende informatie voor het filteren en klik op **controleren**.

Nadat u op **controleren**, de stroom op basis van de criteria die u hebt opgegeven is ingeschakeld. Het resultaat is een **toegang is toegestaan** of **toegang is geweigerd**. Als de toegang is geweigerd, wordt de Netwerkbeveiligingsgroep (NSG) en beveiliging regel die verkeer blokkeren opgegeven. Als de DOS-verkeer verwacht gedrag is, zijn de regel is geslaagd.

> [!NOTE]
> IP-stroom controleren vereist dat de VM-resource wordt toegewezen.

Als u in de volgende afbeelding zien kunt, wordt het uitgaande HTTPS-verkeer is toegestaan.

![IP-stroom overzicht controleren][1]

Zoals u in de volgende afbeelding, verkeer wordt gewijzigd naar inkomende en de binnenkomende poort 123 gewijzigd. Verkeer wordt nu geweigerd, het bericht 'Toegang geweigerd' is opgegeven samen met de opgegeven groep en beveiliging netwerkbeveiligingsregel die door het verkeer geweigerd.

![IP-stroom resultaten][2]

## <a name="next-steps"></a>Volgende stappen

Zie als verkeer wordt geblokkeerd en mag geen [Netwerkbeveiligingsgroepen beheren](../virtual-network/virtual-network-manage-nsg-arm-portal.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die zijn gedefinieerd.

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













