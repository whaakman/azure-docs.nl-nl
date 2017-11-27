---
title: Azure Application Insights schoorstenen
description: Meer informatie over hoe u schoorstenen kunt gebruiken om te ontdekken hoe klanten communiceert met uw toepassing.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: mbullwin
ms.openlocfilehash: 0396c59d9d95ab71f0af04029d87afbb6e47dc35
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="discover-how-customers-are-using-your-application-with-the-application-insights-funnels"></a>Hoe klanten gebruikmaakt van uw toepassing met de Application Insights schoorstenen detecteren

Understanding klantervaring is van het grootste belang voor uw bedrijf. Als uw toepassing meerdere fasen omvat, moet u weten als u de voortgang van de meeste klanten door het hele proces, of als ze beëindigt het proces op een bepaald moment. De voortgang door een reeks stappen in een webtoepassing staat bekend als een 'trechter'. De Application Insights schoorstenen kunt u inzicht in uw gebruikers en de monitor stapsgewijze conversie tarieven. 

## <a name="create-your-funnel"></a>Maken van de trechter
Voordat u uw trechter maakt, moet u beslissen over de vraag die u wilt beantwoorden. Bijvoorbeeld, u mogelijk wilt weten hoeveel gebruikers bekijkt de introductiepagina van een klantprofiel bekijken en het maken van een ticket. In dit voorbeeld wilt de eigenaars van het bedrijf Fabrikam Fiber weet het percentage van de klanten die een klant-ticket maken.

Hier volgen de stappen waarmee ze hun trechter maken.

1. Klik op de knop Nieuw op het hulpprogramma schoorstenen.
1. Selecteer het tijdsbereik van 'Afgelopen 90 dagen' in de **tijdsbereik** vervolgkeuzelijst. Selecteer 'Mijn schoorstenen' of 'Schoorstenen wordt gedeeld'
1. Selecteer de **Index** gebeurtenis op basis van de **stap 1** vervolgkeuzelijst. 
1. Selecteer de **klant** gebeurtenis op basis van de **stap 2** vervolgkeuzelijst.
1. Selecteer de **maken** gebeurtenis op basis van de **stap 3** vervolgkeuzelijst.
1. Een naam in de trechter toevoegen en klik op **opslaan**.

De volgende afbeelding ziet u hoe dat de gegevens van het hulpprogramma schoorstenen genereert. Eigenaars kunnen hier de Fabrikam zien dat de 54.3% aan hun klanten die de startpagina bezocht tijdens de afgelopen 90 dagen een klant-ticket gemaakt. Ze kunnen ook zien die afkomstig zijn 2.7k van hun klanten naar de index van de startpagina, dit kan duiden op een probleem met vernieuwen.


![Schoorstenen hulpprogramma met gegevens](./media/app-insights-understand-usage-patterns/funnel1.png)

### <a name="funnel-features"></a>Functies van de trechter
1. Als uw app door actieve is, ziet u een banner steekproeven. Op de banner te klikken, wordt een context deelvenster tijdens het uitschakelen van steekproeven geopend. 
2. U kunt uw trechter om te exporteren [Power BI](app-insights-export-power-bi.md).
3. Klik op een stap voor dieper inzicht aan de rechterkant. 
4. Historische conversie wordt de conversie gedurende de afgelopen 90 dagen. 
5. Uw gebruikers betere door te gaan naar het hulpprogramma voor gebruikers van schoorstenen begrijpen. Elke stap, krijgt u samengestelde gebruikers filters. 

## <a name="next-steps"></a>Volgende stappen
  * [Overzicht gebruik](app-insights-usage-overview.md)
  * [Gebruikers, sessies en gebeurtenissen](app-insights-usage-segmentation.md)
  * [Retentie](app-insights-usage-retention.md)
  * [Werkmappen](app-insights-usage-workbooks.md)
  * [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)
  * [Exporteren naar Power BI](app-insights-export-power-bi.md)

