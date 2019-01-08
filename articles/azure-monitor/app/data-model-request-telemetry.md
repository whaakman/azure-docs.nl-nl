---
title: Azure Application Insights-telemetriegegevens Model - aanvragen van telemetrie | Microsoft Docs
description: Application Insights-gegevensmodel voor aanvraagtelemetrie
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/07/2019
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 1f9a108a91a2fa917ec58e6cff487c6dca71130f
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54076396"
---
# <a name="request-telemetry-application-insights-data-model"></a>Telemetrie van aanvraag: Application Insights-gegevensmodel

Een aanvraag telemetrie-item (in [Application Insights](../../application-insights/app-insights-overview.md)) Hiermee geeft u de logische volgorde van de uitvoering is geactiveerd door een externe aanvraag naar uw toepassing. De uitvoering van elke aanvraag wordt geïdentificeerd door de unieke `ID` en `url` met alle uitvoeringsparameters. U kunt aanvragen groeperen op logische `name` en definieer de `source` van deze aanvraag. Uitvoering van code kan resulteren in `success` of `fail` en heeft een bepaalde `duration`. Geslaagde en mislukte uitvoeringen kunnen worden gegroepeerd verder door `resultCode`. De begintijd voor de aanvraagtelemetrie op het niveau van de envelop gedefinieerd.

Aanvraag telemetrie biedt ondersteuning voor de standard uitbreidbaarheidsmodel met behulp van aangepaste `properties` en `measurements`.

## <a name="name"></a>Name

Naam van de aanvraag vertegenwoordigt codepad genomen om de aanvraag te verwerken. De kardinaliteit van de lage waarde om toe te staan beter groepering van aanvragen. Voor HTTP-deze aanvragen vertegenwoordigt de HTTP-methode en de sjabloon van de URL-pad, zoals `GET /values/{id}` zonder de werkelijke `id` waarde.

Application Insights web SDK verzendt Aanvraagnaam "as is" met betrekking tot gebruik van hoofdletters. Groeperen op gebruikersinterface is hoofdlettergevoelig, zodat `GET /Home/Index` worden afzonderlijk gerekend vanaf `GET /home/INDEX` Hoewel vaak ze leiden tot de dezelfde controller en de actie kan worden uitgevoerd. De reden hiervoor is dat URL's in het algemeen zijn [hoofdlettergevoelig](https://www.w3.org/TR/WD-html40-970708/htmlweb.html). U kunt om te controleren of alle `404` is er gebeurd voor de URL's in hoofdletters worden getypt. U vindt meer op de naam van verzameling van aanvragen door ASP.Net Web SDK in de [blogbericht](https://apmtips.com/blog/2015/02/23/request-name-and-url/).

Maximumlengte: 1024 tekens

## <a name="id"></a>Id

Id van een exemplaar van de aanroep van aanvraag. Gebruikt voor correlatie tussen aanvragen en andere telemetrie-items. -ID moet globaal uniek zijn. Zie voor meer informatie, [correlatie](../../azure-monitor/app/correlation.md) pagina.

Maximumlengte: 128 tekens

## <a name="url"></a>URL

Aanvraag-URL met alle queryreeksparameters.

Maximumlengte: 2048 tekens

## <a name="source"></a>Bron

De bron van de aanvraag. Voorbeelden hiervan zijn de instrumentatiesleutel van de oproepende functie of het ip-adres van de oproepende functie. Zie voor meer informatie, [correlatie](../../azure-monitor/app/correlation.md) pagina.

Maximumlengte: 1024 tekens

## <a name="duration"></a>Duur

Duur in de indeling van aanvraag: `DD.HH:MM:SS.MMMMMM`. Moet positief en kleiner dan `1000` dagen. Dit veld is verplicht als aanvraagtelemetrie Hiermee geeft u de bewerking opnieuw uit met de begin- en het einde.

## <a name="response-code"></a>Antwoordcode

Resultaat van een aanvraag kan worden uitgevoerd. HTTP-statuscode voor HTTP-aanvragen. Kan het zijn `HRESULT` type waarde of een uitzondering voor andere aanvraagtypen.

Maximumlengte: 1024 tekens

## <a name="success"></a>Geslaagd

Vermelding van geslaagde of mislukte aanroep. Dit veld is vereist. Als niet is ingesteld op expliciet `false` -een aanvraag wordt beschouwd om succesvol te zijn. Deze waarde instelt op `false` als bewerking is onderbroken door uitzondering of foutcode van het resultaat geretourneerde.

Voor de web-apps, Application Insights definiëren een aanvraag als voltooid wanneer de responscode is minder dan `400` of gelijk zijn aan `401`. Er zijn echter gevallen wanneer deze standaardtoewijzing is niet overeenkomt met de semantische van de toepassing. Antwoordcode `404` kan duiden op "Er zijn geen records', Hiermee deel van de normale stroom uitmaken kunnen. Ook kan dit wijzen op een verbroken koppeling. Voor de verbroken koppelingen, kunt u ook meer geavanceerde logica implementeren. Alleen wanneer deze koppelingen bevinden zich op dezelfde site door het analyseren van de url van verwijzende site, kunt u verbroken koppelingen markeren als fouten. Of deze markeren als fouten wanneer deze vanuit de mobiele toepassing van het bedrijf. Op dezelfde manier `301` en `302` geeft aan dat de fout wanneer deze vanuit de client die geen ondersteuning voor omleiden bieden.

Gedeeltelijk geaccepteerd inhoud `206` kan duiden op een storing van een algemene aanvraag. Application Insights-eindpunt ontvangt bijvoorbeeld een batch van telemetrie-items als één aanvraag. Het resultaat `206` wanneer sommige items in de batch zijn niet verwerkt. Toenemende aantal `206` duidt op een probleem dat moet worden onderzocht. Dezelfde logica is van toepassing op `207` meerdere Status waarbij het succes in de slechtste van afzonderlijke responscodes mogelijk.

U kunt meer op aanvraag resultaat lezen code en de statuscode in de [blogbericht](https://apmtips.com/blog/2016/12/03/request-success-and-response-code/).

## <a name="custom-properties"></a>Aangepaste eigenschappen

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Aangepaste metingen

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Volgende stappen

- [Voor aangepaste aanvraagtelemetrie schrijven](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest)
- Zie [gegevensmodel](data-model.md) voor Application Insights-typen en -gegevensmodel.
- Meer informatie over het [configureren van ASP.NET Core](../../azure-monitor/app/asp-net.md) toepassing met Application Insights.
- Bekijk [platforms](../../azure-monitor/app/platforms.md) ondersteund door Application Insights.
