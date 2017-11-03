---
title: Inleiding tot Azure-netwerk-Watcher | Microsoft Docs
description: Deze pagina bevat een overzicht van de netwerk-Watcher-service voor bewaking en visualiseren netwerk verbonden bronnen in Azure
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 14bc2266-99e3-42a2-8d19-bd7257fec35e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.openlocfilehash: eecb20d4a53478471c238018d8fbd5a5f9cb79d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-network-monitoring-overview"></a>Bewaking Azure network-overzicht

Een end-to-end-netwerk in Azure opbouwen klanten door te organiseren en samenstellen van verschillende afzonderlijke netwerkbronnen zoals VNet, ExpressRoute, Application Gateway, Load balancers en meer. Bewaking is beschikbaar op elk van de netwerkbronnen. We verwijzen naar deze bewaking als resource niveau bewaking.

Het end-to-end-netwerk kunt complexe configuraties en interacties tussen resources, het maken van complexe scenario's die bewaking op basis van een scenario via netwerk-Watcher nodig hebben.

In dit artikel bevat informatie over scenario en Broncontrole niveau. Netwerkbewaking in Azure is uitgebreid en bevat informatie over twee hoofdcategorieën:

* [**Netwerk-Watcher** ](#network-watcher) -bewaking op basis van een Scenario met de functies in netwerk-Watcher is opgegeven. Deze service omvat pakketopname, volgende hop, IP-stroom controleren, groep beveiligingsweergave, NSG stroom Logboeken. Scenario niveau bewaking biedt een complete weergave van netwerkbronnen in tegenstelling tot afzonderlijke resource netwerkbewaking.
* [**Broncontrole** ](#network-resource-level-monitoring) -niveau Broncontrole bestaat uit vier onderdelen, diagnostische logboeken, metrische gegevens, het oplossen van problemen en resourcestatus. Al deze functies zijn gebouwd op het niveau van de resource.

## <a name="network-watcher"></a>Network Watcher

Netwerk-Watcher is een regionale service waarmee u kunt bewaken en onderzoeken van de voorwaarden op een netwerk scenario niveau in, en naar Azure. Netwerkcontrole en visualisatie hulpprogramma's beschikbaar met de netwerk-Watcher kunnen u begrijpen, diagnoses stellen en Verkrijg inzicht in uw netwerk in Azure.

Netwerk-Watcher heeft momenteel de volgende mogelijkheden:

* **[Topologie](network-watcher-topology-overview.md)**  -biedt een netwerk niveau weergave voor de verschillende verbindingskabels en koppelingen tussen netwerkbronnen in een resourcegroep.
* **[Variabele pakketopname](network-watcher-packet-capture-overview.md)**  -pakketgegevens naar en vanuit een virtuele machine worden vastgelegd. Geavanceerde filteropties en verfijnd besturingselementen zoals kunnen tijd instellen en de grootte van beperkingen bieden veelzijdigheid. De pakketgegevens kunnen worden opgeslagen in een blob-opslag of op de lokale schijf in de CAP-indeling.
* **[IP-stroom controleren](network-watcher-ip-flow-verify-overview.md)**  -controleert of een pakket wordt toegestaan of geweigerd op basis van stroom informatie 5-tuple pakket parameters (doel-IP, bron-IP, doelpoort, bronpoort en Protocol). Als het pakket is geweigerd door een beveiligingsgroep, wordt de regel en de groep die het pakket geweigerd geretourneerd.
* **[Volgende hop](network-watcher-next-hop-overview.md)**  -bepaalt de volgende hop voor pakketten worden gerouteerd in de Azure-netwerk Fabric, zodat u voor het vaststellen van een gebruiker gedefinieerde routes onjuist geconfigureerd.
* **[Groep beveiligingsweergave](network-watcher-security-group-view-overview.md)**  -opgehaald van de effectieve en toegepaste beveiligingsregels voor verbindingen die worden toegepast op een virtuele machine.
* **[Logboekregistratie NSG Flow](network-watcher-nsg-flow-logging-overview.md)**  -stroom in de logboeken voor Netwerkbeveiligingsgroepen kunt u de logboeken die betrekking hebben op het verkeer dat wordt toegestaan of geweigerd door de beveiligingsregels voor verbindingen in de groep opnemen. De stroom wordt gedefinieerd door de gegevens van een 5-tuple: bron-IP, doel-IP, bronpoort, doelpoort en -Protocol.
* **[Virtuele netwerkgateway en verbinding probleemoplossing](network-watcher-troubleshoot-manage-rest.md)**  -biedt de mogelijkheid om op te lossen virtuele netwerkgateways en verbindingen.
* **[Netwerk-abonnementen](#network-subscription-limits)**  -Hiermee kunt u gebruik van netwerkbronnen op basis van limieten weergeven.
* **[Configureren van diagnostische gegevens logboek](#diagnostic-logs)**  – biedt één of diagnostische logboeken voor netwerkbronnen in een resourcegroep uit te schakelen.
* **[Connectiviteit (Preview)](network-watcher-connectivity-overview.md)**  -controleert of de mogelijkheid tot stand brengen van een directe TCP-verbinding van een virtuele machine naar een opgegeven eindpunt.

### <a name="role-based-access-control-rbac-in-network-watcher"></a>Op rollen gebaseerde toegangsbeheer (RBAC) in de netwerk-Watcher

Netwerk-watcher gebruikt de [gebaseerd toegangsbeheer (RBAC) model](../active-directory/role-based-access-control-what-is.md). De volgende machtigingen zijn vereist voor de netwerk-Watcher. Het is belangrijk om ervoor te zorgen dat de rol die wordt gebruikt voor het initiëren van netwerk-Watcher-API's of het gebruik van netwerk-Watcher van de portal de vereiste toegang heeft.

|Resource| Machtiging|
|---|---| 
|Microsoft.Storage/ |Lezen|
|Microsoft.Authorization/| Lezen| 
|Microsoft.Resources/subscriptions/resourceGroups/| Lezen|
|Microsoft.Storage/storageAccounts/listServiceSas/ | Actie|
|Microsoft.Storage/storageAccounts/listAccountSas/ |Actie|
|Microsoft.Storage/storageAccounts/listKeys/ | Actie|
|Microsoft.Compute/virtualMachines/ |Lezen|
|Microsoft.Compute/virtualMachines/ |Schrijven|
|Microsoft.Compute/virtualMachineScaleSets/ |Lezen|
|Microsoft.Compute/virtualMachineScaleSets/ |Schrijven|
|Microsoft.Network/networkWatchers/packetCaptures/ |Lezen|
|Microsoft.Network/networkWatchers/packetCaptures/| Schrijven|
|Microsoft.Network/networkWatchers/packetCaptures/| Verwijderen|
|Microsoft.Network/networkWatchers/ |Schrijven |
|Microsoft.Network/networkWatchers/| Lezen |
|Microsoft.Insights/alertRules/ |*|
|Microsoft.Support/ | *|

### <a name="network-subscription-limits"></a>Netwerk-abonnementen

Netwerk-abonnementen bieden u meer informatie over het gebruik van elk van de netwerkbron in een abonnement in een regio op basis van het maximum aantal bronnen die beschikbaar zijn.

![limiet van het abonnement][nsl]

## <a name="network-resource-level-monitoring"></a>Resource niveau netwerkbewaking

De volgende functies zijn beschikbaar voor bewaking van niveau resource:

### <a name="audit-log"></a>Controlelogboek

Bewerkingen die worden uitgevoerd als onderdeel van de configuratie van netwerken worden geregistreerd. Deze logboeken kunnen worden weergegeven in de Azure-portal of opgehaald met behulp van Microsoft-hulpprogramma's zoals Power BI of hulpprogramma's van derden. Controlelogboeken zijn beschikbaar via de portal, PowerShell, CLI en Rest-API. Zie voor meer informatie over controlelogboeken [bewerkingen met Resource Manager controleren](../resource-group-audit.md)

Controlelogboeken zijn beschikbaar voor bewerkingen die op alle netwerkbronnen.

### <a name="metrics"></a>Metrische gegevens

Metrische gegevens zijn metingen van de prestaties en prestatiemeteritems die gedurende een periode worden verzameld. Metrische gegevens zijn momenteel beschikbaar voor de toepassingsgateway. Metrische gegevens kan worden gebruikt voor het activeren van waarschuwingen op basis van de drempelwaarde. Zie [Application Gateway Diagnostics](../application-gateway/application-gateway-diagnostics.md) om weer te geven hoe metrische gegevens kunnen worden gebruikt om waarschuwingen te maken.

![metrische gegevens weergeven][metrics]

### <a name="diagnostic-logs"></a>Diagnostische logboeken

Periodieke en eigen initiatief gebeurtenissen zijn gemaakt door netwerkbronnen en storage-accounts, verzonden naar een Event Hub of Log Analytics aangemeld. Deze logboeken bieden inzicht in de status van een resource. Deze logboeken kunnen worden weergegeven in de hulpprogramma's, zoals Power BI en Log Analytics. Als u wilt weten hoe u logboeken met diagnostische gegevens bekijken, gaat u naar [logboekanalyse](../log-analytics/log-analytics-azure-networking-analytics.md).

Diagnostische logboeken beschikbaar zijn voor [Load Balancer](../load-balancer/load-balancer-monitor-log.md), [Netwerkbeveiligingsgroepen](../virtual-network/virtual-network-nsg-manage-log.md), Routes, en [Application Gateway](../application-gateway/application-gateway-diagnostics.md).

Netwerk-Watcher biedt dat een diagnostische logboeken weergeven. Deze weergave bevat alle netwerkresources die ondersteuning bieden voor diagnostische gegevens vastleggen. U kunt in deze weergave inschakelen en uitschakelen van netwerkresources snel en gemakkelijk.

![logboeken][logs]

### <a name="troubleshooting"></a>Problemen oplossen

De probleemoplossing blade een ervaring in de portal is op netwerkbronnen vandaag op te sporen veelvoorkomende problemen die zijn gekoppeld aan een afzonderlijke resource opgegeven. Deze ervaring is beschikbaar voor de volgende resources van netwerk - ExpressRoute, VPN-Gateway, Application Gateway, netwerk-beveiligingslogboeken, Routes, DNS, Load Balancer en Traffic Manager. Voor meer informatie over probleemoplossing van niveau resource, gaat u naar [diagnosticeren en oplossen van problemen bij het oplossen van Azure](https://azure.microsoft.com/blog/azure-troubleshoot-diagonse-resolve-issues/)

![informatie over probleemoplossing][TS]

### <a name="resource-health"></a>Status van resources

De status van een netwerkbron wordt periodiek verstrekt. Deze bronnen omvatten VPN-Gateway en VPN-tunnel. De resourcestatus is toegankelijk is op de Azure-portal. Voor meer informatie over de resourcestatus, gaat u naar [bron Health-overzicht](../resource-health/resource-health-overview.md)

## <a name="next-steps"></a>Volgende stappen

Na het leren over de netwerk-Watcher leert u:

Voer een pakketopname op de virtuele machine in via [variabele pakketopname in de Azure portal](network-watcher-packet-capture-manage-portal.md)

Proactieve controle en diagnostische gegevens met behulp van [waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md).

Beveiligingsproblemen met detecteren [pakketopname met Wireshark analyseren](network-watcher-deep-packet-inspection.md), met open-source hulpprogramma's.

Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure.

<!--Image references-->
[TS]: ./media/network-watcher-monitoring-overview/troubleshooting.png
[logs]: ./media/network-watcher-monitoring-overview/logs.png
[metrics]: ./media/network-watcher-monitoring-overview/metrics.png
[nsl]: ./media/network-watcher-monitoring-overview/nsl.png











