---
title: bestand opnemen
description: bestand opnemen
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 04/29/2019
ms.openlocfilehash: cf07d19966c08a63b9aa50475622aa0a1e5e1600
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236508"
---
## <a name="business-disaster-recovery"></a>Noodherstel voor bedrijven

In deze sectie beschrijft de functies van Azure Time Series Insights dat apps en services die worden uitgevoerd, zelfs als er zich een noodgeval voordoet (**business noodherstel**).

### <a name="high-availability"></a>Hoge beschikbaarheid

Als een Azure-service, Time Series Insights biedt bepaalde **hoge beschikbaarheid** functies met behulp van redundantie op het niveau van de Azure-regio. Bijvoorbeeld Microsoft Azure biedt ondersteuning voor de herstelfunctionaliteit bij noodgevallen via de Azure **regio-overschrijdende beschikbaarheid** functie.

Biedt aanvullende hoge beschikbaarheidsfuncties geleverd via Azure (en ook beschikbaar is voor een Time Series Insights-exemplaar):

1. **Failover**: Azure biedt [geo-replicatie en het laden van netwerktaakverdeling](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
1. **Gegevens worden hersteld op** en **opslag recovery**: Azure biedt [verschillende mogelijkheden om te behouden en gegevens herstellen](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
1. **Site Recovery** functies via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Zorg ervoor dat u het inschakelen van deze Azure-functies voor globale, interregionale, hoge beschikbaarheid van uw apparaten en gebruikers.

> [!NOTE]
> Als Azure is geconfigureerd om in te schakelen **regio-overschrijdende beschikbaarheid**, is geen extra regio-overschrijdende beschikbaarheid-configuratie vereist zijn binnen Azure Time Series Insights.

### <a name="iot-and-event-hubs"></a>IoT en Event hubs

Sommige Azure IoT-services zijn ook ingebouwde herstelfuncties:

1. [IoT Hub hoge beschikbaarheid noodherstel](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr) met inbegrip van redundantie intra-regio.
1. [Event Hub-beleid](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).
1. [Azure Storage-redundantie](https://docs.microsoft.com/azure/storage/common/storage-redundancy).

Time Series Insights integreren met deze andere services biedt aanvullende disaster recovery verkoopkansen. Telemetrie verzonden naar uw Event Hub kan bijvoorbeeld een back-up van de database van Azure Blob Storage worden vastgehouden.

### <a name="time-series-insights"></a>Time Series Insights

Er zijn verschillende manieren om uw Time Series Insights-gegevens, apps en services die worden uitgevoerd, zelfs als ze zijn onderbroken. U kunt ook bepalen dat een kopie van het volledige, dubbele, back-up van uw Azure Time Series-omgeving vereist is:

1. Als een TSI-specifieke **failover-exemplaar** omleiden naar gegevens en verkeer naar.
1. Voor controle- en gegevens bewaren.

In het algemeen is de beste manier om het dupliceren van een omgeving met TSI het maken van een tweede TSI-omgeving in een back-up van de Azure-regio. Gebeurtenissen worden ook verzonden naar deze secundaire omgeving van uw primaire gebeurtenisbron. Zorg ervoor dat een tweede, toegewezen, consumergroep gebruiken en richtlijnen van die bron business disaster recovery (hierboven).

Specifiek, het maken van een dubbele omgeving:

1. Een omgeving maken in een tweede regio ([een nieuwe Time Series Insights-omgeving maken in Azure portal](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started)).
1. Maak een tweede speciale klantengroep voor uw gebeurtenisbron.
1. De bron van die gebeurtenis verbinden met de nieuwe omgeving te zorgen dat de tweede, toegewezen consumergroep aanwijzen.
1. Bekijk de Time Series Insights [IoT-Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) en [Event hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) documentatie.

Ten slotte:

* Als uw primaire regio is last hebben van een incident na noodgevallen, omleiden bewerkingen aan de back-up Time Series Insights-omgeving.
* Gebruik de tweede regio voor back-up en herstel van alle gegevens van TSI Telemetrie en query's uitvoeren.

> [!IMPORTANT]
> * Houd er rekening mee dat een vertraging in het geval van een failover kan worden ervaren.
> * Failover kan ook leiden tot een kortstondige piek in de berichtverwerking van een zoals bewerkingen zijn omgeleid.
> * Zie voor meer informatie, [verminderen latentie in Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).