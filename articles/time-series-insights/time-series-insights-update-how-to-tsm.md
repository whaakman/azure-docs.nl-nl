---
title: Azure Time Series Insights-gegevensmodellering - gegevensmodellen in Azure Time Series Insights Preview | Microsoft Docs
description: Krijg inzicht in gegevens modelleren in Azure Time Series Insights Preview.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: e68bc01d2c0781333454fa753992d0136fac0c06
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269087"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Gegevens modelleren in Azure Time Series Insights-Preview

Dit document wordt beschreven hoe u werkt met Time Series modellen Azure Time Series Insights Preview te volgen. Deze details over verschillende algemene scenario's met gegevens.

Lees voor meer informatie over het gebruik van de update [Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md).

## <a name="types"></a>Typen

### <a name="create-a-single-type"></a>Maken van één type

1. Ga naar het deelvenster van de selectie van Time Series-modellen en selecteer **typen** in het menu. Het deelvenster zich kunt richten op de typen Time Series modellen samenvouwen.

    ![Portal_one][1]

1. Selecteer **Toevoegen**.
1. Alle gegevens die betrekking hebben op de typen en selecteer de invoer **maken**. Deze actie wordt gemaakt van typen in de omgeving.

    ![Portal_two][2]

### <a name="bulk-upload-one-or-more-types"></a>Bulksgewijs uploaden van een of meer typen

1. Selecteer **uploaden JSON**.
1. Selecteer het bestand dat de nettolading van het type bevat.
1. Selecteer **Uploaden**.

    ![Portal_three][3]

### <a name="edit-a-single-type"></a>Één type bewerken

Selecteer het type en selecteer **bewerken**. De benodigde wijzigingen aanbrengen en selecteer **opslaan**.

![Portal_four][4]

### <a name="delete-a-type"></a>Een type verwijderen

Selecteer het type en selecteer **verwijderen**. Als er geen exemplaren gekoppeld aan de typen zijn, wordt deze verwijderd.

![Portal_five][5]

## <a name="hierarchies"></a>Hiërarchieën

### <a name="create-a-single-hierarchy"></a>Maken van een enkele hiërarchie

1. Ga naar het deelvenster van de selectie van Time Series-modellen en selecteer **hiërarchieën** in het menu. Het deelvenster zich kunt richten op de modellen van Time Series-hiërarchieën samenvouwen.

    ![Portal_six][6]

1. Selecteer **Toevoegen**.

    ![Portal_seven][7]

1. Selecteer **niveau toevoegen** in het rechter deelvenster.

    ![Portal_eight][8]

1. Voer de Hiërarchiedetails van de en selecteer **maken**.

    ![Portal_nine][9]

### <a name="bulk-upload-one-or-more-hierarchies"></a>Bulksgewijs uploaden van een of meer hiërarchieën

1. Selecteer **uploaden JSON**.
1. Selecteer het bestand met de nettolading van de hiërarchie.
1. Selecteer **Uploaden**.

    ![Portal_ten][10]

### <a name="edit-a-single-hierarchy"></a>Een enkele hiërarchie bewerken

Selecteer de hiërarchie en **bewerken**. De benodigde wijzigingen aanbrengen en selecteer **opslaan**.

![Portal_eleven][11]

### <a name="delete-a-hierarchy"></a>Een hiërarchie verwijderen

Selecteer de hiërarchie en **verwijderen**. Als er geen exemplaren gekoppeld aan de hiërarchie zijn, wordt deze verwijderd.

![Portal_twelve][12]

## <a name="instances"></a>Exemplaren

### <a name="create-a-single-instance"></a>Een enkele instantie maken

1. Ga naar het deelvenster van de selectie van Time Series-modellen en selecteer **exemplaren** in het menu. Het deelvenster om zich te richten op de modellen van Time Series-instanties samenvouwen.

    ![Portal_thirteen][13]

1. Selecteer **Toevoegen**.

    ![Portal_fourteen][14]

1. Geef de exemplaardetails, selecteer het type en hiërarchie-koppeling en selecteer **maken**.

### <a name="bulk-upload-one-or-more-instances"></a>Bulksgewijs uploaden van een of meer exemplaren

1. Selecteer **uploaden JSON**.
1. Selecteer het bestand met de nettolading van de exemplaren.

    ![Portal_fifteen][15]

1. Selecteer **Uploaden**.

### <a name="edit-a-single-instance"></a>Een enkele instantie bewerken

Selecteer het exemplaar en selecteer **bewerken**. De benodigde wijzigingen aanbrengen en selecteer **opslaan**.

![Portal_sixteen][16]

### <a name="delete-an-instance"></a>Een exemplaar verwijderen

Selecteer het exemplaar en selecteer **verwijderen**. Als er geen gebeurtenissen gekoppeld aan de exemplaren zijn, wordt deze verwijderd.

## <a name="next-steps"></a>Volgende stappen

- Lees voor meer informatie over Time Series modellen [gegevensmodellering](./time-series-insights-update-tsm.md).
- Lees voor meer informatie over de Preview-versie [visualiseren van gegevens in de Verkenner van Azure Time Series Insights Preview](./time-series-insights-update-explorer.md).
- Lees voor meer informatie over ondersteunde JSON-vormen, [ondersteund JSON-vormen](./time-series-insights-send-events.md#json).

<!-- Images -->
[1]: media/v2-update-how-to-tsm/portal_one.png
[2]: media/v2-update-how-to-tsm/portal_two.png
[3]: media/v2-update-how-to-tsm/portal_three.png
[4]: media/v2-update-how-to-tsm/portal_four.png
[5]: media/v2-update-how-to-tsm/portal_five.png
[6]: media/v2-update-how-to-tsm/portal_six.png
[7]: media/v2-update-how-to-tsm/portal_seven.png
[8]: media/v2-update-how-to-tsm/portal_eight.png
[9]: media/v2-update-how-to-tsm/portal_nine.png
[10]: media/v2-update-how-to-tsm/portal_ten.png
[11]: media/v2-update-how-to-tsm/portal_eleven.png
[12]: media/v2-update-how-to-tsm/portal_twelve.png
[13]: media/v2-update-how-to-tsm/portal_thirteen.png
[14]: media/v2-update-how-to-tsm/portal_fourteen.png
[15]: media/v2-update-how-to-tsm/portal_fifteen.png
[16]: media/v2-update-how-to-tsm/portal_sixteen.png