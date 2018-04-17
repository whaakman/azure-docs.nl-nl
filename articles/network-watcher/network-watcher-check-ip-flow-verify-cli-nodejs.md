---
title: Controleer of u verkeer met de Azure-netwerk-Watcher IP-stromen verifiëren - Azure CLI | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt controleren als verkeer naar of van een virtuele machine wordt toegestaan of geweigerd met Azure CLI
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: 92b857ed-c834-4c1b-8ee9-538e7ae7391d
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 8681118e55d8ddddf17ebac3bae63486446c70e9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Controleer als verkeer wordt toegestaan of geweigerd naar of van een virtuele machine met het IP-stromen controleren of een onderdeel van Azure-netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST-API](network-watcher-check-ip-flow-verify-rest.md)


IP-stromen controleren is een functie van netwerk-Watcher die u controleren kunt of er verkeer is toegestaan naar of van een virtuele machine. Dit scenario is nuttig voor het ophalen van de huidige status of een virtuele machine contact met een externe bron of de back-end opnemen kunt. IP-stroom controleren om te controleren of als uw regels Netwerkbeveiligingsgroep (NSG) juist zijn geconfigureerd en problemen oplossen stromen die worden geblokkeerd door het NSG-regels kunnen worden gebruikt. Een andere reden voor het gebruik van IP-stroom controleren om ervoor te zorgen verkeer dat u blokkeren wilt is goed door het NSG worden geblokkeerd.

Dit artikel wordt platformoverschrijdende Azure CLI 1.0 gebruikt die beschikbaar is voor Windows, Mac en Linux.

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher of een bestaand exemplaar van netwerk-Watcher hebben. Het scenario wordt ervan uitgegaan dat er een resourcegroep met een geldige virtuele machine bestaat om te worden gebruikt.

## <a name="scenario"></a>Scenario

Dit scenario maakt gebruik van IP-stromen controleren om te controleren of als een virtuele machine naar een bekende Bing IP-adres kan communiceren. Als het verkeer wordt geweigerd, wordt de beveiligingsregel die dat verkeer wordt geweigerd. Voor meer informatie over IP-stromen controleren, gaat u naar [overzicht van IP-stromen controleren](network-watcher-ip-flow-verify-overview.md)


## <a name="get-a-vm"></a>Een virtuele machine ophalen

IP-stroom controleren tests verkeer naar of vanuit een IP-adres op een virtuele machine naar of vanuit een externe bestemming. Een Id van een virtuele machine is vereist voor de cmdlet. Als u de ID van de virtuele machine wilt gebruiken al weet, kunt u deze stap overslaan.

```
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="get-the-nics"></a>Ophalen van de NIC 's

Het IP-adres van een NIC op de virtuele machine is vereist. De NIC's voor een virtuele machine met de volgende opdracht worden opgehaald. Als u de IP-adres dat u wilt testen op de virtuele machine al weet, kunt u deze stap overslaan.

```
azure network nic show -g resourceGroupName -n nicName
```

## <a name="run-ip-flow-verify"></a>Voer IP-stroom controleren

Voer de `network watcher ip-flow-verify` cmdlet voor het testen van het verkeer. In dit voorbeeld wordt het eerste IP-adres van de eerste NIC gebruikt:

```
azure network watcher ip-flow-verify -g resourceGroupName -n networkWatcherName -t targetResourceId -d directionInboundorOutbound -p protocolTCPorUDP -o localPort -m remotePort -l localIpAddr -r remoteIpAddr
```

> [!NOTE]
> IP-stromen controleren vereist dat de VM-resource wordt toegewezen om te worden uitgevoerd.

## <a name="review-results"></a>Resultaten bekijken

Nadat u `network watcher ip-flow-verify` de resultaten worden geretourneerd, is het volgende voorbeeld de resultaten van de vorige stap.

```
data:    Access                          : Deny
data:    Rule Name                       : defaultSecurityRules/DefaultInboundDenyAll
info:    network watcher ip-flow-verify command OK
```

## <a name="next-steps"></a>Volgende stappen

Zie als verkeer wordt geblokkeerd en mag geen [Netwerkbeveiligingsgroepen beheren](../virtual-network/manage-network-security-group.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die zijn gedefinieerd.

Informatie over het controleren van de instellingen van uw NSG in via [controle Netwerkbeveiligingsgroep groepen (NSG) met de netwerk-Watcher](network-watcher-nsg-auditing-powershell.md).

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
