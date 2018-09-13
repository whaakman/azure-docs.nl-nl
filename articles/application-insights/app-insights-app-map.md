---
title: Overzicht van de toepassing in Azure Application Insights | Microsoft Docs
description: Topologieën voor complexe toepassingen met het overzicht van de toepassing bewaken
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 32d735b24eef90f75e3ab4b9c1af58c4b6b02b15
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643640"
---
# <a name="application-map-triage-distributed-applications"></a>Overzicht van de toepassing: Sorteren gedistribueerde toepassingen

Overzicht van de toepassing kunt u spot knelpunten of fout hotspots voor alle onderdelen van de gedistribueerde toepassing. Elk knooppunt op de kaart vertegenwoordigt een toepassingsonderdeel of de bijbehorende afhankelijkheden; en health KPI heeft en de status van waarschuwingen. U kunt de via elk onderdeel klikken voor meer gedetailleerde diagnostische gegevens, zoals Application Insights-gebeurtenissen. Als uw app gebruikmaakt van Azure-services, kunt u ook verder klikken naar Azure diagnostics, zoals SQL Database Advisor-aanbevelingen.

## <a name="what-is-a-component"></a>Wat is een onderdeel?

Onderdelen zijn onafhankelijk implementeerbare onderdelen van uw toepassing gedistribueerd/microservices. Ontwikkelaars en uitvoerende teams hebben zichtbaarheid van de code op serverniveau of toegang tot telemetrie die is gegenereerd door de onderdelen van deze toepassing. 

* Onderdelen zijn anders dan 'waargenomen' externe afhankelijkheden, zoals SQL, EventHub enz. die uw team/organisatie mogelijk geen toegang tot (code of telemetrie).
* Onderdelen worden uitgevoerd op een willekeurig aantal exemplaren van de rol-server-container.
* Onderdelen kunnen worden afzonderlijke Application Insights-instrumentatiesleutels (zelfs als abonnementen verschillen) of verschillende rollen die rapporteren aan een enkele Application Insights-instrumentatiesleutel. De ervaring van de kaart voorbeeld ziet u de onderdelen onafhankelijk van de manier waarop ze zijn ingesteld.

## <a name="composite-application-map"></a>Samengestelde toepassingstoewijzing

U ziet de volledige toepassing-topologie voor meerdere niveaus van gerelateerde toepassingsonderdelen. Onderdelen mogelijk op verschillende Application Insights-resources of verschillende rollen in een enkele resource. Het toepassingsoverzicht vindt onderdelen door de volgende HTTP-afhankelijkheidsaanroepen tussen servers met de Application Insights-SDK geïnstalleerd. 

Deze ervaring wordt gestart met progressieve detectie van de onderdelen. Wanneer u eerst het toepassingsoverzicht laadt, wordt een set van query's worden geactiveerd voor het detecteren van de onderdelen die betrekking hebben op dit onderdeel. Een knop in de linkerbovenhoek wordt bijgewerkt met het aantal onderdelen in uw toepassing zodra ze worden gedetecteerd. 

Op 'Toewijzingsonderdelen bijwerken' klikt, wordt de kaart met alle onderdelen die zijn gedetecteerd tot vernieuwd.

Deze detectiestap is niet vereist als alle onderdelen van rollen binnen één Application Insights-resource. Het initiële laden voor dergelijke toepassing hebben alle onderdelen daarvan.

![Schermafbeelding van de toepassing-kaart](media/app-insights-app-map/001.png)

Een van de belangrijkste doelstellingen met deze ervaring is het kunnen complexe topologieën met honderden onderdelen te visualiseren.

Klik op een onderdeel om te zien van verwante inzichten en gaat u naar de prestaties en de fout sorteren ervaring voor het betreffende onderdeel.

![Flyout](media/app-insights-app-map/application-map-001.png)

### <a name="investigate-failures"></a>Mislukte pogingen onderzoeken

Selecteer **mislukte pogingen onderzoeken** om te starten van het deelvenster met fouten.

![Schermafbeelding van de knop fouten onderzoeken](media/app-insights-app-map/investigate-failures.png)

![Schermafbeelding van de ervaring van fouten](media/app-insights-app-map/failures.png)

### <a name="investigate-performance"></a>Prestaties onderzoeken

Om op te lossen prestaties problemen Selecteer **prestaties onderzoeken**

![Schermafdruk van knop prestaties onderzoeken](media/app-insights-app-map/investigate-performance.png)

![Schermafbeelding van prestaties](media/app-insights-app-map/performance.png)

### <a name="go-to-details"></a>Naar de details gaan

Selecteer **Ga naar details** verkennen van de end-to-end-transactie-ervaring die weergaven die worden uitgevoerd op het niveau van de call stack kunt bieden.

![Schermafbeelding van de knop Ga voor meer informatie](media/app-insights-app-map/go-to-details.png)

![Schermafbeelding van end-to-end-transactiedetails](media/app-insights-app-map/end-to-end-transaction.png)

### <a name="view-in-analytics"></a>Weergeven in Analytics

Query's uitvoeren en uw toepassingen gegevens verder klikken onderzoeken **weergeven in analytics**.

![Schermafbeelding van de weergave in de knop analytics](media/app-insights-app-map/view-in-analytics.png)

![Schermafbeelding van de analytics-ervaring](media/app-insights-app-map/analytics.png)

### <a name="alerts"></a>Waarschuwingen

Als u wilt weergeven van actieve waarschuwingen en de onderliggende regels die ervoor zorgen de waarschuwingen dat moet deze worden geactiveerd, selecteer **waarschuwingen**.

![Schermafbeelding van de knop waarschuwingen](media/app-insights-app-map/alerts.png)

![Schermafbeelding van de analytics-ervaring](media/app-insights-app-map/alerts-view.png)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Feedback
Geef feedback via de portal feedbackoptie.

![Installatiekopie van het MapLink-1](./media/app-insights-app-map/13.png)

## <a name="next-steps"></a>Volgende stappen

* [Azure Portal](https://portal.azure.com)
