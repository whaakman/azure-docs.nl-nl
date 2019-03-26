---
title: Migreren van Enterprise Agreement naar de klant van Microsoft-overeenkomst API's - Azure | Microsoft Docs
description: In dit artikel krijgt u inzicht in de gevolgen van de migratie van een Microsoft Enterprise Agreement (EA) naar een KLANTOVEREENKOMST van Microsoft.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/20/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 283808c0bd3f5297011b25619d6f978c99d4dc32
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439223"
---
# <a name="migrate-from-enterprise-agreement-to-microsoft-customer-agreement-apis"></a>Migreren van Enterprise Agreement naar de klant van Microsoft-overeenkomst API 's

In dit artikel krijgt u inzicht in de gegevensstructuur, API en andere system integration verschillen tussen Enterprise Agreement (EA) en de overeenkomst voor Microsoft-klanten (MCA). Kostenbeheer van Azure biedt ondersteuning voor API's voor beide accounttypen. Controleer de [factureringsaccount voor Setup](../billing/billing-mca-setup-account.md) KLANTOVEREENKOMST van Microsoft-artikel voordat u doorgaat.

Organisaties met een bestaande EA-account moeten in dit artikel in combinatie met het instellen van een account MCA controleren. Eerder, een EA-account vernieuwen vereist minimale werk te verplaatsen van een oude inschrijving naar een nieuwe. Migreren naar een MCA-account vereist echter extra moeite. Er is een extra moeite vanwege wijzigingen in de onderliggende facturering subsysteem, die invloed hebben op alle kosten met betrekking tot API's en service-aanbiedingen.

## <a name="mca-apis-and-integration"></a>MCA API's en integratie

MCA API's en nieuwe integratie kunnen u:

- Volledige API-beschikbaarheid door middel van systeemeigen Azure-API's hebben.
- Meerdere facturen in een enkele factureringsaccount configureren.
- Toegang tot een gecombineerde API met gebruik van de Azure-service van derden Marketplace en het gebruik Marketplace-aankopen.
- Kosten weergeven via facturering profielen (hetzelfde als inschrijvingen) met behulp van Azure Cost Management.
- Toegang tot nieuwe API's weer te geven van de kosten, een melding ontvangen wanneer kosten vooraf gedefinieerde drempelwaarden overschrijden en automatisch de onbewerkte gegevens exporteren.

## <a name="migration-checklist"></a>Controlelijst voor migratie

De volgende items help die u overgaan op MCA API's.

- Maak uzelf bekend met de nieuwe [Microsoft KLANTOVEREENKOMST facturering account](../billing/billing-mca-overview.md).
- Bepalen welke API's die u gebruikt en zien welke wordt vervangen in de volgende sectie.
- Maak uzelf vertrouwd met [Azure Resource Manager REST API's](/rest/api/azure).
- Als u niet al van Azure Resource Manager-API's gebruikmaakt [uw client-app registreren bij Azure AD](/rest/api/azure/#register-your-client-application-with-azure-ad).
- Bijwerken van elke programmacode aan [gebruik Azure AD-verificatie](/rest/api/azure/#create-the-request).
- Werk elke programmacode ter vervanging van EA API-aanroepen met MCA API-aanroepen.
- Fout bij het verwerken van voor het gebruik van nieuwe foutcodes bijwerken.
- Bekijk meer integratie-aanbiedingen, zoals Cloudyn en Power BI, voor andere actie nodig.

## <a name="ea-apis-replaced-with-mca-apis"></a>EA API's vervangen door MCA API 's

EA API's gebruiken een API-sleutel voor verificatie en autorisatie. MCA API's gebruiken Azure AD-verificatie.

| Doel | EA API | MCA API |
| --- | --- | --- |
| Saldo en het tegoed | [/balancesummary](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) | Microsoft.Billing/billingAccounts/billingProfiles/availableBalanceussae |
| Gebruik (JSON) | [/usagedetails](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format)[/usagedetailsbycustomdate](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format) | [Microsoft.Consumption/usageDetails](/rest/api/consumption/usagedetails)<sup>1</sup> |
| Gebruik (CSV) | [/usagedetails/download](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format)[/usagedetails/submit](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format) | [Microsoft.Consumption/usageDetails/download](/rest/api/consumption/usagedetails)<sup>1</sup> |
| Marketplace-verbruik (CSV) | [/marketplacecharges](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge)[/marketplacechargesbycustomdate](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) | [Microsoft.Consumption/usageDetails/download](/rest/api/consumption/usagedetails)<sup>1</sup> |
| Facturering-punten | [/billingperiods](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) | Microsoft.Billing/billingAccounts/billingProfiles/invoices |
| Prijzenoverzicht | [/pricesheet](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) | Microsoft.Billing/billingAccounts/billingProfiles/pricesheet/default/download format=json|csv Microsoft.Billing/billingAccounts/…/billingProfiles/…/invoices/… /pricesheet/default/Download indeling = json|csv Microsoft.Billing/billingAccounts/../billingProfiles/../providers/Microsoft.Consumption/pricesheets/download  |
| Aankopen in de reservering | [/reservationcharges](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges) | Microsoft.Billing/billingAccounts/billingProfiles/transactions |
| Reservering-aanbevelingen | [/ SharedReservationRecommendations](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-shared-reserved-instance-recommendations)[/](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-single-reserved-instance-recommendations)[SingleReservationRecommendations](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-single-reserved-instance-recommendations) | [Microsoft.Consumption/reservationRecommendations](/rest/api/consumption/reservationrecommendations/list) |
| Gebruik van de reservering | [/reservationdetails](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-details)[/reservationsummaries](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-summary) | [Microsoft.Consumption/reservationDetails](/rest/api/consumption/reservationsdetails)[Microsoft.Consumption/reservationSummaries](/rest/api/consumption/reservationssummaries) |

<sup>1</sup> azure-service en van derden Marketplace gebruik zijn beschikbaar in de [Usage Details API](/rest/api/consumption/usagedetails).

De volgende API's zijn beschikbaar voor MCA facturering accounts:

| Doel | Klant van Microsoft-overeenkomst (MCA) API |
| --- | --- |
| Facturering-accounts<sup>2</sup> | Microsoft.Billing/billingAccounts |
| Facturering profielen<sup>2</sup> | Microsoft.Billing/billingAccounts/billingProfiles |
| Factuur secties<sup>2</sup> | Microsoft.Billing/billingAccounts/invoiceSections |
| Facturen | Microsoft.Billing/billingAccounts/billingProfiles/invoices |
| Facturering-abonnementen | {scope}/billingSubscriptions |

<sup>2</sup> API's geven een lijst met objecten, die bereiken, waarbij ervaringen met Cost Management in Azure portal en API's gebruiken. Zie voor meer informatie over het bereiken van Cost Management [begrijpen en werk met een bereik](understand-work-scopes.md).

Als u een bestaande EA-API's gebruikt, moet u deze ter ondersteuning van MCA facturering accounts kunnen worden bijgewerkt. De volgende tabel ziet u andere integratiewijzigingen:

| Doel | Oude aanbieding | Nieuw aanbod |
| --- | --- | --- |
| Cloudyn | [Cloudyn.com](https://www.cloudyn.com) | [Azure Cost Management](https://azure.microsoft.com/services/cost-management/) |
| Power BI | [Microsoft Consumption Insights](/power-bi/desktop-connect-azure-consumption-insights) en-connector | [Microsoft Azure Consumption Insights-Power BI-app](https://appsource.microsoft.com/product/power-bi/pbi_azureconsumptioninsights.pbi-azure-consumptioninsights?tab=overview) en [ Azure Consumption Insights-connector](/power-bi/desktop-connect-azure-consumption-insights) |

## <a name="apis-to-get-balance-and-credits"></a>API's voor het saldo en het tegoed ophalen

De [saldo-overzicht ophalen](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) -API biedt een maandelijkse samenvatting van:

- Tegoeden
- Nieuwe aankopen
- Azure Marketplace-kosten voor service
- Aanpassingen
- Serviceoverschrijdingskosten

Alle API's voor verbruik worden vervangen door systeemeigen Azure-API's die gebruikmaken van Azure AD voor verificatie en autorisatie. Zie voor meer informatie over de aanroepende Azure REST API's, [aan de slag met REST](/rest/api/azure/#create-the-request).

De API ophalen saldo samenvatting wordt vervangen door de Microsoft.Billing/billingAccounts/billingProfiles/availableBalance-API.

Ophalen van beschikbare tegoeden bij de beschikbare saldo-API:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/availableBalances?api-version=2018-11-01-preview` |

## <a name="apis-to-get-cost-and-usage"></a>API's om de kosten- en gebruiksgegevens ophalen

Kosten per dag opgesplitst ophalen uit Azure-servicegebruik, van derden Marketplace gebruik en andere Marketplace-aankopen met de volgende API's. De volgende afzonderlijke API's zijn voor Azure-services en Marketplace-gebruik van derden samengevoegd. De oude API's zijn vervangen door de [Microsoft.Consumption/usageDetails](/rest/api/consumption/usagedetails) API. Marketplace-aankopen zijn eerder alleen wordt weergegeven in het saldo samenvatting op datum wordt toegevoegd.

- [Gebruik details/downloaden ophalen](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format)
- [Gedetailleerd gebruik/indienen ophalen](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format)
- [Gebruik details/usagedetails ophalen](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format)
- [Gebruik details/usagedetailsbycustomdate ophalen](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format)
- [Marketplace store kosten in rekening gebracht/marketplacecharges ophalen](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge)
- [Marketplace store kosten in rekening gebracht/marketplacechargesbycustomdate ophalen](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge)

Alle API's voor verbruik worden vervangen door systeemeigen Azure-API's die gebruikmaken van Azure AD voor verificatie en autorisatie. Zie voor meer informatie over de aanroepende Azure REST API's, [aan de slag met REST](/rest/api/azure/#create-the-request).

De voorgaande API's worden vervangen door de API van de Details van hun/verbruik.

Ophalen van informatie over het gebruik met de API van de Details van gebruik:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?api-version=2019-01-01` |

De Usage-Details-API, is net als bij alle kosten Management-API's, beschikbaar op meerdere scopes. Voor gefactureerde kosten, zoals u traditioneel op het inschrijvingsniveau van een ontvangen zou, gebruikt u de facturering profiel bereik.  Zie voor meer informatie over het bereiken van Cost Management [begrijpen en werk met een bereik](understand-work-scopes.md).

| Type | De indeling-ID |
| --- | --- |
| Factureringsaccount | `/Microsoft.Billing/billingAccounts/{billingAccountId}` |
| Factureringsprofiel | `/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}` |
| Abonnement | `/subscriptions/{subscriptionId}` |
| Resourcegroep | `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}` |

De volgende querystring-parameters gebruiken om te werken van elke programmacode.

| Oude parameters | Nieuwe parameters |
| --- | --- |
| `billingPeriod={billingPeriod}` | Niet ondersteund |
| `endTime=yyyy-MM-dd` | `endDate=yyyy-MM-dd` |
| `startTime=yyyy-MM-dd` | `startDate=yyyy-MM-dd` |

De hoofdtekst van het antwoord is ook gewijzigd.

Oude antwoordtekst:

```
{
  "id": "string",
  "data": [{...}, ...],
  "nextLink": "string"
}
```

Nieuwe antwoordtekst:

```
{
  "value": [{
    "id": "{scope}/providers/Microsoft.Consumption/usageDetails/###",
    "name": "###",
    "type": "Microsoft.Consumption/usageDetails",
    "tags": {...},
    "properties": [{...}, ...],
    "nextLink": "string"
  }, ...]
}
```

De naam van de eigenschap die de matrix van gebruiksrecords gewijzigd van gegevens naar _waarden_. Elke record die wordt gebruikt om een lijst met gedetailleerde eigenschappen. Maar elke record nu alle gegevens nu in een geneste eigenschap met de naam zijn _eigenschappen_, met uitzondering van tags. De nieuwe structuur is consistent met andere Azure-API's. De namen van sommige eigenschappen zijn gewijzigd. De volgende tabel toont de bijbehorende eigenschappen.

| Oude eigenschap | Nieuwe eigenschap | Opmerkingen |
| --- | --- | --- |
| Account-id | N/A | De maker van het abonnement wordt niet bijgehouden. Gebruik invoiceSectionId (hetzelfde als DepartmentID gemeenschappelijk hebben). |
| AccountNameAccountOwnerId en AccountOwnerEmail | N/A | De maker van het abonnement wordt niet bijgehouden. Gebruik invoiceSectionName (hetzelfde als departmentName). |
| Extra informatie | additionalInfo | &nbsp;  |
| ChargesBilledSeparately | isAzureCreditEligible | Houd er rekening mee dat deze eigenschappen opposites zijn. Als isAzureCreditEnabled true is, zou ChargesBilledSeparately ONWAAR zijn. |
| Verbruikte hoeveelheid | quantity | &nbsp; |
| Verbruikte service | consumedService | Exacte tekenreekswaarden verschillen. |
| Id van de verbruikte service | Geen | &nbsp; |
| Kostenplaats | costCenter | &nbsp; |
| De datum en het usageStartDate | date | &nbsp;  |
| Dag | Geen | Dag vanaf datum worden geparseerd. |
| Afdelings-id | invoiceSectionId | Exacte waarden verschillen. |
| Naam van de afdeling | invoiceSectionName | Exacte tekenreekswaarden verschillen. Configureer factuur secties zodat deze overeenkomt met de afdelingen, indien nodig. |
| ExtendedCost en kosten | costInBillingCurrency | &nbsp;  |
| Instantie-id | resourceId | &nbsp;  |
| Terugkerende kosten | Geen | &nbsp;  |
| Locatie | location | &nbsp;  |
| Metercategorie | meterCategory | Exacte tekenreekswaarden verschillen. |
| Meter-id | meterId | Exacte tekenreekswaarden verschillen. |
| Meternaam | meterName | Exacte tekenreekswaarden verschillen. |
| Meterregio | meterRegion | Exacte tekenreekswaarden verschillen. |
| Metersubcategorie | meterSubCategory | Exacte tekenreekswaarden verschillen. |
| Maand | Geen | Maand van datum worden geparseerd. |
| Naam van aanbieding: | Geen | PublisherName en productOrderName gebruiken. |
| OfferId | Geen | &nbsp;  |
| Ordernummer | Geen | &nbsp;  |
| PartNumber | Geen | MeterId en productOrderName gebruiken voor het aanduiden van prijzen. |
| Naam van het plan | productOrderName | &nbsp;  |
| Product | Product |   |
| Product-id | productId | Exacte tekenreekswaarden verschillen. |
| Naam van de uitgever | publisherName | &nbsp;  |
| ResourceGroup | resourceGroupName | &nbsp;  |
| ResourceGuid | meterId | Exacte tekenreekswaarden verschillen. |
| Resourcelocatie | resourceLocation | &nbsp;  |
| Id van de resourcelocatie | Geen | &nbsp;  |
| Resourcetarief | effectivePrice | &nbsp;  |
| Id van de servicebeheerder | N/A | &nbsp;  |
| Servicegegevens 1 | serviceInfo1 | &nbsp;  |
| Servicegegevens 2 | serviceInfo2 | &nbsp;  |
| ServiceName | meterCategory | Exacte tekenreekswaarden verschillen. |
| ServiceTier | meterSubCategory | Exacte tekenreekswaarden verschillen. |
| Service-id voor de store | N/A | &nbsp;  |
| GUID van het abonnement | subscriptionId | &nbsp;  |
| SubscriptionId | subscriptionId | &nbsp;  |
| Abonnementsnaam | subscriptionName | &nbsp;  |
| Tags | tags | De eigenschap tags is van toepassing op hoofdobject, niet aan de eigenschappen van geneste eigenschap. |
| Maateenheid | unitOfMeasure | Exacte tekenreekswaarden verschillen. |
| usageEndDate | date | &nbsp;  |
| Jaar | Geen | Jaar na de aanmaakdatum worden geparseerd. |
| (nieuw) | billingCurrency | Valuta die wordt gebruikt voor de kosten in rekening gebracht. |
| (nieuw) | billingProfileId | De unieke ID voor de facturering-profiel (hetzelfde als inschrijven). |
| (nieuw) | billingProfileName | Naam van de facturering profiel (hetzelfde als inschrijven). |
| (nieuw) | chargeType | Gebruiken om u te onderscheiden van de Azure-servicegebruik, het gebruik van Marketplace en aankopen. |
| (nieuw) | invoiceId | De unieke ID voor de factuur. Leeg voor de huidige, open maand. |
| (nieuw) | publisherType | Het type van de uitgever voor aankopen. Leeg zijn voor gebruik. |
| (nieuw) | serviceFamily | Het type van aankoop. Leeg zijn voor gebruik. |
| (nieuw) | servicePeriodEndDate | Einddatum voor de aangeschafte-service. |
| (nieuw) | servicePeriodStartDate | Begindatum van de aangeschafte-service. |

## <a name="billing-periods-api-replaced-by-invoices-api"></a>Facturatie-API van punten vervangen door de API voor facturen

Facturering accounts MCA gebruik geen factureringsperioden. In plaats daarvan gebruiken ze facturen bereik van kosten op specifieke factureringsperioden. De [Billing perioden API](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) wordt vervangen door de API voor facturen. Alle API's voor verbruik worden vervangen door systeemeigen Azure-API's die gebruikmaken van Azure AD voor verificatie en autorisatie. Zie voor meer informatie over de aanroepende Azure REST API's, [aan de slag met REST](/rest/api/azure/#create-the-request).

Facturen met de API voor facturen ophalen:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/invoices?api-version=2018-11-01-preview` |

## <a name="price-sheet-apis"></a>Prijs blad API 's

Deze sectie wordt besproken bestaande prijs blad-API's en bevat aanbevelingen om u te verplaatsen naar de prijs blad-API van Microsoft-klant-overeenkomsten. Ook wordt de prijs blad-API voor Microsoft-klant overeenkomsten beschreven en wordt uitgelegd velden in de prijzenoverzichten. De [prijslijst Enterprise ophalen](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) en [Enterprise ophalen factureringsperioden](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) API's zijn vervangen door de prijs blad-API voor Microsoft-klant-overeenkomsten (Microsoft.Billing/billingAccounts/billingProfiles / prijslijst). De nieuwe API biedt ondersteuning voor JSON- en CSV-indelingen in asynchrone REST-indelingen. Alle API's voor verbruik worden vervangen door systeemeigen Azure-API's die gebruikmaken van Azure AD voor verificatie en autorisatie. Zie voor meer informatie over de aanroepende Azure REST API's, [aan de slag met REST](/rest/api/azure/#create-the-request).

### <a name="billing-enterprise-apis"></a>Facturering Enterprise-API 's

Waarmee u facturerings Enterprise-API's met Enterprise-inschrijvingen prijs- en informatie over facturering periode. Verificatie en autorisatie gebruikt Azure Active Directory webtokens.

Prijzen van toepassing voor de opgegeven Enterprise-inschrijving met de prijslijst en facturering periode API's ophalen:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/pricesheet` |
| GET | `https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/pricesheet` |

### <a name="price-sheet-api-for-microsoft-customer-agreements"></a>Prijs blad API voor klant van Microsoft-overeenkomsten

De prijs blad-API voor Microsoft-klant-overeenkomsten gebruiken om de prijzen voor alle Azure-verbruik en de Marketplace-verbruik services weer te geven. De prijzen weergegeven voor de factureringsprofiel gelden voor alle abonnementen die deel uitmaken van het profiel voor facturering.

De prijs blad-API gebruiken voor het weergeven van alle Azure-verbruik services prijslijst-gegevens in CSV-indeling:

| Methode | Aanvraag-URI |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&startDate=2019-01-01&endDate=2019-01-31&format=csv` |

De prijs blad-API gebruiken voor het weergeven van alle Azure-verbruik services prijslijst-gegevens in JSON-indeling:

| Methode | Aanvraag-URI |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&startDate=2019-01-01&endDate=2019-01-31&format=json` |

Met behulp van de API retourneert de prijslijst voor het gehele account. U kunt echter ook een verkorte versie van het Prijzenoverzicht ophalen in PDF-indeling. De samenvatting omvat Azure Consumption and Marketplace-verbruik-services die in rekening voor een specifieke factuur gebracht worden. De factuur wordt geïdentificeerd door {invoiceId}, dit is hetzelfde als de **factuurnummer** wordt weergegeven in de factuur samenvatting PDF-bestanden. Hier volgt een voorbeeld.

![Van de voorbeeldafbeelding met het nummer van de factuur die overeenkomt met aan de InvoiceId](./media/migrate-cost-management-api/invoicesummary.png)

Om factuurinformatie te bekijken met de prijs blad-API in CSV-indeling:

| Methode | Aanvraag-URI |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/2909cffc-b0a2-5de1-bb7b-5d3383764184/billingProfiles/2dcffe0c-ee92-4265-8647-515b8fe7dc78/invoices/{invoiceId}/pricesheet/default/download?api-version=2018-11-01-preview&format=csv` |

Om factuurinformatie te bekijken met de prijs blad-API in JSON-indeling:

| Methode | Aanvraag-URI |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/2909cffc-b0a2-5de1-bb7b-5d3383764184/billingProfiles/2dcffe0c-ee92-4265-8647-515b8fe7dc78/invoices/{invoiceId}/pricesheet/default/download?api-version=2018-11-01-preview&format=json` |

U ziet ook geschatte prijzen voor elke Azure-verbruik of Marketplace-verbruik-service in de huidige factureringscyclus openen of de service-punt.

Geschatte prijzen voor verbruik services met de API van de prijs blad in CSV-indeling om weer te geven:

| Methode | Aanvraag-URI |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billing AccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&format=csv` |

Geschatte prijzen voor verbruik services met de API van de prijs blad in JSON-indeling om weer te geven:

| Methode | Aanvraag-URI |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billing AccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&format=json` |

De Microsoft-overeenkomst prijs blad API's van klanten zijn *asynchrone REST-API's*. De antwoorden voor de API's gewijzigd van de oudere synchrone API's. De hoofdtekst van de API-reactie ook gewijzigd.

#### <a name="old-response-body"></a>Oude antwoordtekst

Hier volgt een voorbeeld van de synchrone REST-API-reactie:

```
[
        {
            "id": "enrollments/573549891/billingperiods/2016011/products/343/pricesheets",
            "billingPeriodId": "201704",
            "meterId": "dc210ecb-97e8-4522-8134-2385494233c0",
            "meterName": "A1 VM",
            "unitOfMeasure": "100 Hours",
            "includedQuantity": 0,
            "partNumber": "N7H-00015",
            "unitPrice": 0.00,
            "currencyCode": "USD"
        },
        {
    ]
```

#### <a name="new-response-body"></a>Nieuwe antwoordtekst

De API's ondersteunen de [Azure REST asynchrone](../azure-resource-manager/resource-manager-async-operations.md) indeling. Roep de API met behulp van ophalen en u het volgende antwoord ontvangen:

```
No Response Body

HTTP Status 202 Accepted
```

De volgende headers worden verzonden door de locatie van de uitvoer:

```
Location:https://management.azure.com/providers/Microsoft.Consumption/operationresults/{operationId}?sessiontoken=XZDFSnvdkbkdsb==

Azure-AsyncOperation:https://managment.azure.com/providers/Microsoft.Consumption/operationStatus/{operationId}?sessiontoken=XZDFSnvdkbkdsb==

Retry-After: 10

OData-EntityId: {operationId}

```

Maken van een andere GET aanroepen naar de locatie. Het antwoord op de GET-aanroep is hetzelfde totdat de bewerking is voltooid of mislukt status bereikt. Wanneer dit is voltooid, retourneert het antwoord naar de locatie van de GET-aanroep van de download-URL. Net als de bewerking is uitgevoerd op hetzelfde moment. Hier volgt een voorbeeld:

```
HTTP Status 200

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

De client ook een GET-aanroep kunt maken voor de `Azure-AsyncOperation`. Het eindpunt wordt de status voor de bewerking.

De volgende tabel bevat de velden in de oudere Enterprise ophalen prijs blad API. Het bevat bijbehorende velden in de nieuwe prijslijst voor Microsoft-klant overeenkomsten:

| Oude eigenschap | Nieuwe eigenschap | Opmerkingen |
| --- | --- | --- |
| billingPeriodId  | _Niet van toepassing_ | Niet van toepassing. Voor Microsoft-klant-overeenkomsten, wordt het concept van billingPeriodId vervangen door de factuur en de bijbehorende prijslijst. |
| meterId  | meterId | &nbsp;  |
| unitOfMeasure  | unitOfMeasure | Exacte tekenreekswaarden verschillen. |
| includedQuantity  | includedQuantity | Niet van toepassing voor services in Microsoft-klant-overeenkomsten. |
| PartNumber  | _Niet van toepassing_ | Gebruik in plaats daarvan een combinatie van productOrderName (hetzelfde als offerId) en meterid. |
| unitPrice  | unitPrice | Prijs per eenheid is van toepassing voor services die worden gebruikt in Microsoft-klant-overeenkomsten. |
| currencyCode  | pricingCurrency | Microsoft Customer overeenkomsten hebben prijs voorstellingen in valuta prijzen en facturering. De currencyCode komt overeen met de pricingCurrency in Microsoft-klant-overeenkomsten. |
| offerId | productOrderName | In plaats van OfferId, u kunt productOrderName maar is niet hetzelfde als OfferId. Echter bepalen productOrderName en meter prijzen in de Microsoft-klant overeenkomsten met betrekking tot meterId en Offerid in verouderde inschrijvingen. |

## <a name="consumption-price-sheet-api-operations"></a>Verbruik prijs blad API-bewerkingen

Voor Enterprise-overeenkomsten, gebruikt u de API-verbruik prijs blad [ophalen](/rest/api/consumption/pricesheet/get) en [ophalen door factureringsperiode](/rest/api/consumption/pricesheet/getbybillingperiod) bewerkingen voor een bereik op abonnements-id of een factureringsperiode. De API maakt gebruik van Azure Resource Management-verificatie.

Voor de prijslijst-informatie voor een bereik met de prijs blad-API:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Consumption/pricesheets/default?api-version=2018-10-01` |

Prijslijst informatie ophalen van de factureringsperiode met de prijs blad-API:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Billing/billingPeriods/{billingPeriodName}/providers/Microsoft.Consumption/pricesheets/default?api-version=2018-10-01` |

Gebruik de volgende query voor Microsoft-klant overeenkomsten in plaats van de bovenstaande API-eindpunten:

**Prijs blad API voor Microsoft-klant-overeenkomsten (asynchrone REST-API)**

Deze API is voor Microsoft-klant-overeenkomsten en biedt aanvullende kenmerken.

**Prijslijst voor een scope facturering profiel in een factureringsaccount**

Deze API is de bestaande API. Het is bijgewerkt zodat de prijslijst voor een profiel voor facturering in factureringsaccount.

## <a name="price-sheet-for-a-scope-by-billing-account"></a>Prijslijst voor een bereik van facturering van account

Azure Resource Manager-verificatie wordt gebruikt wanneer u het Prijzenoverzicht bij het bereik van de inschrijving in een rekening ontvangt.

Om het Prijzenoverzicht bij de apparaatregistratie-account in een factureringsaccount:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `/providers/Microsoft.Billing/billingAccounts/65085863/providers/Microsoft.Consumption/pricesheets/download?api-version=2019-01-01` |

Gebruik de informatie in de volgende sectie voor een overeenkomst van de klant Microsoft. Het biedt de eigenschappen die worden gebruikt voor Microsoft Customer overeenkomsten.

### <a name="price-sheet-for-a-billing-profile-scope-in-a-billing-account"></a>Prijslijst voor een facturering profiel scope in factureringsaccount

De bijgewerkte prijslijst door factureringsaccount API haalt de prijslijst in CSV-indeling. Om het Prijzenoverzicht bij facturering profiel voor het bereik van een MCA:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `/providers/Microsoft.Billing/billingAccounts/28ae4b7f-41bb-581e-9fa4-8270c857aa5f/billingProfiles/ef37facb-cd6f-437a-9261-65df15b673f9/providers/Microsoft.Consumption/pricesheets/download?api-version=2019-01-01` |

In het bereik van de EA-inschrijving zijn de API-reactie en eigenschappen identiek. De eigenschappen overeenkomen met de dezelfde MCA-eigenschappen.

De oudere eigenschappen voor [API's van Azure Resource Manager prijs blad](/rest/api/consumption/pricesheet) en dezelfde nieuwe eigenschappen zijn in de volgende tabel.

| Oude Azure Resource Manager-prijs blad API-eigenschap  | Nieuwe Microsoft Customer overeenkomst prijs blad API-eigenschap   | Description |
| --- | --- | --- |
| Id van de meter | _meterId_ | De unieke id voor de meter. Hetzelfde als meterId. |
| Meternaam | meterName | De naam van de meter. Meter vertegenwoordigt het implementeerbare resource zorgen Azure-service. |
| Metercategorie  | service | De naam van de classificatiecategorie voor de meter. Hetzelfde als de service in de Microsoft Customer Agreement-prijslijst. Exacte tekenreekswaarden verschillen. |
| Subcategorie van de meter | meterSubCategory | De naam van de meter subclassificatie categorie. Op basis van de indeling van de functies op hoog niveau set differentiatie in de service. Bijvoorbeeld: Basic SQL DB vs standaard SQL-database. |
| Meterregio | meterRegion | &nbsp;  |
| Eenheid | _Niet van toepassing_ | Kan worden geparseerd uit unitOfMeasure. |
| Meeteenheid | unitOfMeasure | &nbsp;  |
| Onderdeelnummer | _Niet van toepassing_ | Gebruik in plaats van partNumber, productOrderName en MeterId voor het aanduiden van de prijs voor een profiel voor facturering. Velden worden weergegeven op de factuur MCA in plaats van de partNumber in MCA facturen. |
| Eenheidsprijs | unitPrice | Prijs per eenheid KLANTOVEREENKOMST van Microsoft. |
| Valutacode | pricingCurrency | Microsoft Customer overeenkomsten vertegenwoordigen de prijzen in de valuta-prijzen en facturering valuta. Valutacode is hetzelfde als de pricingCurrency in Microsoft-klant-overeenkomsten. |
| Inbegrepen hoeveelheid | includedQuantity | Niet van toepassing op services in de Microsoft-klant-overeenkomsten. Weergeven met de waarden gelijk is aan nul. |
|  Aanbiedings-id  | productOrderName | Gebruik in plaats van OfferId, productOrderName. Niet gelijk zijn aan OfferId, maar de productOrderName en meter bepalen prijzen in de Microsoft-klant-overeenkomsten. Met betrekking tot meterId en Offerid in verouderde inschrijvingen. |

De prijs voor overeenkomsten voor Microsoft-klanten is anders dan Enterprise agreements gedefinieerd. De prijs voor services in de Enterprise-inschrijving is uniek voor product, PartNumber, meter en aanbieding. De PartNumber niet wordt gebruikt in Microsoft-klant-overeenkomsten.

De prijs van Azure-verbruik-service die deel uitmaakt van een Microsoft-KLANTOVEREENKOMST is uniek voor productOrderName en meterId. Ze vertegenwoordigen de meter service en de product-plan.

Om af te stemmen tussen de prijslijst en het gebruik in de API van de Details van gebruik, kunt u de productOrderName en meterId.

Gebruikers die beschikken over facturering profileren van eigenaar, Inzender, lezer en factureren manager rechten kunnen downloaden van de prijslijst.

Het Prijzenoverzicht bevat prijzen voor services waarvan de prijs is gebaseerd op gebruik. De services zijn Azure-verbruik en Marketplace-verbruik. De meest recente prijzen aan het einde van elke serviceperiode van de is vergrendeld en toegepast op gebruik in een periode van één service. Voor services van Azure-verbruik is de service duurt meestal een kalendermaand.

### <a name="retired-price-sheet-api-fields"></a>Buiten gebruik gestelde prijs blad API-velden

De volgende velden zijn niet beschikbaar in API's van Microsoft klanten overeenkomst prijs blad of dezelfde velden.

|Buiten gebruik gestelde veld| Description|
|---|---|
| billingPeriodId | Niet van toepassing. Komt overeen met InvoiceId voor MCA. |
| offerId | Niet van toepassing. Komt overeen met productOrderName in MCA. |
| meterCategory  | Niet van toepassing. Komt overeen met de Service in MCA. |
| eenheid | Niet van toepassing. Kan worden geparseerd uit unitOfMeasure. |
| currencyCode | Hetzelfde als de pricingCurrency in MCA. |
| meterLocation | Hetzelfde als de meterRegion in MCA. |
| partNumber partnumber | Niet van toepassing omdat het onderdeelnummer niet wordt weergegeven in MCA facturen. In plaats van partnumber, gebruikt u de combinatie van meterId en productOrderName voor het aanduiden van prijzen. |
| totalIncludedQuantity | Niet van toepassing. |
| pretaxStandardRate  | Niet van toepassing. |

## <a name="reservation-instance-charge-api-replaced"></a>Reservering-exemplaar kosten in rekening gebracht API vervangen

U kunt ophalen facturering transacties voor aankopen in de reservering met de [gereserveerde instantie kosten in rekening gebracht API](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges). De nieuwe API omvat alle aankopen, met inbegrip van Marketplace-aankopen van derden. Alle API's voor verbruik worden vervangen door systeemeigen Azure-API's die gebruikmaken van Azure AD voor verificatie en autorisatie. Zie voor meer informatie over de aanroepende Azure REST API's, [aan de slag met REST](/rest/api/azure/#create-the-request). De gereserveerde instantie kosten in rekening gebracht API wordt vervangen door de API-transacties.

Reservering kopen transacties met de API-transacties ophalen:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/transactions?api-version=2018-11-01-preview` |

## <a name="recommendations-apis-replaced"></a>Aanbevelingen voor API's vervangen

Gereserveerde instantie aankoop Recommendations-API's bieden VM-gebruik gedurende de laatste 7, 30 en 60 dagen. API's bieden ook aanschafaanbevelingen reservering. Ze omvatten:

- [Gedeelde gereserveerde instantie aanbeveling API](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-shared-reserved-instance-recommendations)
- [Één gereserveerde instantie Recommendations-API](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-single-reserved-instance-recommendations)

Alle API's voor verbruik worden vervangen door systeemeigen Azure-API's die gebruikmaken van Azure AD voor verificatie en autorisatie. Zie voor meer informatie over de aanroepende Azure REST API's, [aan de slag met REST](/rest/api/azure/#create-the-request). De reservering aanbevelingen die eerder is vermeld, API's zijn vervangen door de [Microsoft.Consumption/reservationRecommendations](/rest/api/consumption/reservationrecommendations/list) API.

Reservering-aanbevelingen met de reservering Recommendations-API ophalen:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Consumption/reservationRecommendations?api-version=2019-01-01` |

## <a name="reservation-usage-apis-replaced"></a>Reservering gebruik API's vervangen

Gebruik van de reservering krijgt u bij een inschrijving met de API voor gebruik van gereserveerde instantie. Als er meer dan één gereserveerde instantie in een registratiegroep, krijgt u ook het gebruik van alle de gereserveerde instantie koopt met behulp van deze API.

Ze omvatten:

- [Informatie over het gebruik gereserveerde instanties](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-details)
- [Samenvatting van gebruik van gereserveerde instanties](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-summary)

Alle API's voor verbruik worden vervangen door systeemeigen Azure-API's die gebruikmaken van Azure AD voor verificatie en autorisatie. Zie voor meer informatie over de aanroepende Azure REST API's, [aan de slag met REST](/rest/api/azure/#create-the-request). De reservering aanbevelingen die eerder is vermeld, API's zijn vervangen door de [Microsoft.Consumption/reservationDetails](/rest/api/consumption/reservationsdetails) en [Microsoft.Consumption/reservationSummaries](/rest/api/consumption/reservationssummaries) API's.

Details van de reservering met de API voor reservering Details ophalen:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Consumption/reservationDetails?api-version=2019-01-01` |

Samenvattingen van de reservering met de reservering samenvattingen van de API ophalen:

| Methode | Aanvraag-URI |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Consumption/reservationSummaries?api-version=2019-01-01` |



## <a name="move-from-cloudyn-to-cost-management"></a>Verplaatsen van Cloudyn naar kostenbeheer

Organisaties met behulp van [Cloudyn](https://cloudyn.com) moet beginnen met behulp van [Azure Cost Management](https://azure.microsoft.com/services/cost-management/) voor de behoeften van cost management. Kostenbeheer is beschikbaar in de Azure portal met geen onboarding en een latentie van acht uur. Zie voor meer informatie de [Cost Management documentatie](index.yml).

Met Azure Cost Management kunt u het volgende doen:

- Kosten weergeven na verloop van tijd ten opzichte van een vooraf gedefinieerde budget. Analyseer dagelijkse kosten patronen identificeren en te stoppen voor de uitgavelimiet afwijkingen. Kosten per tags, de resourcegroep, de service en de locatie opsplitsen.
- Budgetten voor een limiet instellen voor gebruik en kosten en ontvang een melding wanneer belangrijke drempelwaarden zijn genaderd maken. Instellen van automatisering met actiegroepen aangepaste gebeurtenissen activeren en afdwingen van de vaste limieten op uw eigen voorwaarden.
- Optimaliseer kosten en gebruik met aanbevelingen van Azure Advisor. Aankoop optimalisaties met reserveringen detecteren, krimpen onderbezette virtuele machines en verwijderen van niet-gebruikte resources om binnen budgetten te blijven.
- Plan een kosten en gebruik gegevens exporteren naar een CSV-bestand dagelijks publiceren naar uw opslagaccount. Automatiseer integratie met externe systemen voor facturering gegevens gesynchroniseerd en up-to-date te houden.

## <a name="power-bi-integration"></a>Integratie met Power BI

Als u Power BI voor het melden van kosten gebruikt, moet u overstappen op het volgende:

- Microsoft Azure Consumption Insights-Power BI-app
- Azure Consumption Insights-desktop-connector


De connector wordt aanbevolen voor organisaties die de meeste flexibiliteit. De Power BI-app is echter ook beschikbaar voor snelle installatie.

- Installeer de [Microsoft Azure Consumption Insights-Power BI-app](https://appsource.microsoft.com/product/power-bi/pbi_azureconsumptioninsights.pbi-azure-consumptioninsights?tab=overview)
- [Verbinding maken met de Azure Consumption Insights-connector](/power-bi/desktop-connect-azure-consumption-insights)

De oudere Consumption Insights-inhoudspakket en connector werkte op het inschrijvingsniveau van. Het vereist minimaal leestoegang. De nieuwe Consumption Insights-Power BI-app en de nieuwe Azure Consumption Insights-connector zijn beschikbaar voor profielgebruikers van facturering. Teams die extra opties voor het controleren van kosten of kosten bekijken in de facturering van profielen nodig te gebruiken in [Cost analysis](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/costanalysis) de Azure-portal.

## <a name="next-steps"></a>Volgende stappen

- Lees de [Cost Management documentatie](index.yml) voor informatie over het bewaken en beheren van Azure-uitgaven. Of, als u wilt optimaliseren gebruik van de resource met Cost Management.
