---
title: Om weer te geven van het gebruik van Azure kaarten-API | Microsoft Docs
description: Leer hoe u de metrische gegevens voor uw Azure kaarten-API-aanroepen weergeven in de portal.
author: dsk-2015
ms.author: dkshir
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: e62d2ff1fdd6bc94244511a2de95c4268a58d6f9
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "42056831"
---
# <a name="how-to-view-the-azure-maps-api-usage"></a>Het gebruik van Azure kaarten-API weergeven
Dit artikel leest u hoe u om weer te geven de metrische gegevens over gebruik API voor uw Azure-kaarten-account in de [portal](https://portal.azure.com). De metrische gegevens worden weergegeven in een handige grafiek langs een aanpasbare tijdsduur. 

## <a name="view-metric-snapshot"></a>Momentopname van metrische gegevens weergeven 

Ziet u enkele algemene metrische gegevens van de **overzicht** pagina van uw kaarten-account. Toont momenteel *totaal aantal aanvragen*, *totaal aantal fouten*, en *beschikbaarheid* via een tijdsduur worden geselecteerd. 

![Overzicht van metrische gegevens van Azure Maps](media/how-to-view-api-usage/portal-overview.png)

Doorgaan naar de volgende sectie als u nodig hebt om aan te passen van de grafieken voor uw specifieke analyse.


## <a name="view-detailed-metrics"></a>Gedetailleerde metrische gegevens weergeven

1. Aanmelden bij uw Azure-abonnement in de [portal](https://portal.azure.com). 

2. Klik op de **alle resources** menu-item aan de linkerkant en navigeer naar uw *Azure kaarten-Account*.

3. Als uw kaarten-account geopend is, klikt u op de **metrische gegevens** menu aan de linkerkant.

4. Op de **metrische gegevens** deelvenster kiezen tussen een van de volgende:

    1. **Beschikbaarheid**, deze toont de *gemiddelde* API beschikbaar gedurende een bepaalde periode, of 
    2. **Gebruik**, deze toont hoe het gebruik *aantal* voor uw account. 

    ![Deelvenster in Azure Maps metrische gegevens](media/how-to-view-api-usage/portal-metrics.png)

5. Nadat u de metrische gegevens hebt geselecteerd, mag u de *tijdsbereik* door te selecteren op de **afgelopen 12 uur (automatisch)** dit is de standaardwaarde. U kunt ook kiezen de *tijdgranulatie*, evenals wilt weergeven, de tijd als *lokale* of *GMT* in de dezelfde vervolgkeuzelijst. Klik op **Toepassen**.

    ![Tijdsbereik van Azure Maps metrische gegevens](media/how-to-view-api-usage/time-range.png)
 
6. Als u uw metrische gegevens toevoegt, kunt u vervolgens **filter toevoegen** in met de eigenschappen die relevant zijn voor die metrische gegevens en selecteer vervolgens de waarde van de eigenschap die u wilt zien de grafiek voor. 

    ![Azure Maps metrische gegevens filteren](media/how-to-view-api-usage/filter.png)

7. U kunt ook **toepassen splitsen** voor uw metrische gegevens op basis van de eigenschap van de geselecteerde metrische gegevens. Hiermee wordt de grafiek moet worden gesplitst in meerdere diagrammen, één voor elke waarde van deze eigenschap. Bijvoorbeeld, in de volgende afbeelding, de kleur van elke grafiek komt overeen met de waarde van de eigenschap weergegeven aan de onderkant van de grafiek.

    ![Azure kaarten-metrische gegevens splitsen](media/how-to-view-api-usage/splitting.png)
 
8. U kunt ook meerdere metrische gegevens over dezelfde grafiek, leren door te klikken op de **metrische waarde toevoegen** bovenaan op de knop.


## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u uw gebruik van Azure Maps bijhouden, kunt u doorgaan met meer informatie over de API's die u met de:

* [Azure kaarten REST-API-documentatie](https://docs.microsoft.com/rest/api/maps)
