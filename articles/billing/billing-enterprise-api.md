---
title: Azure-facturering voor Enter prise-Api's | Microsoft Docs
description: Meer informatie over de rapportage-Api's waarmee Azure-klanten Bedrijfs gegevens programmatisch kunnen ophalen.
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
ms.author: banders
ms.openlocfilehash: f706ad86493981d5b38248ec209a7c8b936f6817
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443217"
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Overzicht van rapportage-Api's voor zakelijke klanten
Met de rapportage-Api's kunnen zakelijke Azure-klanten programmatisch verbruiks-en facturerings gegevens ophalen in de hulpprogram ma's voor de voor keur voor gegevens analyse. Zakelijke klanten hebben een [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) ondertekend met Azure om onderhandelde monetaire toezeg gingen te maken en toegang te krijgen tot aangepaste prijzen voor Azure-resources.

## <a name="enabling-data-access-to-the-api"></a>Gegevens toegang tot de API inschakelen
* **De API-sleutel genereren of ophalen** : Meld u aan bij de Enter prise Portal en navigeer naar rapporten om de API-sleutel te genereren of op te halen met be> > van de API-toegangs sleutel.
* **Sleutels door geven in de API** : de API-sleutel moet worden door gegeven voor elke aanroep voor verificatie en autorisatie. De volgende eigenschap moet aan de HTTP-headers zijn

|Sleutel aanvraag header | Waarde|
|-|-|
|Authorization| Geef de waarde in deze indeling op: **Bearer {API_KEY}** <br/> Voor beeld: Bearer Eyr.... 09| 

## <a name="consumption-apis"></a>Verbruiks-Api's
Een Swagger-eind punt is [hier](https://consumption.azure.com/swagger/ui/index) beschikbaar voor de api's die hieronder worden beschreven en die eenvoudige introspectie van de API moeten bieden en de mogelijkheid om client-sdk's te genereren met behulp van autorest of [Swagger CodeGen](https://swagger.io/swagger-codegen/). [](https://github.com/Azure/AutoRest) Gegevens die beginnen op 1 mei 2014 zijn beschikbaar via deze API. 

* **Saldo en samen vatting** : het [saldo en de samen vatting-API](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) biedt een maandelijks overzicht van de informatie over saldi, nieuwe aankopen, Azure Marketplace-service kosten, aanpassingen en overschrijding kosten.

* **Gebruiks Details** : de [gebruiks gegevens-API](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) biedt een dagelijkse uitsplitsing van verbruikte hoeveel heden en geschatte kosten door een registratie. Het resultaat bevat ook informatie over instanties, meters en afdelingen. De API kan worden opgevraagd op basis van de facturerings periode of op basis van een opgegeven begin-en eind datum. 

* **Kosten voor Marketplace-opslag** : de kosten voor de tarieven voor Marketplace [Store](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) retourneert de op gebruik gebaseerde Marketplace-uitsplitsing per dag voor de opgegeven facturerings periode of begin-en eind datum (eenmalige kosten zijn niet inbegrepen).

* **Prijzen overzicht** : de [prijs lijst-API](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) biedt het toepasselijke tarief voor elke meter voor de opgegeven inschrijvings-en facturerings periode.

* **Details van gereserveerde exemplaren** -de [gereserveerde gebruiks-API](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) van het exemplaar retourneert het gebruik van de gereserveerde exemplaar aankopen. De [API voor gereserveerde instantie kosten](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) toont de gemaakte facturerings transacties. 

## <a name="data-freshness"></a>Nieuwheid van gegevens
ETags wordt geretourneerd in het antwoord van alle bovenstaande API. Een wijziging in ETAG geeft aan dat de gegevens zijn vernieuwd.  In volgende aanroepen naar dezelfde API met dezelfde para meters geeft u de vastgelegde ETAG door met de sleutel ' If-None-Match ' in de header van de HTTP-aanvraag. De antwoord status code zou ' NotModified ' zijn als de gegevens niet meer zijn vernieuwd en er geen gegevens worden geretourneerd. API retourneert de volledige gegevensset voor de vereiste periode wanneer er sprake is van een ETag-wijziging.

## <a name="helper-apis"></a>Helper-Api's
 **Facturerings perioden weer geven** : de [API](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) voor facturerings perioden retourneert een lijst met facturerings perioden die verbruiks gegevens bevatten voor de opgegeven inschrijving in omgekeerde chronologische volg orde. Elke periode bevat een eigenschap die verwijst naar de API-route voor de vier sets gegevens BalanceSummary, UsageDetails, Marketplace-kosten en prijs lijst.


## <a name="api-response-codes"></a>API-respons codes   
|Status code van antwoord|Message|Description|
|-|-|-|
|200| OK|Geen fout|
|401| Niet geautoriseerd| De API-sleutel is niet gevonden, is ongeldig, is verlopen, enzovoort.|
|404| Niet beschikbaar| Rapport eindpunt niet gevonden|
|400| Onjuiste aanvraag| Ongeldige params-datumbereiken, EA-cijfers etc.|
|500| Serverfout| Onverwachte fout bij de verwerking van de aanvraag| 









