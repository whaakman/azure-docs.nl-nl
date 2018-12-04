---
title: Gegevens modelleren in Azure Time Series Insights | Microsoft Docs
description: Wat is Azure Time Series Insights modelleren van gegevens?
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: 926d1a35cb10d50ddeacbed5476e2dcf14d0999d
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856158"
---
# <a name="data-modeling-in-azure-time-series-insights"></a>Gegevens modelleren in Azure Time Series Insights

Dit document wordt beschreven hoe u werkt met **Time Series modellen** Azure Time Series Insights (preview) te volgen. Deze details over verschillende algemene scenario's met gegevens.

Lees de [Azure TSI (preview) Explorer](./time-series-insights-update-explorer.md) artikel voor meer informatie over de Private Preview te navigeren.

## <a name="types"></a>Typen

### <a name="how-to-create-a-single-type"></a>Over het maken van één type

1. Begin met de kop van het paneel TSM model selector en typen selecteren in het menu. Vervolgens samen het paneel zich kunt richten op TSM typen:

    ![portal_one][1]

1. Klik op **Toevoegen**.
1. Voer alle details met betrekking tot de typen en op **maken**. In dat geval moet typen maken in de omgeving:

    ![portal_two][2]

### <a name="how-to-bulk-upload-one-or-more-types"></a>Bulksgewijs uploaden van een of meer typen

1. Klik op **uploaden JSON**.
1. Kies dit bestand met de nettolading van het type.
1. Klik op **uploaden**

    ![portal_three][3]

### <a name="how-to-edit-a-single-type"></a>Het bewerken van één type

* Selecteer het type en klik op **bewerken** knop. Benodigde wijzigingen aanbrengen en klik op **opslaan**:

    ![portal_four][4]

### <a name="how-to-delete-a-type"></a>Het verwijderen van een type

* Selecteer het type en klik op **verwijderen** knop. Als er geen exemplaren gekoppeld aan de typen zijn, wordt deze verwijderd:

    ![portal_five][5]

## <a name="hierarchies"></a>Hiërarchieën

### <a name="how-to-create-a-single-hierarchy"></a>Over het maken van een enkele hiërarchie

1. Begin met de kop van het paneel TSM model selector en hiërarchieën selecteren in het menu. Vervolgens samen het paneel zich kunt richten op TSM typen:

    ![portal_six][6]

1. Klik op **toevoegen**

    ![portal_seven][7]

1. Klik op **niveau toevoegen** in het rechterdeelvenster:

    ![portal_eight][8]

1. Voer de Hiërarchiedetails van de en klik op **maken**:

    ![portal_nine][9]

### <a name="how-to-bulk-upload-one-or-more-hierarchies"></a>Bulksgewijs uploaden van een of meer hiërarchieën

1. Klik op **uploaden JSON**.
1. Kies het bestand met de nettolading van de hiërarchie.
1. Klik op **uploaden**:

    ![portal_ten][10]

### <a name="how-to-edit-a-single-hierarchy"></a>Een enkele hiërarchie bewerken

* De hiërarchie selecteren en klik op de **bewerken** knop. Benodigde wijzigingen aanbrengen en klik op **opslaan**:

    ![portal_eleven][11]

### <a name="how-to-delete-a-hierarchy"></a>Een hiërarchie verwijderen

* De hiërarchie selecteren en klik op **verwijderen** knop. Als er geen exemplaren gekoppeld aan de hiërarchie zijn, wordt deze verwijderd.

    ![portal_twelve][12]

## <a name="instances"></a>Exemplaren

### <a name="how-to-create-a-single-instance"></a>Over het maken van één exemplaar

1. Begin met de kop van het paneel TSM model selector en exemplaren selecteren in het menu. Vervolgens samen het paneel zich kunt richten op TSM typen:

    ![portal_thirteen][13]

1. Klik op **toevoegen**:

    ![portal_fourteen][14]

1. Geef de exemplaardetails, type en de hiërarchie koppeling selecteren en op **maken**.

### <a name="how-to-bulk-upload-one-or-more-instances"></a>Bulksgewijs uploaden van een of meer exemplaren

1. Klik op **uploaden JSON**.
1. Kies het bestand met de nettolading van de exemplaren:

    ![portal_fifteen][15]

1. Klik op **uploaden**.

### <a name="how-to-edit-a-single-instance"></a>Het bewerken van één exemplaar

* Selecteer het exemplaar en klik op de **bewerken** knop. Benodigde wijzigingen aanbrengen en klik op **opslaan**:

    ![portal_sixteen][16]

### <a name="how-to-delete-an-instance"></a>Een exemplaar verwijderen

* Selecteer het exemplaar en klik op **verwijderen** knop. Als er geen gebeurtenissen gekoppeld aan de exemplaren zijn, wordt deze verwijderd.

## <a name="next-steps"></a>Volgende stappen

Lezen [Time Series modellen](./time-series-insights-update-tsm.md) voor meer informatie over **Time Series modellen**.

De Azure TSI (preview) Explorer weergeven [artikel](./time-series-insights-update-explorer.md) voor meer informatie over de Preview-versie.

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