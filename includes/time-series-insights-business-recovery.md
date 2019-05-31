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
ms.openlocfilehash: e3e7044148e262e6977ae3be96f7cb61218114a3
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388753"
---
## <a name="business-disaster-recovery"></a>Noodherstel voor bedrijven

In deze sectie beschrijft de functies van Azure Time Series Insights dat apps en services die worden uitgevoerd, zelfs als er zich een noodgeval voordoet. Dit concept staat bekend als zakelijke noodherstel.

### <a name="high-availability"></a>Hoge beschikbaarheid

Als een Azure-service biedt Time Series Insights van bepaalde functies voor hoge beschikbaarheid via redundantie op het niveau van de Azure-regio. Microsoft Azure ondersteunt bijvoorbeeld de herstelfunctionaliteit bij noodgevallen via Azure regio-overschrijdende beschikbaarheid functie.

Extra functies voor hoge beschikbaarheid geleverd via Azure, en dat zijn ook beschikbaar is voor een Time Series Insights-exemplaar, opnemen:

* **Failover**: Azure biedt [geo-replicatie en taakverdeling](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
* **Gegevens worden hersteld op** en **opslag recovery**: Azure biedt [verschillende mogelijkheden om te behouden en gegevens herstellen](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
* **Siteherstel**: Functies die beschikbaar via zijn [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Zorg ervoor dat u het inschakelen van deze Azure-functies voor globale, interregionale hoge beschikbaarheid van uw apparaten en gebruikers.

> [!NOTE]
> Als Azure is geconfigureerd als regio-overschrijdende beschikbaarheid wilt inschakelen, is geen configuratie extra regio-overschrijdende beschikbaarheid in Azure Time Series Insights.

### <a name="iot-and-event-hubs"></a>IoT en event hubs

Sommige Azure IoT-services zijn ook ingebouwde herstelfuncties:

* [Hoge beschikbaarheid noodherstel van Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr), waaronder intra-regio redundantie.
* [Beleid voor Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).
* [Azure Storage-redundantie](https://docs.microsoft.com/azure/storage/common/storage-redundancy).

Time Series Insights integreren met deze andere services biedt aanvullende disaster recovery verkoopkansen. Telemetrie verzonden naar uw event hub mogelijk bijvoorbeeld persistent worden gemaakt met een back-up van de database van Azure Blob-opslag.

### <a name="time-series-insights"></a>Time Series Insights

Er zijn verschillende manieren om uw Time Series Insights-gegevens, apps en services die worden uitgevoerd, zelfs als ze zijn onderbroken. U kunt ook bepalen dat een volledige back-up van uw Azure Time Series-omgeving vereist is:

* Als een exemplaar van de failover-Time Series Insights-specifieke gegevens omleiden en verkeer naar.
* Voor controle- en gegevens bewaren.

In het algemeen is de beste manier om het dupliceren van een Time Series Insights-omgeving te maken van een tweede Time Series Insights-omgeving in een back-up van de Azure-regio. Gebeurtenissen worden ook verzonden naar deze secundaire omgeving van uw primaire gebeurtenisbron. Controleer of de gebruiken een tweede speciale klantengroep en richtlijnen van die bron business disaster recovery, zoals eerder is opgegeven.

Specifiek, het maken van een dubbele omgeving:

1. Maak een omgeving in een tweede regio. Zie voor instructies [een nieuwe Time Series Insights-omgeving maken in Azure portal](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started).
1. Maak een tweede speciale klantengroep voor uw gebeurtenisbron.
1. De bron van die gebeurtenis verbinden met de nieuwe omgeving. Zorg ervoor dat de tweede speciale klantengroep aanwijst.
1. Bekijk de Time Series Insights [IoT-hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) en [gebeurtenishub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) documentatie.

Ten slotte:

1. Als uw primaire regio is last hebben van een incident na noodgevallen, omleiden bewerkingen aan de back-up Time Series Insights-omgeving.
1. Gebruik de tweede regio back-up en herstellen van alle gegevens van Time Series Insights Telemetrie en query's uitvoeren.

> [!IMPORTANT]
> * Houd er rekening mee dat een vertraging in het geval van een failover kan worden ervaren.
> * Failover kan ook leiden tot een kortstondige piek in de berichtverwerking van een zoals bewerkingen zijn omgeleid.
> * Zie voor meer informatie, [verminderen latentie in Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).
