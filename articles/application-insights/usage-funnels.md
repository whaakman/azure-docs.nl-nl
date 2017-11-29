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
ms.openlocfilehash: bbb25af888f34737f6a61cf43890ff248c4cc4de
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="discover-how-customers-are-using-your-application-with-application-insights-funnels"></a>Hoe klanten uw toepassing gebruiken met Application Insights schoorstenen detecteren

Inzicht in de klantervaring is van het grootste belang voor uw bedrijf. Als uw toepassing meerdere fasen omvat, moet u weten als u de voortgang van de meeste klanten door het hele proces, of als ze beëindigt het proces op een bepaald moment. De voortgang door een reeks stappen in een webtoepassing wordt ook wel een *trechter*. U kunt Azure Application Insights schoorstenen gebruiken om inzicht in uw gebruikers te krijgen, en stapsgewijze conversie tarieven controleren. 

## <a name="create-your-funnel"></a>Maken van de trechter
Voordat u uw trechter maakt, kunt u beslissen over de vraag die u wilt beantwoorden. Bijvoorbeeld, u mogelijk wilt weten hoeveel gebruikers bekijkt de introductiepagina van een klantprofiel bekijken en het maken van een ticket. In dit voorbeeld wilt de eigenaars van het bedrijf Fabrikam Fiber weet het percentage van de klanten die een klant-ticket maken.

Hier volgen de stappen waarmee ze hun trechter maken.

1. Selecteer in het hulpprogramma Application Insights schoorstenen **nieuw**.
1. Van de **tijdsbereik** vervolgkeuzelijst, selecteer **laatste 90 dagen**. Selecteer een **mijn schoorstenen** of **schoorstenen gedeeld**.
1. Van de **stap 1** vervolgkeuzelijst, selecteer **Index**. 
1. Van de **stap 2** selecteert **klant**.
1. Van de **stap 3** selecteert **maken**.
1. Voeg een naam toe aan de trechter en selecteer **opslaan**.

De volgende Schermafbeelding toont dat een voorbeeld van het type gegevens het hulpprogramma schoorstenen genereert. De eigenaren van Fabrikam kunnen zien dat tijdens de afgelopen 90 dagen 54.3 procent van hun klanten die de startpagina van een klant-ticket gemaakt bezocht. Ze kunnen ook zien dat 2700 van hun klanten naar de index afkomstig zijn van de startpagina. Dit kan duiden op een probleem met vernieuwen.


![Schermopname van schoorstenen hulpprogramma met gegevens](./media/app-insights-understand-usage-patterns/funnel1.png)

### <a name="funnels-features"></a>Schoorstenen functies
De vorige schermafbeelding omvat vijf gemarkeerde gebieden. Dit zijn functies van schoorstenen. De volgende lijst worden meer over elk overeenkomend gebied in de schermafbeelding:
1. Als uw app door actieve is, ziet u een banner steekproeven. De banner selecteren Hiermee opent u een deelvenster context waarin wordt uitgelegd hoe steekproeven om uit te schakelen. 
2. U kunt uw trechter om te exporteren [Power BI](app-insights-export-power-bi.md).
3. Selecteer een stap voor meer informatie over het recht. 
4. De conversie van historische grafiek toont de tarieven voor conversie gedurende de afgelopen 90 dagen. 
5. Uw gebruikers betere door het openen van het hulpprogramma gebruikers begrijpen. U kunt filters gebruiken in elke stap. 

## <a name="next-steps"></a>Volgende stappen
  * [Overzicht gebruik](app-insights-usage-overview.md)
  * [Gebruikers, sessies en gebeurtenissen](app-insights-usage-segmentation.md)
  * [Retentie](app-insights-usage-retention.md)
  * [Werkmappen](app-insights-usage-workbooks.md)
  * [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)
  * [Exporteren naar Power BI](app-insights-export-power-bi.md)

