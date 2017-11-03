---
title: Inleiding tot connectiviteit controleren in de Azure-netwerk-Watcher | Microsoft Docs
description: Deze pagina bevat een overzicht van de mogelijkheid van netwerk-Watcher-connectiviteit
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.openlocfilehash: 16ceef9c923b6a933a5caf752991b466346e0ebc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-connectivity-check-in-azure-network-watcher"></a>Inleiding tot connectiviteit controle in de Azure-netwerk-Watcher

De connectiviteit van netwerk-Watcher biedt de mogelijkheid om te controleren van een directe TCP-verbinding van een virtuele machine aan een virtuele machine (VM), volledig gekwalificeerde domeinnaam (FQDN), URI, of IPv4-adres. Scenario's met netwerken complex zijn, worden geïmplementeerd met behulp van Netwerkbeveiligingsgroepen, firewalls, gebruiker gedefinieerde routes en resources die worden geleverd door Azure. Complexe configuraties maakt met het oplossen van verbindingsproblemen moeilijker. Netwerk-Watcher reduceert de hoeveelheid tijd om te zoeken en verbindingsproblemen detecteren. De resultaten biedt mogelijk inzicht in of een verbindingsprobleem veroorzaakt door een platform of een probleem met de gebruiker wordt. Connectiviteit kan worden gecontroleerd met [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md), en [REST-API](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Controle van de verbinding zijn nodig voor de extensie van een virtuele machine `AzureNetworkWatcherExtension`. Voor het installeren van de extensie op een Windows-virtuele machine gaat u naar [extensie voor het virtuele machine voor Windows Azure-netwerk-Watcher Agent](../virtual-machines/windows/extensions-nwa.md) en voor Linux-VM naar [Azure-netwerk-Watcher Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="response"></a>Antwoord

De volgende tabel ziet u de eigenschappen geretourneerd bij een controle van de verbinding is voltooid.

|Eigenschap  |Beschrijving  |
|---------|---------|
|ConnectionStatus     | De status van de controle van de verbinding. Mogelijke resultaten zijn **bereikbaar** en **onbereikbaar**.        |
|AvgLatencyInMs     | Gemiddelde latentie tijdens de controle connectiviteit in milliseconden. (Alleen weergegeven als de status bereikbaar is)        |
|MinLatencyInMs     | Minimale latentie tijdens de controle connectiviteit in milliseconden. (Alleen weergegeven als de status bereikbaar is)        |
|MaxLatencyInMs     | Maximale duur geldt tijdens de controle connectiviteit in milliseconden. (Alleen weergegeven als de status bereikbaar is)        |
|ProbesSent     | Aantal tests die worden verzonden tijdens de controle. De maximale waarde is 100.        |
|ProbesFailed     | Aantal tests die zijn mislukt bij de controle. De maximale waarde is 100.        |
|Hops     | Hop door hop pad van bron naar doel.        |
|[] Hops. Type     | Type resource. Mogelijke waarden zijn **bron**, **VirtualAppliance**, **VnetLocal**, en **Internet**.        |
|[] Hops. ID | De unieke id van de hop.|
|[] Hops. Adres | IP-adres van de hop.|
|[] Hops. ResourceId | De ResourceID van de hop als de hop een Azure-resource is. Als het een internet-bron, ResourceID is **Internet**. |
|[] Hops. NextHopIds | De unieke id van de volgende hop genomen.|
|[] Hops. Problemen | Een verzameling van problemen die zijn opgetreden tijdens de controle op die hop. Als er geen problemen zijn, wordt de waarde leeg is.|
|[] Hops. [] Verstrekt. Oorsprong | Op de huidige hop waar probleem is opgetreden. Mogelijke waarden zijn:<br/> **Inkomende** -probleem is op de koppeling van de vorige hop naar de huidige hop<br/>**Uitgaande** -probleem is op de koppeling van de huidige hop naar de volgende hop<br/>**Lokale** -probleem is bij de huidige hop.|
|[] Hops. [] Verstrekt. Ernst | De ernst van het gedetecteerde probleem. Mogelijke waarden zijn **fout** en **waarschuwing**. |
|[] Hops. [] Verstrekt. Type |Het type van het probleem gevonden. Mogelijke waarden zijn: <br/>**CPU**<br/>**Geheugen**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|[] Hops. [] Verstrekt. Context |Gegevens met betrekking tot het probleem dat is gevonden.|
|[] Hops. [] Verstrekt. Context [] .key |Sleutel van de sleutel-waardepaar geretourneerd.|
|[] Hops. [] Verstrekt. Context [] .value |Waarde van de sleutel-waardepaar geretourneerd.|

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

De controle van de connectiviteit retourneert fout typen over de verbinding. De volgende tabel bevat een lijst van de huidige fout typen geretourneerd.

|Type  |Beschrijving  |
|---------|---------|
|CPU     | Hoog CPU-verbruik.       |
|Geheugen     | Hoog geheugengebruik.       |
|GuestFirewall     | Verkeer wordt geblokkeerd door een firewall-configuratie van virtuele machine.        |
|DNSResolution     | DNS-omzetting is mislukt voor het doeladres.        |
|NetworkSecurityRule    | Verkeer wordt geblokkeerd door een regel voor het NSG (de regel is geretourneerd)        |
|UserDefinedRoute|Verkeer wordt verwijderd als gevolg van een gebruiker gedefinieerde of systeemroute. |

### <a name="next-steps"></a>Volgende stappen

Meer informatie over het controleren van de verbinding met een resource in via: [Controleer de verbinding met Azure-netwerk-Watcher](network-watcher-connectivity-powershell.md).

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png

