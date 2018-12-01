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
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: mbullwin
ms.openlocfilehash: 820c1baaf920dc2ada2aba1a4a8cc3b11ab2db2d
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726811"
---
# <a name="enterprise-plan-details"></a>Abonnementsdetails voor Enterprise

Azure Application Insights heeft twee abonnementen: Basic en Enterprise. De [Basic](app-insights-pricing.md) prijsplan is het standaardabonnement. Deze bevat alle Enterprise-plan functies, zonder extra kosten. De Basic-abonnement van rekeningen voornamelijk op het volume van gegevens die wordt opgenomen. 

Het Enterprise-plan heeft een per-knooppunt in rekening gebracht en elk knooppunt ontvangt een dagelijkse hoeveelheid. In de onderneming prijsplan, in rekening gebracht voor gegevens die zijn opgenomen boven de limiet opgenomen. Als u van Operations Management Suite gebruikmaakt, moet u het Enterprise-plan kiezen. 

Zie voor de huidige prijzen in de valuta en de regio [prijzen van Application Insights](https://azure.microsoft.com/pricing/details/application-insights/).

> [!NOTE]
> In April 2018, we [geïntroduceerd](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) een nieuw prijsmodel voor het bewaken van Azure. Dit model neemt een eenvoudige 'betalen per gebruik'-model in de volledige portfolio van services controleren. Meer informatie over de [nieuwe prijsmodel](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), hoe naar [beoordelen wat de impact van het verplaatsen naar dit model](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) op basis van uw gebruikspatronen en [over opt-in het nieuwe model](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model)

## <a name="how-the-enterprise-plan-works"></a>De werking van het Enterprise-plan

* U betaalt voor elk knooppunt dat telemetrie voor alle apps in het Enterprise-plan verzendt.
 * Een *knooppunt* is een fysieke of virtuele server-machine of een rol van de platform-as-a-service-exemplaar dat als host fungeert voor uw app.
 * Ontwikkeling machines, clientbrowsers en mobiele apparaten wordt niet gerekend als knooppunten.
 * Als uw app heeft verschillende onderdelen die verzenden van telemetriegegevens, zoals een webservice en een back-end-worker, worden de onderdelen afzonderlijk gerekend.
 * [Live Metrics Stream](app-insights-live-stream.md) gegevens wordt niet geteld voor prijsbepaling. In een abonnement zijn uw kosten per knooppunt, niet per app. Als u vijf knooppunten hebt die verzenden van telemetrie voor 12 apps, de kosten zijn voor vijf knooppunten.
* Hoewel de kosten worden genoteerd per maand, u betaalt alleen voor elk uur dat een knooppunt telemetrie vanuit een app verzendt. Het uurtarief is het tussen aanhalingstekens maandelijkse bedrag gedeeld door 744 (het aantal uren in een maand van 31 dagen).
* Een toewijzing van de gegevens-volume van 200 MB per dag is opgegeven voor elk knooppunt dat wordt gedetecteerd (met een granulariteit per uur). Toewijzing van niet-gebruikte gegevens wordt niet van één dag overgedragen naar de volgende.
 * Als u ervoor de onderneming prijsplan kiest, krijgt elk abonnement een dagelijks toegestane hoeveelheid gegevens op basis van het aantal knooppunten dat telemetrie wordt verzonden naar de Application Insights-resources in het desbetreffende abonnement. Dus als u vijf knooppunten die gegevens alle dag verzenden hebt, u hebt een gezamenlijke limiet van 1 GB die worden toegepast op alle Application Insights-resources in het desbetreffende abonnement. Het maakt niet uit of bepaalde knooppunten meer gegevens dan andere knooppunten verzenden omdat de hoeveelheid gegevens verdeeld over alle knooppunten. Als de Application Insights-resources op een bepaalde dag meer gegevens ontvangen dan is opgenomen in de dagelijkse toewijzing van gegevens voor dit abonnement, de per GB-overschrijding gegevens in rekening gebracht. 
 * De dagelijkse limiet voor de gegevens wordt berekend als het aantal uren op een dag (met behulp van UTC) dat elk knooppunt gedeeld door 24 uur per dag verzendt, vermenigvuldigd met 200 MB telemetrie verzendt. Dus als u vier knooppunten die tijdens 15 van de 24 uur per dag telemetriegegevens verzendt hebt, de hoeveelheid gegevens voor die dag zou zijn ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB. Voor de prijs van 2.30 USD per GB voor overschrijding van gegevens, zou de kosten in rekening gebracht 1.15 USD zijn als de knooppunten 1 GB aan gegevens op die dag verzenden.
 * De dagelijks toegestane voor Enterprise-plan niet worden gedeeld met toepassingen waarvoor u het Basic-abonnement hebt gekozen. Niet-gebruikte limiet wordt niet overgedragen van dag tot dag. 

## <a name="examples-of-how-to-determine-distinct-node-count"></a>Voorbeelden van het aantal afzonderlijke knooppunten bepalen

| Scenario                               | Totale dagelijks aantal knooppunten |
|:---------------------------------------|:----------------:|
| 1 toepassing met behulp van 3-Azure App Service-exemplaren en 1 virtuele server | 4 |
| 3-toepassingen die worden uitgevoerd op 2 virtuele machines; de Application Insights-resources voor deze toepassingen zijn in hetzelfde abonnement en het Enterprise-plan | 2 | 
| 4 toepassingen waarvan Applications Insights-resources zich in hetzelfde abonnement; elke toepassing met 2 exemplaren 16 na kantooruren en 4 exemplaren tijdens piekuren 8 | 13.33 | 
| Cloudservices met 1 Werkrol en 1 Webrol, elk met 2-instanties | 4 | 
| Een Azure Service Fabric-cluster van het 5-knooppunten met 50 microservices; elke microservice 3 instanties | 5|

* De precieze knooppunt geteld, hangt ervan af op welke Application Insights-SDK van uw toepassing gebruikt. 
  * In de SDK versie 2.2 of hoger, zowel de Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) en de [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) rapporteren van elke toepassingshost als een knooppunt. Voorbeelden hiervan zijn de naam van de computer voor fysieke servers en VM-hosts of de naam van het exemplaar voor cloudservices.  De enige uitzondering hierop is een toepassing die alleen wordt gebruikt door de [.NET Core](https://dotnet.github.io/) en de Application Insights Core SDK. In dat geval wordt slechts één knooppunt voor alle hosts gerapporteerd, omdat de hostnaam is niet beschikbaar. 
  * Voor eerdere versies van de SDK de [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) gedraagt zich als de nieuwere versies van de SDK, maar de [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) rapporten slechts één knooppunt, ongeacht het aantal hosts voor toepassingen. 
  * Als uw toepassing gebruikmaakt van de SDK om in te stellen **roleInstance** naar een aangepaste waarde, standaard, die dezelfde waarde wordt gebruikt om te bepalen aantal knooppunten. 
  * Als u een nieuwe versie van de SDK met een app die op clientcomputers of mobiele apparaten wordt uitgevoerd gebruikt, kan het aantal knooppunten een getal dat is te lang. (vanwege het grote aantal clientmachines of mobiele apparaten) retourneren. 
