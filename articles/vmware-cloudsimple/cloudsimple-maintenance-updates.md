---
title: Azure VMware-oplossing door CloudSimple - CloudSimple onderhoud en updates
description: Beschrijft het proces van de service CloudSimple voor gepland onderhoud en updates
author: sharaths-cs
ms.author: dikamath
ms.date: 04/30/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4dde358f10e9ac5054297ff68a0971404c0dc135
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65160243"
---
# <a name="cloudsimple-maintenance-and-updates"></a>CloudSimple onderhoud en updates

De privécloud-omgeving is ontworpen om u te hebben geen één storingspunt is:

* ESXi-clusters zijn geconfigureerd met hoge beschikbaarheid van vSphere. De clusters worden aangepast om ten minste één extra knooppunt voor tolerantie.
* Redundante primaire opslag wordt verstrekt door virtuele SAN, waarvoor is vereist ten minste drie knooppunten voor beveiliging tegen een storing. virtueel SAN kan worden geconfigureerd om een hogere tolerantie voor grotere clusters.
* vCenter, PSC-server en NSX Manager virtuele machines zijn geconfigureerd met RAID 10-beleid voor opslag om te beveiligen tegen fout bij de opslag. De virtuele machines worden beschermd tegen storingen in knooppunt/netwerk door vSphere HA.
* ESXi-hosts hebben redundante fans en NIC's.
* TOR- en -spine switches worden geconfigureerd in de HA-paren over tolerantie.

CloudSimple continu controleert de volgende virtuele machines voor actieve tijdsduur en de beschikbaarheid en biedt een beschikbaarheid van SLA's:

* ESXi-hosts
* vCenter
* PSC
* NSX Manager

CloudSimple bewaakt ook de volgende continu voor fouten:

* Harde schijven
* Fysieke NIC-poorten
* Servers
* Fans
* Energiebeheer
* Switches
* Switchpoorten

Als een schijf of een knooppunt mislukt, wordt een nieuw knooppunt automatisch toegevoegd aan de betreffende VMware-cluster onmiddellijk terug naar de status te brengen.

Back-ups van CloudSimple, onderhoudt en werkt deze VMware-elementen in het persoonlijke clouds:

* ESXi
* vCenter Platform Services
* Controller
* vSAN
* NSX

## <a name="back-up-and-restore"></a>Back-up en herstellen

Back-up CloudSimple bevat:

* Elke nacht incrementele back-ups van vCenter, PSC-server en DVS regels.
* Gebruik van vCenter systeemeigen API's om de back-up van onderdelen op het niveau van de toepassing.
* Automatische back-up voordat u een update of upgrade van de software voor het beheer van VMware.
* De versleuteling van de gegevens in de bron, door een vCenter, voordat de overdracht van gegevens via een versleuteld kanaal TLS1.2 aan Azure. De gegevens worden opgeslagen in een Azure-blob waar deze worden gerepliceerd tussen regio's.

U kunt een herstelpunt aanvragen door het openen van een [ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="maintenance"></a>Onderhoud

CloudSimple heeft verschillende typen gepland onderhoud.

### <a name="backendinternal-maintenance"></a>Back-end/intern onderhoud

Dit onderhoud omvat doorgaans opnieuw configureren van fysieke activa of installeren van softwarepatches. Dit heeft geen invloed op normale verbruik van de activa wordt onderhouden. Met redundante NIC's gaan met elke fysieke rack, worden niet normale netwerkverkeer en privécloud bewerkingen beïnvloed. Alleen als uw organisatie wordt verwacht dat de volledige redundante bandbreedte gebruiken tijdens het onderhoud interval valt u wellicht een prestatie-impact op.

### <a name="cloudsimple-portal-maintenance"></a>CloudSimple portal onderhoud

Enige uitvaltijd beperkt service is vereist wanneer de controlelaag CloudSimple of infrastructuur wordt bijgewerkt. Op dit moment kunnen onderhoud intervallen zijn zo vaak als één keer per maand. De frequentie wordt verwacht na verloop van tijd weigeren. CloudSimple biedt melding voor het onderhoud van de portal en het interval zo kort mogelijk blijft. De volgende services blijven werken zonder enige gevolgen tijdens een interval van de portal onderhoud:

* VMware-beheerlaag en de toepassingen
* vCenter-toegang
* Alle netwerken en opslag
* Alle Azure-verkeer

### <a name="vmware-infrastructure-maintenance"></a>Onderhoud van de VMware-infrastructuur

Het is soms nodig wijzigingen aanbrengen in de configuratie van de VMware-infrastructuur.  Op dit moment wordt deze intervallen kunnen zich voordoen na 1-2 maanden, maar wordt verwacht dat de frequentie weigeren na verloop van tijd. Dit type onderhoud kan doorgaans worden uitgevoerd zonder dat normale verbruik van de services CloudSimple wordt onderbroken. De volgende services blijven werken zonder enige gevolgen tijdens een interval van het onderhoud VMware:

* VMware-beheerlaag en de toepassingen
* vCenter-toegang
* Alle netwerken en opslag
* Alle Azure-verkeer

## <a name="updates-and-upgrades"></a>Updates en Upgrades

CloudSimple is verantwoordelijk voor het beheer van de levenscyclus van VMware-software (ESXi, vCenter, PSC-server en NSX) in de privécloud.

Software-updates zijn onder andere:

* **Patches**. Beveiligingspatches of oplossingen voor problemen die zijn uitgebracht met VMware.
* **Updates**. Secundaire versie is gewijzigd van een VMware-stack-onderdeel.
* **Upgrades**. Het wijzigen van de primaire versie van een VMware-stack-onderdeel.

CloudSimple test een essentiële beveiligingspatch zodra deze beschikbaar van VMware. Per SLA zichtbare CloudSimple de beveiligingspatch voor privécloudomgevingen binnen een week.

CloudSimple biedt elk kwartaal onderhoudsupdates voor softwareonderdelen van VMware. Wanneer een nieuwe primaire versie van VMware-software beschikbaar is, wordt de CloudSimple werkt met klanten voor de coördinatie van een geschikte onderhoudsvenster voor een upgrade.

## <a name="next-steps"></a>Volgende stappen

[Maak een back-up van werkbelasting van virtuele machines met behulp van Veeam](https://docs.azure.cloudsimple.com/backup-workloads-veeam/).