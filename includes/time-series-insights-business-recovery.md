---
ms.topic: include
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 04/29/2019
ms.openlocfilehash: 8a3c630b54ff95a9b1200e2421c787a514a0aa52
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431029"
---
## <a name="business-disaster-recovery"></a>Noodherstel voor bedrijven

In deze sectie beschrijft de functies van Azure Time Series Insights dat apps en services die worden uitgevoerd, zelfs als er zich een noodgeval voordoet (ook wel *business noodherstel*).

### <a name="high-availability"></a>Hoge beschikbaarheid

Als een Azure-service, Time Series Insights biedt bepaalde *hoge beschikbaarheid* functies met behulp van redundantie op het niveau van de Azure-regio. Azure ondersteunt bijvoorbeeld de herstelfunctionaliteit bij noodgevallen via de Azure *regio-overschrijdende beschikbaarheid* functie.

Extra functies voor hoge beschikbaarheid geleverd via Azure (en ook beschikbaar is voor een Time Series Insights-exemplaar) zijn onder andere:

- **Failover**: Azure biedt [geo-replicatie en taakverdeling](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
- **Gegevens worden hersteld op** en **opslag recovery**: Azure biedt [verschillende mogelijkheden om te behouden en gegevens herstellen](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
- **Siteherstel**: Azure biedt site recovery-functies via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Zorg ervoor dat u de relevante Azure-functies voor globale, interregionale hoge beschikbaarheid van uw apparaten en gebruikers inschakelen.

> [!NOTE]
> Als Azure is geconfigureerd als regio-overschrijdende beschikbaarheid wilt inschakelen, wordt er geen extra regio-overschrijdende beschikbaarheid-configuratie in Azure Time Series Insights vereist.

### <a name="iot-and-event-hubs"></a>IoT en event hubs

Sommige Azure IoT-services zijn ook ingebouwde herstelfuncties:

- [IoT Hub hoge beschikbaarheid noodherstel](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr), met inbegrip van redundantie intra-regio
- [Beleidsregels voor Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)
- [Azure Storage-redundantie](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

Time Series Insights integreren met de andere services biedt aanvullende disaster recovery verkoopkansen. Telemetrie verzonden naar uw event hub mogelijk bijvoorbeeld persistent worden gemaakt met een back-up van de database van Azure Blob-opslag.

### <a name="time-series-insights"></a>Time Series Insights

Er zijn verschillende manieren om u te houden van uw Time Series Insights-gegevens, apps en services die worden uitgevoerd, zelfs als ze zijn onderbroken. 

Echter, kunt u bepalen dat een volledige back-up van uw Azure Time Series-omgeving ook vereist voor de volgende doeleinden is:

- Als een *failover-exemplaar* specifiek voor Time Series Insights gegevens omleiden en verkeer naar
- Behouden van gegevens en controle-informatie

In het algemeen is de beste manier om het dupliceren van een Time Series Insights-omgeving te maken van een tweede Time Series Insights-omgeving in een back-up van de Azure-regio. Gebeurtenissen worden ook verzonden naar deze secundaire omgeving van uw primaire gebeurtenisbron. Zorg ervoor dat u een tweede, toegewezen consumergroep. Ga als volgt van die bron business disaster recovery richtlijnen, zoals eerder beschreven.

Een dubbele omgeving maken:

1. Maak een omgeving in een tweede regio. Zie voor meer informatie, [een nieuwe Time Series Insights-omgeving maken in Azure portal](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started).
1. Maak een tweede speciale klantengroep voor uw gebeurtenisbron.
1. De bron van die gebeurtenis verbinden met de nieuwe omgeving. Zorg ervoor dat de tweede, toegewezen consumergroep aan te wijzen.
1. Bekijk de Time Series Insights [IoT-Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) en [Event Hubs](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) documentatie.

Als er een gebeurtenis optreedt:

1. Als uw primaire regio is last hebben van een incident na noodgevallen, omleiden bewerkingen aan de back-up Time Series Insights-omgeving.
1. Gebruik de tweede regio back-up en herstellen van alle gegevens van Time Series Insights Telemetrie en query's uitvoeren.

> [!IMPORTANT]
> Als er een failover optreedt:
> 
> * Een vertraging kan ook gebeuren.
> * Een tijdelijke piek in berichtverwerking optreden, zoals bewerkingen zijn omgeleid.
> 
> Zie voor meer informatie, [verminderen latentie in Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).

