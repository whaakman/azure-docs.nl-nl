---
title: Problemen vaststellen en oplossen van problemen bij het gebruik van Azure Cosmos DB .NET SDK
description: Gebruik functies zoals client-side-logboekregistratie en andere hulpprogramma's van derden om te identificeren, onderzoeken en Azure Cosmos DB-problemen oplossen bij het gebruik van .NET SDK.
author: j82w
ms.service: cosmos-db
ms.date: 01/19/2019
ms.author: jawilley
ms.devlang: c#
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 7f969ab6059140ec32c9c5bf5045c546602a3c15
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57411513"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-net-sdk"></a>Problemen vaststellen en oplossen van problemen bij het gebruik van Azure Cosmos DB .NET SDK
In dit artikel bevat informatie over veelvoorkomende problemen, oplossingen, diagnostische stappen en hulpprogramma's wanneer u de [.NET SDK](sql-api-sdk-dotnet.md) met Azure Cosmos DB SQL API-accounts.
De .NET SDK bevat logische representatie van de client-side voor toegang tot de Azure Cosmos DB SQL API. Dit artikel beschrijft de hulpmiddelen en benaderingen om u te helpen als u problemen ondervindt.

## <a name="checklist-for-troubleshooting-issues"></a>Controlelijst voor het oplossen van problemen:
Houd rekening met de volgende controlelijst voordat u uw toepassing naar productie verplaatsen. Met behulp van de controlelijst wordt voorkomen dat enkele veelvoorkomende problemen die u ziet. U kunt ook snel vaststellen wanneer er een probleem optreedt:

*   De laatste [SDK](https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/changelog.md). SDK's voor Preview-versie mag niet worden gebruikt voor productie. Hiermee wordt voorkomen dat tegen bekende problemen die al zijn opgelost.
*   Controleer de [tips voor betere prestaties](performance-tips.md), en volg de aanbevolen procedures. Dit helpt te voorkomen dat schalen, latentie en andere prestatieproblemen.
*   De SDK-logboekregistratie om op te lossen van een probleem inschakelen. De logboekregistratie inschakelen kan de prestaties beïnvloeden, dus het is raadzaam om alleen bij het oplossen van problemen. U kunt de volgende Logboeken inschakelen:
    *   [Meld u metrische gegevens](monitor-accounts.md) met behulp van de Azure-portal. Portal prestatiegegevens geven de telemetrie van Azure Cosmos DB, wat nuttig is om te bepalen of als het probleem komt met de Azure Cosmos DB overeen is vanaf de client.
    *   Logboek de [diagnostics tekenreeks](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring?view=azure-dotnet) uit de antwoorden van de bewerking punt.
    *   Logboek de [metrische gegevens over SQL-Query](sql-api-query-metrics.md) van alle reacties van de query's 
    *   Ga als volgt de instellingen voor [SDK-logboekregistratie]( https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/docs/documentdb-sdk_capture_etl.md)

Bekijk de [veelvoorkomende problemen en tijdelijke oplossingen](#common-issues-workarounds) sectie in dit artikel.

Controleer de [GitHub sectie problemen](https://github.com/Azure/azure-cosmos-dotnet-v2/issues) die actief worden bewaakt. Controleer als er al een soortgelijk probleem met een tijdelijke oplossing is ingediend. Als u een oplossing niet hebt gevonden, is het bestand een GitHub-probleem. U kunt een tick ondersteuning voor urgente problemen kunt openen.


## <a name="common-issues-workarounds"></a>Veelvoorkomende problemen en oplossingen

### <a name="general-suggestions"></a>Algemene suggesties
* Uitvoeren van uw app in dezelfde Azure-regio als uw Azure Cosmos DB-account, indien mogelijk. 
* U kunt uitvoeren in problemen met de connectiviteit/beschikbaarheid vanwege gebrek aan resources op de clientcomputer. Is het raadzaam bewaking van de CPU-gebruik op knooppunten die de Azure Cosmos DB-client uit te voeren, en schaal omhoog/uit als deze nu worden uitgevoerd bij hoge belasting.

### <a name="check-the-portal-metrics"></a>De portal metrische gegevens controleren
Controle van de [portal metrische gegevens](monitor-accounts.md) kunt u bepalen of het is een probleem aan de clientzijde of als er een probleem met de service. Bijvoorbeeld als de metrische gegevens een groot aantal aanvragen van de tarief-limited (HTTP-statuscode 429 bevatten) wat betekent dat de aanvraag is ophalen beperkt en schakelt de [Snelheid van aanvragen te groot] sectie. 

### <a name="request-timeouts"></a>Time-outs van aanvragen
RequestTimeout gebeurt meestal wanneer u Direct/TCP, maar kan gebeuren in de modus van de Gateway. Dit zijn de algemene bekende oorzaken en suggesties voor het oplossen van het probleem.

* CPU-gebruik is hoog, dat zal leiden tot latentie en/of time-outs van aanvragen. De klant de hostmachine meer resources, zodat deze kan opschalen of de belasting over meer virtuele machines kan worden gedistribueerd.
* Socket / poort beschikbaarheid mogelijk te weinig. Wanneer u de .NET-SDK's die zijn uitgebracht vóór versie 2.0, clients die worden uitgevoerd in Azure kunnen bereikt de [Azure SNAT (PAT) poortuitputting]. Dit een voorbeeld van waarom het verdient aanbeveling om uit te voeren altijd de meest recente SDK-versie.
* Het maken van meerdere exemplaren van de DocumentClient kan leiden tot conflicten verbinding en time-outproblemen. Ga als volgt de [tips voor betere prestaties](performance-tips.md), en het gebruik van één DocumentClient-exemplaar in een hele proces.
* Gebruikers zien soms verhoogde latentie of aanvraag time-outs omdat hun verzamelingen onvoldoende is ingericht, de back-end aanvragen vertraagt en de client opnieuw intern probeert zonder dat dit zichtbaar te maken voor de oproepende functie. Controleer de [portal metrische gegevens](monitor-accounts.md).
* Azure Cosmos DB wordt de totale ingerichte doorvoer gelijkmatig verspreid over fysieke partities. Controleer de portal metrische gegevens om te zien als de werkbelasting is er een hot [partitiesleutel](partition-data.md). Dit zorgt ervoor dat de totale verbruikte doorvoer (RU/s) worden weergegeven onder de ingerichte ru's, maar een enkele partitie verbruikt doorvoer (RU/s) overschrijdt de ingerichte doorvoer. 
* Bovendien wordt de SDK 2.0 kanaal semantiek toegevoegd aan direct/TCP-verbindingen. Een TCP-verbinding wordt gebruikt voor meerdere aanvragen op hetzelfde moment. Dit kan leiden tot twee problemen in bepaalde gevallen:
    * Een hoge mate van gelijktijdigheid kan leiden tot conflicten op het kanaal.
    * Grote aanvragen of antwoorden kunnen leiden tot het hoofd-of-line blokkeren op het kanaal en exacerbate conflicten, zelfs met een relatief lage mate van gelijktijdigheid.
    * Als de aanvraag in een van deze twee categorieën valt (of als hoog CPU-verbruik is verdacht), deze mogelijke oplossingen zijn:
        * Probeer te schalen van de toepassing omhoog/uit.
        * Bovendien SDK-logboeken kunnen worden vastgelegd via [Trace Listener](https://github.com/Azure/azure-cosmosdb-dotnet/blob/master/docs/documentdb-sdk_capture_etl.md) voor meer informatie.

### <a name="connection-throttling"></a>Bandbreedtebeperking van verbinding
Beperking van de verbinding kan gebeuren vanwege een limiet voor verbindingen op een hostcomputer. Vorige 2.0, clients die worden uitgevoerd in Azure kunnen worden bereikt de [Azure SNAT (PAT) poortuitputting].

### <a name="snat"></a>Azure SNAT (PAT) poortuitputting

Als uw app op Azure Virtual Machines geïmplementeerd zonder een openbaar IP-adres, standaard [Azure SNAT poorten](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) verbindingen instellen met een willekeurig eindpunt buiten uw virtuele machine. Het aantal verbindingen van de virtuele machine met de Azure Cosmos DB-eindpunt wordt beperkt door de [Azure SNAT configuratie](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

 Azure SNAT poorten worden gebruikt alleen wanneer uw virtuele machine een privé IP-adres heeft en een proces van de virtuele machine probeert verbinding maken met een openbaar IP-adres. Er zijn twee oplossingen om te voorkomen dat Azure SNAT beperking:

* Uw Azure Cosmos DB-service-eindpunt toevoegen aan het subnet van het virtuele netwerk van Azure Virtual Machines. Zie voor meer informatie, [Azure Virtual Network service-eindpunten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). 

    Wanneer het service-eindpunt is ingeschakeld, worden de aanvragen niet meer vanaf een openbaar IP-adres verzonden met Azure Cosmos DB. In plaats daarvan worden het virtuele netwerk en subnet-id verzonden. Deze wijziging kan leiden tot firewall val als alleen openbare IP-adressen zijn toegestaan. Als u een firewall gebruikt wanneer u het service-eindpunt inschakelt, voegt u een subnet toe aan de firewall met behulp van [virtueel netwerk-ACL's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
* Een openbaar IP-adres toewijzen aan uw Azure-VM.

### <a name="http-proxy"></a>HTTP-proxy
Als u een HTTP-proxy gebruikt, zorg ervoor dat het aantal verbindingen die zijn geconfigureerd in de SDK kan ondersteunen `ConnectionPolicy`.
U wordt geconfronteerd anders verbindingsproblemen.

### Snelheid van aanvragen te groot<a name="request-rate-too-large"></a>
'Aanvraagsnelheid te groot' of foutcode 429 geeft aan dat uw aanvragen worden beperkt, omdat de verbruikte doorvoer (RU/s) heeft de ingerichte doorvoer overschreden. De SDK probeert automatisch aanvragen op basis van de opgegeven [beleid voor opnieuw proberen](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.retryoptions?view=azure-dotnet). Als u deze fout vaak krijgt, kunt u overwegen om de de doorvoer van de verzameling. Controleer de [metrische gegevens van portal](use-metrics.md) om te zien als u wel 429-fouten. Controleer uw [partitiesleutel](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey) om te controleren of dit resulteert in een gelijkmatige verdeling van opslag en aanvraag volume. 

### <a name="slow-query-performance"></a>Prestaties van trage query 's
De [metrische query](sql-api-query-metrics.md) kunt u bepalen waar de query de meeste van de tijd besteedt. In de query metrische gegevens, kunt u zien hoeveel ervan wordt de client die wordt besteed aan het vs back-end.
* Als de back-end-query retourneert het snel en een grote tijd op de client doorbrengt controleert u de belasting van de machine. Is het waarschijnlijk dat er onvoldoende bronnen beschikbaar zijn en de SDK wordt gewacht tot resources beschikbaar zijn voor het afhandelen van het antwoord.
* Als de back-end-query is traag [optimaliseren van de query](optimize-cost-queries.md) en kijken naar de huidige [indexeringsbeleid](index-overview.md) 

 <!--Anchors-->
[Common issues and workarounds]: #common-issues-workarounds
[Enable client SDK logging]: #logging
[Snelheid van aanvragen te groot]: #request-rate-too-large
[Request Timeouts]: #request-timeouts
[Azure SNAT (PAT) poortuitputting]: #snat
[Production check list]: #production-check-list


