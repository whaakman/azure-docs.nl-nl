---
title: Azure VMware-oplossing door CloudSimple-CloudSimple onderhoud en updates
description: Beschrijft het CloudSimple-service proces voor gepland onderhoud en updates
author: sharaths-cs
ms.author: dikamath
ms.date: 04/30/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5d6eeecbecc89995c25e687cc6808ed3b0c5dc5c
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816215"
---
# <a name="cloudsimple-maintenance-and-updates"></a>Onderhoud en updates voor CloudSimple

De privécloud is zodanig ontworpen dat deze geen Single Point of Failure heeft:

* ESXi-clusters worden geconfigureerd met een hoge Beschik baarheid van vSphere. De clusters hebben een grootte van ten minste één reserve knooppunt voor tolerantie.
* Redundante primaire opslag wordt geleverd door vSAN, waarvoor ten minste drie knoop punten zijn vereist om beveiliging tegen één storing te bieden. vSAN kan worden geconfigureerd om hogere toleranties te bieden voor grotere clusters.
* de virtuele machines van vCenter, PSC en NSX Manager zijn geconfigureerd met RAID-10-opslag beleid om te beschermen tegen opslag storingen. De virtuele machines worden beschermd tegen knoop punten/netwerk fouten door vSphere HA.
* ESXi-hosts hebben redundante ventilatoren en Nic's.
* TOR-en spin-switches worden in HA-paren geconfigureerd om tolerantie te bieden.

CloudSimple bewaakt voortdurend de volgende Vm's voor de uptime en beschik baarheid en biedt Beschik baarheid van service overeenkomsten:

* ESXi-hosts
* vCenter
* PSC
* NSX Manager

CloudSimple controleert ook de volgende taken continu voor fouten:

* Harde schijven
* Fysieke NIC-poorten
* Servers
* Fans
* /Uitschakelaar
* Focus
* Switch poorten

Als een schijf of knoop punt uitvalt, wordt er automatisch een nieuw knoop punt aan het betrokken VMware-cluster toegevoegd om de status onmiddellijk weer te geven.

CloudSimple maakt een back-up, onderhoudt en werkt deze VMware-elementen bij in de privécloud:

* ESXi
* vCenter-platform Services
* Controller
* vSAN
* NSX

## <a name="back-up-and-restore"></a>Back-ups maken en herstellen

CloudSimple back-up omvat:

* Nacht incrementele back-ups van vCenter-, PSC-en DVS-regels.
* Het gebruik van systeem eigen vCenter-Api's voor het maken van back-ups van onderdelen in de toepassingslaag.
* Automatische back-up vóór een update of upgrade van de VMware-beheer software.
* Gegevens versleuteling op de bron, door vCenter, vóór gegevens overdracht via een met TLS 1.2 versleuteld kanaal naar Azure. De gegevens worden opgeslagen in een Azure-Blob waar ze worden gerepliceerd tussen regio's.

U kunt een terugzet bewerking aanvragen door een [ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)te openen.

## <a name="maintenance"></a>Onderhoud

CloudSimple voert diverse soorten gepland onderhoud uit.

### <a name="backendinternal-maintenance"></a>Back-end/intern onderhoud

Dit onderhoud omvat doorgaans het opnieuw configureren van fysieke assets of het installeren van software patches. Dit heeft geen invloed op het normale verbruik van de activa die worden onderhouden. Met redundante Nic's die aan elk fysiek rack worden gekoppeld, worden normale netwerk verkeer en privé-Cloud bewerkingen niet beïnvloed. U kunt alleen invloed hebben op de prestaties als uw organisatie verwacht dat de volledige redundante band breedte tijdens het onderhouds interval wordt gebruikt.

### <a name="cloudsimple-portal-maintenance"></a>Onderhoud van de CloudSimple-Portal

Een bepaalde downtime van de service is vereist wanneer het CloudSimple-besturings vlak of de infra structuur wordt bijgewerkt. Op dit moment kunnen onderhouds intervallen even frequent per maand zijn. De frequentie wordt naar verwachting in de loop van de tijd geweigerd. CloudSimple biedt een melding voor portal onderhoud en houdt het interval zo kort mogelijk. Tijdens een portal-onderhouds interval blijven de volgende services werken zonder dat dit van invloed is:

* VMware-beheer vlak en-toepassingen
* toegang tot vCenter
* Alle netwerken en opslag
* Al het Azure-verkeer

### <a name="vmware-infrastructure-maintenance"></a>VMware-infrastructuur onderhoud

Het is nu af en toe nood zakelijk om wijzigingen aan te brengen in de configuratie van de VMware-infra structuur.  Op dit moment kunnen deze intervallen elke 1-2 maanden optreden, maar wordt de frequentie naar verwachting in de loop van de tijd geweigerd. Dit type onderhoud kan meestal worden uitgevoerd zonder het normale gebruik van de CloudSimple-services te onderbreken. Tijdens een VMware-onderhouds interval blijven de volgende services functioneren zonder enige impact:

* VMware-beheer vlak en-toepassingen
* toegang tot vCenter
* Alle netwerken en opslag
* Al het Azure-verkeer

## <a name="updates-and-upgrades"></a>Updates en upgrades

CloudSimple is verantwoordelijk voor het levenscyclus beheer van VMware-software (ESXi, vCenter, PSC en NSX) in de privécloud.

Software-updates zijn onder andere:

* **Patches**. Beveiligings patches of oplossingen voor oplossingen die worden vrijgegeven door VMware.
* **Updates**. Wijziging van de secundaire versie van een VMware-stack onderdeel.
* **Upgrades**. Wijziging van de hoofd versie van een VMware-stack onderdeel.

CloudSimple test een kritieke beveiligings patch zodra deze beschikbaar is in VMware. Per SLA implementeert CloudSimple de beveiligings patch naar privécloud binnen een week.

CloudSimple biedt updates voor het driemaandelijkse onderhoud aan VMware-software onderdelen. Wanneer er een nieuwe belang rijke versie van VMware-software beschikbaar is, werkt CloudSimple samen met klanten om een geschikt onderhouds venster voor de upgrade te coördineren.

## <a name="next-steps"></a>Volgende stappen

[Maak een back-up van werkbelasting machines met Veeam](https://docs.azure.cloudsimple.com/backup-workloads-veeam/).