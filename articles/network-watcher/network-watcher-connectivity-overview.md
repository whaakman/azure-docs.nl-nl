---
title: Inleiding tot Azure-netwerk-Watcher verbinding oplossen | Microsoft Docs
description: Deze pagina bevat een overzicht van de netwerk-Watcher verbinding voor probleemoplossing mogelijkheid
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.openlocfilehash: 0268c7e54aa82df12243f98fd72de836fbc82070
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
ms.locfileid: "30833996"
---
# <a name="introduction-to-connection-troubleshoot-in-azure-network-watcher"></a>Inleiding tot verbinding oplossen in de Azure-netwerk-Watcher

Het oplossen van de verbinding van netwerk-Watcher biedt de mogelijkheid om te controleren van een directe TCP-verbinding van een virtuele machine aan een virtuele machine (VM), volledig gekwalificeerde domeinnaam (FQDN), URI, of IPv4-adres. Scenario's met netwerken complex zijn, worden geïmplementeerd met behulp van netwerkbeveiligingsgroepen, firewalls, gebruiker gedefinieerde routes en resources die worden geleverd door Azure. Complexe configuraties maakt met het oplossen van verbindingsproblemen moeilijker. Netwerk-Watcher reduceert de hoeveelheid tijd om te zoeken en verbindingsproblemen detecteren. De resultaten biedt mogelijk inzicht in of een verbindingsprobleem veroorzaakt door een platform of een probleem met de gebruiker wordt. Connectiviteit kan worden gecontroleerd met [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md), en [REST-API](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Het oplossen van de verbinding vereist dat de virtuele machine die u wilt van oplossen de `AzureNetworkWatcherExtension` VM-extensie is geïnstalleerd. Voor het installeren van de extensie op een Windows-virtuele machine gaat u naar [extensie voor het virtuele machine voor Windows Azure-netwerk-Watcher Agent](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en voor Linux-VM naar [Azure-netwerk-Watcher Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). De extensie is niet vereist op de doeleindpunt.

## <a name="response"></a>Antwoord

De volgende tabel bevat de eigenschappen geretourneerd wanneer de verbinding oplossen is voltooid.

|Eigenschap  |Beschrijving  |
|---------|---------|
|ConnectionStatus     | De status van de controle van de verbinding. Mogelijke resultaten zijn **bereikbaar** en **onbereikbaar**.        |
|AvgLatencyInMs     | Gemiddelde latentie tijdens de controle connectiviteit in milliseconden. (Alleen weergegeven als de status bereikbaar is)        |
|MinLatencyInMs     | Minimale latentie tijdens de controle connectiviteit in milliseconden. (Alleen weergegeven als de status bereikbaar is)        |
|MaxLatencyInMs     | Maximale duur geldt tijdens de controle connectiviteit in milliseconden. (Alleen weergegeven als de status bereikbaar is)        |
|ProbesSent     | Aantal tests die worden verzonden tijdens de controle. De maximale waarde is 100.        |
|ProbesFailed     | Aantal tests die zijn mislukt bij de controle. De maximale waarde is 100.        |
|Hops     | Hop door hop pad van bron naar doel.        |
|Hops[].Type     | Type resource. Mogelijke waarden zijn **bron**, **VirtualAppliance**, **VnetLocal**, en **Internet**.        |
|Hops[].Id | De unieke id van de hop.|
|Hops[].Address | IP-adres van de hop.|
|Hops[].ResourceId | De ResourceID van de hop als de hop een Azure-resource is. Als het een internet-bron, ResourceID is **Internet**. |
|Hops[].NextHopIds | De unieke id van de volgende hop genomen.|
|Hops[].Issues | Een verzameling van problemen die zijn opgetreden tijdens de controle op die hop. Als er geen problemen zijn, wordt de waarde leeg is.|
|Hops[].Issues[].Origin | Op de huidige hop waar probleem is opgetreden. Mogelijke waarden zijn:<br/> **Inkomende** -probleem is op de koppeling van de vorige hop naar de huidige hop<br/>**Uitgaande** -probleem is op de koppeling van de huidige hop naar de volgende hop<br/>**Lokale** -probleem is bij de huidige hop.|
|Hops[].Issues[].Severity | De ernst van het gedetecteerde probleem. Mogelijke waarden zijn **fout** en **waarschuwing**. |
|Hops[].Issues[].Type |Het type van het probleem gevonden. Mogelijke waarden zijn: <br/>**CPU**<br/>**Geheugen**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|Hops[].Issues[].Context |Gegevens met betrekking tot het probleem dat is gevonden.|
|Hops[].Issues[].Context[].key |Sleutel van de sleutel-waardepaar geretourneerd.|
|Hops[].Issues[].Context[].value |Waarde van de sleutel-waardepaar geretourneerd.|

Hier volgt een voorbeeld van een probleem gevonden bij een hop.

```json
"Issues": [
    {
        "Origin": "Outbound",
        "Severity": "Error",
        "Type": "NetworkSecurityRule",
        "Context": [
            {
                "key": "RuleName",
                "value": "UserRule_Port80"
            }
        ]
    }
]
```
## <a name="fault-types"></a>Fout-typen

Verbinding oplossen retourneert fout typen over de verbinding. De volgende tabel bevat een lijst van de huidige fout typen geretourneerd.

|Type  |Beschrijving  |
|---------|---------|
|CPU     | Hoog CPU-verbruik.       |
|Geheugen     | Hoog geheugengebruik.       |
|GuestFirewall     | Verkeer wordt geblokkeerd door een firewall-configuratie van virtuele machine.        |
|DNSResolution     | DNS-omzetting is mislukt voor het doeladres.        |
|NetworkSecurityRule    | Verkeer wordt geblokkeerd door een regel voor het NSG (de regel is geretourneerd)        |
|UserDefinedRoute|Verkeer wordt verwijderd als gevolg van een gebruiker gedefinieerde of systeemroute. |

### <a name="next-steps"></a>Volgende stappen

Meer informatie over het oplossen van problemen met verbindingen met behulp van de [Azure-portal](network-watcher-connectivity-portal.md), [PowerShell](network-watcher-connectivity-powershell.md), wordt de [Azure CLI](network-watcher-connectivity-cli.md), of [REST-API](network-watcher-connectivity-rest.md).