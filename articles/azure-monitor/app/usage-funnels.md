---
title: Azure Application Insights Trechters
description: Lees hoe u Trechters kunt gebruiken om te ontdekken hoe klanten zijn interactie met uw toepassing.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/17/2017
ms.pm_owner: daviste;NumberByColors
ms.reviewer: mbullwin
ms.author: daviste
ms.openlocfilehash: 2cb7e15b701b53e74618c21bf219a355d495f985
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54076277"
---
# <a name="discover-how-customers-are-using-your-application-with-application-insights-funnels"></a>Ontdek hoe klanten uw toepassing gebruiken met Application Insights Trechters

Inzicht in de ervaring van de klant is van het grootste belang voor uw bedrijf. Als uw toepassing bestaat uit meerdere fasen, moet u weten als u de voortgang van de meeste klanten door het hele proces, of als ze zijn het proces op een bepaald moment beëindigen. De voortgang in een reeks stappen in een web-App wordt ook wel een *trechter*. U kunt het gebruiken van Azure Application Insights Trechters voor inzicht in uw gebruikers, en stapsgewijze conversieverhoudingen controleren. 

## <a name="create-your-funnel"></a>Maken van uw trechter
Voordat u uw trechter maakt, kunt u beslissen over de vraag die u wilt beantwoorden. Bijvoorbeeld, kunt u weten hoeveel gebruikers bekijkt de startpagina van een klantprofiel bekijken en het maken van een ticket. In dit voorbeeld wordt de eigenaren van het bedrijf Fabrikam-Fiber horen graag het percentage van de klanten die een klant-ticket maken.

Hier volgen de stappen waarmee ze hun trechter maken.

1. Selecteer in het hulpprogramma Application Insights Trechters **nieuw**.
1. Uit de **tijdsbereik** vervolgkeuzelijst in het menu **afgelopen 90 dagen**. Selecteer een **mijn Trechters** of **gedeelde Trechters**.
1. Uit de **stap 1** vervolgkeuzelijst, selecteer **Index**. 
1. Uit de **stap 2** in de lijst met **klant**.
1. Uit de **stap 3** in de lijst met **maken**.
1. Een naam toevoegen aan de trechter en selecteer **opslaan**.

De volgende schermafbeelding ziet u dat een voorbeeld van het type gegevens het programma Trechters wordt gegenereerd. De eigenaren van Fabrikam kunnen zien dat tijdens de afgelopen 90 dagen, 54.3 procent van hun klanten, die de startpagina van een klant-ticket gemaakt bezocht. Ze kunnen ook zien dat 2700 van hun klanten vanaf de startpagina ging bij de index werken. Dit kan duiden op een probleem met vernieuwen.


![Schermafbeelding van Trechters met gegevens](media/usage-funnels/funnel1.png)

### <a name="funnels-features"></a>Trechters functies
De vorige schermafbeelding omvat vijf gemarkeerde gebieden. Dit zijn functies van Trechters. De volgende lijst over elk overeenkomend gebied in de schermafbeelding wordt toegelicht:
1. Als uw app verzameld, ziet u een banner steekproeven. De banner te selecteren, wordt een deelvenster context, waarin wordt uitgelegd hoe u kunt uitschakelen van steekproeven geopend. 
2. U kunt uw trechter om te exporteren [Power BI](../../azure-monitor/app/export-power-bi.md ).
3. Selecteer een stap voor meer informatie aan de rechterkant. 
4. De historische conversie-grafiek toont de conversieverhoudingen gedurende de afgelopen 90 dagen. 
5. Uw gebruikers beter begrijpen door het openen van het hulpprogramma gebruikers. U kunt filters gebruiken in elke stap. 

## <a name="next-steps"></a>Volgende stappen
  * [Gebruiksoverzicht](usage-overview.md)
  * [Gebruikers, sessies en gebeurtenissen](usage-segmentation.md)
  * [Retentie](usage-retention.md)
  * [Werkmappen](../../azure-monitor/app/usage-workbooks.md)
  * [Gebruikerscontext toevoegen](usage-send-user-context.md)
  * [Exporteren naar Power BI](../../azure-monitor/app/export-power-bi.md )

