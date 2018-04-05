---
title: Verouderde Enterprise plan prijsinformatie voor Azure Application Insights | Microsoft Docs
description: Telemetrie volumes beheren en bewaken van kosten in Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: mbullwin
ms.openlocfilehash: 6e7591ccf0f21099474a08dda088422c377135f6
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="enterprise-plan-details"></a>Details van het Enterprise

Application Insights heeft twee prijscategorie plannen. Het standaardplan heet [Basic](app-insights-pricing.md), waaronder alle dezelfde functies als het plan Enterprise op geen toevoeging kosten en facturen voornamelijk op het volume gegevens ingenomen. Als u de Operations Management Suite gebruikt, moet u ervoor kiezen voor de onderneming plan heeft een per knooppunt kosten in rekening gebracht samen met dagelijks gegevens rechten en vervolgens brengt voor gegevens boven het inbegrepen tegoed ingenomen.

Zie de [Application Insights pagina met prijzen](http://azure.microsoft.com/pricing/details/application-insights/) voor huidige prijzen in uw valuta en de regio.

## <a name="heres-how-the-enterprise-plan-works"></a>Hier wordt de werking van het Enterprise-plan

* U betaalt per knooppunt dat telemetrie voor alle apps in de Enterprise-serviceplan verzendt.
 * Een *knooppunt* is een fysieke of virtuele server-computer of op een exemplaar van de rol Platform as a Service, die als host fungeert voor uw app.
 * Machines voor ontwikkeling, clientbrowsers en mobiele apparaten worden niet meegeteld als knooppunten.
 * Als uw app heeft verschillende onderdelen die verzenden van telemetrie, zoals een webservice en een back-end-worker, worden ze afzonderlijk meegeteld.
 * [Metrische gegevens livestream](app-insights-live-stream.md) gegevens wordt niet meegeteld voor de prijzen purposes.* over een abonnement, zijn uw kosten per knooppunt niet per app. Als er vijf knooppunten verzenden van telemetrie voor 12 apps en vervolgens de kosten is voor vijf knooppunten.
* Hoewel kosten worden genoteerd per maand, bent u in rekening gebracht voor elk uur waarin een knooppunt telemetrie vanuit een app verzendt alleen. De kosten per uur is het tussen aanhalingstekens maandelijkse kosten / 744 (het aantal uren in een maand 31 dagen).
* De toewijzing van een data volume van 200 MB per dag wordt gegeven voor elk knooppunt (met de Uurlijkse samenvattingen) zijn gedetecteerd. Ongebruikte gegevens toewijzing is niet van één dag overgedragen naar de volgende.
 * Als u de prijscategorie Enterprise-optie kiest, wordt elk abonnement een dagelijkse vergoeding van gegevens op basis van het aantal knooppunten verzenden van telemetrie naar de Application Insights-resources in het desbetreffende abonnement. Als er 5 verzenden van gegevens alle dag-knooppunten, hebt u dus een gegroepeerde toegestane van 1 GB toegepast op de Application Insights-bronnen in het desbetreffende abonnement. Het maakt niet uit als u bepaalde knooppunten zijn meer gegevens dan andere knooppunten verzenden omdat de ingesloten gegevens wordt gedeeld door alle knooppunten. Als op een bepaalde dag, de Application Insights-resources meer gegevens ontvangt dan is opgenomen in de toewijzing van de dagelijkse gegevens voor dit abonnement, gelden de overschrijding gegevens per GB. 
 * De dagelijkse gegevens vergoeding wordt berekend als het aantal uren van de dag (UTC met) dat elk knooppunt gedeeld door 24 keer 200 MB telemetrie verzendt. Dus als er 4 knooppunten voor het verzenden van telemetrie tijdens 15 van de 24 uur van de dag, de opgenomen gegevens voor die dag zou ((4 x 15) / 24) x 200 MB = 500 MB. Voor de prijs van 2.30 USD per GB voor data overschrijding, zou de kosten 1.15 USD zijn als de knooppunten 1 GB aan gegevens die dag verzonden.
 * Het bedrijf van plan dagelijkse vergoeding wordt niet gedeeld met toepassingen die u ervoor de optie en ongebruikte toegestane gekozen hebt wordt niet overgenomen van dagelijkse. 

## <a name="here-are-some-examples-of-determining-distinct-node-count"></a>Hier volgen enkele voorbeelden van het aantal afzonderlijke knooppunten bepalen

| Scenario                               | Totaal aantal per dag knooppunt |
|:---------------------------------------|:----------------:|
| 1 toepassing gebruik maakt van 3-Azure App Service-exemplaren en 1 virtuele server | 4 |
| 3-toepassingen met 2 virtuele machines en de Application Insights-resources voor deze toepassingen zijn in hetzelfde abonnement en de Enterprise-serviceplan | 2 | 
| 4 toepassingen waarvan toepassingen Insights-bronnen bevinden zich in hetzelfde abonnement. Elke toepassing wordt uitgevoerd 2 exemplaren 16 piekuren en 4 exemplaren tijdens de piekuren die 8. | 13.33 | 
| Cloudservices met Werkrol 1 en 1 Webrol met elk 2 exemplaren | 4 | 
| 5-knooppunt Service Fabric-Cluster met 50 micro-services, elke micro-service met 3 exemplaren | 5|

* De precieze knooppunt tellen gedrag is afhankelijk van waarop Application Insights-SDK uw toepassing wordt gebruikt. 
  * In de SDK-versies 2.2 vanaf, de Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) of [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) rapport elke toepassingshost wordt als een knooppunt, bijvoorbeeld de computernaam voor de fysieke server en de VM-hosts of de de naam van het exemplaar in het geval van cloudservices.  De enige uitzondering hierop is de toepassingen alleen [.NET Core](https://dotnet.github.io/) en de Application Insights Core SDK, waarin u slechts één case-knooppunt wordt gerapporteerd voor alle hosts omdat de hostnaam niet beschikbaar is. 
  * Voor eerdere versies van de SDK de [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) gedraagt zich net zoals de nieuwere versies van de SDK, maar het [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) slechts één knooppunt, ongeacht het aantal huidige toepassingshosts rapporteert. 
  * Als uw toepassing van de SDK gebruikmaakt roleInstance instellen op een aangepaste waarde, standaard zal die dezelfde waarde worden gebruikt om te bepalen het aantal knooppunten. 
  * Als u een nieuwe versie van de SDK gebruikt met een app die wordt uitgevoerd vanaf clientcomputers of mobiele apparaten, is het mogelijk dat het aantal knooppunten een getal dat heel groot (van het grote aantal clientcomputers of mobiele apparaten) mogelijk retourneren. 
