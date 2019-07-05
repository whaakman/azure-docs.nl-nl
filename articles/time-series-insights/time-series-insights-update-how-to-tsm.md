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
ms.date: 06/26/2019
ms.custom: seodec18
ms.openlocfilehash: 05faf77d22f77da87e7c22d47473e6debf0f77c8
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67460883"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Gegevens modelleren in Azure Time Series Insights-Preview

Dit document wordt beschreven hoe u werkt met Time Series modellen Azure Time Series Insights Preview te volgen. Deze details over verschillende algemene scenario's met gegevens.

Lees voor meer informatie over het gebruik van de update [Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md).

## <a name="types"></a>Types

### <a name="create-a-single-type"></a>Maken van één type

1. Ga naar het deelvenster van de selectie van Time Series-modellen en selecteer **typen** in het menu. Het deelvenster zich kunt richten op de typen Time Series modellen samenvouwen.

    [![Maken van één type](media/v2-update-how-to-tsm/portal-one.png)](media/v2-update-how-to-tsm/portal-one.png#lightbox)

1. Selecteer **Toevoegen**.
1. Alle gegevens die betrekking hebben op de typen en selecteer de invoer **maken**. Deze actie wordt gemaakt van typen in de omgeving.

    [![Een type toevoegen](media/v2-update-how-to-tsm/portal-two.png)](media/v2-update-how-to-tsm/portal-two.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Bulksgewijs uploaden van een of meer typen

1. Selecteer **uploaden JSON**.
1. Selecteer het bestand dat de nettolading van het type bevat.
1. Selecteer **Uploaden**.

    [![JSON uploaden](media/v2-update-how-to-tsm/portal-three.png)](media/v2-update-how-to-tsm/portal-three.png#lightbox)

### <a name="edit-a-single-type"></a>Één type bewerken

1. Selecteer het type en selecteer **bewerken**. 
1. De benodigde wijzigingen aanbrengen en selecteer **opslaan**.

    [![Een type bewerken](media/v2-update-how-to-tsm/portal-four.png)](media/v2-update-how-to-tsm/portal-four.png#lightbox)

### <a name="delete-a-type"></a>Een type verwijderen

1. Selecteer het type en selecteer **verwijderen**.
1. Als er geen exemplaren gekoppeld aan de typen zijn, wordt deze verwijderd.

    [![Een type verwijderen](media/v2-update-how-to-tsm/portal-five.png)](media/v2-update-how-to-tsm/portal-five.png#lightbox)

## <a name="hierarchies"></a>Hiërarchieën

### <a name="create-a-single-hierarchy"></a>Maken van een enkele hiërarchie

1. Ga naar het deelvenster van de selectie van Time Series-modellen en selecteer **hiërarchieën** in het menu. Het deelvenster zich kunt richten op de modellen van Time Series-hiërarchieën samenvouwen.

    [![Selecteer hiërarchieën](media/v2-update-how-to-tsm/portal-six.png)](media/v2-update-how-to-tsm/portal-six.png#lightbox)

1. Selecteer **Toevoegen**.

    [![Een hiërarchie toevoegen](media/v2-update-how-to-tsm/portal-seven.png)](media/v2-update-how-to-tsm/portal-seven.png#lightbox)

1. Selecteer **niveau toevoegen** in het rechter deelvenster.

    [![Een niveau toevoegen](media/v2-update-how-to-tsm/portal-eight.png)](media/v2-update-how-to-tsm/portal-eight.png#lightbox)

1. Voer de Hiërarchiedetails van de en selecteer **maken**.

    [![Een niveau maken](media/v2-update-how-to-tsm/portal-nine.png)](media/v2-update-how-to-tsm/portal-nine.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Bulksgewijs uploaden van een of meer hiërarchieën

1. Selecteer **uploaden JSON**.
1. Selecteer het bestand met de nettolading van de hiërarchie.
1. Selecteer **Uploaden**.

    [![Bulksgewijs uploaden hiërarchieën](media/v2-update-how-to-tsm/portal-ten.png)](media/v2-update-how-to-tsm/portal-ten.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Een enkele hiërarchie bewerken

1. Selecteer de hiërarchie en **bewerken**.
1. De benodigde wijzigingen aanbrengen en selecteer **opslaan**.

    [![Een enkele hiërarchie bewerken](media/v2-update-how-to-tsm/portal-eleven.png)](media/v2-update-how-to-tsm/portal-eleven.png#lightbox)

### <a name="delete-a-hierarchy"></a>Een hiërarchie verwijderen

1. Selecteer de hiërarchie en **verwijderen**. 
1. Als er geen exemplaren gekoppeld aan de hiërarchie zijn, wordt deze verwijderd.

    [![Een hiërarchie verwijderen](media/v2-update-how-to-tsm/portal-twelve.png)](media/v2-update-how-to-tsm/portal-twelve.png#lightbox)

## <a name="instances"></a>exemplaren

### <a name="create-a-single-instance"></a>Een enkele instantie maken

1. Ga naar het deelvenster van de selectie van Time Series-modellen en selecteer **exemplaren** in het menu. Het deelvenster om zich te richten op de modellen van Time Series-instanties samenvouwen.

    [![Een enkele instantie maken](media/v2-update-how-to-tsm/portal-thirteen.png)](media/v2-update-how-to-tsm/portal-thirteen.png#lightbox)

1. Selecteer **Toevoegen**.

    [![Een exemplaar toevoegen](media/v2-update-how-to-tsm/portal-fourteen.png)](media/v2-update-how-to-tsm/portal-fourteen.png#lightbox)

1. Geef de exemplaardetails, selecteer het type en hiërarchie-koppeling en selecteer **maken**.

### <a name="bulk-upload-one-or-more-instances"></a>Bulksgewijs uploaden van een of meer exemplaren

1. Selecteer **uploaden JSON**.
1. Selecteer het bestand met de nettolading van de exemplaren.

    [![Bulksgewijs uploaden van een of meer exemplaren](media/v2-update-how-to-tsm/portal-fifteen.png)](media/v2-update-how-to-tsm/portal-fifteen.png#lightbox)

1. Selecteer **Uploaden**.

### <a name="edit-a-single-instance"></a>Een enkele instantie bewerken

1. Selecteer het exemplaar en selecteer **bewerken**. 
1. De benodigde wijzigingen aanbrengen en selecteer **opslaan**.

    [![Een enkele instantie bewerken](media/v2-update-how-to-tsm/portal-sixteen.png)](media/v2-update-how-to-tsm/portal-sixteen.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

- Lees voor meer informatie over Time Series modellen [gegevensmodellering](./time-series-insights-update-tsm.md).

- Lees voor meer informatie over de Preview-versie [visualiseren van gegevens in de Verkenner van Azure Time Series Insights Preview](./time-series-insights-update-explorer.md).

- Lees voor meer informatie over ondersteunde JSON-vormen, [ondersteund JSON-vormen](./time-series-insights-send-events.md#json).
