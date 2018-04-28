---
title: Azure Enterprise-API's facturering | Microsoft Docs
description: Meer informatie over de rapportage-API's waarmee Azure Enterprise-klanten voor het ophalen van gegevens over het verbruik programmatisch.
services: ''
documentationcenter: ''
author: anandedwin
manager: aedwin
editor: ''
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.openlocfilehash: 7ae04c7c1fdddecf40583d14f7b08dcf41941c6f
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Overzicht van de rapportage-API's voor Enterprise-klanten
De rapportage-API's kunnen klanten Azure Enterprise programmatisch gebruiks- en factureringsgegevens ophalen in de gewenste hulpprogramma's voor gegevensanalyse. 

## <a name="enabling-data-access-to-the-api"></a>Toegang tot gegevens in de API inschakelen
* **Genereren of het ophalen van de API-sleutel** -aanmelden bij de Enterprise portal en navigeer naar rapporten > gebruiksgegevens downloaden > API-toegangssleutel voor het genereren of het ophalen van de API-sleutel.
* **Het doorgeven van sleutels in de API** -de API-sleutel moet worden doorgegeven voor elke aanroep voor verificatie en autorisatie. De volgende eigenschap nodig is voor de HTTP-headers

|Aanvraag-Header-sleutel | Waarde|
|-|-|
|Autorisatie| Geef de waarde in deze indeling: **bearer {API_KEY}** <br/> Voorbeeld: bearer eyr... 09|

## <a name="consumption-apis"></a>Verbruik API 's
Een Swagger-eindpunt is beschikbaar [hier](https://consumption.azure.com/swagger/ui/index) voor de API's die zijn beschreven hieronder moet inschakelen eenvoudig introspection van de API en de mogelijkheid voor het genereren van client-SDK's met behulp van [AutoRest](https://github.com/Azure/AutoRest) of [Swagger Codegen gebruikt](http://swagger.io/swagger-codegen/). Gegevens vanaf 1 mei 2014 is beschikbaar via deze API. 

* **Saldo en samenvatting** : de [saldo en samenvatting API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) biedt een maandelijkse samenvatting van gegevens op tegoeden, nieuwe aankopen, kosten voor Azure Marketplace, correcties en overschrijding kosten.

* **Informatie over het gebruik** : de [gebruik Detail API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) per dag opgesplitst verbruikte aantallen en de geschatte kosten door een inschrijving biedt. Het resultaat bevat ook informatie over exemplaren, meters en afdelingen. De API kan worden doorzocht door facturering periode of door een opgegeven begin- en datum. 

* **Marketplace Store kosten** : de [Marketplace Store kosten API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) retourneert de uitsplitsing op gebruik gebaseerde marketplace-kosten per dag voor de opgegeven periode facturering of de begin- en einddatums (één keer kosten zijn niet opgenomen).

* **Prijslijst** : de [Price Sheet API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) het tarief voor elke Meter biedt voor de opgegeven registratie en facturering periode. 

## <a name="data-freshness"></a>Nieuwheid van gegevens
Etags geretourneerd in het antwoord van de bovenstaande API. Een wijziging in de Etag Hiermee geeft u dat de gegevens zijn vernieuwd.  In de volgende aanroepen naar de dezelfde API met de parameters die de vastgelegde Etag worden doorgegeven met de sleutel 'If-None-Match' in de kop van http-aanvraag. De statuscode van antwoord zou 'NotModified' zijn als de gegevens niet verdere is vernieuwd en worden er geen gegevens geretourneerd. API wordt de volledige gegevensset geretourneerd voor de vereiste periode wanneer er een wijziging in de etag.

## <a name="helper-apis"></a>Helper-API 's
 **Lijst van facturering perioden** : de [facturering perioden API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) retourneert een lijst met facturering perioden met gegevens over het verbruik voor de opgegeven registratie in omgekeerde volgorde. Elke categorie bevat een eigenschap die verwijst naar de API-route voor de vier gegevenssets - BalanceSummary, UsageDetails Marketplace-kosten en prijslijst.


## <a name="api-response-codes"></a>API-reactiecodes  
|Statuscode van antwoord|Bericht|Beschrijving|
|-|-|-|
|200| OK|Geen fout|
|401| Niet geautoriseerd| API-sleutel niet wordt gevonden, ongeldig, verlopen enzovoort.|
|404| Niet beschikbaar| Rapport eindpunt is niet gevonden|
|400| Ongeldig verzoek| Ongeldige parameters: datumbereiken, EA nummers, enz.|
|500| Serverfout| Fout bij verwerking van aanvraag Unexoected| 









