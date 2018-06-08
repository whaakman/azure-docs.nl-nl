---
title: Azure-facturering en kosten management automatiseringsscenario's | Microsoft Docs
description: Informatie over hoe algemene facturering en kostenbeheer scenario's zijn toegewezen aan verschillende API's.
services: billing
documentationcenter: ''
author: Erikre
manager: dougeby
editor: ''
tags: billing
ms.assetid: 204b15b2-6667-4b6c-8ea4-f32c06f287fd
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 6/07/2018
ms.author: erikre
ms.openlocfilehash: ac0c4c0bad98c3057fb554e6011f1c601cd97dee
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34851272"
---
# <a name="billing-and-cost-management-automation-scenarios"></a>Facturering en kosten management automation-scenario 's

Algemene scenario's voor de ruimte van de management facturering en kosten zijn geïdentificeerd hieronder en toegewezen aan verschillende API's die kunnen worden gebruikt in deze scenario's. Een overzicht van alle beschikbare API's en de functionaliteit bieden vindt u onder het scenario voor API-toewijzing. 

## <a name="common-scenarios"></a>Algemene scenario's 

U kunt de facturering en beheer-API's in verschillende scenario's voor antwoord kosten en gebruik gerelateerde vragen.  Hieronder vindt u een overzicht van veelvoorkomende scenario's.

- **Afstemming factuur** -heeft Microsoft kosten in rekening gebracht mij de juiste tijd?  Wat is mijn factuur en kan ik berekenen zelf?

- **Cross-kosten** - nu dat ik hoeveel ik ben in rekening worden gebracht weet, die in mijn organisatie nodig heeft om te betalen?

- **Optimalisatie kosten** -ik weet hoeveel ik hebt in rekening gebracht, maar hoe vind ik meer uit het geld ik in Azure uitgaven ben?

- **Kosten bijhouden** : ik wil weergeven hoeveel ik ben uitgaven en gebruik van Azure gedurende een bepaalde periode. Wat zijn de trends? Hoe kan ik doen beter?

- **Azure besteden tijdens de maand** – hoeveel is mijn huidige maand tot datum de besteden? Heb ik nodig om alle aanpassingen in mijn uitgaven en/of informatie over het gebruik van Azure? Wanneer tijdens de maand ben ik verbruikt Azure de meeste?

- **Stel waarschuwingen** : ik wil verbruik op basis van bronnen of monetaire gebaseerde waarschuwingen instellen.

## <a name="scenario-to-api-mappings"></a>Scenario in API-toewijzingen

|         API/Scenario        | Factuurafstemming    | Cross-kosten    | Kosten optimalisatie    | Kosten bijhouden    | Midden maand besteden    | Waarschuwingen    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Budgetten                     |                           |                  |           X          |                  |                    |     X     |
| Marktplaatsen                |             X             |         X        |           X          |         X        |          X         |     X     |
| Prijzenoverzicht                 |             X             |         X        |           X          |         X        |          X         |           |
| Reservering aanbevelingen |                           |                  |           X          |                  |                    |           |
| Details van de reservering         |                           |                  |           X          |         X        |                    |           |
| Samenvattingen van de reservering       |                           |                  |           X          |         X        |                    |           |
| Gebruiksdetails               |             X             |         X        |           X          |         X        |          X         |     X     |
| Perioden facturering             |             X             |         X        |           X          |         X        |                    |           |
| Facturen                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Niet-geclassificeerde gebruik               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> Het scenario in API-toewijzingen hieronder niet de Enterprise-verbruik API's opnemen. Waar mogelijk, neem de algemene verbruik API's gebruiken voor adressen net nieuwe ontwikkelscenario's u verder gaat.

## <a name="api-summaries"></a>Samenvattingen van de API

### <a name="consumption"></a>Verbruik
(*Direct web + Enterprise-klanten voor alle API's, behalve die opgeroepen uit hieronder*)

-   **Budgetten** (*Enterprise-klanten*): Gebruik de [budgetten API](https://docs.microsoft.com/rest/api/consumption/budgets) kosten of gebruik budgetten voor resources, resourcegroepen of facturering meters maken.  Nadat u hebt gemaakt budgetten, kan waarschuwingen worden geconfigureerd om te melden wanneer de gebruiker gedefinieerde budgetdrempels zijn overschreden. Acties kunnen ook worden geconfigureerd voor optreden wanneer budgetbedragen zijn bereikt.
-   **Marktplaatsen**: Gebruik de [Marketplace kosten API](https://docs.microsoft.com/rest/api/consumption/marketplaces) kosten en gebruiksgegevens ophalen voor alle Marketplace-resources (Azure 3e partij aanbiedingen). Deze gegevens kunnen worden gebruikt voor kosten optellen over alle Marketplace-resources of kosten/gebruik op de specifieke resources onderzoeken.
-   **Prijslijst** (*Enterprise-klanten*): bedrijfsklanten kunnen via de [Price Sheet API](https://docs.microsoft.com/rest/api/consumption/pricesheet) voor het ophalen van de aangepaste prijscategorie voor alle meters. Ondernemingen kunnen deze gegevens gebruiken in combinatie met details en marktplaatsen gebruik gebruiksinformatie kosten berekeningen met behulp van gebruiks- en marketplace-gegevens. 
-   **Reservering aanbevelingen**: Gebruik de [reservering aanbevelingen API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations) aanbevelingen voor het aanschaffen van een VM gereserveerd exemplaren verkrijgen. Aanbevelingen zijn ontworpen om te kunnen klanten voor het analyseren van verwachte kostenbesparingen en inkoop bedragen.
-   **Details van de reservering**: Gebruik de [reservering Details API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails) voor informatie over eerder is gekocht VM-reserveringen, zoals hoeveel verbruik is gereserveerd versus hoeveel daadwerkelijk wordt gebruikt. Hier ziet u gegevens in detail per VM-niveau.
-   **Reservering samenvattingen**: Gebruik de [reservering samenvattingen API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries) voor statistische informatie over eerder is gekocht VM-reserveringen, zoals hoeveel verbruik is gereserveerd versus hoeveel daadwerkelijk wordt gebruikt in de statistische functie. 
-   **Informatie over het gebruik**: Gebruik de [gebruik Details API](https://docs.microsoft.com/rest/api/consumption/usagedetails) ophalen van de kosten en gebruik op alle Azure 1e resources van derden. Informatie is in de vorm van detail gebruiksrecords, die momenteel worden verzonden, één keer per meter per dag. Informatie kan toevoegen aan de kosten voor alle resources of onderzoeken kosten/gebruik op de specifieke resources worden gebruikt.
-   **RateCard**: Web Direct klanten kunnen gebruiken om de [RateCard API](https://msdn.microsoft.com/library/azure/mt219005.aspx) hun tarieven meter ophalen. Kan vervolgens worden gebruikt de geretourneerde informatie met hun informatie over het gebruik van de bron voor het handmatig berekenen verwachte factuur. 
-   **Gebruik zonder restricties**: U kunt de [niet-geclassificeerde gebruik API](https://msdn.microsoft.com/library/azure/mt219003.aspx) verkrijgen van informatie over het gebruik van onbewerkte, voordat u een meting/opladen gedaan door Azure.

### <a name="billing"></a>Billing
-   **Facturering perioden**: Gebruik de [facturering perioden API](https://docs.microsoft.com/rest/api/billing/billingperiods) om te bepalen een factureringsperiode kunt analyseren, samen met de factuur-id's voor deze periode. Factuur-id's kunnen worden gebruikt met de onderstaande factuur-API. 
-   **Facturen**: Gebruik de [facturen API](https://docs.microsoft.com/rest/api/billing/invoices) ophalen van de download-URL voor een factuur voor een bepaalde factureringsperiode in PDF-formulier.

### <a name="enterprise-consumption"></a>Enterprise-verbruik
*(Alle API's voor Enterprise alleen)*

-   **Samenvatting saldo**: Gebruik de [saldo samenvatting API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) per maand samenvatting van informatie over tegoeden, nieuwe aankopen Azure Marketplace kosten, aanpassingen en overschrijding kosten ophalen. U kunt deze informatie voor de huidige factureringsperiode of een punt in het verleden. Ondernemingen kunnen deze gegevens gebruiken om uit te voeren van een vergelijking met handmatig berekende samenvatting kosten. Deze API biedt geen resource-specifieke informatie of een samengestelde weergave van de kosten.
-   **Informatie over het gebruik**: Gebruik de [gebruik Details API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) 1e partij Azure gebruiksgegevens details voor de huidige maand, een specifieke factureringsperiode of een aangepaste periode ophalen. Ondernemingen kunnen deze gegevens kunnen gebruiken voor het handmatig berekenen factuur op basis van de frequentie en het verbruik en ook afdeling/organisatiegegevens aanwezig kenmerk kosten tussen verschillende organisaties. De gegevens biedt een weergave van de resource-specifieke van kosten/gebruik.
-   **Marketplace Store kosten**: Gebruik de [Marketplace Store kosten API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) 3e partij Azure gebruiksgegevens details voor de huidige maand, een specifieke factureringsperiode of een aangepaste periode ophalen. Ondernemingen kunnen deze gegevens kunnen gebruiken voor het handmatig berekenen factuur op basis van de frequentie en het verbruik en ook afdeling/organisatiegegevens aanwezig kenmerk kosten tussen verschillende organisaties. De Marketplace store kosten API biedt een weergave van de resource-specifieke van kosten/gebruik.
-   **Prijslijst**: de [Price Sheet API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) het tarief voor elke Meter biedt voor de opgegeven registratie en facturering periode. Deze informatie voor de frequentie waarmee kan worden gebruikt in combinatie met details en marktplaatsen gebruik gebruiksinformatie verwachte factuur handmatig te berekenen.
-   **Facturering perioden**: Gebruik de [facturering perioden API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) voor een lijst met punten samen met een eigenschap die verwijst naar de API-route voor de vier gegevenssets Enterprise-API die betrekking op die factureringsperiode - BalanceSummary hebben, facturering UsageDetails, Marketplace-kosten en prijslijst.
-   **Gereserveerd exemplaar aanbevelingen**: de [gereserveerde exemplaar aanbevelingen API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation) van klant 7 dagen, 30 dagen of 60 dagen van het gebruik van virtuele machines wordt bekeken en aanbevelingen voor één en -aankoop gedeeld. Het gereserveerde exemplaar dat API kan klanten analyseren verwacht kostenbesparingen en aanbevolen aankoopbedragen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het gebruik van Azure Billing-API's om programmatisch inzicht verkrijgen in uw Azure-gebruik [API-overzicht van Azure Billing](billing-usage-rate-card-overview.md).