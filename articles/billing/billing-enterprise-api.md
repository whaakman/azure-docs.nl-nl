---
title: Azure Enterprise-API's facturering | Microsoft Docs
description: Meer informatie over de rapportage-API's waarmee Azure Enterprise-klanten voor het ophalen van gegevens over het verbruik programmatisch.
services: 
documentationcenter: 
author: anandedwin
manager: aedwin
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.openlocfilehash: 62a69aeb7499a961f95739fb3836942b670c7320
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Overzicht van de rapportage-API's voor Enterprise-klanten
De rapportage-API's kunnen klanten Azure Enterprise programmatisch gebruiks- en factureringsgegevens ophalen in de gewenste hulpprogramma's voor gegevensanalyse. 

## <a name="enabling-data-access-to-the-api"></a>Toegang tot gegevens in de API inschakelen
* **Genereren of het ophalen van de API-sleutel** - Meld u aan op de Enterprise portal en volg de zelfstudie onder Help - rapportage-API's. De eerste sectie in dit help-artikel wordt uitgelegd hoe u voor het genereren of het ophalen van de API-sleutel voor de opgegeven registratie.
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

## <a name="helper-apis"></a>Helper-API 's
 **Lijst van facturering perioden** : de [facturering perioden API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) retourneert een lijst met facturering perioden met gegevens over het verbruik voor de opgegeven registratie in omgekeerde volgorde. Elke categorie bevat een eigenschap die verwijst naar de API-route voor de vier gegevenssets - BalanceSummary, UsageDetails Marketplace-kosten en prijslijst.


## <a name="api-response-codes"></a>API-reactiecodes  
|Statuscode van antwoord|Bericht|Beschrijving|
|-|-|-|
|200| OK|Er is geen fout|
|401| Niet geautoriseerd| API-sleutel niet wordt gevonden, ongeldig, verlopen enzovoort.|
|404| Niet beschikbaar| Rapport eindpunt is niet gevonden|
|400| Onjuiste aanvraag| Ongeldige parameters: datumbereiken, EA nummers, enz.|
|500| Server-fout| Fout bij verwerking van aanvraag Unexoected| 









