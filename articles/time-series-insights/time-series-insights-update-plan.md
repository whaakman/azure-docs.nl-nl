---
title: Plannen van uw omgeving Azure Time Series Insights (Preview) | Microsoft Docs
description: Uw omgeving Azure Time Series Insights (Preview) plannen
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: c385d10aac01c844f1d4b390c0bb3d064b9befa3
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52878700"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Uw omgeving Azure Time Series Insights (Preview) plannen

Dit artikel wordt beschreven aanbevolen procedures voor het plannen en gaan snel met behulp van de Azure Time Series Insights (Preview).

## <a name="best-practices-for-planning-and-preparation"></a>Aanbevolen procedures voor het plannen en voorbereiden

Om te beginnen met Time Series Insights (TSI), is het beste als u het volgende weten:

* Wat u ontvangt wanneer u een omgeving met TSI (Preview) inricht.
* Wat uw **-id's van Time Series** en **Timestamp** eigenschappen zijn.
* Wat de nieuwe **Tijdreeksmodel** is en over het bouwen van uw eigen.
* Klik hier voor meer informatie over het verzenden van gebeurtenissen efficiënt in JSON.  
* TSI business opties voor noodherstel.

De Time Series Insights-update implementeert een model voor betalen per gebruik business.  Zie voor meer informatie over de kosten in rekening gebracht en capaciteit [Time Series Insights prijzen](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-time-series-insights-preview-environment"></a>De omgeving Time Series Insights (Preview)

Als u een omgeving met TSI (Preview) inricht, kunt u twee Azure-resources maken:

* TSI (Preview)-omgeving
* Azure storage-account voor algemeen gebruik V1

Aan de slag wilt, moet u drie extra items.  De eerste is een [Tijdreeksmodel](./time-series-insights-update-tsm.md), de tweede is een [bron van gebeurtenis die is verbonden met Time Series Insights](./time-series-insights-how-to-add-an-event-source-iothub.md), en de derde is [gebeurtenissen die worden doorgestuurd naar de gebeurtenisbron](./time-series-insights-send-events.md) die geen van beide toegewezen aan het model en geldige JSON-indeling hebben.  

## <a name="configure-your-time-series-ids-and-timestamp-properties"></a>De eigenschappen van de Time Series-id en tijdstempel configureren

Voor het maken van een nieuwe TSI-omgeving, selecteer een **Time Series-ID**. Hiermee wordt dus fungeert als een logische partitie voor uw gegevens. Zoals opgemerkt, zorg ervoor dat u hebt uw **-id's van Time Series** gereed.

> [!IMPORTANT]
> **Time Series-id's** zijn **onveranderbare** en **kan later worden gewijzigd**. Controleer voordat u de selectie van de laatste en eerste gebruik.

U kunt maximaal selecteren **drie** (3) sleutels aan een unieke onderscheid maken tussen uw resources. Lees de [aanbevolen procedures voor het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md) en [opslag- en uitgangsclaims](./time-series-insights-update-storage-ingress.md) artikelen voor meer informatie.

De **Timestamp** eigenschap is ook heel belangrijk. U kunt deze eigenschap opgeven bij het toevoegen van bronnen van gebeurtenissen. De bron van elke gebeurtenis is een optionele **Timestamp** eigenschap die wordt gebruikt voor het bijhouden gebeurtenisbronnen na verloop van tijd. **Tijdstempel** waarden zijn hoofdlettergevoelig en moet worden geformatteerd met de afzonderlijke specificatie van de bron van elke gebeurtenis.

> [!TIP]
> Controleer of de vereisten voor de bronnen van gebeurtenissen opmaak en parseren.

Wanneer dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** van een gebeurtenis bron wordt gebruikt als de gebeurtenis **Timestamp**. Als u historische gegevens of batch gebeurtenissen verzendt, zult u waarschijnlijk merken aanpassen van de **Timestamp** eigenschap niet meer dan de standaardwaarde van nuttige **tijd van de gebeurtenis in de wachtrij plaatsen**. Lees voor meer informatie over [toevoegen van bronnen van gebeurtenissen in IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md).  

## <a name="understand-the-time-series-model"></a>Inzicht in de tijdreeks Model

U kunt nu uw TSI-omgeving configureren **Tijdreeksmodel**. Het nieuwe model maakt het gemakkelijk te vinden en IoT-gegevens te analyseren. Het door het inschakelen van de curatie, onderhoud en verrijking van time series-gegevens en helpt bij het voorbereiden van het gegevenssets gereed is voor consumenten. Het model gebruikt **-id's van Time Series**, die worden toegewezen aan een exemplaar van de unieke resource koppelen aan variabelen (ook wel typen) en hiërarchieën. Meer informatie over de nieuwe [Tijdreeksmodel](./time-series-insights-update-tsm.md).

Het model is dynamisch, zodat deze op elk gewenst moment kunt gebouwd. Echter, u zult kunnen snel aan de slag meer als deze is gemaakt en geüpload voordat u begint met het pushen van gegevens naar TSI. Voor het bouwen van uw model, Controleer de [over het gebruik van TSM](./time-series-insights-update-how-to-tsm.md) artikel.

Voor veel klanten we verwachten dat de **Tijdreeksmodel** om toe te wijzen aan een bestaande asset model of ERP-systeem al op locatie. Voor klanten die nog geen een bestaand model, een vooraf gemaakte gebruikerservaring is [opgegeven](https://github.com/Microsoft/tsiclient) snel actief en werkend. U kunt combineren hoe een model kan u helpen hand onze [voorbeeld demo-omgeving](https://insights.timeseries.azure.com/preview/demo).  

## <a name="shaping-your-events"></a>Uw gebeurtenissen vormgeven

Het is belangrijk om te controleren of de manier waarop u gebeurtenissen verzenden naar TSI. In het ideale geval wordt de gebeurtenissen worden gedenormaliseerd goed en efficiënt.

Een goede vuistregel:

* Metagegevens moet worden opgeslagen in uw **Time Series-Model**
* **Time Series-modus;**  exemplaarvelden en gebeurtenissen moeten u alleen de benodigde informatie zoals:
  * **Time Series-ID**
  * **Timestamp**

Controleer de [hoe vorm gebeurtenissen](./time-series-insights-send-events.md#json) artikel voor meer details.

## <a name="business-disaster-recovery"></a>Noodherstel voor bedrijven

TSI is als een Azure-service, een service met hoge beschikbaarheid (HA) met behulp van redundantie op het niveau van de Azure-regio. Er is geen configuratie is vereist voor het gebruik van deze inherente functionaliteiten. De Microsoft Azure-platform bevat ook functies voor informatie over het bouwen van oplossingen met mogelijkheden voor disaster recovery (DR) of regio-overschrijdende beschikbaarheid. Als u wilt voor globale, interregionale HA voor apparaten of gebruikers profiteren van deze functies Azure herstel na Noodgevallen. Het artikel [technische richtlijnen voor Azure Business Continuity](https://docs.microsoft.com/azure/resiliency/resiliency-technical-guidance) beschrijving van de ingebouwde functies in Azure voor bedrijfscontinuïteit en herstel na Noodgevallen. De [na noodgevallen](https://docs.microsoft.com/azure/architecture/resiliency/index) herstel en hoge beschikbaarheid voor Azure-toepassingen papier uitleg bij architectuur biedt over strategieën voor Azure-toepassingen om HA en DR te realiseren.

> [!NOTE]
> Azure Time Series Insights heeft geen ingebouwde BCDR.
> Standaard hebben Azure Storage, Azure IoT Hub en Event Hubs herstel, zijn geïntegreerd.

Voor meer informatie:

* Meer informatie over [Azure opslagredundantie](https://docs.microsoft.com/azure/storage/common/storage-redundancy).
* Meer informatie over [HA herstel na Noodgevallen van IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr).
* Meer informatie over [van Event Hub-beleid](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).

Klanten die behoefte BCDR hebben kunnen echter nog steeds een strategie voor herstel implementeren met het maken van een tweede TSI-omgeving in een back-up van de Azure-regio. Klanten verzenden gebeurtenissen naar deze secundaire omgeving van de primaire gegevensbron gebruik te maken van een tweede speciale klantengroep en van de bron van die gebeurtenis BCDR richtlijnen.

De specifieke stappen om dit te bereiken zijn als volgt:

1. Maak een omgeving in een tweede regio. Meer informatie over [TSI omgevingen](./time-series-insights-get-started.md).
1. Maken van een tweede speciale klantengroep voor uw gebeurtenisbron en de bron van die gebeurtenis verbinden met de nieuwe omgeving. Zorg ervoor dat de tweede, toegewezen consumergroep aanwijzen. Meer informatie uit de [IoT Hub-documentatie](./time-series-insights-how-to-add-an-event-source-iothub.md) of de [Event Hub-documentatie](./time-series-insights-data-access.md).
1. Als uw primaire regio wordt beïnvloed tijdens een incident na noodgevallen, omleiden bewerkingen aan de back-up TSI-omgeving.

> [!IMPORTANT]
> * Houd er rekening mee dat een vertraging in het geval van een failover kan worden ervaren.
> * Failover kan ook een piek mometary veroorzaken bij het verwerken van een bericht zoals bewerkingen zijn omgeleid.
> * Raadpleeg voor meer informatie, [beperkende latentie in TSI](./time-series-insights-environment-mitigate-latency.md).

## <a name="next-steps"></a>Volgende stappen

Lees de [opslag van Azure TSI (Preview) en ingress](./time-series-insights-update-storage-ingress.md).

Meer informatie over [gegevensmodellering](./time-series-insights-update-tsm.md).