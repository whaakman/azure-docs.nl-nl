---
title: Gegevens modelleren in Azure Time Series Insights-Preview | Microsoft Docs
description: Krijg inzicht in gegevens modelleren in Azure Time Series Insights Preview.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: 73384868deb8f0e33b233e363c42a12adbcbe402
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237569"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Gegevens modelleren in Azure Time Series Insights-Preview

Dit document wordt beschreven hoe u werkt met Time Series modellen Azure Time Series Insights Preview te volgen. Deze details over verschillende algemene scenario's met gegevens.

Lees voor meer informatie over het gebruik van de update [Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md).

## <a name="types"></a>Types

### <a name="create-a-single-type"></a>Maken van één type

1. Ga naar het deelvenster van de selectie van Time Series-modellen en selecteer **typen** in het menu. Het deelvenster zich kunt richten op de typen Time Series modellen samenvouwen.

    [![Maken van één type](media/v2-update-how-to-tsm/portal_one.png)](media/v2-update-how-to-tsm/portal_one.png#lightbox)

1. Selecteer **Toevoegen**.
1. Alle gegevens die betrekking hebben op de typen en selecteer de invoer **maken**. Deze actie wordt gemaakt van typen in de omgeving.

    [![Een type toevoegen](media/v2-update-how-to-tsm/portal_two.png)](media/v2-update-how-to-tsm/portal_two.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Bulksgewijs uploaden van een of meer typen

1. Selecteer **uploaden JSON**.
1. Selecteer het bestand dat de nettolading van het type bevat.
1. Selecteer **Uploaden**.

    [![JSON uploaden](media/v2-update-how-to-tsm/portal_three.png)](media/v2-update-how-to-tsm/portal_three.png#lightbox)

### <a name="edit-a-single-type"></a>Één type bewerken

1. Selecteer het type en selecteer **bewerken**. 
1. De benodigde wijzigingen aanbrengen en selecteer **opslaan**.

    [![Een type bewerken](media/v2-update-how-to-tsm/portal_four.png)](media/v2-update-how-to-tsm/portal_four.png#lightbox)

### <a name="delete-a-type"></a>Een type verwijderen

1. Selecteer het type en selecteer **verwijderen**.
1. Als er geen exemplaren gekoppeld aan de typen zijn, wordt deze verwijderd.

    [![Een type verwijderen](media/v2-update-how-to-tsm/portal_five.png)](media/v2-update-how-to-tsm/portal_five.png#lightbox)

## <a name="hierarchies"></a>Hiërarchieën

### <a name="create-a-single-hierarchy"></a>Maken van een enkele hiërarchie

1. Ga naar het deelvenster van de selectie van Time Series-modellen en selecteer **hiërarchieën** in het menu. Het deelvenster zich kunt richten op de modellen van Time Series-hiërarchieën samenvouwen.

    [![Selecteer hiërarchieën](media/v2-update-how-to-tsm/portal_six.png)](media/v2-update-how-to-tsm/portal_six.png#lightbox)

1. Selecteer **Toevoegen**.

    [![Een hiërarchie toevoegen](media/v2-update-how-to-tsm/portal_seven.png)](media/v2-update-how-to-tsm/portal_seven.png#lightbox)

1. Selecteer **niveau toevoegen** in het rechter deelvenster.

    [![Een niveau toevoegen](media/v2-update-how-to-tsm/portal_eight.png)](media/v2-update-how-to-tsm/portal_eight.png#lightbox)

1. Voer de Hiërarchiedetails van de en selecteer **maken**.

    [![Een niveau maken](media/v2-update-how-to-tsm/portal_nine.png)](media/v2-update-how-to-tsm/portal_nine.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Bulksgewijs uploaden van een of meer hiërarchieën

1. Selecteer **uploaden JSON**.
1. Selecteer het bestand met de nettolading van de hiërarchie.
1. Selecteer **Uploaden**.

    [![Bulksgewijs uploaden hiërarchieën](media/v2-update-how-to-tsm/portal_ten.png)](media/v2-update-how-to-tsm/portal_ten.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Een enkele hiërarchie bewerken

1. Selecteer de hiërarchie en **bewerken**.
1. De benodigde wijzigingen aanbrengen en selecteer **opslaan**.

    [![Een enkele hiërarchie bewerken](media/v2-update-how-to-tsm/portal_eleven.png)](media/v2-update-how-to-tsm/portal_eleven.png#lightbox)

### <a name="delete-a-hierarchy"></a>Een hiërarchie verwijderen

1. Selecteer de hiërarchie en **verwijderen**. 
1. Als er geen exemplaren gekoppeld aan de hiërarchie zijn, wordt deze verwijderd.

    [![Een hiërarchie verwijderen](media/v2-update-how-to-tsm/portal_twelve.png)](media/v2-update-how-to-tsm/portal_twelve.png#lightbox)

## <a name="instances"></a>exemplaren

### <a name="create-a-single-instance"></a>Een enkele instantie maken

1. Ga naar het deelvenster van de selectie van Time Series-modellen en selecteer **exemplaren** in het menu. Het deelvenster om zich te richten op de modellen van Time Series-instanties samenvouwen.

    [![Een enkele instantie maken](media/v2-update-how-to-tsm/portal_thirteen.png)](media/v2-update-how-to-tsm/portal_thirteen.png#lightbox)

1. Selecteer **Toevoegen**.

    [![Een exemplaar toevoegen](media/v2-update-how-to-tsm/portal_fourteen.png)](media/v2-update-how-to-tsm/portal_fourteen.png#lightbox)

1. Geef de exemplaardetails, selecteer het type en hiërarchie-koppeling en selecteer **maken**.

### <a name="bulk-upload-one-or-more-instances"></a>Bulksgewijs uploaden van een of meer exemplaren

1. Selecteer **uploaden JSON**.
1. Selecteer het bestand met de nettolading van de exemplaren.

    [![Bulksgewijs uploaden van een of meer exemplaren](media/v2-update-how-to-tsm/portal_fifteen.png)](media/v2-update-how-to-tsm/portal_fifteen.png#lightbox)

1. Selecteer **Uploaden**.

### <a name="edit-a-single-instance"></a>Een enkele instantie bewerken

1. Selecteer het exemplaar en selecteer **bewerken**. 
1. De benodigde wijzigingen aanbrengen en selecteer **opslaan**.

    [![Een enkele instantie bewerken](media/v2-update-how-to-tsm/portal_sixteen.png)](media/v2-update-how-to-tsm/portal_sixteen.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

- Lees voor meer informatie over Time Series modellen [gegevensmodellering](./time-series-insights-update-tsm.md).

- Lees voor meer informatie over de Preview-versie [visualiseren van gegevens in de Verkenner van Azure Time Series Insights Preview](./time-series-insights-update-explorer.md).

- Lees voor meer informatie over ondersteunde JSON-vormen, [ondersteund JSON-vormen](./time-series-insights-send-events.md#json).
