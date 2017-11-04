---
title: Kaart van de toepassing in Azure Application Insights | Microsoft Docs
description: Een visuele presentatie van de afhankelijkheden tussen de onderdelen van de app, voorzien van KPI's en waarschuwingen.
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: e1eb2177d6032142781e6e31af6c7f6313d38f4d
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="application-map-in-application-insights"></a>De toepassingstoewijzing in Application Insights
In [Azure Application Insights](app-insights-overview.md), toewijzing van de toepassing is een visuele indeling van de afhankelijkheidsrelaties tussen onderdelen van uw toepassing. Elk onderdeel toont KPI's zoals belasting, prestaties, fouten en waarschuwingen, om te zien van de onderdelen die zijn veroorzaakt door een prestatieprobleem of fout. U kunt via van elk onderdeel klikken om meer gedetailleerde diagnostische gegevens, zoals Application Insights-gebeurtenissen te. Als uw app gebruikmaakt van Azure-services, kunt u ook doorklikken naar Azure diagnostics, zoals SQL Database Advisor aanbevelingen.

U kunt een toepassing toewijzen aan de Azure-dashboard, waar deze zich volledig functioneel vastmaken zoals andere grafieken. 

## <a name="open-the-application-map"></a>Open de toepassing-kaart
De kaart openen vanuit de overzichtsblade van uw toepassing:

![Open de app-kaart](./media/app-insights-app-map/01.png)

![App-kaart](./media/app-insights-app-map/02.png)

De kaart wordt weergegeven:

* Beschikbaarheidstests
* Client-side-onderdeel (bewaakt met de JavaScript SDK)
* Server-side-onderdeel
* Afhankelijkheden van de client en server-onderdelen

U kunt uitvouwen en samenvouwen afhankelijkheid koppeling groepen:

![samenvouwen](./media/app-insights-app-map/03.png)

Als er veel afhankelijkheden van een bepaald type (SQL, HTTP-enzovoort), kunnen ze gegroepeerde weergegeven. 

![gegroepeerde afhankelijkheden](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a>Ter plaatse problemen
Elk knooppunt heeft relevante prestatie-indicatoren, zoals de belasting, prestaties en fout tarieven voor het desbetreffende onderdeel. 

De pictogrammen waarschuwing Markeer mogelijke problemen. Een oranje waarschuwing betekent dat er fouten zijn in aanvragen, paginaweergaven of afhankelijkheidsaanroepen. Rood betekent een Faalpercentage 5% of hoger. Als u deze drempels aanpassen wilt, opent u de opties.

![Fout pictogrammen](./media/app-insights-app-map/04.png)

Actieve waarschuwingen ook weergeven van: 

![actieve waarschuwingen](./media/app-insights-app-map/05.png)

Als u SQL Azure gebruikt, is er een pictogram dat ziet u wanneer er zijn aanbevelingen voor hoe u kunt de prestaties verbeteren. 

![Azure aanbeveling](./media/app-insights-app-map/06.png)

Klik op een pictogram voor meer informatie:

![Azure aanbeveling](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a>Diagnostische klik door
Elk van de knooppunten op de kaart biedt gerichte klik door voor diagnostische gegevens. De opties variëren afhankelijk van het type van het knooppunt.

![Serveropties](./media/app-insights-app-map/09.png)

De opties omvatten voor onderdelen die worden gehost in Azure, directe koppelingen naar deze.

## <a name="filters-and-time-range"></a>Filters en tijdsbereik
Standaard de kaart bevat een overzicht van alle gegevens die beschikbaar zijn voor het gekozen tijdsbereik. Maar u kunt deze zodanig dat alleen de namen van de specifieke bewerking of afhankelijkheden filteren.

* Naam van de bewerking: dit omvat zowel paginaweergaven en serverzijde aanvraagtypen. Met deze optie wordt de kaart de KPI op het knooppunt server-clientzijde voor de geselecteerde bewerkingen alleen. De afhankelijkheden aangeroepen in de context van deze specifieke bewerkingen worden weergegeven.
* Basisnaam voor afhankelijkheid: dit omvat de AJAX browser afhankelijkheden en afhankelijkheden van de serverzijde. Als u aangepaste afhankelijkheidstelemetrie met de API TrackDependency rapporteert, weergegeven ze ook hier. U kunt de afhankelijkheden om weer te geven op de kaart selecteren. Deze selectie filteren op dit moment niet de aanvragen van de server of de client-side paginaweergaven.

![Filters instellen](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a>Filters opslaan
Pincode voor het opslaan van de filters die u hebt toegepast, de gefilterde weergave naar een [dashboard](app-insights-dashboards.md).

![Vastmaken aan dashboard](./media/app-insights-app-map/12.png)

## <a name="error-pane"></a>Foutvenster
Wanneer u een knooppunt in het overzicht klikt, wordt een foutvenster weergegeven aan de rechterkant van fouten voor dat knooppunt samen te vatten. Fouten zijn gegroepeerd eerst op bewerkings-ID en vervolgens gegroepeerd op probleem-ID.

![Foutvenster](./media/app-insights-app-map/error-pane.png)

Op een fout te klikken, gaat u naar de meest recente exemplaar van deze fout.

## <a name="resource-health"></a>Status van resources
Voor sommige brontypen resourcestatus weergegeven aan de bovenkant van het deelvenster fout. Bijvoorbeeld, ziet te klikken op een SQL-knooppunt de status van de database en waarschuwingen die zijn gestart.

![Status van resources](./media/app-insights-app-map/resource-health.png)

U kunt klikken op de naam van de resource standaard overzicht metrische gegevens voor die bron weergeven.

## <a name="end-to-end-system-app-maps"></a>End-to-end-systeem app maps

*SDK-versie 2.3 of hoger vereist*

Als uw toepassing verschillende onderdelen - bijvoorbeeld een back-endservice verder naar de web-app heeft - moet u ze kunt weergeven op één geïntegreerde app-kaart.

![Filters instellen](./media/app-insights-app-map/multi-component-app-map.png)

De app-kaart wordt opgezocht serverknooppunten HTTP afhankelijkheid-aanroepen tussen servers met de Application Insights-SDK geïnstalleerd. Elke Application Insights-resource wordt uitgegaan van één server bevatten.

### <a name="multi-role-app-map-preview"></a>Met meerdere functie-app-kaart (preview)

De preview-functie voor het toewijzen van meerdere functie-app kunt u de app-kaart gebruiken met meerdere servers die gegevens te verzenden naar dezelfde Application Insights-resource / instrumentatiesleutel. Servers in de kaart worden gesegmenteerd op de eigenschap cloud_RoleName op telemetrie-items. Ingesteld *toepassing van meerdere rollenoverzicht* naar *op* op de blade Previews zodat deze configuratie.

Deze aanpak is het wellicht wenselijk in een toepassing micro-services of in andere scenario's waarvoor gebeurtenissen met elkaar correleren over meerdere servers in één Application Insights-resource.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Feedback
Geef feedback via de portal feedbackoptie.

![Afbeelding van MapLink-1](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a>Volgende stappen

* [Azure Portal](https://portal.azure.com)
