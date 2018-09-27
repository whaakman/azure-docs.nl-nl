---
title: Azure-facturering en kosten beheerscenario's automatiseren | Microsoft Docs
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
ms.openlocfilehash: 97f6339e5c9532250245a97a81dade12db139e98
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227364"
---
# <a name="billing-and-cost-management-automation-scenarios"></a>Facturering en de kosten management automation-scenario 's

Algemene scenario's voor de facturering en de kosten management ruimte worden hieronder aangegeven en toegewezen aan verschillende API's die kunnen worden gebruikt in deze scenario's. Een overzicht van alle beschikbare API's en de functionaliteit die ze bieden, kunt u vinden onder het scenario voor de API-toewijzing. 

## <a name="common-scenarios"></a>Algemene scenario's

U kunt de facturering en beheer-API's in een verscheidenheid aan scenario's voor het antwoord kosten en gebruik vragen.  Hieronder vindt u een overzicht van veelvoorkomende scenario's.

- **Factuur afstemming** -heeft Microsoft kosten in rekening gebracht mij de juiste hoeveelheid?  Wat is mijn factuur en kan ik berekenen zelf?

- **Cross-kosten in rekening gebracht** - nu dat ik hoeveel ik ben wordt in rekening gebracht weten, wie in mijn organisatie nodig heeft om te betalen?

- **Kosten van optimalisatie** -weet ik hoeveel ik hebt gebracht, maar hoe vind ik meer uit het geld ik besteed aan Azure?

- **Kosten bijhouden** : ik wil zien hoeveel ik ben uitgaven en met behulp van Azure na verloop van tijd. Wat zijn de trends? Hoe kan ik doen beter?

- **Azure-uitgaven gedurende de maand** – hoeveel is mijn huidige maand tot heden de uitgaven? Moet ik breng de wijzigingen in mijn uitgaven en/of gebruik van Azure? Wanneer tijdens de maand ben ik verbruikt Azure het meest?

- **Waarschuwingen instellen** : ik wil graag om in te stellen op basis van een resource of op uw monetaire op basis van waarschuwingen.

## <a name="scenario-to-api-mappings"></a>Scenario voor API-toewijzingen

|         API/Scenario        | Factuurafstemming    | Cross-kosten in rekening gebracht    | Kosten optimaliseren    | Kosten bijhouden    | Uitgaven halverwege de maand    | Waarschuwingen    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Budgetten                     |                           |                  |           X          |                  |                    |     X     |
| Marktplaatsen                |             X             |         X        |           X          |         X        |          X         |     X     |
| Prijzenoverzicht                 |             X             |         X        |           X          |         X        |          X         |           |
| Reservering-aanbevelingen |                           |                  |           X          |                  |                    |           |
| Reserveringsdetails         |                           |                  |           X          |         X        |                    |           |
| Samenvattingen van de reservering       |                           |                  |           X          |         X        |                    |           |
| Gebruiksgegevens               |             X             |         X        |           X          |         X        |          X         |     X     |
| Facturering-punten             |             X             |         X        |           X          |         X        |                    |           |
| Facturen                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Niet-geclassificeerde gebruik               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> Het scenario voor API-toewijzingen onderstaande omvatten niet de Enterprise-verbruik-API's. Waar mogelijk, neem de algemene verbruik API's gebruiken om netto nieuwe ontwikkelscenario's vooruit op te lossen.

## <a name="api-summaries"></a>Samenvattingen van de API

### <a name="consumption"></a>Verbruik
(*Web Direct- en Enterprise-klanten voor alle API's met uitzondering van die met de naam uit onder*)

-   **Budgetten** (*alleen Enterprise-klanten*): Gebruik de [budgetten API](https://docs.microsoft.com/rest/api/consumption/budgets) budgetten voor kosten of gebruik voor resources, resourcegroepen of factureringsmeters maken.  Nadat u hebt gemaakt budgetten, kan waarschuwingen worden geconfigureerd om te melden wanneer de gebruiker gedefinieerde budgetdrempels zijn overschreden. Acties kunnen ook worden geconfigureerd moet plaatsvinden wanneer budgetbedragen zijn bereikt.
-   **Marktplaatsen**: Gebruik de [Marketplace-kosten API](https://docs.microsoft.com/rest/api/consumption/marketplaces) voor kosten in rekening gebracht en gebruiksgegevens over alle Marketplace-resources (Azure 3e partij aanbiedingen). Deze gegevens kan worden gebruikt om de kosten oplopen voor alle Marketplace-resources of onderzoeken kosten/gebruik op een specifieke resource (s).
-   **Prijslijst** (*alleen Enterprise-klanten*): Enterprise-klanten kunnen gebruikmaken van de [prijs blad API](https://docs.microsoft.com/rest/api/consumption/pricesheet) om op te halen van de aangepaste prijzen voor alle meters. Ondernemingen kunnen deze gegevens gebruiken in combinatie met gebruik van gegevens en marktplaatsen gebruiksinformatie om uit te voeren van de berekening van de kosten met behulp van gebruiks-en marketplace. 
-   **Aanbevelingen voor reservering**: Gebruik de [reservering Recommendations-API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations) aanbevelingen ophalen voor het kopen van gereserveerde VM-instanties. Aanbevelingen zijn ontworpen om te kunnen klanten voor het analyseren van verwachte kosten te besparen en inkoop bedragen.
-   **Details van de reservering**: Gebruik de [reservering Details API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails) voor informatie over eerder aangeschafte VM-reserveringen, zoals hoeveel verbruik is gereserveerd en hoeveel daadwerkelijk wordt gebruikt. U kunt data-at-per-VM-niveau detail zien.
-   **Samenvattingen van de reservering**: Gebruik de [reservering samenvattingen API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries) voor statistische gegevens over eerder aangeschafte VM-reserveringen, zoals hoeveel verbruik versus hoeveel daadwerkelijk wordt gebruikt is gereserveerd in de statistische functie. 
-   **Informatie over het gebruik**: Gebruik de [Usage Details API](https://docs.microsoft.com/rest/api/consumption/usagedetails) voor kosten in rekening gebracht en het gebruik over alle Azure 1e partij resources. Informatie is in de vorm van detail gebruiksrecords, die momenteel worden uitgezonden, één keer per meter per dag. Gegevens kunnen worden gebruikt om toevoegen om de kosten voor alle resources of onderzoeken kosten/gebruik op een specifieke resource (s).
-   **RateCard**: Web Direct-klanten kunnen gebruikmaken van de [RateCard API](https://msdn.microsoft.com/library/azure/mt219005.aspx) om op te halen van de meter-tarieven. Kan vervolgens worden gebruikt de geretourneerde gegevens met hun informatie over het gebruik van de resource voor het handmatig berekenen verwachte factuur. 
-   **Gebruik zonder restricties**: kunt u de [niet-geclassificeerde Usage API](https://msdn.microsoft.com/library/azure/mt219003.aspx) verkrijgen van informatie over het gebruik van onbewerkte, voordat u een meting/kosten in rekening gebracht uitgevoerd door Azure.

### <a name="billing"></a>Billing
-   **Facturering perioden**: Gebruik de [Billing perioden API](https://docs.microsoft.com/rest/api/billing/billingperiods) om te bepalen van een factureringsperiode kunt analyseren, samen met de factuur voor die periode-id's. Factuur-id's kunnen worden gebruikt met de onderstaande factuur-API. 
-   **Facturen**: Gebruik de [facturen API](https://docs.microsoft.com/rest/api/billing/invoices) om op te halen van de download-URL voor een factuur voor een bepaalde factureringsperiode in PDF-formulier.

### <a name="enterprise-consumption"></a>Enterprise-verbruik
*(Alle API's voor Enterprise alleen)*

-   **Saldo-overzicht**: Gebruik de [saldo samenvatting API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) een maandelijkse samenvatting van informatie over de saldi, aankopen in de nieuwe Azure Marketplace servicekosten, aanpassingen en overschrijdingskosten ophalen. U kunt deze informatie voor de huidige factureringsperiode of een punt in het verleden. Ondernemingen kunnen deze gegevens gebruiken om uit te voeren van een vergelijking met handmatig berekende samenvatting kosten in rekening gebracht. Deze API biedt geen resource-specifieke informatie of een samengevoegde weergave van de kosten.
-   **Informatie over het gebruik**: Gebruik de [Usage Details API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) om op te halen van de 1e partij gebruik van Azure gedetailleerde informatie over de huidige maand, een bepaalde factureringsperiode of een aangepaste periode. Ondernemingen deze gegevens kunnen gebruiken voor het handmatig berekenen factuur op basis van de snelheid en het verbruik en kunnen ook informatie over afdeling/organisatie die aanwezig zijn voor het kenmerk kosten tussen verschillende organisaties. De gegevens biedt een overzicht van de resource-specifieke van gebruik/kosten.
-   **Marketplace store kosten in rekening gebracht**: Gebruik de [Marketplace Store kosten in rekening gebracht API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) om op te halen 3e partij gebruik van Azure gedetailleerde informatie over de huidige maand, een bepaalde factureringsperiode of een aangepaste periode. Ondernemingen deze gegevens kunnen gebruiken voor het handmatig berekenen factuur op basis van de snelheid en het verbruik en kunnen ook informatie over afdeling/organisatie die aanwezig zijn voor het kenmerk kosten tussen verschillende organisaties. De kosten voor Marketplace store API biedt een overzicht van de resource-specifieke van gebruik/kosten.
-   **Prijslijst**: de [prijs blad API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) biedt van het tarief voor elke Meter voor de opgegeven inschrijving en factureringsperiode. Deze snelheid informatie kan worden gebruikt in combinatie met gebruik van gegevens en marktplaatsen gebruiksinformatie handmatig verwachte om factuur te berekenen.
-   **Facturering perioden**: Gebruik de [Billing perioden API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) voor een lijst van factureringsperioden samen met een eigenschap die verwijst naar de API-route voor de vier sets Enterprise API-gegevens die betrekking op de factureringsperiode - BalanceSummary hebben, UsageDetails, Marketplace-kosten en prijslijst.
-   **Aanbevelingen voor Azure-reservering**: de [gereserveerde instantie Recommendations-API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation) kijkt naar klant 7 dagen, 30 dagen of 60 dagen van het gebruik van virtuele machines en aanbevelingen voor één en gedeelde aankoop biedt. De gereserveerde instantie die API kan klanten voor het analyseren van verwachte kosten te besparen en aanbevolen hoeveelheden aankoop.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="what-is-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>Wat is het verschil tussen de Enterprise Reporting API's en de API-verbruik? Wanneer moet ik elke gebruiken?
Deze API's hebben een aantal soortgelijke functionaliteit en de dezelfde breed scala aan vragen in de ruimte facturering en cost management kunnen beantwoorden. Elke API zijn echter bedoeld voor verschillende doelgroepen: 

- **Enterprise Reporting API's**: deze API's zijn beschikbaar voor klanten die zich hebben aangemeld voor een Enterprise Agreement van Microsoft die deze toegang tot in de onderhandelde monetaire toezeggingen en aangepaste prijzen verleent. De API's vereisen een sleutel te gebruiken die kan worden verkregen via de [Enterprise Portal](https://ea.azure.com). Zie voor een beschrijving van deze API's, [overzicht van de rapportage-API's voor zakelijke klanten](billing-enterprise-api.md).

- **Gebruik API's**: deze API's zijn beschikbaar voor alle klanten, met een paar uitzonderingen. Zie voor meer informatie, [overzicht van Azure-verbruik API](billing-consumption-api-overview.md) en de [Azure-verbruik API-verwijzingen](https://docs.microsoft.com/rest/api/consumption/). De opgegeven API's zijn de aanbevolen oplossing voor de meest recente ontwikkelscenario's. 

### <a name="what-is-the-difference-between-the-usage-details-api-and-the-usage-api"></a>Wat is het verschil tussen de API van de Details van gebruik en de Usage-API?
Deze API's bieden fundamentele verschillende gegevens:

- **Informatie over het gebruik**: de [Usage Details API](https://docs.microsoft.com/rest/api/consumption/usagedetails) biedt Azure gebruiks- en -gegevens per exemplaar van de meter. De verstrekte gegevens al doorgegeven via de Azure-kosten meetsysteem en had kosten toegepast op deze samen met andere mogelijke wijzigingen:

    - Wijzigingen in rekening voor het gebruik van vooraf betaalde monetaire toezeggingen
    - Wijzigingen in voor gebruik discrepanties gedetecteerd door Azure-account

- **Gebruik**: de [Usage API](https://msdn.microsoft.com/library/Mt219003.aspx) biedt informatie over het gebruik van onbewerkte Azure voordat deze wordt doorgegeven via Azure kosten voor softwarelicentiecontrole system. Deze gegevens mogelijk geen een correlatie met het gebruik en/of kosten in rekening gebracht bedrag dat wordt weergegeven nadat de door Azure kosten in rekening gebracht meetsysteem.

### <a name="what-is-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>Wat is het verschil tussen de factuur-API en de Details Usage API?
Deze API's bieden een andere weergave van dezelfde gegevens. De [factuur API](https://docs.microsoft.com/rest/api/billing/invoices) is voor Web Direct klanten alleen en biedt een maandelijkse roll up van uw factuur op basis van de totale kosten in rekening gebracht voor elk metertype. De [Usage Details API](https://docs.microsoft.com/rest/api/consumption/usagedetails) biedt een gedetailleerde weergave van de gebruikskosten /-records voor elke dag en kan worden gebruikt door Enterprise en het Web Direct-klanten.

### <a name="what-is-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>Wat is het verschil tussen de prijs blad API en de RateCard API?
Deze API's bieden vergelijkbare gegevenssets, maar hebben verschillende doelgroepen. Onderstaande informatie.

- Prijs blad API: De [prijs blad API](https://docs.microsoft.com/rest/api/consumption/pricesheet) biedt de aangepaste prijzen die voor een Enterprise-klant is onderhandeld.
- RateCard API: De [RateCard API](https://msdn.microsoft.com/library/mt219005.aspx) biedt de prijzen die openbaar is van toepassing op Web Direct-klanten.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het gebruik van Azure-API's voor het programmatisch Krijg inzicht in uw Azure-gebruik [API-overzicht van Azure-verbruik](billing-consumption-api-overview.md) en [Azure Billing API-overzicht](billing-usage-rate-card-overview.md).
- Als u wilt vergelijken in uw factuur met de gedetailleerde dagelijks gebruik van bestands- en -rapporten voor de kosten in de Azure-portal, Zie [meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-bill.md)
- Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
