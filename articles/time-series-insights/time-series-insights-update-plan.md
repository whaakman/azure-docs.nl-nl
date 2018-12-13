---
title: Azure Time Series Insights Preview planning - uw Azure Time Series Insights Preview-omgeving plannen | Microsoft Docs
description: Uw Azure Time Series Insights Preview-omgeving plannen.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: 1df4847f20329e924352adfe782faa43d10dde98
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53277043"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Uw Azure Time Series Insights Preview-omgeving plannen

Dit artikel wordt beschreven aanbevolen procedures voor het plannen en gaan snel met behulp van Azure Time Series Insights Preview.

## <a name="best-practices-for-planning-and-preparation"></a>Aanbevolen procedures voor het plannen en voorbereiden

Als u wilt aan de slag met Time Series Insights, is het beste als u bekend bent:

* Wat u krijgt wanneer u een Time Series Insights Preview-omgeving inrichten.
* Wat uw Time Series-id en tijdstempel eigenschappen zijn.
* Wat de nieuwe Time Series-Model is, en over het bouwen van uw eigen.
* Klik hier voor meer informatie over het verzenden van gebeurtenissen efficiënt in JSON. 
* Time Series Insights business opties voor noodherstel.

Time Series Insights maakt gebruik van een model voor betalen per gebruik business. Zie voor meer informatie over de kosten in rekening gebracht en capaciteit [Time Series Insights prijzen](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-time-series-insights-preview-environment"></a>De Time Series Insights Preview-omgeving

Als u een Time Series Insights Preview-omgeving inricht, kunt u twee Azure-resources maken:

* Time Series Insights-Preview-omgeving
* Azure Storage voor algemeen gebruik V1-account

Als u wilt starten, moet u drie extra items:
 
- Een [Time Series-Model](./time-series-insights-update-tsm.md) 
- Een [bron van gebeurtenis die is verbonden met Time Series Insights](./time-series-insights-how-to-add-an-event-source-iothub.md) 
- [Gebeurtenissen die worden doorgestuurd naar de gebeurtenisbron](./time-series-insights-send-events.md) die zijn beide toegewezen aan het model en geldige JSON-indeling hebben 

## <a name="configure-your-time-series-ids-and-timestamp-properties"></a>De eigenschappen van de Time Series-id en tijdstempel configureren

Voor het maken van een nieuwe Time Series Insights-omgeving, selecteer een Time Series-ID. Hiermee wordt dus fungeert als een logische partitie voor uw gegevens. Zoals opgemerkt, zorg ervoor dat u hebt uw Time Series-id's gereed.

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
* Time Series-modus, instantievelden en gebeurtenissen opnemen alleen die nodig zijn, zoals:
  * Tijdreeks-id
  * Tijdstempel

Zie voor meer informatie, [gebeurtenissen vorm](./time-series-insights-send-events.md#json).

## <a name="business-disaster-recovery"></a>Noodherstel voor bedrijven

Time Series Insights is een service met hoge beschikbaarheid die gebruikmaakt van redundantie op het niveau van de Azure-regio. Configuratie is niet vereist om te gebruiken van deze inherente functionaliteiten. De Microsoft Azure-platform bevat ook functies voor informatie over het bouwen van oplossingen met herstelfunctionaliteit bij noodgevallen of regio-overschrijdende beschikbaarheid. Voor globale, interregionale hoge beschikbaarheid voor apparaten of gebruikers profiteren van deze Azure disaster recovery-functies. 

Zie voor meer informatie over ingebouwde functies in Azure voor bedrijfscontinuïteit en herstel na noodgevallen (BCDR) [Azure business continuity technisch advies](https://docs.microsoft.com/azure/resiliency/resiliency-technical-guidance). Zie het document op voor de architectuur van informatie over strategieën voor Azure-toepassingen om hoge beschikbaarheid en herstel na noodgevallen te realiseren, [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](https://docs.microsoft.com/azure/architecture/resiliency/index).

> [!NOTE]

>  Time Series Insights beschikt niet over ingebouwde BCDR.
> Standaard hebben Azure Storage, Azure IoT Hub en Azure Event Hubs herstel, zijn geïntegreerd.

Lees voor meer informatie over:

* [Azure Storage-redundantie](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
* [IoT Hub-noodherstel hoge beschikbaarheid](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr)
* [Event Hub-beleid](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)

Als u BCDR nodig hebt, kunt u nog steeds een strategie voor herstel te implementeren. Maak een tweede Time Series Insights-omgeving in een back-up van de Azure-regio. Gebeurtenissen verzenden naar deze secundaire omgeving van uw primaire gebeurtenisbron. Gebruik een tweede speciale klantengroep en van de bron van die gebeurtenis BCDR richtlijnen.

Volg deze stappen voor het maken en gebruiken van een secundaire Time Series Insights-omgeving.

1. Maak een omgeving in een tweede regio. Zie voor meer informatie, [Time Series Insights-omgevingen](./time-series-insights-get-started.md).
1. Maak een tweede speciale klantengroep voor uw gebeurtenisbron. De bron van die gebeurtenis verbinden met de nieuwe omgeving. Zorg ervoor dat de tweede speciale klantengroep aanwijzen. Zie voor meer informatie, de [IoT Hub-documentatie](./time-series-insights-how-to-add-an-event-source-iothub.md) of de [Event Hub-documentatie](./time-series-insights-data-access.md).
1. Als uw primaire regio is last hebben van een incident na noodgevallen, omleiden bewerkingen aan de back-up Time Series Insights-omgeving.

> [!IMPORTANT]
> * Houd er rekening mee dat een vertraging in het geval van een failover kan worden ervaren.
> * Failover kan ook een kortstondige piek veroorzaken bij het verwerken van een bericht zoals bewerkingen zijn omgeleid.
> * Zie voor meer informatie, [verminderen latentie in Time Series Insights](./time-series-insights-environment-mitigate-latency.md).

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over:

- [Azure Time Series Insights Preview opslag en inkomend](./time-series-insights-update-storage-ingress.md)
- [Gegevens modelleren](./time-series-insights-update-tsm.md)