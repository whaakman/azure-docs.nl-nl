---
title: Inleiding tot Azure Stream Analytics windowing functies
description: Dit artikel worden de drie windowing functies (tumbling, hopping, Verschuivend) die worden gebruikt in Azure Stream Analytics-taken.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: c6f5dbe49cb60e3c7b2bc6562acf2d7fd79096ec
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-stream-analytics-window-functions"></a>Inleiding tot Stream Analytics-vensterfuncties
In veel realtime streaming-scenario's, is het nodig zijn voor het uitvoeren van bewerkingen alleen op de gegevens in de tijdelijke windows. Systeemeigen ondersteuning voor windowing functies is een belangrijke functie van Azure Stream Analytics die de wijzer op de productiviteit van ontwikkelaars verplaatst in taken voor de verwerking van complexe stroom ontwerpen. Stream Analytics biedt ontwikkelaars kunnen gebruiken [ **Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [ **Hopping** ](https://msdn.microsoft.com/library/dn835041.aspx) en [ **schuifregelaar** ](https://msdn.microsoft.com/library/dn835051.aspx) windows tijdelijke bewerkingen op het streamen van gegevens uit te voeren. Hierbij moet worden opgemerkt dat alle [venster](https://msdn.microsoft.com/library/dn835019.aspx) uitvoerresultaten voor bewerkingen op de **end** van het venster. De uitvoer van het venster worden één keer de gebeurtenis op basis van de statistische functie gebruikt. De gebeurtenis heeft de tijdstempel van het einde van het venster en alle functies van het venster met een vaste lengte zijn gedefinieerd. Ten slotte is het belangrijk te weten dat alle functies van het venster moeten worden gebruikt een [ **GROUP BY** ](https://msdn.microsoft.com/library/dn835023.aspx) component.

![Stream Analytics venster werkt concepten](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Tumblingvenster
Tumbling venster functies worden gebruikt voor een gegevensstroom segmenteren in segmenten van verschillende tijd en het uitvoeren van een functie hiertegen, zoals het onderstaande voorbeeld. De belangrijkste differentiators van een tumblingvenster zijn dat ze herhalen, elkaar niet overlappen en een gebeurtenis kan niet tot meer dan één tumblingvenster behoren.

![Stream Analytics-vensterfuncties tumbling intro](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Hoppingvenster
Hopping venster functies hop doorsturen door een vaste periode. Kan het zijn gemakkelijk te zien ze als daling windows die elkaar overlappen kunnen, zodat gebeurtenissen kunnen tot meer dan één Hopping venster-resultatenset behoren. Als u wilt maken van een venster Hopping hetzelfde zijn als een daling geeft venster één gewoon de hopgrootte om dezelfde zijn als de venstergrootte. 

![Stream Analytics-vensterfuncties hopping intro](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Sliding Window
Verschuivende vensterfuncties, in tegenstelling tot Tumbling of windows, Hopping produceren uitvoer **alleen** wanneer een gebeurtenis zich voordoet. Elk venster ten minste één gebeurtenis zijn en het venster continu verplaatst doorsturen door een € (epsilon). Gebeurtenissen kunnen zoals Windows Hopping behoren tot meer dan één Verschuivend venster.

![Stream Analytics-vensterfuncties Verschuivend intro](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Help opvragen bij vensterfuncties
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

