---
title: Plannen van uw omgeving Azure Time Series Insights (preview) | Microsoft Docs
description: Uw omgeving Azure Time Series Insights (preview) plannen
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 438d71997d2c92e377cd068615d274af6b8b5edb
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856108"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Uw omgeving Azure Time Series Insights (preview) plannen

Dit artikel wordt beschreven aanbevolen procedures voor het plannen en gaan snel met behulp van de Azure Time Series Insights (preview).

## <a name="best-practices-for-planning-and-preparation"></a>Aanbevolen procedures voor het plannen en voorbereiden

Het is raadzaam te beschikken over de volgende items gereed is voordat u aan de slag:

* U hebt geïdentificeerd uw **Time Series-id's**
* U hebt uw **Timestamp** eigenschap gereed
* U hebt gebouwd uw **Time Series-Model**
* U begrijpt hoe voor het verzenden van gebeurtenissen die efficiënt gedenormaliseerde in JSON zijn

Deze items gereed helpt om te plannen en voorbereiden vereenvoudigen hebben. Bovendien is het verstandig plan vooruit en bepalen uw business-noodherstel (BCDR) moet voordat u uw exemplaar maakt (en niet daarna). In dat geval tevoren helpt om te controleren of dat uw exemplaar bewaartemperatuur wordt voorbereid.

> [!TIP]
> Uw omgeving aan uw BCDR-behoeften voor en niet na het maken van uw instantie configureren.

De Azure TSI (preview) maakt gebruik van een model voor betalen per gebruik business. Zie voor meer informatie over de kosten in rekening gebracht en capaciteit [Time Series Insights prijzen](https://azure.microsoft.com/pricing/details/time-series-insights).

## <a name="configure-your-time-series-ids-and-timestamp-properties"></a>De eigenschappen van de Time Series-id en tijdstempel configureren

Voor het maken van een nieuwe TSI-omgeving, selecteer een **Time Series-ID**. Hiermee wordt dus fungeert als een logische partitie voor uw gegevens. Zoals opgemerkt, zorg ervoor dat u hebt uw **-id's van Time Series** gereed.

> [!IMPORTANT]
> **Time Series-id's** zijn **onveranderbare** en **kan later worden gewijzigd**. Controleer voordat u de selectie van de laatste en eerste gebruik.

U kunt maximaal selecteren **drie** (3) sleutels aan een unieke onderscheid maken tussen uw resources. Lees de [aanbevolen procedures voor het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md) artikel voor meer informatie.

De bron van elke gebeurtenis is een optionele **Timestamp** eigenschap die wordt gebruikt voor het bijhouden gebeurtenisbronnen na verloop van tijd. **Tijdstempel** waarden zijn hoofdlettergevoelig en moet worden geformatteerd met de afzonderlijke specificatie van de bron van elke gebeurtenis.

> [!TIP]
> Controleer of de vereisten voor de bronnen van gebeurtenissen opmaak en parseren.

Wanneer dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** van een gebeurtenis bron wordt gebruikt als de gebeurtenis **Timestamp**. Als u historische gegevens of batch gebeurtenissen verzendt, zult u waarschijnlijk merken aanpassen van de **Timestamp** eigenschap niet meer dan de standaardwaarde van nuttige **tijd van de gebeurtenis in de wachtrij plaatsen**. Lees voor meer informatie over [toevoegen van bronnen van gebeurtenissen in IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md).  

## <a name="understand-the-time-series-model"></a>Inzicht in de tijdreeks Model

U kunt nu uw TSI-omgeving configureren **Tijdreeksmodel**. Het nieuwe model maakt het gemakkelijk te vinden en IoT-gegevens te analyseren. Het door het inschakelen van de curatie, onderhoud en verrijking van time series-gegevens en helpt bij het voorbereiden van het gegevenssets gereed is voor consumenten. Het model gebruikt **-id's van Time Series**, die worden toegewezen aan een exemplaar van de unieke resource koppelen aan variabelen (ook wel typen) en hiërarchieën. Meer informatie over de nieuwe [Tijdreeksmodel](./time-series-insights-update-tsm.md).

Het model is dynamisch, zodat deze op elk gewenst moment kunt gebouwd. Echter, u zult kunnen snel aan de slag meer als deze is gemaakt en geüpload voordat u begint met het pushen van gegevens naar TSI. Voor het bouwen van uw model, Controleer de [Tijdreeksmodel](./time-series-insights-update-tsm.md) artikel.

Voor veel klanten we verwachten dat de **Tijdreeksmodel** om toe te wijzen aan een bestaande asset model of ERP-systeem al op locatie. Voor klanten die nog geen een bestaand model, een vooraf gemaakte gebruikerservaring is [opgegeven](https://github.com/Microsoft/tsiclient) snel actief en werkend.

## <a name="shaping-your-events"></a>Uw gebeurtenissen vormgeven

Het is belangrijk om te controleren of de manier waarop u gebeurtenissen verzenden naar TSI. In het ideale geval wordt de gebeurtenissen worden gedenormaliseerd goed en efficiënt.

Een goede vuistregel:

* Metagegevens moet worden opgeslagen in uw **Time Series-Model**
* **Time Series-modus;**  exemplaarvelden en gebeurtenissen moeten u alleen de benodigde informatie zoals:
  * **Time Series-ID**
  * **Timestamp**

Controleer de [hoe vorm gebeurtenissen](./time-series-insights-update-how-to-shape-events.md) artikel voor meer details.

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
1. Als uw primaire regio bij een incident na noodgevallen betrokken zijn, omleiden bewerkingen aan de back-up TSI-omgeving.

> [!IMPORTANT]
> * Houd er rekening mee dat een vertraging in het geval van een failover kan worden ervaren.
> * Failover kan ook een piek mometary veroorzaken bij het verwerken van een bericht zoals bewerkingen zijn omgeleid.
> * Raadpleeg voor meer informatie, [beperkende latentie in TSI](./time-series-insights-environment-mitigate-latency.md).

## <a name="next-steps"></a>Volgende stappen

Lees de [Azure TSI (preview) Storage en binnenkomende](./time-series-insights-update-storage-ingress.md).

Meer informatie over de nieuwe [Tijdreeksmodel](./time-series-insights-update-tsm.md).