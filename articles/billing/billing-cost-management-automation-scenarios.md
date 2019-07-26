---
title: Automatiserings scenario's voor facturering en kosten beheer van Azure | Microsoft Docs
description: Meer informatie over hoe algemene facturerings-en kosten beheer scenario's worden toegewezen aan verschillende Api's.
services: billing
documentationcenter: ''
author: bandersmsft
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
ms.author: banders
ms.openlocfilehash: 3d06df9b9a90f26b39afc17b8fcd02c85da567a1
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443254"
---
# <a name="automation-scenarios-for-billing-and-cost-management"></a>Automatiserings scenario's voor facturering en kosten beheer

Dit artikel bevat algemene scenario's voor facturering en kosten beheer van Azure. Deze scenario's worden toegewezen aan Api's die u kunt gebruiken. Onder elke scenario-naar-API-toewijzing vindt u een overzicht van de Api's en de functionaliteit die ze bieden.

## <a name="common-scenarios"></a>Algemene scenario's

U kunt de facturerings-en Cost Management-Api's in verschillende scenario's gebruiken om vragen met betrekking tot kosten en gebruik te beantwoorden. Hier volgt een overzicht van algemene scenario's:

- **Factuur afstemming**: Heeft micro soft het juiste bedrag in rekening gebracht?  Wat is mijn factuur en kan ik deze zelf berekenen?

- **Cross-kosten**: Nu weet ik hoeveel er in rekening wordt gebracht, wie in mijn organisatie moet betalen?

- **Kosten optimalisatie**: Ik weet hoeveel er in rekening is gebracht. Hoe kan ik meer profiteren van het geld dat ik op Azure heb besteed?

- **Kosten bijhouden**: Ik wil zien hoeveel ik besteed aan het gebruik van Azure in de loop van de tijd. Wat zijn de trends? Hoe kan ik beter doen?

- **Azure-uitgaven gedurende de maand**: Wat is de uitgaven van mijn huidige maand tot nu toe? Moet ik wijzigingen aanbrengen in mijn uitgaven en/of het gebruik van Azure? Wanneer kan ik in de maand de meeste van Azure gebruiken?

- **Waarschuwingen**: Hoe kan ik op resources gebaseerde verbruik of op basis van financiële waarschuwingen instellen?

## <a name="scenario-to-api-mapping"></a>Koppeling van scenario naar API

|         API        | Factuur afstemming    | Cross-charges    | Kostenoptimalisatie    | Kosten bijhouden    | Midmonth-uitgaven    | Waarschuwingen    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Budgetten                     |                           |                  |           X          |                  |                    |     X     |
| Marketplace-kosten                |             X             |         X        |           X          |         X        |          X         |     X     |
| Prijzenoverzicht                 |             X             |         X        |           X          |         X        |          X         |           |
| Aanbevelingen voor reserveringen |                           |                  |           X          |                  |                    |           |
| Reserveringsdetails         |                           |                  |           X          |         X        |                    |           |
| Reserveringssamenvattingen       |                           |                  |           X          |         X        |                    |           |
| Gebruiksdetails               |             X             |         X        |           X          |         X        |          X         |     X     |
| Facturerings perioden             |             X             |         X        |           X          |         X        |                    |           |
| Facturen                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Niet-geclassificeerd gebruik               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> De toewijzing scenario-naar-API bevat geen Api's voor Enter prise-verbruik. Gebruik waar mogelijk de algemene verbruiks-Api's voor nieuwe ontwikkel scenario's.

## <a name="api-summaries"></a>API-samen vattingen

### <a name="consumption"></a>Verbruik
Web direct-en Enter prise-klanten kunnen alle volgende Api's gebruiken, tenzij anders vermeld:

-   [API](https://docs.microsoft.com/rest/api/consumption/budgets) voor budgetten (*Alleen Enter prise-klanten*): Maak kosten-of gebruiks budgetten voor resources, resource groepen of facturerings meters. Wanneer u budgetten hebt gemaakt, kunt u waarschuwingen configureren om u te waarschuwen wanneer u gedefinieerde budget drempels hebt overschreden. U kunt ook acties configureren die moeten worden uitgevoerd wanneer u budget bedragen hebt bereikt.

-   [API voor Marketplace-kosten](https://docs.microsoft.com/rest/api/consumption/marketplaces): Ontvang kosten-en gebruiks gegevens op alle Azure Marketplace-resources (Azure-partner aanbiedingen). U kunt deze gegevens gebruiken om kosten toe te voegen aan alle resources in Marketplace of om de kosten/het gebruik van specifieke bronnen te onderzoeken.

-   [Prijs lijst-API](https://docs.microsoft.com/rest/api/consumption/pricesheet) (*Alleen Enter prise-klanten*): Aangepaste prijzen voor alle meters ophalen. Ondernemingen kunnen deze gegevens gebruiken in combi natie met gebruiks gegevens en informatie over het gebruik van Marketplace om de kosten te berekenen door gebruik te maken van gebruiks-en Marketplace-gegevens. 

-   [API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations)voor reserverings aanbevelingen: Krijg aanbevelingen voor het kopen van gereserveerde VM-instanties. Aanbevelingen helpen u verwachte kosten besparingen en aankoop bedragen te analyseren. Zie [api's voor automatisering van Azure-reserve ringen](billing-reservation-apis.md)voor meer informatie.

-   [Reserverings details API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails): Bekijk informatie over eerder gekochte VM-reserve ringen, zoals hoeveel verbruik is gereserveerd en hoeveel er wordt gebruikt. U kunt gegevens weer geven op Details per VM-niveau. Zie [api's voor automatisering van Azure-reserve ringen](billing-reservation-apis.md)voor meer informatie.

-   [Reserverings samenvattingen API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries): Bekijk geaggregeerde gegevens over VM-reserve ringen die uw organisatie heeft gekocht, zoals hoeveel verbruik is gereserveerd en hoeveel er in de samen voeging wordt gebruikt. Zie [api's voor automatisering van Azure-reserve ringen](billing-reservation-apis.md)voor meer informatie.

-   [Gebruiks Details-API](https://docs.microsoft.com/rest/api/consumption/usagedetails): Ontvang kosten-en gebruiks gegevens voor alle Azure-resources van micro soft. De gegevens hebben de vorm van detail records voor gebruik, die momenteel per meter per dag worden verzonden. U kunt de informatie gebruiken om de kosten voor alle resources toe te voegen of om de kosten/het gebruik van specifieke resources te onderzoeken.

-   [RateCard-API](/previous-versions/azure/reference/mt219005(v=azure.100)): Ontvang de snelheid van de meter als u een web direct-klant bent. U kunt vervolgens de geretourneerde informatie gebruiken met de informatie over het resource gebruik om de verwachte factuur hand matig te berekenen. 

-   Niet-geclassificeerde [gebruiks-API](/previous-versions/azure/reference/mt219003(v=azure.100)): Gegevens over onbewerkte gebruik ophalen voordat een meting/kosten worden berekend door Azure.

### <a name="billing"></a>Billing
-   [API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods)voor facturerings perioden: Bepaal welke facturerings periode u wilt analyseren, samen met de factuur-Id's voor die periode. U kunt factuur-Id's gebruiken met de facturerings-API.

-   [API voor facturen](https://docs.microsoft.com/rest/api/billing/2018-11-01-preview/invoices): Down load de download-URL voor een factuur voor een facturerings periode in PDF-vorm.

### <a name="enterprise-consumption"></a>Bedrijfs verbruik
De volgende Api's gelden alleen voor bedrijven:

-   [API voor saldo overzicht](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary): Ontvang een maandelijks overzicht van de informatie over saldi, nieuwe aankopen, Azure Marketplace-service kosten, aanpassingen en overschrijding kosten. U kunt deze informatie ophalen voor de huidige facturerings periode of een periode in het verleden. Ondernemingen kunnen deze gegevens gebruiken om te vergelijken met hand matig berekende samenvattings kosten. Deze API biedt geen specifieke informatie over resources of een geaggregeerde weer gave van kosten.

-   [Gebruiks Details-API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail): Informatie over Azure-gebruik (van micro soft-aanbiedingen) ontvangen voor de huidige maand, een specifieke facturerings periode of een aangepaste datum periode. Ondernemingen kunnen deze gegevens gebruiken om facturen hand matig te berekenen op basis van het aantal en het verbruik. Ondernemingen kunnen ook afdelings-en organisatie gegevens gebruiken om de kosten van verschillende organisaties te beperken. De gegevens bevatten een resource-specifieke weer gave van het gebruik/de kosten.

-   Berekenings- [API voor Marketplace-opslag](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge): Informatie over Azure-gebruik (van partner aanbiedingen) ophalen voor de huidige maand, een specifieke facturerings periode of een aangepaste datum periode. Ondernemingen kunnen deze gegevens gebruiken om facturen hand matig te berekenen op basis van het aantal en het verbruik. Ondernemingen kunnen ook afdelings-en organisatie gegevens gebruiken om de kosten van verschillende organisaties te beperken. Deze API biedt een resource-specifieke weer gave van het gebruik/de kosten.

-   [API voor prijzen overzicht](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet): Het toepasselijke tarief voor elke meter voor de opgegeven inschrijvings-en facturerings periode ophalen. U kunt deze frequentie gegevens gebruiken in combi natie met gebruiks gegevens en informatie over het gebruik van Marketplace om de verwachte factuur hand matig te berekenen.

-   [API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods)voor facturerings perioden: Een lijst met facturerings perioden ophalen. De API biedt u ook een eigenschap die verwijst naar de API-route voor de vier sets Enter prise API-gegevens die betrekking hebben op de facturerings periode: BalanceSummary, UsageDetails, Marketplace-kosten en prijzen overzicht.

-   [API voor gereserveerde instanties](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation): Bekijk 7 dagen, 30 dagen of 60 dagen aan gebruik van virtuele machines en ontvang enkele en gedeelde inkoop aanbevelingen. U kunt deze API gebruiken om verwachte kosten besparingen en aanbevolen aankoop bedragen te analyseren. Zie [api's voor automatisering van Azure-reserve ringen](billing-reservation-apis.md)voor meer informatie.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="whats-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>Wat is het verschil tussen de Enter prise Reporting Api's en de verbruiks-Api's? Wanneer moet ik elk gebruiken?
Deze Api's hebben een vergelijk bare set functionaliteit en kunnen dezelfde uitgebreide set vragen beantwoorden in de facturerings-en kosten beheer ruimte. Maar ze zijn gericht op verschillende doel groepen: 

- Enter prise Reporting Api's zijn beschikbaar voor klanten die een Enterprise Agreement met micro soft hebben ondertekend waarmee ze toegang krijgen tot onderhandelde monetaire toezeg gingen en aangepaste prijzen. De Api's vereisen een sleutel die u van de [Enterprise Portal](https://ea.azure.com)kunt verkrijgen. Zie [overzicht van rapportage-api's voor zakelijke klanten](billing-enterprise-api.md)voor een beschrijving van deze api's.

- Verbruiks-Api's zijn beschikbaar voor alle klanten, met enkele uitzonde ringen. Zie overzicht van Azure- [verbruiks-API](billing-consumption-api-overview.md) en de [naslag](https://docs.microsoft.com/rest/api/consumption/)informatie voor Azure-verbruiks-API. We raden de meegeleverde Api's aan als de oplossing voor de nieuwste ontwikkelings scenario's. 

### <a name="whats-the-difference-between-the-usage-details-api-and-the-usage-api"></a>Wat is het verschil tussen de API voor gebruiks gegevens en de gebruiks-API?
Deze Api's bieden fundamenteel verschillende gegevens:

- De [gebruiks gegevens-API](https://docs.microsoft.com/rest/api/consumption/usagedetails) biedt Azure-gebruik en kosten gegevens per meter exemplaar. De opgegeven gegevens zijn al door gegeven via het systeem voor kosten meting in Azure en er zijn kosten op toegepast, samen met andere mogelijke wijzigingen:

   - Wijzigingen in rekening voor het gebruik van vooraf betaalde monetaire toezeg gingen
   - Wijzigingen in het account voor gebruiks verschillen die door Azure zijn gedetecteerd

- De [gebruiks-API](/previous-versions/azure/reference/mt219003(v=azure.100)) biedt onbewerkte gegevens over Azure-gebruik voordat deze het systeem voor de kosten meting in azure passeren. Deze gegevens hebben mogelijk geen correlatie met het gebruik of de berekenings hoeveelheid die na het Azure-meet berekenings systeem wordt weer gegeven.

### <a name="whats-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>Wat is het verschil tussen de API voor factuur en gebruiks gegevens?
Deze Api's bieden een andere weer gave van dezelfde gegevens:

- De [factuur-API](https://docs.microsoft.com/rest/api/billing/2018-11-01-preview/invoices) is alleen voor web direct-klanten. Het biedt een maandelijks Rollup van uw factuur op basis van de cumulatieve kosten voor elk meter type. 

- De [gebruiks gegevens-API](https://docs.microsoft.com/rest/api/consumption/usagedetails) biedt een gedetailleerd overzicht van de gebruiks-en kosten records voor elke dag. Zakelijke en Web direct-klanten kunnen deze gebruiken.

### <a name="whats-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>Wat is het verschil tussen de prijzen voor het prijs overzicht en de RateCard-API?
Deze Api's bieden vergelijk bare gegevens sets, maar hebben verschillende doel groepen:

- De [prijs lijst-API](https://docs.microsoft.com/rest/api/consumption/pricesheet) biedt de aangepaste prijzen die zijn onderhandeld voor een Enter prise-klant.

- De [RateCard-API](/previous-versions/azure/reference/mt219005(v=azure.100)) biedt de open bare prijzen die van toepassing zijn op Web direct-klanten.

## <a name="next-steps"></a>Volgende stappen

- Zie overzicht van [Azure Usage API](billing-consumption-api-overview.md) en [Azure billing API overview](billing-usage-rate-card-overview.md)(Engelstalig) voor meer informatie over het gebruik van Azure api's om programmatisch inzicht te krijgen in uw Azure-gebruik.

- Zie [inzicht in uw factuur voor Microsoft Azure](billing-understand-your-bill.md)om uw factuur te vergelijken met het gedetailleerde dagelijks gebruiks bestand en de kosten beheer rapporten in de Azure Portal.

- Als u vragen hebt of hulp nodig hebt, kunt u [een ondersteunings aanvraag maken](https://go.microsoft.com/fwlink/?linkid=2083458).
