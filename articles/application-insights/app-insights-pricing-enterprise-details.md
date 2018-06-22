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
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: mbullwin
ms.openlocfilehash: 65307eab0bf1b5f502f11c14c369826cd12e0966
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309832"
---
# <a name="enterprise-plan-details"></a>Abonnementsdetails voor Enterprise

Azure Application Insights heeft twee prijscategorie plannen: Basic- en Enterprise. De [Basic](app-insights-pricing.md) prijzen van plan is het standaardplan. Dit omvat alle plan bedrijfsonderdelen, zonder extra kosten. De facturen basisniveau voornamelijk op het volume van de gegevens die wordt ingenomen. 

De Enterprise-serviceplan heeft een kosten per knooppunt en elk knooppunt een dagelijkse gegevens vergoeding ontvangt. In de onderneming prijzen plan, u in rekening worden gebracht voor gegevens boven het inbegrepen tegoed ingenomen. Als u Operations Management Suite gebruikt, moet u de Enterprise-serviceplan kiezen. 

Zie voor de huidige prijzen in uw valuta en regio, [Application Insights prijzen](http://azure.microsoft.com/pricing/details/application-insights/).

> [!NOTE]
> In April 2018, we [geïntroduceerd](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) een nieuw prijsmodel voor het bewaken van Azure. Dit model neemt een eenvoudige 'betalen naar gebruik' model in de volledige portfolio van de bewaking van services. Meer informatie over de [nieuwe prijzen model](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), hoe naar [beoordeling van de gevolgen voor de upgrade naar dit model](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) op basis van de gebruikspatronen en [hoe u kunt kiezen voor het nieuwe model](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model)

## <a name="how-the-enterprise-plan-works"></a>Hoe werkt dit plan Enterprise

* U betaalt voor elk knooppunt dat telemetrie voor alle apps in de Enterprise-serviceplan verzendt.
 * Een *knooppunt* is een fysieke of virtuele server-machine of een platform as a service-rolexemplaar dat als host fungeert voor uw app.
 * Machines voor ontwikkeling, clientbrowsers en mobiele apparaten meetellen niet als knooppunten.
 * Als uw app heeft verschillende onderdelen die verzenden van telemetrie, zoals een webservice en een back-end-worker, worden de onderdelen afzonderlijk meegeteld.
 * [Metrische gegevens livestream](app-insights-live-stream.md) gegevens wordt niet meegeteld voor de doeleinden prijzen. In een abonnement zijn uw kosten per knooppunt niet per app. Als u vijf knooppunten hebt die verzenden van telemetrie voor 12 apps, de kosten zijn voor vijf knooppunten.
* Hoewel kosten worden genoteerd per maand, bent u in rekening gebracht voor elk uur waarin een knooppunt telemetrie vanuit een app verzendt alleen. De kosten per uur is het tussen aanhalingstekens maandelijkse kosten gedeeld door 744 (het aantal uren in een maand 31 dagen).
* De toewijzing van een data volume van 200 MB per dag wordt gegeven voor elk knooppunt dat wordt gedetecteerd (met de Uurlijkse samenvattingen). Ongebruikte gegevens toewijzing wordt niet overgenomen van één dag naar de volgende.
 * Als u ervoor de onderneming prijzen plan kiest, krijgt elk abonnement een dagelijkse vergoeding van gegevens op basis van het aantal knooppunten dat verzenden van telemetrie naar de Application Insights-resources in het desbetreffende abonnement. Dus als er vijf knooppunten die gegevens alle dag verzenden, hebt u een gegroepeerde toegestane van 1 GB toegepast op alle Application Insights-resources in het desbetreffende abonnement. Het maakt niet uit als u meer gegevens dan andere knooppunten in bepaalde knooppunten worden verzonden omdat de ingesloten gegevens wordt gedeeld door alle knooppunten. Als op een bepaalde dag, de Application Insights-resources meer gegevens ontvangt dan is opgenomen in de toewijzing van de dagelijkse gegevens voor dit abonnement, gelden de overschrijding gegevens per GB. 
 * De dagelijkse gegevens vergoeding wordt berekend als het aantal uren van de dag (UTC met) dat elk knooppunt gedeeld door 24 vermenigvuldigd met 200 MB telemetrie verzendt. Dus als u vier knooppunten die telemetrie tijdens 15 van de 24 uur van de dag hebt verzenden, de opgenomen gegevens voor die dag zou zijn ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB. Voor de prijs van 2.30 USD per GB voor data overschrijding, zou de kosten 1.15 USD zijn als de knooppunten 1 GB aan gegevens die dag verzonden.
 * De dagelijkse vergoeding voor Enterprise-abonnement wordt niet met toepassingen die u ervoor het basisniveau gekozen hebt gedeeld. Ongebruikte toegestane wordt niet overgenomen van dagelijkse. 

## <a name="examples-of-how-to-determine-distinct-node-count"></a>Voorbeelden van het aantal afzonderlijke knooppunten bepalen

| Scenario                               | Totaal aantal per dag knooppunt |
|:---------------------------------------|:----------------:|
| 1 toepassing met behulp van 3-Azure App Service-exemplaren en 1 virtuele server | 4 |
| 3 toepassingen die worden uitgevoerd op 2 virtuele machines; de Application Insights-resources voor deze toepassingen zijn in hetzelfde abonnement en in de Enterprise-serviceplan | 2 | 
| 4 toepassingen waarvan toepassingen Insights-bronnen bevinden zich in hetzelfde abonnement; elke toepassing met 2 exemplaren 16 piekuren en 4 exemplaren tijdens 8 piekuren | 13.33 | 
| Cloudservices met Werkrol 1 en 1 Webrol met elk 2 exemplaren | 4 | 
| Een Azure Service Fabric-cluster van het 5-knooppunten waarop 50 microservices; elke microservice actieve 3 exemplaren | 5|

* De precieze knooppunt telling, is afhankelijk van op welke Application Insights-SDK wordt gebruikt door uw toepassing. 
  * In de SDK versie 2.2 of hoger, de Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) en de [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) rapporteren van elke toepassingshost als een knooppunt. Voorbeelden zijn de computernaam voor de fysieke server en de VM-hosts of de naam van het exemplaar voor cloudservices.  De enige uitzondering hierop is een toepassing die alleen wordt gebruikt door de [.NET Core](https://dotnet.github.io/) en de Application Insights Core-SDK. In dat geval wordt slechts één knooppunt voor alle hosts gerapporteerd, omdat de hostnaam is niet beschikbaar. 
  * Voor eerdere versies van de SDK de [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) gedraagt zich als de nieuwere versies van de SDK, maar de [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) slechts één knooppunt, ongeacht het aantal toepassingshosts rapporteert. 
  * Als uw toepassing gebruikmaakt van de SDK om in te stellen **roleInstance** op een aangepaste waarde standaard die dezelfde waarde wordt gebruikt om te bepalen aantal knooppunten. 
  * Als u een nieuwe SDK-versie met een app die op clientcomputers of mobiele apparaten wordt uitgevoerd, kan het aantal knooppunten een getal dat is te lang. (vanwege het grote aantal clientcomputers of mobiele apparaten) geretourneerd. 
