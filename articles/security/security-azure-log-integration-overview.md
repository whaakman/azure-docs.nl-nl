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
ms.date: 08/10/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 6c3a2ac18fdb7a7a722447af720b9dee28adef08
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Inleiding tot Microsoft Azure log-integratie
Meer informatie over de integratie van Azure-logboekanalyse, de belangrijkste mogelijkheden en hoe het werkt.

## <a name="overview"></a>Overzicht

Azure-logboekanalyse-integratie is een gratis oplossing waarmee u kunt het integreren van onbewerkte logboeken van uw Azure-resources in uw on-premises Security Information en Event Management SIEM ()-systemen.

Integratie van Azure-logboekanalyse verzamelt Windows-gebeurtenissen van Windows-Logboeken, [Azure activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [Azure Security Center waarschuwingen](../security-center/security-center-intro.md), en [Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) van Azure-resources. Deze integratie helpt uw SIEM-oplossing een uniforme dashboard bieden voor alle activa, on-premises of in de cloud, zodat u samenvoegen kunt, correleren, analyseren en waarschuwen voor beveiligingsgebeurtenissen.

>[!NOTE]
Op dit moment zijn de enige ondersteunde clouds Azure commercieel en Azure Government. Andere clouds worden niet ondersteund.

![Azure-logboekintegratie][1]

## <a name="what-logs-can-i-integrate"></a>Welke logboeken kan ik worden geïntegreerd?
Azure produceert uitgebreide logboekregistratie in voor elke Azure-service. Deze logboeken vertegenwoordigen drie verschillende typen logboeken:

* **Besturingselement/management-logboeken** bieden inzicht in de [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) maken, bijwerken en DELETE-bewerkingen. Azure activiteitenlogboeken is een voorbeeld van dit type van het logboek.
* **Gegevens vlak logboeken** bieden inzicht in de gebeurtenissen die deze gebeurtenis treedt op als onderdeel van het gebruik van een Azure-resource. Een voorbeeld van dit type logboekbestand is de Windows-logboeken **System**, **beveiliging**, en **toepassing** kanalen in een virtuele Windows-computer. Een ander voorbeeld is Diagnostics Logging geconfigureerd via de Azure-Monitor
* **Verwerkte gebeurtenissen** bieden geanalyseerde gebeurtenis- en waarschuwingsgegevens namens jou worden verwerkt. Een voorbeeld van dit type gebeurtenis is Azure Security Center Alerts, waarin het Azure Beveiligingscentrum heeft verwerkt en uw abonnement om op te geven van waarschuwingen die relevant zijn voor uw huidige beveiligingspostuur geanalyseerd.

Integratie van Azure Log ondersteunt ArcSight, QRadar en Splunk. In alle gevallen Neem contact op met de leverancier van uw SIEM om te bepalen of ze een eigen connector hebben. In sommige gevallen moet u geen Azure Log integratie gebruiken wanneer systeemeigen connectors beschikbaar zijn. Voor meer informatie over logboek typen Neem gaat u naar de veelgestelde vragen ondersteunde.

>[!NOTE]
Azure Log-integratie is een gratis oplossing, maar er zijn Azure opslagkosten ten gevolge van het logboek informatie bestandsopslag.

Hulp is beschikbaar via de [Azure Log integratie MSDN-Forum](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Het forum kunt u de community AzLog ter ondersteuning van elkaar met vragen, antwoorden, tips en trucs over het ophalen van de meest buiten Azure Log-integratie. Bovendien de integratie van Azure Log-team dit forum wordt bewaakt en wanneer we kunt.

U kunt ook openen een [ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Om dit te doen, selecteert u **logboek integratie** als de service waarvoor u ondersteuning aanvraagt.

## <a name="next-steps"></a>Volgende stappen
In dit document, kunt u kennisgemaakt met Azure-logboekanalyse-integratie. Zie de volgende onderwerpen voor meer informatie over Azure-logboekanalyse-integratie en de verschillende typen logboeken die worden ondersteund:

* [Microsoft Azure Log integratie](https://www.microsoft.com/download/details.aspx?id=53324) : Download Center voor meer informatie over de systeemvereisten en instructies op Azure-logboekanalyse-integratie installeren.
* [Aan de slag met Azure-logboekanalyse integratie](security-azure-log-integration-get-started.md) - in deze zelfstudie doorloopt u de installatie van de integratie van Azure-logboekanalyse en logboeken vanuit Azure af opslag, Azure activiteitenlogboeken, integratie van Azure Security Center-waarschuwingen en Azure Active Directory controlelogboeken.
* [Partner configuratiestappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – dit blogbericht ziet u de integratie van Azure-logboekanalyse werken met partneroplossingen Splunk, HP ArcSight en IBM QRadar configureren. Deze blog vertegenwoordigt onze huidige positie voor het configureren van de partneroplossingen. In alle gevallen, Raadpleeg de documentatie van de partner-oplossing eerst.
* [Activiteit en ASC waarschuwingen gedurende een syslog op QRadar](https://blogs.msdn.microsoft.com/azuresecurity/2016/09/24/integrate-azure-logs-to-qradar/) – dit blogbericht voorziet de stappen voor de activiteit en Azure Security Center waarschuwingen via syslog naar QRadar verzonden
* [Azure-logboekanalyse integratie Veelgestelde vragen (FAQ)](security-azure-log-integration-faq.md) -deze Veelgestelde vragen over de antwoorden op vragen over Azure-logboekanalyse-integratie.
* [Waarschuwingen met Azure Security Center integreren Meld integratie](../security-center/security-center-integrating-alerts-with-log-integration.md) : dit document ziet u hoe u wilt synchroniseren van waarschuwingen van Azure Security Center met Azure Log-integratie.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
