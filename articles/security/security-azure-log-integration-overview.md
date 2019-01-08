---
title: Logboeken van Azure-resources met uw SIEM-systemen integreren | Microsoft Docs
description: Meer informatie over Azure-Logboekintegratie, de belangrijkste mogelijkheden en hoe het werkt.
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
ms.date: 06/07/2018
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: e9b547b6b582e38a51196815522132dd807c971b
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54079031"
---
# <a name="introduction-to-azure-log-integration"></a>Inleiding tot Azure-Logboekintegratie

>[!IMPORTANT]
> De functie van de integratie met Azure Log worden afgeschaft door 06/01/2019. AzLog downloads uitgeschakeld door 27 juni 2018. Voor informatie over wat te doen verplaatsen forward revisie van het bericht [gebruikt Azure monitor om te integreren met SIEM-hulpprogramma's](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Azure-Logboekintegratie is beschikbaar voor vereenvoudigde integratie van Azure logboeken met uw on-premises Security Information and Event Management (SIEM)-systeem.

 De aanbevolen methode voor logboeken integreren in Azure is het gebruik van van de leverancier van uw SIEM-connectors. Azure Monitor biedt de mogelijkheid om de logboeken streamen naar eventhubs en SIEM-leveranciers connectoren voor de logboeken van de event hub verder te integreren in de SIEM kunnen schrijven.  Volg de instructies in voor een beschrijving van hoe dit werkt, [Monitor stream controleren voor gegevens van gebeurtenishubs](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). Het artikel bevat ook de siem's waarvoor al directe Azure-connectors beschikbaar zijn.  

> [!IMPORTANT]
> Als uw primaire interesse met het verzamelen van Logboeken van de virtuele machine, bevatten de meeste SIEM-leveranciers met deze optie in de oplossing. Met behulp van de SIEM is-connector van de leverancier altijd de voorkeur alternatief.

De documentatie op de Azure-Logboekintegratie-functie wordt nog steeds wordt onderhouden totdat de functie is afgeschaft.

Lees verder voor meer informatie over de functie Azure-Logboekintegratie:

Azure-Logboekintegratie Windows-gebeurtenissen verzamelt van Windows-Logboeken, [Azure-activiteitenlogboeken](../azure-monitor/platform/activity-logs-overview.md), [Azure Security Center-waarschuwingen](../security-center/security-center-intro.md), en [Azure Diagnostics-logboeken](../azure-monitor/platform/diagnostic-logs-overview.md) uit Azure-resources. Integratie van uw SIEM-oplossing of geef op via het uniforme dashboard voor al uw activa, helpt on-premises of in de cloud. Een dashboard kunt u ontvangen, samenvoegen, correleren en analyseren van waarschuwingen voor beveiligingsgebeurtenissen.

> [!NOTE]
> Azure-Logboekintegratie ondersteunt momenteel alleen Azure commerciële en Azure Government-clouds. Andere clouds worden niet ondersteund.

![Het proces van het Azure-Logboekintegratie][1]

## <a name="what-logs-can-i-integrate"></a>Welke logboeken Integreer ik?

Azure levert uitgebreide logboekregistratie in voor elke Azure-service. De logboeken staan drie typen van de logboeken:

* **Beheer/beheer logboeken**: Bieden inzicht in de [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) maken, bijwerken en DELETE-bewerkingen. Een Azure-activiteitenlogboek volgt een voorbeeld van dit type van het logboek.
* **Gegevens vlak logboeken**: Bieden inzicht in gebeurtenissen die worden gegenereerd wanneer u een Azure-resource. Een voorbeeld van dit type logboekbestand is de Windows-logboeken **System**, **Security**, en **toepassing** kanalen in een Windows-machine. Een ander voorbeeld is de Azure Diagnostics-logboekregistratie, waardoor u via Azure Monitor configureren.
* **Gebeurtenissen verwerkt**: Geef geanalyseerde gebeurtenis en informatie over waarschuwingen die worden verwerkt voor u. Een voorbeeld van dit type gebeurtenis is Azure Security Center-waarschuwingen. Azure Security Center worden verwerkt en analyseert uw abonnement voor waarschuwingen die relevant voor uw huidige beveiligingsstatus zijn.

Azure-Logboekintegratie ondersteunt ArcSight, QRadar en Splunk. Neem contact op met de leverancier van uw SIEM om te beoordelen of de leverancier een eigen connector heeft. Gebruik de Azure-Logboekintegratie niet als een systeemeigen connector beschikbaar is.

Als er geen andere opties beschikbaar zijn, overweeg het gebruik van Azure-Logboekintegratie. De volgende tabel bevat onze aanbevelingen:

|SIEM | Klant al gebruikmaakt van de Azure log integrator | Klant onderzoekt SIEM-integratie-opties|
|---------|--------------------------|-------------------------------------------|
|**Splunk** | Kunt u migreren naar de [Azure Monitor-invoegtoepassing voor Splunk](https://splunkbase.splunk.com/app/3534/). | Gebruik de [Splunk connector](https://splunkbase.splunk.com/app/3534/). |
|**QRadar** | Migratie naar of gebruikmaken van de QRadar-connector die wordt beschreven in de laatste sectie van [Stream Azure-bewakingsgegevens naar een event hub voor gebruik door een extern hulpprogramma](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). | Gebruik van de QRadar-connector die wordt beschreven in de laatste sectie van [Stream Azure-bewakingsgegevens naar een event hub voor gebruik door een extern hulpprogramma](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). |
|**ArcSight** | Echter ook doorgaan met de Azure log integrator totdat een connector beschikbaar is, en vervolgens migreren naar de oplossing op basis van een connector.  | Overweeg het gebruik van Azure Log Analytics als alternatief. Geen onboarding voor Azure-Logboekintegratie, tenzij u bereid bent te doorlopen van het migratieproces zodra de connector beschikbaar. |

> [!NOTE]
> Hoewel Azure-Logboekintegratie gratis oplossing is, er zijn met een Azure storage-kosten voor gegevensopslag log-bestand.

Als u hulp nodig hebt, kunt u een [ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Selecteer voor de service, **Logboekintegratie**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u kennisgemaakt met de Azure-Logboekintegratie. Zie de volgende artikelen voor meer informatie over Azure-Logboekintegratie en de typen logboeken die worden ondersteund:

* [Aan de slag met Azure-Logboekintegratie](security-azure-log-integration-get-started.md). In deze zelfstudie helpt u bij de installatie van Azure-Logboekintegratie. Ook wordt beschreven hoe u logboeken van Windows Azure Diagnostics (WAD), opslag, Azure-activiteitenlogboeken, waarschuwingen van Azure Security Center en auditlogboeken van Azure Active Directory integreren.
* [Veelgestelde vragen (FAQ) Azure-Logboekintegratie](security-azure-log-integration-faq.md). Deze Veelgestelde vragen vindt u antwoorden op veelgestelde vragen over Azure-Logboekintegratie.
* Meer informatie over het [streamen van Azure-bewakingsgegevens naar een event hub voor gebruik door een extern hulpprogramma](../azure-monitor/platform/stream-monitoring-data-event-hubs.md).

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
