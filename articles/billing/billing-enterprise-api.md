---
title: Azure Enterprise API's facturering | Microsoft Docs
description: Meer informatie over de rapportage-API's waarmee de Azure Enterprise-klanten voor het ophalen van gegevens via een programma.
services: ''
documentationcenter: ''
author: mumami
manager: mumami
editor: ''
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: erikre
ms.openlocfilehash: 5722e05e5a5e3a57b4d12b70b14f8674364f824b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244820"
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Overzicht van de rapportage-API's voor zakelijke klanten
De rapportage-API's kunnen Azure Enterprise-klanten om op te halen via een programma gebruiks- en factureringsgegevens in de gewenste hulpprogramma's voor gegevensanalyse. Zakelijke klanten zich hebben aangemeld een [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) met Azure om te maken in de onderhandelde monetaire toezeggingen en toegang krijgen tot aangepaste prijzen voor Azure-resources.

## <a name="enabling-data-access-to-the-api"></a>Voor gegevenstoegang tot de API
* **Genereren of de API-sleutel ophalen** -Meld u aan bij de Enterprise portal en navigeert u naar rapporten > Download Usage > API Access Key genereren of de API-sleutel ophalen.
* **Doorgeven van sleutels in de API** -om de API-sleutel moet worden doorgegeven voor elke aanroep voor verificatie en autorisatie. De volgende eigenschap nodig is voor de HTTP-headers

|Header-sleutel aanvragen | Value|
|-|-|
|Autorisatie| Geef de waarde in deze indeling: **bearer {API_KEY}** <br/> Voorbeeld: bearer eyr... 09| 

## <a name="consumption-apis"></a>Gebruik API 's
Een Swagger-eindpunt is beschikbaar [hier](https://consumption.azure.com/swagger/ui/index) voor de API's die zijn beschreven waaronder moet inschakelen eenvoudig introspection van de API en de mogelijkheid voor het genereren van client-SDK's met behulp van [AutoRest](https://github.com/Azure/AutoRest) of [Swagger CodeGen](https://swagger.io/swagger-codegen/). Gegevens die vanaf 1 mei 2014 is beschikbaar via deze API. 

* **Saldo en het overzicht** : de [saldo en het overzicht API](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) biedt een maandelijkse samenvatting van informatie over de saldi, nieuwe aankopen, servicekosten voor Azure Marketplace, aanpassingen en kosten voor overschrijding.

* **Informatie over het gebruik** : de [Usage details API](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) per dag opgesplitst verbruikte hoeveelheden en geschatte kosten in rekening gebracht door een inschrijving biedt. Het resultaat bevat ook informatie over instanties, meters en afdelingen. De API kan worden opgevraagd door factureringsperiode of door een opgegeven begin- en datum. 

* **Marketplace-Store kosten in rekening gebracht** : de [Marketplace Store kosten in rekening gebracht API](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) retourneert de uitsplitsing van de kosten in rekening gebracht op basis van gebruik marketplace per dag voor de opgegeven periode facturering of het begin- en einddatums (één keer kosten zijn niet inbegrepen).

* **Prijslijst** : de [prijs blad API](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) biedt van het tarief voor elke Meter voor de opgegeven inschrijving en factureringsperiode.

* **Exemplaardetails gereserveerd** : de [gebruik van gereserveerde instanties API](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) retourneert het gebruik van de gereserveerde instantie koopt. De [gereserveerde instanties in rekening gebracht API](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) ziet de facturering transacties die zijn gemaakt. 

## <a name="data-freshness"></a>Nieuwheid van gegevens
In het antwoord van de bovenstaande API Etags geretourneerd. Een wijziging in de Etag geeft aan dat de gegevens zijn vernieuwd.  In volgende aanroepen naar de dezelfde API met behulp van dezelfde parameters geeft u de vastgelegde Etag met de sleutel 'If-None-Match' in de header van http-aanvraag. De responscode van de status is 'NotModified' als de gegevens niet verdere is vernieuwd en er zijn geen gegevens worden geretourneerd. API wordt de volledige gegevensset voor de vereiste periode wanneer er sprake is van een wijziging in de ETag die is geretourneerd.

## <a name="helper-apis"></a>Helper-API 's
 **Facturering periode lijst** : de [Billing perioden API](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) retourneert een lijst van factureringsperioden die gegevens voor de opgegeven inschrijving in omgekeerde volgorde hebben. Elke periode bevat een eigenschap die verwijst naar de API-route voor de vier gegevenssets - BalanceSummary, UsageDetails, Marketplace-kosten en prijslijst.


## <a name="api-response-codes"></a>API-responscodes   
|Statuscode van antwoord|Message|Description|
|-|-|-|
|200| OK|Er wordt geen fout|
|401| Niet geautoriseerd| API-sleutel niet wordt gevonden, ongeldig, verlopen enzovoort.|
|404| Niet beschikbaar| Rapporteindpunt is niet gevonden|
|400| Onjuiste aanvraag| Ongeldige parameters – datumbereiken, EA-nummers, enz.|
|500| Server Error| Onverwachte fout bij verwerken van de aanvraag| 









