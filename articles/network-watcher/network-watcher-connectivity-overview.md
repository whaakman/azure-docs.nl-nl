---
title: Inleiding tot Azure Network Watcher-verbinding oplossen | Microsoft Docs
description: Deze pagina biedt een overzicht van de mogelijkheden van Network Watcher-verbinding-probleemoplossing
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: kumud
ms.openlocfilehash: 4b1164c3dedc5d8a2fa02d70f66ff00afe604836
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60532459"
---
# <a name="introduction-to-connection-troubleshoot-in-azure-network-watcher"></a>Inleiding tot verbinding oplossen in Azure Network Watcher

Oplossen van de verbinding van Network Watcher biedt de mogelijkheid om te controleren of een rechtstreekse TCP-verbinding van een virtuele machine aan een virtuele machine (VM), volledig gekwalificeerde domeinnaam (FQDN), URI, of het IPv4-adres. Scenario's met netwerken zijn complexe, ze zijn geïmplementeerd met behulp van netwerkbeveiligingsgroepen, firewalls, gebruiker gedefinieerde routes en verstrekt door Azure-resources. Complexe configuraties maakt het oplossen van verbindingsproblemen moeilijker. Network Watcher reduceert de hoeveelheid tijd om te zoeken en detecteren van problemen met de netwerkverbinding. De geretourneerde resultaten krijgt u inzicht in of een probleem met de netwerkverbinding veroorzaakt door een platform of een probleem met de gebruiker wordt. Connectiviteit kan worden gecontroleerd met [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md), en [REST-API](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Probleemoplossing voor verbindingen vereist dat de virtuele machine bij het oplossen van problemen met van heeft de `AzureNetworkWatcherExtension` VM-extensie is geïnstalleerd. Ga voor het installeren van de extensie op een Windows-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en voor Linux-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). De extensie is niet vereist op het eindpunt van de bestemming.

## <a name="response"></a>Antwoord

De volgende tabel ziet u de eigenschappen geretourneerd bij probleemoplossing voor verbindingen is voltooid.

|Eigenschap  |Description  |
|---------|---------|
|connectionStatus     | De status van de connectiviteitscontrole. Mogelijke resultaten zijn **bereikbaar** en **onbereikbaar**.        |
|AvgLatencyInMs     | Gemiddelde latentie tijdens de controle van de netwerkverbinding in milliseconden. (Alleen weergegeven als de status van de controle bereikbaar is)        |
|MinLatencyInMs     | Minimale latentie tijdens de controle van de netwerkverbinding in milliseconden. (Alleen weergegeven als de status van de controle bereikbaar is)        |
|MaxLatencyInMs     | Maximale latentie tijdens de controle van de netwerkverbinding in milliseconden. (Alleen weergegeven als de status van de controle bereikbaar is)        |
|ProbesSent     | Aantal tests die tijdens de controle worden verzonden. De maximale waarde is 100.        |
|ProbesFailed     | Aantal tests die tijdens de controle is mislukt. De maximale waarde is 100.        |
|Hops     | Hop-by-hop-pad van bron naar bestemming.        |
|Hops[].Type     | Het type resource. Mogelijke waarden zijn **bron**, **VirtualAppliance**, **VnetLocal**, en **Internet**.        |
|Hops[].Id | De unieke id van de hop.|
|Hops[].Address | IP-adres van de hop.|
|Hops[].ResourceId | ResourceID van de hop als de hop een Azure-resource is. Als het een internet-bron, ResourceID is **Internet**. |
|Hops[].NextHopIds | De unieke id van de volgende hop genomen.|
|[] Hops. Problemen met | Een verzameling van problemen die zijn opgetreden tijdens de controle op die hop. Als er geen problemen zijn, wordt de waarde leeg is.|
|[] Hops. Problemen met []. Oorsprong | Op de huidige hop waar probleem is opgetreden. Mogelijke waarden zijn:<br/> **Inkomende** -probleem is op de koppeling van de vorige hop naar de huidige hop<br/>**Uitgaande** -probleem is op de koppeling van de huidige hop naar de volgende hop<br/>**Lokale** -probleem is op de huidige hop.|
|[] Hops. Problemen met []. Ernst | De ernst van het probleem dat is gedetecteerd. Mogelijke waarden zijn **fout** en **waarschuwing**. |
|[] Hops. Problemen met []. Type |Het type probleem gevonden. Mogelijke waarden zijn: <br/>**CPU**<br/>**Geheugen**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|Hops[].Issues[].Context |Als u meer informatie over het probleem gevonden.|
|[] Hops. Problemen met []. Context [] .key |Sleutel van de sleutel-waardepaar geretourneerd.|
|Hops[].Issues[].Context[].value |De waarde van de sleutel-waardepaar geretourneerd.|

Hier volgt een voorbeeld van een probleem gevonden op een hop.

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
## <a name="fault-types"></a>Fout met betrekking tot typen

Probleemoplossing voor verbindingen retourneert fout typen over de verbinding. De volgende tabel geeft een lijst van de huidige fault-typen die zijn geretourneerd.

|Type  |Description  |
|---------|---------|
|CPU     | Hoge CPU-gebruik.       |
|Geheugen     | Hoog geheugengebruik.       |
|GuestFirewall     | Verkeer wordt geblokkeerd vanwege de firewall-configuratie van een virtuele machine.        |
|DNSResolution     | Er is een DNS-omzetting mislukt voor het doeladres.        |
|NetworkSecurityRule    | Verkeer wordt geblokkeerd door een NSG-regel (de regel is geretourneerd)        |
|UserDefinedRoute|Verkeer wordt verwijderd vanwege een door de gebruiker gedefinieerde of systeemroute. |

### <a name="next-steps"></a>Volgende stappen

Informatie over het oplossen van verbindingen met behulp van de [Azure-portal](network-watcher-connectivity-portal.md), [PowerShell](network-watcher-connectivity-powershell.md), wordt de [Azure CLI](network-watcher-connectivity-cli.md), of [REST-API](network-watcher-connectivity-rest.md).