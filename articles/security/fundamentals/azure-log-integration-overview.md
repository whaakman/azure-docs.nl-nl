---
title: Logboeken van Azure-resources integreren met uw SIEM-systemen | Microsoft Docs
description: Meer informatie over Azure Log Integration, de belangrijkste mogelijkheden en hoe deze werkt.
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/28/2019
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 9ba4a64268bcc57f04e92be83edb2ac71f18bcaf
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727600"
---
# <a name="introduction-to-azure-log-integration"></a>Inleiding tot Azure Log Integration

>[!IMPORTANT]
> De Azure-functie voor logboek integratie wordt afgeschaft door 06/15/2019. AzLog-down loads zijn op 27 juni 2018 uitgeschakeld. Raadpleeg voor meer informatie over wat u verder kunt doen met [Azure monitor om te integreren met Siem-hulpprogram ma's](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Azure Log Integration is beschikbaar gesteld om de taak van het integreren van Azure-logboeken met uw on-premises Security Information and Event Management-systeem (SIEM) te vereenvoudigen.

 De aanbevolen methode voor het integreren van Azure-Logboeken is het gebruik van de connectors van uw SIEM-leverancier. Azure Monitor biedt de mogelijkheid om de logboeken te streamen naar Event hubs en SIEM leveranciers kunnen Connect oren schrijven om logboeken te integreren van de Event Hub in de SIEM.  Volg de instructies in [Stream bewaking controleren op Data Event hubs](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)voor een beschrijving van hoe dit werkt. In dit artikel wordt ook de Siem's vermeld waarvoor direct Azure-connectors beschikbaar zijn.  

> [!IMPORTANT]
> Als de logboeken van de virtuele machine worden verzameld, bevatten de meeste SIEM-leveranciers deze optie in hun oplossing. Het gebruik van de SIEM-connector van de leverancier is altijd het voor keur alternatief.

De documentatie over de Azure Log Integration-functie wordt nog steeds onderhouden totdat de functie is afgeschaft.

Lees verder voor meer informatie over de functie Azure Log Integration:

Azure Log Integration verzamelt Windows-gebeurtenissen van Windows Logboeken-Logboeken, [activiteiten logboeken van Azure](/azure/azure-monitor/platform/activity-logs-overview), [Azure Security Center waarschuwingen](/azure/security-center/security-center-intro)en [Azure Diagnostics logboeken](/azure/azure-monitor/platform/diagnostic-logs-overview) van Azure-resources. Integratie helpt uw SIEM-oplossing een gecombineerd dash board te bieden voor al uw assets, zowel on-premises als in de Cloud. U kunt een dash board gebruiken om waarschuwingen voor beveiligings gebeurtenissen te ontvangen, samen te stellen, te correleren en te analyseren.

> [!NOTE]
> Momenteel ondersteunt Azure Log Integration alleen Azure commerciële en Azure Government Clouds. Andere Clouds worden niet ondersteund.

![Het Azure Log Integration proces](media/azure-log-integration-overview/azure-log-integration.png)

## <a name="what-logs-can-i-integrate"></a>Welke logboeken kan ik integreren?

Azure produceert uitgebreide logboek registratie voor elke Azure-service. De logboeken vertegenwoordigen drie logboek typen:

* **Beheer-en beheer logboeken**: Geef inzicht in de [Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview) Create-, update-en delete-bewerkingen. Een Azure-activiteiten logboek is een voor beeld van dit type logboek.
* **Logboeken voor gegevens vlak**: Geef inzicht in gebeurtenissen die worden gegenereerd wanneer u een Azure-resource gebruikt. Een voor beeld van dit type logboek is de **systeem**-, **beveiligings**-en **toepassings** kanalen van Windows logboeken op een virtuele Windows-machine. Een ander voor beeld is Azure Diagnostics logboek registratie, die u configureert via Azure Monitor.
* **Verwerkte gebeurtenissen**: Geef de geanalyseerde gebeurtenis-en waarschuwings informatie op die voor u wordt verwerkt. Een voor beeld van dit type gebeurtenis is Azure Security Center-waarschuwingen. Azure Security Center processen en analyseert uw abonnement om waarschuwingen te geven die relevant zijn voor uw huidige beveiligings postuur.

Azure Log Integration ondersteunt ArcSight, QRadar en Splunk. Neem contact op met de leverancier van uw SIEM om te beoordelen of de leverancier een systeem eigen connector heeft. Gebruik Azure Log Integration niet als er een systeem eigen connector beschikbaar is.

Als er geen andere opties beschikbaar zijn, kunt u overwegen Azure Log Integration te gebruiken. De volgende tabel bevat onze aanbevelingen:

|SIEM | De klant maakt al gebruik van de Azure-logboek integrator | De klant onderzoekt SIEM-integratie opties|
|---------|--------------------------|-------------------------------------------|
|**Splunk** | Begin met migreren naar de [Azure monitor-invoeg toepassing voor Splunk](https://splunkbase.splunk.com/app/3534/). | Gebruik de [Splunk-connector](https://splunkbase.splunk.com/app/3534/). |
|**QRadar** | Migreer naar of begin met het gebruik van de QRadar-connector die is gedocumenteerd in de laatste sectie van [Stream Azure-bewakings gegevens naar een event hub voor het gebruik van een extern hulp programma](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs). | Gebruik de QRadar-connector die is gedocumenteerd in de laatste sectie van [Stream Azure-bewakings gegevens naar een event hub voor het gebruik van een extern hulp programma](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs). |
|**ArcSight** | Blijf de Azure-logboek Integrator gebruiken totdat er een connector beschikbaar is en migreer vervolgens naar de oplossing op basis van een connector.  | Overweeg Azure Monitor-Logboeken als alternatief te gebruiken. Onboarding voor Azure Log Integration tenzij u bereid bent het migratie proces door te lopen wanneer de connector beschikbaar wordt. |

> [!NOTE]
> Hoewel Azure Log Integration een gratis oplossing is, zijn er kosten verbonden aan de opslag van het logboek bestand in azure Storage.

Als u hulp nodig hebt, kunt u een [ondersteunings aanvraag](/azure/azure-supportability/how-to-create-azure-support-request)maken. Selecteer voor de service **logboek integratie**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt u geïntroduceerd om Azure Log Integration. Raadpleeg de volgende artikelen voor meer informatie over Azure Log Integration en de typen logboeken die worden ondersteund:

* [Aan de slag met Azure log Integration](azure-log-integration-get-started.md). In deze zelf studie wordt u begeleid bij de installatie van Azure Log Integration. Ook wordt beschreven hoe u Logboeken integreert van Windows Azure Diagnostics (WAD)-opslag, Azure-activiteiten logboeken, Azure Security Center waarschuwingen en Azure Active Directory audit Logboeken.
* [Azure log Integration Veelgestelde vragen (FAQ)](azure-log-integration-faq.md). In deze veelgestelde vragen vindt u antwoorden op veelgestelde vragen over Azure Log Integration.
* Meer informatie over het [streamen van Azure-bewakings gegevens naar een event hub voor het gebruik van een extern hulp programma](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs).

