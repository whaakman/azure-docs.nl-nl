---
title: Migreren van Enterprise Agreement naar de klant van Microsoft-overeenkomst API's - Azure | Microsoft Docs
description: In dit artikel krijgt u inzicht in de impact op API's wanneer u van een Microsoft Enterprise Agreement (EA) naar een KLANTOVEREENKOMST van Microsoft migreert.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/02/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: a12360e60026fd4251cc232caccd1bc52a34d2ff
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57344781"
---
# <a name="migrate-from-enterprise-agreement-to-microsoft-customer-agreement-apis"></a>Migreren van Enterprise Agreement naar de klant van Microsoft-overeenkomst API 's

In dit artikel krijgt u inzicht in de impact op API's wanneer u van een Microsoft Enterprise Agreement (EA) naar een KLANTOVEREENKOMST van Microsoft migreert. De volgende nieuwe API's worden gebruikt om kosten en prijzen weer te geven:

- Prijzenlijst-API voor klant van Microsoft-overeenkomsten
  - Prijslijst per factuur
  - Prijslijst door facturering-profiel
  - Prijslijst voor de open factureringsperiode
- Informatie over API voor klant van Microsoft-overeenkomsten
  - Informatie over het gebruik per factuur voor facturering-profiel (filteren op factuur sectie)
    - Informatie over het gebruik door gebruiksdatum
    - Informatie over het gebruik van het factureringsaccount
    - Informatie over het gebruik voor facturering-profiel
    - Informatie over het gebruik voor factuur-sectie
    - Informatie over het gebruik voor het abonnement
  - Informatie over het gebruik van maand tot vandaag
    - Informatie over het gebruik van het factureringsaccount
    - Informatie over het gebruik voor facturering-profiel
    - Informatie over het gebruik voor factuur-sectie
    - Informatie over het gebruik voor het abonnement
  - Informatie over het gebruik van de factureringsperiode
- API-tegoed voor de klant van Microsoft-overeenkomst
  - Huidige saldo samenvatting API door facturering-profiel
  - Tegoed gebeurtenissen API door facturering-profiel
  - Veel API door facturering profiel tegoed
- Query-API
- Kosten voor API
  - Kosten voor API door factureringsaccount
  - Kosten voor API door facturering-profiel
  - Kosten voor API voor factuur-sectie
- Factuurlijst API
  - Facturering-profiel
  - Facturering-account
- Reserverings-API's
- API voor Power BI-integratie
- Uitvoer-API
  - Facturering-account
  - Facturering-profiel
  - Factuur-sectie
- Transactie-API
- Budgetten API
- Samengevoegde kosten voor API

## <a name="effect-on-automation"></a>Invloed op automation

De API's gebruikt met een Microsoft-KLANTOVEREENKOMST verschillen van die gebruikt in combinatie met een Enterprise Agreement. U mogelijk hebt ingesteld API-automatisering eerder met behulp van enterprise-sleutel op basis van API's of op basis van een Azure Resource Manager-API's voor enterprise-inschrijvingen. Als dit het geval is, moet u de automation-configuratie voor nieuwere API's die worden gebruikt in combinatie met Microsoft-klant overeenkomsten bijwerken.

Zie voor een lijst met oudere, niet-ondersteunde API door Microsoft Customer overeenkomsten [niet-ondersteunde API's](#unsupported-apis).

API's voor Microsoft-klant overeenkomsten ondersteunen alleen de Azure Resource Manager-verificatie: ze bieden geen ondersteuning voor API-sleutel gebaseerde verificatie.

## <a name="pricesheet-apis-show-prices-for-azure-services"></a>Pricesheet APIs weergeven prijzen voor Azure-services

De APIs Pricesheet voor KLANTOVEREENKOMST van Microsoft, zijn alle services die zijn beoordeeld voor facturering, informatie over de prijzen gebaseerd op verbruikte hoeveelheid. De API bevat geen recht aankopen. Bijvoorbeeld, waar u recht op aankopen op Marketplace, prijzen voor gereserveerde instanties en support-aanbiedingen. De API biedt de unieke prijs voor elke service voor een specifieke meter en productnaam volgorde.

### <a name="view-the-pricesheet-for-azure-consumption-services-by-billing-profile"></a>De prijslijst voor services van Azure-verbruik door facturering profiel weergeven

Prijzen voor alle services voor eerste en de externe Azure-verbruik vindt u in de prijslijst voor een profiel voor facturering. De API gebruiken om de prijzen voor Azure-verbruik services weer te geven. De prijzen weergegeven voor de factureringsprofiel gelden voor alle abonnementen die deel uitmaken van het profiel voor facturering.

- CSV-indeling
  - VERZENDEN -`https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&startDate=2019-01-01&endDate=2019-01-31& format=csv`
- JSON-indeling
  - POST
    - `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&startDate=2019-01-01&endDate=2019-01-31&format=json`

### <a name="view-current-open-billing-or-service-period-estimated-prices"></a>Weergave van de huidige open facturering of -service de geschatte periode prijzen

Als u wilt weergeven van de geschatte prijzen voor elke service in de huidige open factureringscyclus of service-punt, kunt u de prijslijst weergeven in CSV of JSON-indeling.

- CSV-indeling
  - POST
    - `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billing AccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&format=csv`
- JSON-indeling
  - POST
    - `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billing AccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&format=json`

Prijzen die wordt weergegeven in de prijslijst voor de huidige serviceperiode altijd zijn de meest recente prijzen voor de meter. Prijzen worden dagelijks vernieuwd met behulp van de meest recente prijzen voor de volgorde van de meter en product.

Bij het beoordelen van kosten op basis van de prijs van de open periode, worden de kosten worden geschat. De laatste vergrendelde koers aan het einde van de open-service-periode geldt voor gebruik in de open-periode.

De open periode is de huidige serviceperiode waar het gebruik nog niet nog beoordeeld voor kosten en facturering.

### <a name="view-invoice-by-billing-profile-prices"></a>Factuur weergeven door het profiel prijzen facturering

U kunt de prijzen voor Azure-verbruik-services die worden in rekening gebracht weergeven in een specifieke factuur voor een profiel voor facturering. Gebruik de API prijzen alleen voor de services die worden vermeld in de factuur weergeven. De factuur wordt aangeduid met het nummer van de factuur.

- CSV-indeling
  - POST
    - `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/2909cffc-b0a2-5de1-bb7b-5d3383764184/billingProfiles/2dcffe0c-ee92-4265-8647-515b8fe7dc78/invoices/{invoiceId}/pricesheet/default/download?api-version=2018-11-01-preview&format=csv`

- JSON-indeling
  - POST
    - `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/2909cffc-b0a2-5de1-bb7b-5d3383764184/billingProfiles/2dcffe0c-ee92-4265-8647-515b8fe7dc78/invoices/{invoiceId}/pricesheet/default/download?api-version=2018-11-01-preview&format=json`

De `{invoiceId}` vermeld eerder is hetzelfde als de **factuurnummer** wordt weergegeven in de factuur samenvatting PDF-bestand. Hier volgt een voorbeeld.

![Van de voorbeeldafbeelding met het nummer van de factuur die overeenkomt met aan de InvoiceId](./media/migrate-cost-management-api/invoicesummary.png)

## <a name="pricesheet-api-asynchronous-response"></a>Prijslijst API asynchroon antwoord

De API's ondersteunen de [Azure REST asynchrone](../azure-resource-manager/resource-manager-async-operations.md) indeling. Nadat u de met behulp van GET-API's aanroepen, ziet u het volgende antwoord:

```
            No Response Body
            HTTP Status 202 Accepted
```

 De volgende headers worden verzonden door de locatie van de uitvoer.

```
Location: https://management.azure.com/providers/Microsoft.Consumption/operationresults/{operationId}?sessiontoken=XZDFSnvdkbkdsb==
Azure-AsyncOperation: https://managment.azure.com/providers/Microsoft.Consumption/operationStatus/{operationId}?sessiontoken=XZDFSnvdkbkdsb==
   Retry-After: 10
 OData-EntityId: {operationId}
```

De client kunt maken van een GET-aanroep naar de locatie. Het antwoord op de GET-aanroep is hetzelfde als de voorbeeldinformatie eerder weergegeven totdat een status is voltooid of mislukt de bewerking is bereikt. Wanneer dit is voltooid, wordt het antwoord op de aanroep voor het ophalen van locatie het dezelfde antwoord geretourneerd als de bewerking is uitgevoerd op hetzelfde moment. Hier volgt een voorbeeld.

```
HTTP Status 200
i.e.
            {
    “id”: “providers/Microsoft.Consumption/operationresults/{operationId}”,
    “name”: {operationId},
    “type”: “Microsoft.Consumption/operationResults”,
    “properties” : {
          “downloadUrl”: {urltoblob},
          “vaildTill”: “Date”
}
}
```

Daarnaast de client kan ook maken een GET-aanroep voor de `Azure-AsyncOperation`. Het eindpunt wordt de status voor de bewerking. Hier volgt een voorbeeld:

```
HTTP Status 200
               {
"id": “providers/Microsoft.Consumption/operationStatus/{operationId}",
"name": "operationId",
"status" : "Succeeded | Failed | Canceled",
"startTime": "<DateLiteral per ISO8601>",
"endTime": "<DateLiteral per ISO8601>",
"percentComplete": <double between 0 and 100>
"properties": {
    /\* The resource provider can choose the values here, but it should only be
       returned on a successful operation (status being "Succeeded"). \*/
},
"error" : {
    /\* This is the OData v4 format, used by the RPC and will go into the
     v2.2 Azure REST API guidelines \*/
    "code": "BadArgument",
    "message": "The provided database &#39;foo&#39; has an invalid username."
}
}
```

### <a name="pricesheet-fields"></a>Velden van de prijslijst

Lees de informatie op [meer informatie over de voorwaarden in de prijslijst voor een Microsoft-KLANTOVEREENKOMST](../billing/billing-mca-understand-pricesheet.md) om alle velden van de prijslijst en de bijbehorende beschrijvingen weer te geven.

De prijs van het gebruik is vergrendeld voor informatie over prijzen op basis van gebruik voor de duur van de service sinds het begin van de periode. Prijs van de service is toegepast en vergrendeld op de datum van aankoop. De serviceperiode is meestal een kalendermaand voor Azure-services.

De prijs voor een Azure-verbruik-service in een Microsoft-overeenkomst voor de klant is uniek voor een productieorder en een meter.

De velden die u gebruiken kunt voor het aanduiden van de prijs voor Azure-verbruik services zijn:

- productOrderName
- meterId (ook meterName)

De prijs voor overeenkomsten voor Microsoft-klanten is anders dan Enterprise agreements waarbij de prijs voor services in de inschrijving is uniek voor gedefinieerd:

- Product
- PartNumber
- meter
- aanbieding

### <a name="pricesheet-support"></a>Ondersteuning van de prijslijst

De prijslijst API ondersteunt momenteel alleen Microsoft KLANTOVEREENKOMST prijzen voor Azure eigen en externe verbruik services. Het biedt geen ondersteuning voor de prijzen voor Marketplace-services, gereserveerde instanties en Azure Hybrid voordelen (AHUB).

De API biedt alleen de prijs per eenheid die in rekening op basis van de service gebracht wordt. Deze prijs is gelijk aan of lager is dan de overeengekomen prijs.

## <a name="query-for-usage-and-costs"></a>Query voor het gebruik en kosten

De Usage-Details-API biedt toegang tot Azure en gebruik van Marketplace-service van derden en kosten. Prijzen zijn gebaseerd op de overeengekomen prijzen voor uw overeenkomst. EA-klanten verplaatsen naar een KLANTOVEREENKOMST moeten informatie over het gebruik van het factureringsaccount of facturering profiel bereik gebruiken. U kunt de query op een bereik, met de volgende uitzonderingen zijn:

|   | KLANTOVEREENKOMST | Enterprise Agreement | Afzonderlijke overeenkomst (gebruik) |
| --- | --- | --- | --- |
| Factureringsaccount | Ja | Ja | N/A |
| Facturering-profielen (factuur) | Ja | N.v.t. Zie factureringsaccount. | N.v.t. Zie abonnementen. |
| Factuursecties | Nee. Facturering profielen met een filter gebruiken. | N/A | N/A |
| EA-afdelingen | N/A | Ja | N/A |
| Accounts voor EA-inschrijving | N/A | Ja | N.v.t. Zie abonnementen. |
| Beheergroepen | Nee. Facturering-account gebruiken. | Ja | Ja |
| Abonnementen | Ja | Ja | Ja |
| Resourcegroepen | Nee. Abonnementen met een filter gebruiken. | Nee. Abonnementen met een filter gebruiken. | Nee. Abonnementen met een filter gebruiken. |

De eigenschappen die zijn geretourneerd door UsageDetails per overeenkomsttype zijn verschillend. Zie [UsageDetails API-documentatie](/rest/api/consumption/usagedetails/) voor een volledige lijst.

### <a name="power-bi-integration"></a>Integratie met Power BI

EA-klanten verplaatsen naar een Microsoft-KLANTOVEREENKOMST moeten worden gestopt met behulp van een bestaande integratie met Power BI met Microsoft Azure Consumption Insights met behulp van de gegevens van hun EA. U kunt in plaats daarvan het volgende gebruiken:

- Power BI Desktop – maken nieuwe Power BI-rapporten voor de Microsoft Customer-overeenkomst met de [Azure Cost Management connector](https://docs.microsoft.com/power-bi/desktop-connect-azure-consumption-insights).
- Power BI-Service – verplaatsen uit de [Microsoft Azure Consumption Insights](https://docs.microsoft.com/power-bi/service-connect-to-azure-consumption-insights) -inhoudspakket naar de app Azure Cost Management (KLANTOVEREENKOMST) op [AppSource](https://appsource.microsoft.com/product/power-bi/pbi_azureconsumptioninsights.pbi-azure-consumptioninsights?tab=overview).

## <a name="unsupported-apis"></a>Niet-ondersteunde API 's

De volgende API's voor Enterprise-overeenkomst worden niet ondersteund door Microsoft-klant-overeenkomsten. Alternatieven voor niet-ondersteunde API's worden beschreven.

[Prijslijst door abonnement](/rest/api/consumption/pricesheet/get/) wordt niet ondersteund. De tabel wordt de prijzen voor het bereik van een abonnements-id. In plaats daarvan kunt u **prijslijst door facturering profiel**. De prijs van een service in het bereik van een abonnement is hetzelfde als de prijs in het bereik van een profiel voor facturering. De gebruiker die de API aanroept, moet machtigingen zijn vereist.

**Prijs blad - ophalen door factureringsperiode** wordt niet ondersteund. Het wordt het Prijzenoverzicht voor een bereik door de abonnements-id en de factureringsperiode. In plaats daarvan kunt u **prijslijst door facturering profiel**. De serviceprijs in het bereik van een abonnement is gelijk aan de serviceprijs in een facturering profiel bereik. De gebruiker die de API aanroept, moet machtigingen zijn vereist.

[Prijslijst voor zakelijke klanten door inschrijving](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet/) wordt niet ondersteund. En [prijslijst voor zakelijke klanten van inschrijving voor een factureringsperiode](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet/) wordt niet ondersteund. Zowel bieden het tarief voor elke meter voor de inschrijving en factureringsperiode. In plaats daarvan kunt u **prijslijst door facturering profiel**. Het Prijzenoverzicht bij het bereik van de inschrijving is de prijslijst die beschikbaar zijn voor het profiel voor facturering. De gebruiker die de API aanroept, moet machtigingen zijn vereist.

## <a name="see-also"></a>Zie ook
- Meer informatie over [Azure Consumption REST-API's](/rest/api/consumption/).
