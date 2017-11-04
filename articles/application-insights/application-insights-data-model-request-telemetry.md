---
title: Azure Application Insights telemetrie Data Model - telemetrie aanvragen | Microsoft Docs
description: Application Insights-gegevensmodel voor aanvraagtelemetrie
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin
ms.openlocfilehash: 0073f38097ffbebd669754eac5f2d48a620941bf
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="request-telemetry-application-insights-data-model"></a>Telemetrie aanvragen: Application Insights-gegevensmodel

Een aanvraag telemetrie-item (in [Application Insights](app-insights-overview.md)) vertegenwoordigt de logische volgorde van de uitvoering is geactiveerd door een externe aanvraag aan uw toepassing. Elke uitvoering van een aanvraag wordt geïdentificeerd met unieke `ID` en `url` met alle uitvoeringsparameters. U kunt aanvragen groeperen op logische `name` en definieer de `source` van deze aanvraag. De uitvoering van code kan leiden tot `success` of `fail` en heeft een bepaalde `duration`. Geslaagde en mislukte uitvoeringen kunnen worden gegroepeerd door verdere `resultCode`. Begintijd voor de aanvraagtelemetrie gedefinieerd op het niveau van de envelop.

Aanvraag telemetrie biedt ondersteuning voor het model van de standaard uitbreidbaarheid met behulp van aangepaste `properties` en `measurements`.

## <a name="name"></a>Naam

Naam van de aanvraag vertegenwoordigt codepad uitgevoerd om de aanvraag niet verwerken. De kardinaliteit van de lage waarde om toe te staan beter groepering van aanvragen. Voor deze HTTP-aanvragen de HTTP-methode en de URL-pad sjabloon zoals vertegenwoordigt `GET /values/{id}` zonder de werkelijke `id` waarde.

Application Insights web SDK verzendt een Aanvraagnaam 'als zodanig' met betrekking tot gebruik van hoofdletters. Groeperen op UI is hoofdlettergevoelig zodat `GET /Home/Index` afzonderlijk wordt geteld van `GET /home/INDEX` Hoewel vaak ze ertoe leiden dat de uitvoering van de dezelfde controller en de actie. De reden hiervoor is dat URL's in het algemeen zijn [hoofdlettergevoelig](http://www.w3.org/TR/WD-html40-970708/htmlweb.html). U kunt om te controleren of alle `404` opgetreden voor de URL's in hoofdletters worden getypt. Vindt u meer op de naam van verzameling request door ASP.Net Web SDK in de [blogbericht](http://apmtips.com/blog/2015/02/23/request-name-and-url/).

Maximale lengte: 1024 tekens

## <a name="id"></a>Id

Id van een exemplaar van de aanvraag-aanroep. Gebruikt voor correlatie tussen aanvraag en andere telemetrie-items. ID moet uniek zijn. Zie voor meer informatie [correlatie](application-insights-correlation.md) pagina.

Maximale lengte: 128 tekens

## <a name="url"></a>URL

Aanvraag-URL met alle parameters voor query-tekenreeks.

Maximale lengte: 2048 tekens bestaan

## <a name="source"></a>Bron

Bron van de aanvraag. Voorbeelden zijn de instrumentatiesleutel van de aanroepfunctie of het ip-adres van de aanroepfunctie. Zie voor meer informatie [correlatie](application-insights-correlation.md) pagina.

Maximale lengte: 1024 tekens

## <a name="duration"></a>Duur

Duur in de indeling van aanvraag: `DD.HH:MM:SS.MMMMMM`. Moet positief zijn en kleiner dan `1000` dagen. Dit veld is vereist als aanvraagtelemetrie Hiermee geeft u het opnieuw met de begin- en het einde.

## <a name="response-code"></a>Reactiecode

Resultaat van een aanvraag kan worden uitgevoerd. HTTP-statuscode voor HTTP-aanvragen. Kan `HRESULT` waarde of uitzondering type voor andere aanvraagtypen.

Maximale lengte: 1024 tekens

## <a name="success"></a>Geslaagd

De vermelding van geslaagde of mislukte aanroep. Dit veld is vereist. Wanneer niet ingesteld op expliciet `false` -aanvraag beschouwd als geslaagd. Deze waarde instelt op `false` als de bewerking is onderbroken door uitzondering of foutcode resultaat geretourneerd.

Voor de webtoepassingen Application Insights definiëren aanvraag is mislukt tijdens de antwoordcode ligt de `400` of gelijk zijn aan `401`. Er zijn echter gevallen wanneer deze standaardtoewijzing is niet overeenkomt met de semantische van de toepassing. Antwoordcode `404` kan duiden op 'geen records', die deel van de normale stroom uitmaken kunnen. Ook kan dit wijzen op een verbroken koppeling. U kunt zelfs meer geavanceerde logica implementeren voor de verbroken koppelingen. Alleen wanneer deze koppelingen zich op dezelfde site bevinden door de url verwijzende analyseren, kunt u verbroken koppelingen markeren als fouten. Of gemarkeerd als codefouten wanneer deze vanuit de mobiele toepassing van het bedrijf. Op dezelfde manier `301` en `302` geeft aan wanneer deze vanuit de client die biedt geen ondersteuning voor omleiding is mislukt.

Gedeeltelijk geaccepteerd inhoud `206` kan duiden op een storing van een algemene aanvraag. Application Insights-eindpunt ontvangt bijvoorbeeld een batch van telemetrie-items als één aanvraag. Deze retourneert `206` wanneer sommige items in de batch zijn niet verwerkt. Toenemende aantal `206` duidt op een probleem dat moet worden onderzocht. Vergelijkbare logica van toepassing op `207` meerdere Status waar het succes de slechtste van afzonderlijke responscodes wellicht.

U kunt meer op aanvraag resultaat lezen code en statuscode in de [blogbericht](http://apmtips.com/blog/2016/12/03/request-success-and-response-code/).

## <a name="custom-properties"></a>Aangepaste eigenschappen

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Aangepaste metingen

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste aanvraagtelemetrie schrijven](app-insights-api-custom-events-metrics.md#trackrequest)
- Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en data model.
- Meer informatie over hoe [configureren van ASP.NET Core](app-insights-asp-net.md) een toepassing met Application Insights.
- Bekijk [platforms](app-insights-platforms.md) ondersteund door de Application Insights.
