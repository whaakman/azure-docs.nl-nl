---
title: Logboeken van de Azure-resources integreren met uw SIEM-systemen | Microsoft Docs
description: Meer informatie over de integratie van Azure Log, de belangrijkste mogelijkheden en hoe het werkt.
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 6d91692a64a4d3def80990a439fe0a0898bf2f09
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="introduction-to-azure-log-integration"></a>Inleiding tot Azure-logboekanalyse-integratie

Integratie van Azure Log kunt u onbewerkte logboeken integreren van uw Azure-resources met uw on-premises Security Information en Event Management SIEM ()-systemen. Integratie van Azure Log alleen gebruiken als een connector voor [Azure Monitor](../monitoring-and-diagnostics/monitoring-get-started.md) is niet beschikbaar zijn van de leverancier van uw SIEM.

De voorkeursmethode voor het integreren van Azure Logboeken is met behulp van de leverancier van uw SIEM Azure Monitor-connector. Volg de instructies in voor het gebruik van de connector [Monitor stroom bewaking voor gegevens van event hubs](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). 

Echter als de leverancier van uw SIEM geen een connector Azure bewaken biedt, u mogelijk gebruikmaken van integratie van Azure-logboek als tijdelijke oplossing totdat een connector beschikbaar is. Integratie van Azure Log is een optie alleen als Azure Log integratie uw SIEM ondersteunt.

> [!IMPORTANT]
> Als uw primaire interesse met het verzamelen van Logboeken van de virtuele machine, omvatten de meeste leveranciers van SIEM deze optie in de oplossing. Met behulp van de SIEM is-connector van de leverancier altijd het aanbevolen alternatief is.

Integratie van Azure logboek verzamelt Windows-gebeurtenissen van Windows-Logboeken, [Azure activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [Azure Security Center waarschuwingen](../security-center/security-center-intro.md), en [Azure Diagnostics logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) uit Azure-resources. Integratie helpt uw SIEM-oplossing een uniforme dashboard of opgeven voor alle activa, lokaal of in de cloud. Een dashboard kunt u ontvangen, samenvoegen, correleren en waarschuwingen voor beveiligingsgebeurtenissen te analyseren.

> [!NOTE]
> Integratie van Azure Log ondersteunt momenteel alleen commerciële Azure en Azure Government clouds. Andere clouds worden niet ondersteund.

![Diagram van het proces voor de integratie van Azure-logboek][1]

## <a name="what-logs-can-i-integrate"></a>Welke logboeken kan ik worden geïntegreerd

Azure produceert uitgebreide logboekregistratie in voor elke Azure-service. De logboeken staan drie logboek typen:

* **Besturingselement/management-logboeken**: inzicht in de [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) maken, bijwerken en DELETE-bewerkingen. Een Azure activiteitenlogboek is een voorbeeld van dit type van het logboek.
* **Gegevens vlak logboeken**: inzicht in gebeurtenissen die worden gegenereerd wanneer u een Azure-resource. Een voorbeeld van dit type logboekbestand is de Windows-logboeken **System**, **beveiliging**, en **toepassing** kanalen in een virtuele Windows-computer. Een ander voorbeeld is Azure Diagnostics logboekregistratie die u via de Azure-Monitor configureert.
* **Verwerkte gebeurtenissen**: Geef geanalyseerde gebeurtenis en informatie over waarschuwingen die worden verwerkt voor u. Een voorbeeld van dit type gebeurtenis is Azure Security Center-waarschuwingen. Azure Security Center verwerkt en uw abonnement om op te geven van waarschuwingen die relevant voor uw huidige beveiligingspostuur zijn analyseert.

Integratie van Azure Log ondersteunt ArcSight, QRadar en Splunk. Neem contact op met de leverancier van uw SIEM om te beoordelen of de leverancier een eigen connector heeft. Gebruik de Azure Log-integratie niet als een systeemeigen connector beschikbaar is.

Als er geen andere opties beschikbaar zijn, kunt u overwegen Azure Log-integratie. De volgende tabel bevat onze aanbevelingen:

|SIEM | Klant al gebruikmaakt van de Azure-logboekanalyse integrator | Klant onderzoekt SIEM integratie-opties|
|---------|--------------------------|-------------------------------------------|
|**Splunk** | Beginnen met migreren naar de [Azure Monitor-invoegtoepassing voor Splunk](https://splunkbase.splunk.com/app/3534/). | Gebruik de [Splunk connector](https://splunkbase.splunk.com/app/3534/). |
|**QRadar** | Om te migreren of te beginnen met behulp van de QRadar connector beschreven in de laatste sectie van [stroom Azure voor het bewaken van gegevens naar een event hub voor verbruik door een extern hulpprogramma](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). | Gebruik van de QRadar connector beschreven in de laatste sectie van [stroom Azure voor het bewaken van gegevens naar een event hub voor verbruik door een extern hulpprogramma](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). |
|**ArcSight** | Blijven de Azure-logboekanalyse integrator gebruiken totdat een connector beschikbaar is. Migreer vervolgens naar de oplossing op basis van een connector.  | Overweeg het gebruik van Azure-logboekanalyse als alternatief. Niet vrijgeven aan Azure Log integratie tenzij u bereid bent te doorlopen van het migratieproces zodra de connector beschikbaar. |

> [!NOTE]
> Hoewel Azure Log-integratie een gratis oplossing is, zijn er Azure-opslagkosten in verband met informatie van logboek bestandsopslag.

Als u hulp nodig hebt, kunt u een [ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Selecteer voor de service **logboek integratie**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel vindt u de integratie van Azure-logboek. Zie de volgende artikelen voor meer informatie over de integratie van Azure-logboek en de verschillende typen logboeken die worden ondersteund:

* [Aan de slag met Azure Log integratie](security-azure-log-integration-get-started.md). Deze zelfstudie leert u de installatie van Azure Log-integratie. Ook wordt beschreven hoe u logboeken van Windows Azure Diagnostics (af) opslag, Azure activiteitenlogboeken, Azure Security Center-waarschuwingen en auditlogboeken van Azure Active Directory integreren.
* [Integratie van Azure Log Veelgestelde vragen (FAQ)](security-azure-log-integration-faq.md). Deze Veelgestelde vragen over de antwoorden op veelgestelde vragen over Azure Log-integratie.
* Meer informatie over het [stream Azure voor het bewaken van gegevens naar een event hub voor verbruik door een extern hulpprogramma](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
