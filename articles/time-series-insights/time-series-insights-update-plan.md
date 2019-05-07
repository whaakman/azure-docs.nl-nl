---
title: Uw Azure Time Series Insights Preview-omgeving plannen | Microsoft Docs
description: Uw Azure Time Series Insights Preview-omgeving plannen.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: b3fab86b2b2f0ad892e02cd089dbd7c45ce601d6
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205772"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Uw Azure Time Series Insights Preview-omgeving plannen

Dit artikel wordt beschreven aanbevolen procedures voor het plannen en gaan snel met behulp van Azure Time Series Insights Preview.

> [!NOTE]
> Zie [plannen van uw omgeving Azure Time Series Insights GA](time-series-insights-environment-planning.md), voor aanbevolen procedures voor het plannen van een algemene beschikbaarheid TSI-exemplaar.

## <a name="best-practices-for-planning-and-preparation"></a>Aanbevolen procedures voor het plannen en voorbereiden

Als u wilt aan de slag met Time Series Insights, is het beste als u bekend bent:

* Wat u krijgt wanneer u [inrichten van een Time Series Insights Preview-omgeving](#the-preview-environment).
* Wat uw [Time Series-id en tijdstempel eigenschappen zijn](#configure-time-series-ids-and-timestamp-properties).
* Wat de nieuwe [Time Series-Model is](#understand-the-time-series-model), en over het bouwen van uw eigen.
* Hoe u [efficiënt verzenden van gebeurtenissen in JSON](#shape-your-events).
* Time Series Insights [opties voor herstel na noodgevallen](#business-disaster-recovery).

Azure Time Series Insights maakt gebruik van een model voor betalen per gebruik business. Zie voor meer informatie over de kosten in rekening gebracht en capaciteit [Time Series Insights prijzen](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-preview-environment"></a>De Preview-omgeving

Als u een Time Series Insights Preview-omgeving inricht, kunt u twee Azure-resources maken:

* Een Azure Time Series Insights Preview-omgeving
* Een Azure Storage-account voor algemeen gebruik V1

Als u wilt starten, moet u drie extra items:

* Een [Time Series-Model](./time-series-insights-update-tsm.md).
* Een [bron van gebeurtenis die is verbonden met Time Series Insights](./time-series-insights-how-to-add-an-event-source-iothub.md).
* [Gebeurtenissen die worden doorgestuurd naar de gebeurtenisbron](./time-series-insights-send-events.md) die zijn beide toegewezen aan het model en geldige JSON-indeling hebben.

## <a name="configure-time-series-ids-and-timestamp-properties"></a>Eigenschappen van Time Series-id en tijdstempel configureren

Voor het maken van een nieuwe Time Series Insights-omgeving, selecteer een **Time Series-ID**. Hiermee wordt dus fungeert als een logische partitie voor uw gegevens. Zoals opgemerkt, zorg ervoor dat u hebt uw Time Series-id's gereed.

> [!IMPORTANT]
> Time Series-id's zijn *onveranderbare* en *kan later worden gewijzigd*. Controleer of elk criterium voordat definitieve selectie en voor het eerst gebruiken.

U kunt maximaal drie (3) sleutels om unieke onderscheid uw resources te selecteren. Lees voor meer informatie, [aanbevolen procedures voor het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md) en [opslag- en uitgangsclaims](./time-series-insights-update-storage-ingress.md).

De eigenschap Timestamp is ook belangrijk. U kunt deze eigenschap opgeven wanneer u de bronnen van gebeurtenissen toevoegt. De bron van elke gebeurtenis is een optionele Timestamp eigenschap die is gebruikt voor het bijhouden gebeurtenisbronnen na verloop van tijd. Tijdstempel waarden zijn hoofdlettergevoelig en moeten worden geformatteerd met de afzonderlijke specificatie van de bron van elke gebeurtenis.

> [!TIP]
> Controleer of de vereisten voor de bronnen van gebeurtenissen opmaak en parseren.

Wanneer dit veld leeg blijft, wordt de tijd van de gebeurtenis in de wachtrij plaatsen van een gebeurtenisbron wordt gebruikt als de gebeurtenis-Timestamp. Als u historische gegevens of batch gebeurtenissen verzendt, is aanpassen van de tijdstempeleigenschap meer nuttige dan de standaardwaarde van tijd van de gebeurtenis in de wachtrij plaatsen. Lees voor meer informatie over [toevoegen van bronnen van gebeurtenissen in IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md).

## <a name="understand-the-time-series-model"></a>Inzicht in de tijdreeks Model

U kunt nu uw Time Series Insights-omgeving Tijdreeksmodel configureren. Het nieuwe model maakt het gemakkelijk te vinden en IoT-gegevens te analyseren. Hiermee kunnen de curatie, onderhoud en verrijking van time series-gegevens en helpt bij het voorbereiden van het gegevenssets gereed is voor consumenten. Het model gebruikt **-id's van Time Series**, die worden toegewezen aan een exemplaar dat wordt gekoppeld aan de unieke resource met variabelen, typen en hiërarchieën genoemd. Meer informatie over de nieuwe [Tijdreeksmodel](./time-series-insights-update-tsm.md).

Het model is dynamisch is, zodat deze kan worden gebouwd op elk gewenst moment. Als u wilt snel aan de slag, bouwen en upload het vóór het pushen van gegevens naar de Time Series Insights. Zie voor het bouwen van uw model, [gebruikt u de Time Series-Model](./time-series-insights-update-how-to-tsm.md).

Voor veel klanten wordt de Time Series-Model toegewezen aan een bestaande asset model of ERP-systeem al op locatie. Als u een bestaand model hebt, is het een vooraf gedefinieerde gebruikerservaring [opgegeven](https://github.com/Microsoft/tsiclient) snel actief en werkend. Weergeven om te voorzien van hoe een model voor u kan helpen de [voorbeeld demo-omgeving](https://insights.timeseries.azure.com/preview/demo).

## <a name="shape-your-events"></a>Vorm uw gebeurtenissen

U kunt controleren of de manier waarop u gebeurtenissen naar de Time Series Insights verzenden. De gebeurtenissen zijn in het ideale geval gedenormaliseerd goed en efficiënt.

Een goede vuistregel:

* Store-metagegevens in uw Time Series-Model
* Time Series-modus, instantievelden en gebeurtenissen alleen nodig gegevens bevatten, zoals een **Time Series-ID** of **Timestamp**.

Zie voor meer informatie, [gebeurtenissen vorm](./time-series-insights-send-events.md#json).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [opslag- en uitgangsclaims](./time-series-insights-update-storage-ingress.md) in de Preview van Time Series Insights.

- Meer informatie over [gegevensmodellering](./time-series-insights-update-tsm.md) in de Preview van Time Series Insights.