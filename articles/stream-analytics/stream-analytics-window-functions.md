---
title: Inleiding tot Azure Stream Analytics windowing functies
description: Dit artikel worden vier windowing functies (tumbling, hopping, Verschuivend, sessie) die worden gebruikt in Azure Stream Analytics-taken.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: 2650058e277bc0338c779655ce381be046fb120a
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="introduction-to-stream-analytics-windowing-functions"></a>Inleiding tot Stream Analytics windowing functies
In scenario's tijd streaming is bewerkingen uitvoert op de gegevens in de tijdelijke windows een algemene patroon. Stream Analytics biedt systeemeigen ondersteuning voor windowing functies, waarmee ontwikkelaars op taken voor de verwerking van de auteur van complexe stream met een minimale inspanning.

Er zijn vier typen van de tijdelijke windows om te kiezen uit: [ **Tumbling**](https://msdn.microsoft.com/azure/stream-analytics/reference/tumbling-window-azure-stream-analytics), [ **Hopping**](https://msdn.microsoft.com/azure/stream-analytics/reference/hopping-window-azure-stream-analytics), [  **Bij Verschuivend**](https://msdn.microsoft.com/azure/stream-analytics/reference/sliding-window-azure-stream-analytics), en [ **sessie** ](https://msdn.microsoft.com/azure/stream-analytics/reference/session-window-azure-stream-analytics) windows.  U het vensterfuncties gebruiken in de [ **GROUP BY** ](https://msdn.microsoft.com/azure/stream-analytics/reference/group-by-azure-stream-analytics) -component van de syntaxis van de query in uw Stream Analytics-taken.

Alle de [windowing](https://msdn.microsoft.com/azure/stream-analytics/reference/windowing-azure-stream-analytics) uitvoerresultaten voor bewerkingen op de **end** van het venster. De uitvoer van het venster worden één keer de gebeurtenis op basis van de statistische functie gebruikt. De uitvoergebeurtenis heeft de tijdstempel van het einde van het venster en alle vensterfuncties met een vaste lengte zijn gedefinieerd. 

![Stream Analytics venster werkt concepten](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Tumblingvenster
Tumbling venster functies worden gebruikt voor een gegevensstroom segmenteren in segmenten van verschillende tijd en het uitvoeren van een functie hiertegen, zoals het onderstaande voorbeeld. De belangrijkste differentiators van een tumblingvenster zijn dat ze herhalen, niet overlappen, en een gebeurtenis kan niet behoren tot meer dan één tumblingvenster.

![Stream Analytics-tumblingvenster](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Hoppingvenster
Hopping venster functies hop doorsturen door een vaste periode. Kan het zijn gemakkelijk te zien ze als daling windows die elkaar overlappen kunnen, zodat gebeurtenissen kunnen tot meer dan één Hopping venster-resultatenset behoren. Wilt maken van een venster Hopping geeft hetzelfde zijn als een tumblingvenster de hopgrootte moet hetzelfde zijn als de venstergrootte. 

![Stream Analytics-hoppingvenster](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Venster Verschuivend
Verschuivende vensterfuncties, in tegenstelling tot Tumbling of windows, Hopping produceren uitvoer **alleen** wanneer een gebeurtenis zich voordoet. Elk venster ten minste één gebeurtenis zijn en het venster continu verplaatst doorsturen door een € (epsilon). Gebeurtenissen kunnen zoals hopping windows, behoren tot meer dan één verschuivende venster.

![Stream Analytics verschuivende venster](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="session-window-preview"></a>Sessievenster (Preview)
Sessie-vensterfuncties groep gebeurtenissen die op hetzelfde tijdstip binnenkomen uitgefilterd perioden wanneer er geen gegevens zijn. Deze heeft drie belangrijkste parameters: een time-out opgetreden, maximale duur en nemen partitionerende sleutel (optioneel).

![Stream Analytics sessievenster](media/stream-analytics-window-functions/stream-analytics-window-functions-session-intro.png)

Het sessievenster van een begint wanneer de eerste gebeurtenis plaatsvindt. Als een andere gebeurtenis binnen de opgegeven time-out van de laatste gebeurtenis van de opgenomen plaatsvindt, vervolgens uitbreidt het venster zodanig dat de nieuwe gebeurtenis. Anders als er geen gebeurtenissen binnen de time-out plaatsvinden, klikt u vervolgens het venster wordt gesloten op de time-out.

Als de gebeurtenissen behouden binnen de opgegeven time-out optreedt, wordt het sessievenster houden uitbreiden tot maximale duur is bereikt. De maximale duur controleren intervallen zijn ingesteld op dezelfde grootte hebben als de opgegeven maximale duur. Bijvoorbeeld, als de maximale duur is 10, en vervolgens de controle op als het venster overschrijdt maximale duur wordt geval op t = 0, 10, 20, 30, enzovoort.

Wanneer een partitiesleutel is opgegeven, wordt de gebeurtenissen zijn gegroepeerd door de sleutel en sessievenster wordt onafhankelijk toegepast op elke groep. Deze partitioneren is handig in situaties waarin u een andere sessie windows voor verschillende gebruikers of apparaten moet.


## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

