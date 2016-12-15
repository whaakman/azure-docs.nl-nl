---
title: Gegevensbeveiliging in Azure Security Center | Microsoft Docs
description: In dit document wordt uitgelegd hoe gegevens worden beheerd en beveiligd in Azure Security Center.
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f20d77a43185a6b22a5da1ee1c2744707df13dae


---
# <a name="azure-security-center-data-security"></a>Gegevensbeveiliging in Azure Security Center
Om klanten te helpen bedreigingen te voorkomen, te detecteren en erop te reageren, verzamelt en verwerkt Azure Security Center gegevens over uw Azure-resources, zoals configuratie-informatie, metagegevens, gebeurtenislogboeken, crashdumpbestanden en nog veel meer. We doen er alles aan om de privacy van gegevens te beschermen en deze gegevens te beveiligen. Microsoft voldoet aan strikte nalevings- en beveiligingsrichtlijnen - van het schrijven van code tot de uitvoering van een service. 

In dit artikel wordt uitgelegd hoe gegevens worden beheerd en beveiligd in Azure Security Center.

## <a name="data-sources"></a>Gegevensbronnen
Azure Security Center analyseert gegevens uit de volgende bronnen:

* Azure Services: leest informatie over de configuratie van de Azure-services die u hebt geïmplementeerd door te communiceren met de resourceprovider van die service.
* Netwerkverkeer: leest steekproefgewijs netwerkverkeermetagegevens uit de infrastructuur van Microsoft, zoals bron-/doel-IP/poort, pakketgrootte en netwerkprotocol.
* Oplossingen van partners: verzamelt beveiligingswaarschuwingen van geïntegreerde partneroplossingen, zoals firewalls en antimalwareoplossingen. Deze gegevens worden opgeslagen in de Azure Security Center-opslag, die zich momenteel in de Verenigde Staten bevindt.
* Uw virtuele machines: Azure Security Center kan configuratie-informatie en informatie over beveiligingsgebeurtenissen verzamelen, zoals Windows-gebeurtenis- en auditlogboeken, IIS-logboeken, syslog-berichten en crashdumpbestanden van uw virtuele machines met behulp van gegevensverzamelingsagents. Zie het gedeelte 'Gegevensverzameling beheren' hieronder voor meer informatie.  

Bovendien wordt informatie over beveiligingswaarschuwingen, aanbevelingen en de status van de beveiliging opgeslagen in de Azure Security Center-opslag, die zich momenteel in de Verenigde Staten bevindt. Deze informatie omvat mogelijk gerelateerde configuratie-informatie en beveiligingsgebeurtenissen die indien nodig zijn verzameld van uw virtuele machines om u de beveiligingswaarschuwing, aanbeveling of de beveiligingsstatus door te geven.

## <a name="data-protection"></a>Gegevensbeveiliging
**Scheiding van gegevens**: gegevens worden op een logische manier apart van elkaar gehouden, in elk onderdeel van de service. Alle gegevens worden gemarkeerd per organisatie. Deze markering blijft aanwezig gedurende de levenscyclus van de gegevens en deze wordt afgedwongen op elke laag van de service. Bovendien worden gegevens die zijn verzameld van uw virtuele machines opgeslagen in uw opslagaccount(s).

**Gegevenstoegang**: om aanbevelingen voor beveiliging te bieden en mogelijke bedreigingen te onderzoeken, kunnen medewerkers van Microsoft toegang hebben tot gegevens die worden verzameld of geanalyseerd door de Azure-services, met inbegrip van crashdumpbestanden. Crashdumpbestanden en procesgebeurtenissen omvatten mogelijk per ongeluk klantgegevens of persoonlijke gegevens van uw virtuele machines. We voldoen aan de [voorwaarden voor Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en de [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), die stellen dat Microsoft niet de klantgegevens gebruikt of gegevens ervan afleidt voor reclame- of vergelijkbare commerciële doeleinden. We gebruiken klantgegevens alleen indien nodig om u Azure-services te bieden, met inbegrip van doeleinden die compatibel zijn met het leveren van die services. U behoudt alle rechten op de klantgegevens.

**Gegevensgebruik**: Microsoft gebruikt informatie over patronen en bedreigingen die worden gezien tussen meerdere tenants voor het verbeteren van onze mogelijkheden voor voorkoming en detectie; wij doen dit in overeenstemming met de privacyverplichtingen beschreven in onze [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

**Gegevenslocatie**: voor elke regio waarin de virtuele machines worden uitgevoerd, wordt een opslagaccount opgegeven. Hierdoor kunt u gegevens in dezelfde regio opslaan als de virtuele machine waarvan de gegevens worden verzameld. Deze gegevens, inclusief crashdumpbestanden, worden permanent opgeslagen in uw opslagaccount. De service slaat ook informatie op over beveiligingswaarschuwingen, inclusief waarschuwingen van geïntegreerde oplossingen van partners, aanbevelingen en de beveiligingsstatus, in de Azure Security Center-opslag, die zich momenteel in de Verenigde Staten bevindt.

## <a name="managing-data-collection-from-virtual-machines"></a>Gegevensverzameling van virtuele machines beheren
Wanneer u ervoor kiest om Azure Security Center in te schakelen, wordt gegevensverzameling ingeschakeld voor elk van uw abonnementen. U kunt gegevensverzameling uitschakelen in het gedeelte 'Beveiligingsbeleid' in het dashboard van Azure Security Center. Wanneer gegevensverzameling is ingeschakeld, levert Azure Security Center de Azure Monitoring Agent op alle bestaande virtuele machines en op nieuwe VM‘s die worden gemaakt. De Azure Security Monitoring-extensie scant op verschillende aan beveiliging gerelateerde configuraties en legt deze vast in [Event Tracing voor Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx)-traceringen (ETW). Bovendien worden door het besturingssysteem gebeurtenislogboekgebeurtenissen gegenereerd tijdens het uitvoeren van de machine. Voorbeelden van dergelijke gegevens zijn: besturingssysteemtype en -versie, besturingssysteemlogboeken (Windows-gebeurtenislogboeken), actieve processen, computernaam, IP-adressen, aangemelde gebruiker en tenant-ID. De Azure Monitoring Agent leest gebeurtenislogboekvermeldingen en ETW-traceringen en kopieert deze naar uw opslagaccount voor analyse. 

Voor elke regio waarin u virtuele machines uitvoert, wordt een opslagaccount opgegeven, waarin de gegevens die worden verzameld van virtuele machines in diezelfde regio worden opgeslagen. Hierdoor kunt u eenvoudig gegevens in hetzelfde geografische gebied bewaren met het oog op privacy en ten behoeve van de onafhankelijkheid van de gegevens. U kunt opslagaccounts configureren voor elke regio in het gedeelte 'Beveiligingsbeleid' in het dashboard van Azure Security Center.

De Azure Monitoring Agent kopieert ook crashdumpbestanden naar uw opslagaccount.  Azure Security Center verzamelt tijdelijke kopieën van uw crashdumpbestanden en analyseert deze op bewijs van pogingen tot misbruik en geslaagde aanvallen.  Azure Security Center voert deze analyse uit binnen hetzelfde geografische gebied als het opslagaccount en verwijdert de tijdelijke kopieën wanneer de analyse is voltooid.

U kunt op elk gewenst moment gegevensverzameling van virtuele machines uitschakelen. Alle Monitoring Agents die eerder door Azure Security Center zijn geïnstalleerd, worden dan verwijderd.

## <a name="see-also"></a>Zie ook
In dit document hebt u geleerd hoe gegevens worden beheerd en beveiligd in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

* [Plannings- en bedieningsgids voor het Azure Beveiligingscentrum](security-center-planning-and-operations-guide.md): leer de ontwerpoverwegingen kennen en plan hiervoor bij de overstap naar Azure Security Center.
* [Security health monitoring in Azure Security Center](security-center-monitoring.md) (Beveiligingsstatus controleren in Azure Security Center): meer informatie over het controleren van de status van uw Azure-resources
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure




<!--HONumber=Dec16_HO1-->


