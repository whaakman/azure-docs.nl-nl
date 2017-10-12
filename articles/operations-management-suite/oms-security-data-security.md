---
title: Gegevensbeveiliging in de oplossing Beveiliging en controle in Operations Management Suite | Microsoft Docs
description: In dit document wordt uitgelegd hoe gegevens worden beheerd en bewaakt in de oplossing Beveiliging en controle in Operations Management Suite.
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 9cdf7deb-2a30-4672-b89f-71179ee8326a
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 3b6327b1f5150f32afd71639f32c55d823f1d1f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Gegevensbeveiliging in de oplossing Beveiliging en controle in Operations Management Suite
Om klanten te helpen bedreigingen te voorkomen, ze te detecteren en erop te reageren, verzamelt en verwerkt de [oplossing Beveiliging en controle in Operations Management Suite (OMS)](operations-management-suite-overview.md) gegevens over uw resources waaronder:

* Beveiligingslogboek
* ETW-gebeurtenissen (Event Tracing for Windows)
* Controlegebeurtenissen AppLocker
* Windows Firewall-logboek
* Advanced Threat Analytics-gebeurtenissen
* Resultaten van de evaluatie van de basislijn
* Resultaten van de antimalware-evaluatie
* Resultaten van de evaluatie van updates/patching
* Syslogs-streams die expliciet zijn ingeschakeld op de agent

We doen er alles aan om de privacy van gegevens te beschermen en deze gegevens te beveiligen. Microsoft voldoet aan strikte nalevings- en beveiligingsrichtlijnen - van het schrijven van code tot de uitvoering van een service.
In dit artikel wordt uitgelegd hoe gegevens worden beheerd en beveiligd in Beveiliging en controle in OMS.

## <a name="data-sources"></a>Gegevensbronnen
Met de oplossing Beveiliging en controle in OMS worden gegevens geanalyseerd die afkomstig zijn van uw virtuele machines en fysieke computers waarop OMS Agent is geïnstalleerd. Met de oplossing Beveiliging en controle in OMS worden configuratiegegevens over beveiligingsgebeurtenissen verzameld, zoals Windows-gebeurtenissen, controlelogboeken, IIS-logboeken en syslog-berichten. Voorbeelden van dergelijke gegevens zijn: besturingssysteemtype en -versie, actieve processen, computernaam, IP-adressen, aangemelde gebruiker en tenant-id.  

## <a name="data-protection"></a>Gegevensbeveiliging
**Scheiding van gegevens**: gegevens worden op een logische manier apart van elkaar gehouden, in elk onderdeel van de service. Alle gegevens worden gemarkeerd per organisatie. Deze markering blijft aanwezig gedurende de levenscyclus van de gegevens en deze wordt afgedwongen op elke laag van de service. 

**Gegevenstoegang**: om aanbevelingen voor beveiliging te bieden en mogelijke bedreigingen te onderzoeken, kunnen medewerkers van Microsoft toegang hebben tot gegevens die door services worden verzameld of geanalyseerd. We voldoen aan de [voorwaarden voor Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en de [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), die stellen dat Microsoft niet de klantgegevens gebruikt of gegevens ervan afleidt voor reclame- of vergelijkbare commerciële doeleinden. Om aanbevelingen voor beveiliging te bieden en mogelijke bedreigingen te onderzoeken, kunnen medewerkers van Microsoft toegang hebben tot gegevens die door services worden verzameld of geanalyseerd. We gebruiken klantgegevens alleen indien nodig om u Azure-services te bieden, met inbegrip van doeleinden die compatibel zijn met het leveren van die services. U behoudt alle rechten op uw eigen gegevens.

**Gegevensgebruik**: Microsoft gebruikt informatie over patronen en bedreigingen die worden gezien tussen meerdere tenants voor het verbeteren van onze mogelijkheden voor voorkoming en detectie; wij doen dit in overeenstemming met de privacyverplichtingen beschreven in onze [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [!NOTE]
> Gegevenslocatie is geconfigureerd op het niveau van de OMS-werkruimte, tijdens het maken van de werkruimte, dat deel uitmaakt van het eerste configuratieproces voor Beveiliging en controle in OMS.
> 
> 

## <a name="see-also"></a>Zie ook
In dit document hebt u geleerd hoe gegevens worden beheerd en bewaakt in OMS. Voor meer informatie over de oplossing Beveiliging en controle in OMS, zie:

* [Overzicht van Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Beveiligingswaarschuwingen in de oplossing Beveiliging en controle van Operations Management Suite bewaken en erop reageren](oms-security-responding-alerts.md)
* [Resources bewaken in de oplossing Beveiliging en controle van Operations Management Suite ](oms-security-monitoring-resources.md)

