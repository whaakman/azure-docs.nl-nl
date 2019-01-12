---
title: Inleiding tot Azure Stream Analytics windowing functies
description: Dit artikel wordt beschreven vier windowing functies (tumbling, Hopping plaatsvindt, schuiven, sessie) die worden gebruikt in Azure Stream Analytics-taken.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: 04c19e7e51777db4c59bfab3d5a8a7598560556a
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54231645"
---
# <a name="introduction-to-stream-analytics-windowing-functions"></a>Inleiding tot Stream Analytics windowing functies
In scenario's voor het streamen van tijd is uitvoeren van bewerkingen op de gegevens die zijn opgenomen in windows temporal een algemeen patroon. Stream Analytics biedt ingebouwde ondersteuning voor windowing functies, zodat ontwikkelaars kunnen de auteur van complexe verwerking taken met een minimale inspanning.

Er zijn vier soorten tijdelijke windows kiezen uit: [**Tumbling**](https://msdn.microsoft.com/azure/stream-analytics/reference/tumbling-window-azure-stream-analytics), [ **Hopping**](https://msdn.microsoft.com/azure/stream-analytics/reference/hopping-window-azure-stream-analytics), [ **Sliding**](https://msdn.microsoft.com/azure/stream-analytics/reference/sliding-window-azure-stream-analytics), en [ **sessie**  ](https://msdn.microsoft.com/azure/stream-analytics/reference/session-window-azure-stream-analytics) windows.  Gebruik van de vensterfuncties in de [ **GROUP BY** ](https://msdn.microsoft.com/azure/stream-analytics/reference/group-by-azure-stream-analytics) -component van de query-syntaxis in de Stream Analytics-taken.

Alle de [windowing](https://msdn.microsoft.com/azure/stream-analytics/reference/windowing-azure-stream-analytics) uitvoerresultaten van bewerkingen op de **end** van het venster. De uitvoer van het venster is één gebeurtenis op basis van de statistische functie gebruikt. De uitvoergebeurtenis heeft de tijdstempel van het einde van het venster en alle vensterfuncties zijn gedefinieerd met een vaste lengte. 

![Concepten van Stream Analytics venster-functies](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Tumblingvenster
Tumbling venster functies worden gebruikt voor het segmenteren van een gegevensstroom in segmenten van verschillende tijd en uitvoeren van een functie met betrekking tot deze, zoals in het onderstaande voorbeeld. De belangrijkste verschillen van een tumblingvenster zijn dat ze herhalen, niet overlappen, en een gebeurtenis kan niet behoren tot meer dan één tumblingvenster.

![Stream Analytics-tumblingvenster](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Hoppingvenster
Verspringen venster functies hop doorsturen door een bepaalde periode. Kan het zijn gemakkelijk te ze beschouwen als tumblingvensters die overlappen kunnen, zodat gebeurtenissen deel van meer dan één Hopping venster-resultatenset uitmaken kunnen. Als u een venster Hopping geeft gelijk zijn aan een tumblingvenster de grootte van de vensterverplaatsing moet gelijk zijn aan de venstergrootte van het. 

![Stream Analytics-hoppingvenster](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Sliding window van
Sliding vensterfuncties, in tegenstelling tot Tumbling of windows, Hopping er uitvoer gegenereerd **alleen** wanneer een gebeurtenis plaatsvindt. Elk venster moet ten minste één gebeurtenis en het venster continu wordt verplaatst naar voren door een € (epsilon). Net als windows hopping, gebeurtenissen kunnen deel uitmaken van sliding window van meer dan één.

![Sliding window van Stream Analytics](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="session-window"></a>Sessievenster
Sessie-vensterfuncties groep gebeurtenissen die op hetzelfde tijdstip binnenkomen, gefilterd op perioden wanneer er geen gegevens zijn. Er drie belangrijke parameters: time-out voor de maximale duur en (optioneel) te nemen partitionerende sleutel.

![Stream Analytics-sessievenster](media/stream-analytics-window-functions/stream-analytics-window-functions-session-intro.png)

Het sessievenster van een begint wanneer de eerste gebeurtenis plaatsvindt. Als een andere gebeurtenis binnen de opgegeven time-out van de laatste gebeurtenis van opgenomen plaatsvindt, klikt u vervolgens het venster uitgebreid om op te nemen van de nieuwe gebeurtenis. Anders als er geen binnen de time-out gebeurtenissen, klikt u vervolgens het venster wordt gesloten op de time-out.

Als gebeurtenissen binnen de opgegeven time-out plaatsvinden blijven, wordt het sessievenster bewaren uitbreiden tot maximale duur is bereikt. De maximale duur voor controle-intervallen worden ingesteld op even groot is als de opgegeven maximale duur. Bijvoorbeeld, als de maximale duur 10, wordt de controles op als het venster overschrijdt maximale duur zal gebeuren op t = 0, 10, 20, 30, enzovoort.

Wanneer een partitiesleutel is opgegeven, wordt de gebeurtenissen zijn gegroepeerd door de sleutel en sessievenster wordt onafhankelijk toegepast op elke groep. Deze partitioneren is handig in situaties waar u andere sessie windows nodig hebt voor verschillende gebruikers of apparaten.


## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

