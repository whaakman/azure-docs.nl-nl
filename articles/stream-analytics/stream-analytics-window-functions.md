---
title: Inleiding tot Stream Analytics-vensterfuncties | Microsoft Docs
description: Meer informatie over de drie functies venster in Stream Analytics (tumbling, hopping, Verschuivend).
keywords: venster Verschuivend venster venster hopping tumbling
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 0d8d8717-5d23-43f0-b475-af078ab4627d
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 2a6559551f608cf435e89997392a6a0ba995c583
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

