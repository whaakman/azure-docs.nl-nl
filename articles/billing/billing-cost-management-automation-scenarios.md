---
title: Automation-scenario's voor Azure-facturering en kostenbeheer | Microsoft Docs
description: Meer informatie over hoe veelvoorkomende facturering en kostenbeheer scenario's zijn toegewezen aan verschillende API's.
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
ms.date: 6/13/2018
ms.author: erikre
ms.openlocfilehash: 0d2b0f2d3fad318ac0152d92fe92614d8dadda1e
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918515"
---
# <a name="automation-scenarios-for-billing-and-cost-management"></a>Automation-scenario's voor het beheer van facturering en kosten

Dit artikel worden algemene scenario's voor Azure-facturering en kostenbeheer. Het volgende scenario's wordt toegewezen aan de API's die u kunt gebruiken. Onder elk scenario naar de API-toewijzing vindt u een overzicht van de API's en de functionaliteit die ze bieden.

## <a name="common-scenarios"></a>Algemene scenario's

U kunt de facturering gebruiken en kosten van beheer-API's in verschillende scenario's met betrekking tot kosten en gebruik gerelateerde vragen te beantwoorden. Hier volgt een overzicht van veelvoorkomende scenario's:

- **Factuur afstemming**: Microsoft kosten in rekening gebracht mij de juiste hoeveelheid?  Wat is mijn factuur en kan ik berekenen zelf?

- **Cross-kosten**: Nu dat ik hoeveel ik ben in rekening gebracht weet, wie in mijn organisatie nodig heeft om te betalen?

- **Kosten van optimalisatie**: Weet ik hoeveel ik hebt gebracht. Hoe vind ik meer uit het geld dat ik besteed aan Azure?

- **Kosten bijhouden**: Ik wil zien hoeveel ik ben uitgaven en met behulp van Azure na verloop van tijd. Wat zijn de trends? Hoe kan ik beter?

- **Azure-uitgaven gedurende de maand**: Hoeveel is mijn huidige maand tot heden uitgaven? Heb ik nodig voor het aanbrengen van wijzigingen in mijn uitgaven en/of gebruik van Azure? Wanneer tijdens de maand ben ik verbruikt Azure het meest?

- **Waarschuwingen**: Hoe kan ik instellen op basis van een resource verbruik of waarschuwingen monetaire op basis van?

## <a name="scenario-to-api-mapping"></a>Scenario voor de API-toewijzing

|         API        | Factuurafstemming    | Cross-kosten    | Kosten optimaliseren    | Kosten bijhouden    | Midmonth uitgaven    | Waarschuwingen    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Budgetten                     |                           |                  |           X          |                  |                    |     X     |
| Marketplace-kosten                |             X             |         X        |           X          |         X        |          X         |     X     |
| Prijzenoverzicht                 |             X             |         X        |           X          |         X        |          X         |           |
| Aanbevelingen voor reserveringen |                           |                  |           X          |                  |                    |           |
| Reserveringsdetails         |                           |                  |           X          |         X        |                    |           |
| Reserveringssamenvattingen       |                           |                  |           X          |         X        |                    |           |
| Gebruiksgegevens               |             X             |         X        |           X          |         X        |          X         |     X     |
| Facturering-punten             |             X             |         X        |           X          |         X        |                    |           |
| Facturen                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Niet-geclassificeerde gebruik               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> De toewijzing van het scenario-naar-API omvat niet de Enterprise-verbruik-API's. Waar mogelijk, gebruikt u de algemene verbruik-API's voor nieuwe ontwikkelscenario's.

## <a name="api-summaries"></a>Samenvattingen van de API

### <a name="consumption"></a>Verbruik
Web Direct- en Enterprise-klanten kunnen alle de volgende API's gebruiken, tenzij anders wordt vermeld:

-   [Budgetten API](https://docs.microsoft.com/rest/api/consumption/budgets) (*alleen Enterprise-klanten*): Budgetten voor kosten of gebruik voor resources, resourcegroepen of factureringsmeters maken. Wanneer u budgetten hebt gemaakt, kunt u waarschuwingen op de hoogte stellen wanneer u hebt overschreden budgetdrempels gedefinieerd configureren. Ook kunt u acties moet plaatsvinden wanneer u budgetbedragen hebt bereikt.

-   [Marketplace in rekening gebracht API](https://docs.microsoft.com/rest/api/consumption/marketplaces): Kosten in rekening gebracht en het gebruik gegevens ophalen voor alle Azure Marketplace-resources (Azure-partners). U kunt deze gegevens gebruiken om toe te voegen kosten over alle Marketplace-resources of voor het onderzoeken van kosten/gebruik op specifieke resources.

-   [Prijs blad API](https://docs.microsoft.com/rest/api/consumption/pricesheet) (*alleen Enterprise-klanten*): Aangepaste prijzen voor alle meters ophalen Ondernemingen kunnen deze gegevens gebruiken in combinatie met informatie over het gebruik en informatie over het gebruik van marketplace om kosten te berekenen met behulp van gebruiks-en marketplace. 

-   [Reservering Recommendations-API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations): Ontvang aanbevelingen voor het kopen van gereserveerde VM-instanties. Aanbevelingen helpen bij het analyseren van verwachte kosten te besparen en inkoop bedragen. Zie voor meer informatie, [API's voor het automatiseren van Azure-reservering](billing-reservation-apis.md).

-   [Reservering Details API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails): Zie informatie over eerder aangeschafte VM-reserveringen, zoals hoeveel verbruik is gereserveerd en hoeveel wordt gebruikt. U kunt data-at-per-VM-niveau detail zien. Zie voor meer informatie, [API's voor het automatiseren van Azure-reservering](billing-reservation-apis.md).

-   [Samenvattingen van de reservering API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries): Zie verzamelde informatie over VM-reserveringen die uw organisatie hebt gekocht, zoals hoeveel verbruik versus hoeveel wordt gebruikt in het aggregaat is gereserveerd. Zie voor meer informatie, [API's voor het automatiseren van Azure-reservering](billing-reservation-apis.md).

-   [Gebruik API Details](https://docs.microsoft.com/rest/api/consumption/usagedetails): Informatie over alle Azure-resources kosten in rekening gebracht en gebruik van Microsoft krijgen. Informatie is in de vorm van detail gebruiksrecords, die momenteel worden uitgezonden, één keer per meter per dag. U kunt de informatie toe te voegen van de kosten voor alle resources of onderzoeken kosten/gebruik op specifieke resources gebruiken.

-   [RateCard API](/previous-versions/azure/reference/mt219005(v=azure.100)): Meter-tarieven krijgen als u een Web Direct-klant bent. U kunt de geretourneerde informatie vervolgens gebruiken met de informatie over het gebruik van de resource voor het berekenen van de verwachte factuur handmatig. 

-   [API voor gebruik zonder restricties](/previous-versions/azure/reference/mt219003(v=azure.100)): Informatie over het gebruik van onbewerkte ophalen voordat Azure een meting/kosten in rekening gebracht heeft.

### <a name="billing"></a>Billing
-   [Facturering perioden API](https://docs.microsoft.com/rest/api/billing/billingperiods): Een factureringsperiode kunt analyseren, samen met de factuur id's voor die periode bepalen. Factuur-id's kunt u met de API voor facturen.

-   [API-facturen](https://docs.microsoft.com/rest/api/billing/invoices): Haal de download-URL voor een factuur voor een factureringsperiode in PDF-formulier.

### <a name="enterprise-consumption"></a>Enterprise-verbruik
De volgende API's zijn alleen voor Enterprise:

-   [Overzicht API in balans brengen](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary): Krijg een maandelijkse samenvatting van informatie over de saldi, aankopen in de nieuwe Azure Marketplace servicekosten, aanpassingen en overschrijdingskosten. U kunt deze informatie voor de huidige factureringsperiode of een punt in het verleden. Ondernemingen kunnen deze gegevens gebruiken om te vergelijken met handmatig berekende samenvatting kosten in rekening gebracht. Deze API biedt geen resource-specifieke informatie of een samengevoegde weergave van de kosten.

-   [Gebruik API Details](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail): Krijg informatie over het gebruik van Azure (van Microsoft-aanbiedingen) voor de huidige maand, een bepaalde factureringsperiode of een aangepaste periode. Ondernemingen kunnen deze gegevens gebruiken om te handmatig berekenen op basis van de snelheid en het verbruik facturen. Ondernemingen kunnen ook gebruiken voor afdeling/organisatiegegevens kenmerk kosten tussen verschillende organisaties. De gegevens biedt een overzicht van de resource-specifieke van gebruik/kosten.

-   [Marketplace-Store kosten in rekening gebracht API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge): Informatie over het gebruik van Azure (aanbiedingen van partners) ophalen voor de huidige maand, een bepaalde factureringsperiode of een aangepaste periode. Ondernemingen kunnen deze gegevens gebruiken om te handmatig berekenen op basis van de snelheid en het verbruik facturen. Ondernemingen kunnen ook gebruiken voor afdeling/organisatiegegevens kenmerk kosten tussen verschillende organisaties. Deze API biedt een resource-specifieke weergave van de gebruikskosten /.

-   [Prijs blad API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet): Haal het tarief voor elke meter voor de bepaalde inschrijving en de factureringsperiode. U kunt deze informatie van de snelheid in combinatie met informatie over het gebruik en informatie over het gebruik van marketplace gebruiken voor het berekenen van de verwachte factuur handmatig.

-   [Facturering perioden API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods): Een lijst met factureringsperioden ophalen. De API kunt u ook een eigenschap die verwijst naar de API-route voor de vier sets Enterprise API-gegevens die betrekking op de factureringsperiode hebben: BalanceSummary, UsageDetails, Marketplace-kosten en prijslijst.

-   [Gereserveerde instantie Recommendations-API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation): Kijken naar 7 dagen, 30 dagen of 60 dagen van de virtuele machine gebruik en ontvang aanbevelingen voor één en gedeelde aankoop. U kunt deze API gebruiken voor het analyseren van verwachte kosten te besparen en aanbevolen aankoopbedragen. Zie voor meer informatie, [API's voor het automatiseren van Azure-reservering](billing-reservation-apis.md).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="whats-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>Wat is het verschil tussen de Enterprise Reporting API's en de API-verbruik? Wanneer moet ik elke gebruiken?
Deze API's hebben een aantal soortgelijke functionaliteit en de dezelfde breed scala aan vragen in de ruimte facturering en cost management kunnen beantwoorden. Maar ze zich richten op verschillende doelgroepen: 

- Enterprise Reporting API's zijn beschikbaar voor klanten die zich hebben aangemeld voor een Enterprise Agreement van Microsoft die deze toegang tot in de onderhandelde monetaire toezeggingen en aangepaste prijzen verleent. De API's vereisen een sleutel die u kunt ophalen uit de [Enterprise Portal](https://ea.azure.com). Zie voor een beschrijving van deze API's, [overzicht van de rapportage-API's voor zakelijke klanten](billing-enterprise-api.md).

- Gebruik API's zijn beschikbaar voor alle klanten, met een paar uitzonderingen. Zie voor meer informatie, [overzicht van Azure-verbruik API](billing-consumption-api-overview.md) en de [Azure-verbruik API-verwijzingen](https://docs.microsoft.com/rest/api/consumption/). We raden aan de opgegeven API's als de oplossing voor de meest recente ontwikkelscenario's. 

### <a name="whats-the-difference-between-the-usage-details-api-and-the-usage-api"></a>Wat is het verschil tussen de API van de Details van gebruik en de Usage-API?
Deze API's bieden fundamentele verschillende gegevens:

- De [Usage Details API](https://docs.microsoft.com/rest/api/consumption/usagedetails) biedt Azure gebruiks- en -gegevens per exemplaar van de meter. De opgegeven gegevens al doorgegeven via de kosten voor het meten van systeem in Azure en had toegepast, samen met andere mogelijke wijzigingen kosten:

   - Wijzigingen in rekening voor het gebruik van vooraf betaalde monetaire toezeggingen
   - Wijzigingen in voor gebruik discrepanties gedetecteerd door Azure-account

- De [Usage API](/previous-versions/azure/reference/mt219003(v=azure.100)) biedt informatie over het gebruik van onbewerkte Azure voordat deze wordt doorgegeven via de kosten voor het meten van systeem in Azure. Deze gegevens wellicht een correlatie met het gebruik of kosten in rekening gebracht bedrag dat wordt weergegeven nadat de door Azure kosten in rekening gebracht meetsysteem.

### <a name="whats-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>Wat is het verschil tussen de factuur-API en de Details Usage API?
Deze API's bieden een andere weergave van dezelfde gegevens:

- De [factuur API](https://docs.microsoft.com/rest/api/billing/invoices) is bedoeld voor Web Direct klanten alleen. Dit zorgt voor een maandelijkse samenvoeging van uw factuur op basis van de totale kosten in rekening gebracht voor elk metertype. 

- De [Usage Details API](https://docs.microsoft.com/rest/api/consumption/usagedetails) biedt een gedetailleerde weergave van de gebruikskosten /-records voor elke dag. Enterprise en het Web Direct-klanten kunnen gebruiken.

### <a name="whats-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>Wat is het verschil tussen de prijs blad API en de RateCard API?
Deze API's bieden vergelijkbare gegevenssets, maar hebben verschillende doelgroepen:

- De [prijs blad API](https://docs.microsoft.com/rest/api/consumption/pricesheet) biedt de aangepaste prijzen die is gebruikt voor een Enterprise-klant bent.

- De [RateCard API](/previous-versions/azure/reference/mt219005(v=azure.100)) bevat de openbare prijzen die van toepassing is voor Web Direct-klanten.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het gebruik van Azure-API's voor het programmatisch Krijg inzicht in uw Azure-gebruik [overzicht van de API van Azure-verbruik](billing-consumption-api-overview.md) en [Azure Billing API-overzicht](billing-usage-rate-card-overview.md).

- Als u wilt vergelijken in uw factuur met de gedetailleerde dagelijks gebruik van bestands- en -rapporten voor de kosten in de Azure-portal, Zie [meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-bill.md).

- Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).
