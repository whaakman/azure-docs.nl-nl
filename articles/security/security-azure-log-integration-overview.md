---
title: Logboeken van de Azure-resources in uw SIEM-systemen integreert | Microsoft Docs
description: Meer informatie over de integratie van Azure-logboekanalyse, de belangrijkste mogelijkheden en hoe het werkt.
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
ms.openlocfilehash: e427e2f7dafb6db9bc5c15d841fbbf83a02fc0e1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Inleiding tot Microsoft Azure log-integratie

De Azure-logboekanalyse-integratie kunt u onbewerkte logboeken van uw Azure-resources integreren met uw on-premises Security Information en Event Management SIEM ()-systemen in het geval dat een connector voor [Azure Monitor](../monitoring-and-diagnostics/monitoring-get-started.md) is nog niet beschikbaar is via de leverancier van uw SIEM.

De voorkeursmethode voor het integreren van Azure Logboeken is met behulp van de leverancier van uw SIEM Azure Monitor connector en volgende [instructies](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). Echter, als de leverancier van uw SIEM geen een connector Azure bewaken biedt, u mogelijk gebruikmaken van integratie van Azure-logboek als tijdelijke oplossing (als uw SIEM wordt ondersteund door Azure Log-integratie) totdat deze connector beschikbaar is.

>[!IMPORTANT]
>Als uw primaire interesse in het verzamelen van Logboeken van de virtuele machine, bevatten de meeste leveranciers van SIEM dit in de oplossing. Met behulp van de SIEM moet-connector van de leverancier altijd het aanbevolen alternatief is.

Integratie van Azure-logboekanalyse verzamelt Windows-gebeurtenissen van Windows-Logboeken, [Azure activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [Azure Security Center waarschuwingen](../security-center/security-center-intro.md), en [Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) van Azure-resources. Deze integratie helpt uw SIEM-oplossing een uniforme dashboard bieden voor alle activa, on-premises of in de cloud, zodat u samenvoegen kunt, correleren, analyseren en waarschuwen voor beveiligingsgebeurtenissen.

>[!NOTE]
Op dit moment zijn de enige ondersteunde clouds Azure commercieel en Azure Government. Andere clouds worden niet ondersteund.

![Azure-logboekintegratie][1]

## <a name="what-logs-can-i-integrate"></a>Welke logboeken kan ik worden geïntegreerd?

Azure produceert uitgebreide logboekregistratie in voor elke Azure-service. Deze logboeken vertegenwoordigen drie verschillende typen logboeken:

* **Besturingselement/management-logboeken** bieden inzicht in de [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) maken, bijwerken en DELETE-bewerkingen. Azure activiteitenlogboeken is een voorbeeld van dit type van het logboek.
* **Gegevens vlak logboeken** bieden inzicht in de gebeurtenissen die deze gebeurtenis treedt op als onderdeel van het gebruik van een Azure-resource. Een voorbeeld van dit type logboekbestand is de Windows-logboeken **System**, **beveiliging**, en **toepassing** kanalen in een virtuele Windows-computer. Een ander voorbeeld is Diagnostics Logging geconfigureerd via de Azure-Monitor
* **Verwerkte gebeurtenissen** bieden geanalyseerde gebeurtenis- en waarschuwingsgegevens namens jou worden verwerkt. Een voorbeeld van dit type gebeurtenis is Azure Security Center Alerts, waarin het Azure Beveiligingscentrum heeft verwerkt en uw abonnement om op te geven van waarschuwingen die relevant zijn voor uw huidige beveiligingspostuur geanalyseerd.

Integratie van Azure Log ondersteunt ArcSight, QRadar en Splunk. In alle gevallen contact op met de leverancier van uw SIEM om vast te stellen of ze een eigen connector hebben. U moet Azure Log-integratie niet gebruiken wanneer systeemeigen connectors beschikbaar zijn.

Als er geen andere opties beschikbaar zijn, kan Azure Log integratie worden overwogen. De volgende tabel bevat onze aanbevelingen.

|**SIEM** | **De klant al gebruikmaakt van logboek integrator** | **Klant onderzoekt de mogelijkheden van SIEM-integratie**|
|---------|--------------------------|-------------------------------------------|
|**SPLUNK** | Beginnen met migreren naar de [Azure monitor-invoegtoepassing voor Splunk](https://splunkbase.splunk.com/app/3534/) | Gebruik [SPLUNK-connector](https://splunkbase.splunk.com/app/3534/) |
|**IBM QRADAR** | Om te migreren of te beginnen met behulp van de QRadar connector beschreven aan het einde van http://aka.ms/azmoneventhub | Gebruik van de QRadar connector beschreven aan het einde van http://aka.ms/azmoneventhub  |
|**ARCSIGHT** | Blijven de Integrator logboek gebruiken totdat een connector beschikbaar is en vervolgens migreren naar de oplossing op basis van een connector.  | Azure-logboekanalyse beschouwen als een alternatief. Ga naar Azure Log-integratie niet vrijgeven tenzij u bereid bent te doorlopen van het migratieproces zodra de connector beschikbaar. |

>[!NOTE]
>Azure Log-integratie is een gratis oplossing, maar er zijn Azure opslagkosten ten gevolge van het logboek informatie bestandsopslag.

Als u hulp nodig hebt opent u een [ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Om dit te doen, selecteert u **logboek integratie** als de service waarvoor u ondersteuning aanvraagt.

## <a name="next-steps"></a>Volgende stappen

In dit document, kunt u kennisgemaakt met Azure-logboekanalyse-integratie. Zie de volgende onderwerpen voor meer informatie over Azure-logboekanalyse-integratie en de verschillende typen logboeken die worden ondersteund:

* [Aan de slag met Azure-logboekanalyse integratie](security-azure-log-integration-get-started.md) - in deze zelfstudie doorloopt u de installatie van de integratie van Azure-logboekanalyse en logboeken vanuit Azure af opslag, Azure activiteitenlogboeken, integratie van Azure Security Center-waarschuwingen en Azure Active Directory controlelogboeken.
* [Azure-logboekanalyse integratie Veelgestelde vragen (FAQ)](security-azure-log-integration-faq.md) -deze Veelgestelde vragen over de antwoorden op vragen over Azure-logboekanalyse-integratie.
* [Stroom Azure voor het bewaken van gegevens naar een event hub voor verbruik door een extern hulpprogramma](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md)

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
