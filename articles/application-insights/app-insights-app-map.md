---
title: Kaart van de toepassing in Azure Application Insights | Microsoft Docs
description: "Bewaken van complexe toepassingen topologieën met de toepassing-kaart"
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
ms.openlocfilehash: 3bbed59bf93eab5e729fbdd3ccae04599ac47081
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/03/2018
---
# <a name="application-map-triage-distributed-applications"></a>De toepassingstoewijzing: Sorteren gedistribueerde toepassingen
De toepassingstoewijzing helpt u ter plaatse prestatieknelpunten of fout hotspots voor alle onderdelen van de gedistribueerde toepassing. Elk knooppunt op de kaart vertegenwoordigt een toepassingsonderdeel van de of de afhankelijkheden ervan; health KPI en heeft de status van waarschuwingen. U kunt via van elk onderdeel klikken om meer gedetailleerde diagnostische gegevens, zoals Application Insights-gebeurtenissen te. Als uw app gebruikmaakt van Azure-services, kunt u ook doorklikken naar Azure diagnostics, zoals SQL Database Advisor aanbevelingen.

## <a name="what-is-a-component"></a>Wat is een onderdeel?

Onderdelen zijn onafhankelijk implementeerbare onderdelen van uw toepassing gedistribueerd/microservices. Ontwikkelaars en operations teams hebben zichtbaarheid van de code-niveau of toegang tot de telemetrie die is gegenereerd door de onderdelen van deze toepassing. 

* Onderdelen zijn anders dan 'waargenomen' externe afhankelijkheden, zoals SQL, enz. EventHub die uw team/organisatie mogelijk geen toegang tot (code of telemetrie).
* Onderdelen worden uitgevoerd op een willekeurig aantal exemplaren van de rol-server-container.
* Onderdelen kunnen worden afzonderlijke Application Insights-instrumentatiesleutels (zelfs als abonnementen verschillend zijn) of verschillende rollen die rapporteren aan een enkele Application Insights-instrumentatiesleutel. De ervaring voor de preview-kaart bevat de onderdelen ongeacht hoe ze zijn ingesteld.

## <a name="composite-application-map-preview"></a>Samengestelde toepassingstoewijzing (Preview)
*Dit is een vroege voorbeeld en er meer functies toevoegen aan deze kaart. We graag uw feedback over de nieuwe ervaring ophalen. U kunt eenvoudig schakelen tussen de preview en klassieke ervaringen.*

"De samengestelde toepassingstoewijzing" inschakelen via de [previews lijst](app-insights-previews.md), of klik op 'Preview kaart' in de wisselknop in de rechterbovenhoek. Overschakelen naar de klassieke ervaring kunt u deze in-of uitschakelen.
![Inschakelen van de preview-kaart](media/app-insights-app-map/preview-from-classic.png)

>[!Note]
Deze preview vervangt de voorafgaande "Mult-toepassing rollenoverzicht" preview. Op dit moment gebruiken deze om de volledige topologie op meerdere niveaus van de toepassing onderdeel afhankelijkheden weer te geven. Geef ons uw feedback, we meer mogelijkheden vergelijkbaar met wat de klassieke kaart ondersteunt gaat toevoegen.

U ziet de topologie van de volledige toepassing op meerdere niveaus van gerelateerde toepassingsonderdelen. Onderdelen worden verschillende Application Insights-bronnen of verschillende rollen in één resource. De kaart app zoekt onderdelen met de volgende HTTP-afhankelijkheidsaanroepen tussen servers met de Application Insights-SDK geïnstalleerd. 

Deze ervaring begint met het progressief detectie van de onderdelen. Wanneer u het voorbeeld voor het eerst laadt, wordt een set van query's worden geactiveerd voor het detecteren van de onderdelen die betrekking hebben op dit onderdeel. Een knop op de linkerbovenhoek wordt bijgewerkt met het aantal onderdelen in uw toepassing zodra ze worden gedetecteerd. 
![Preview-kaart](media/app-insights-app-map/preview.png)

Op "Update kaart-onderdelen" klikt, wordt de kaart met alle onderdelen die zijn gedetecteerd tot vernieuwd.
![Preview geladen-kaart](media/app-insights-app-map/components-loaded-hierarchical.png)

Deze stap van de detectie is niet vereist als alle onderdelen rollen binnen één Application Insights-resource. De eerste belasting voor een dergelijke toepassing hebben alle onderdelen daarvan.

Een van de belangrijkste doelstellingen met de nieuwe ervaring is mogelijk te visualiseren complexe topologieën met honderden of onderdelen. De nieuwe ervaring ondersteunt zoomen en details toegevoegd, zoals u inzoomen. U kunt uitzoomen om meer onderdelen in één oogopslag en nog steeds ter plaatse onderdelen met hogere snelheid van de fout te bekijken. 

![Zoomen niveaus](media/app-insights-app-map/zoom-levels.png)

Klik op een onderdeel om te zien van gerelateerde insights en gaat u naar de prestaties en de fout selectie ervaring voor dat onderdeel.

![Doel](media/app-insights-app-map/preview-flyout.png)


## <a name="classic-application-map"></a>Klassieke toepassingstoewijzing

De kaart wordt weergegeven:

* Beschikbaarheidstests
* Client-side-onderdeel (bewaakt met de JavaScript SDK)
* Server-side-onderdeel
* Afhankelijkheden van de client en server-onderdelen

![App-kaart](./media/app-insights-app-map/02.png)

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

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Feedback
Geef feedback via de portal feedbackoptie.

![Afbeelding van MapLink-1](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a>Volgende stappen

* [Azure Portal](https://portal.azure.com)
