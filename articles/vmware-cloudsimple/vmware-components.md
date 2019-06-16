---
title: Azure VMware-oplossing door CloudSimple - privécloud VMware-onderdelen
description: Hierin wordt beschreven hoe VMware-onderdelen worden geïnstalleerd op de privécloud
author: sharaths-cs
ms.author: dikamath
ms.date: 04/30/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5e6548a5a04e32b374a8a9c29d2ca5f89fd65c78
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65160183"
---
# <a name="private-cloud-vmware-components"></a>Privécloud VMware-onderdelen

Een privécloud is een geïsoleerde VMware-stack (ESXi-hosts, vCenter, vSAN en NSX) omgeving die wordt beheerd door een vCenter-server in een domein management.  De service CloudSimple kunt u systeemeigen implementatie in VMware op Azure bare metal-infrastructuur in Azure-locaties.  Persoonlijke clouds zijn geïntegreerd met de rest van de Azure-Cloud.  Een privécloud wordt geïmplementeerd met de volgende onderdelen van de VMware-stack:

* **VMware ESXi -** Hypervisor op Azure toegewezen knooppunten
* **VMware vCenter -** apparaat voor gecentraliseerd beheer van vSphere privécloudomgeving
* **VMware virtuele SAN -** Hyper-geconvergeerde infrastructuuroplossing
* **VMware NSX Data Center -** netwerk virtualisatie en beveiligingssoftware  

## <a name="vmware-component-versions"></a>Versies van VMware-onderdeel

De VMware-stack van een privécloud wordt geïmplementeerd met de versie van de volgende software.

| Onderdeel | Version | Versie met licentie |
|-----------|---------|------------------|
| ESXi | 6.7U1 | Enterprise Plus |
| vCenter | 6.7U1 | vCenter Standard |
| vSAN | 6.7 | Zakelijk |
| NSX Data Center | 2.3 | Geavanceerd |

## <a name="esxi"></a>ESXi

VMware ESXi is geïnstalleerd op de aangeschafte CloudSimple knooppunten bij het maken van een privécloud.  De hypervisor biedt ESXi voor het implementeren van de werkbelasting van virtuele machines (VM's).  Knooppunten vindt u hyper-geconvergeerde infrastructuur (berekeningen en opslag) van uw privécloud.  De knooppunten zijn onderdeel van de vSphere-cluster in de privécloud.  Elk knooppunt heeft vier fysieke netwerkinterfaces met aan de basis liggen netwerk verbonden.  Twee fysieke netwerkinterfaces worden gebruikt voor het maken van een **vSphere gedistribueerde overschakelen (VDS)** op vCenter en twee gegevensbronnen worden gebruikt voor het maken een **NSX beheerde virtuele gedistribueerde switch (N-VDS)** .  Netwerkinterfaces worden geconfigureerd in de modus actief-actief voor hoge beschikbaarheid.

Meer informatie over VMware ESXi

## <a name="vcenter-server-appliance"></a>vCenter server-apparaat

vCenter server appliance (VCSA) biedt de functies voor verificatie, beheer en indeling voor VMware-oplossing door CloudSimple. VCSA met ingesloten Platform Services Controller (PSC) wordt geïmplementeerd bij het maken van uw privécloud.  VCSA wordt geïmplementeerd op de vSphere-cluster dat wordt gemaakt wanneer u uw persoonlijke cloud implementeren.  Elke privécloud heeft een eigen VCSA.  Uitbreiding van een privécloud worden de knooppunten toegevoegd aan de VCSA in de privécloud.

### <a name="vcenter-single-sign-on"></a>vCenter eenmalige aanmelding

Ingesloten Platform Services Controller op VCSA is gekoppeld aan een **vCenter Single Sign-On domein**.  De domeinnaam is **cloudsimple.local**.  Een standaardgebruiker **CloudOwner@cloudsimple.com** voor toegang tot vCenter wordt gemaakt.  U kunt uw on-premises/Azure active directory toevoegen [identiteit bronnen voor vCenter](https://docs.azure.cloudsimple.com/set-vcenter-identity/).

## <a name="vsan-storage"></a>virtueel SAN-opslag

Privéclouds worden gemaakt met een volledig geconfigureerde all-flash-virtueel SAN-opslag, lokaal op het cluster.  Ten minste drie knooppunten van de dezelfde SKU moet een vSphere-cluster maken met vSAN gegevensopslag.  Deduplicatie en compressie zijn standaard ingeschakeld in de gegevensopslag virtueel SAN.  Twee schijfgroepen worden gemaakt op elk knooppunt van het cluster vSphere. Elke schijfgroep bevat één cacheschijf en drie schijven van de capaciteit.

Een standaardbeleid virtueel SAN-opslag is gemaakt op de vSphere-cluster en toegepast op het gegevensarchief virtueel SAN.  Dit beleid bepaalt hoe de VM-opslag-objecten zijn ingericht en toegewezen binnen het gegevensarchief naar het vereiste serviceniveau garanderen.  Het beleid voor opslag definieert de **te tolereren (FTT) fouten** en de **fouttolerantie methode**.  U kunt nieuwe beleid voor opslag maken en deze toepassen op de virtuele machines. Als u wilt behouden SLA, moet 25% reservecapaciteit in de gegevensopslag vSAN worden onderhouden.  

### <a name="default-vsan-storage-policy"></a>Standaard virtueel SAN-beleid voor opslag

De onderstaande tabel ziet u de beleidsparameters virtueel SAN-opslag.

| Aantal knooppunten in Cluster vSphere | FTT | Fouttolerantie methode |
|------------------------------------|-----|--------------------------|
| 3 en 4 knooppunten | 1 | RAID 1 (mirroring) - 2 exemplaren wordt gemaakt |
| 5 tot en met 16 knooppunten | 2 | RAID 1 (mirroring) - 3 kopieën gemaakt |

## <a name="nsx-data-center"></a>NSX Data Center

NSX Data Center biedt netwerkvirtualisatie, micro-Segmentatie en netwerk-beveiligingsmogelijkheden in uw privécloud.  U kunt alle services die worden ondersteund door NSX Data Center in uw privécloud via NSX configureren.  Wanneer u een privécloud maakt, worden de volgende NSX-onderdelen geïnstalleerd en geconfigureerd.

* NSXT Manager
* Transport Zones
* Host- en Edge Uplinkprofiel
* Logische Switch voor het Transport van Edge en Ext1 Ext2
* IP-adresgroep voor het Transport van de ESXi-knooppunt
* IP-adresgroep voor Transport-Edge-knooppunt
* Edge-knooppunten
* DRS anti-affinity regel voor controller en Edge-VM 's
* Tier 0 Router
* BGP op Tier0 Router inschakelen

## <a name="vsphere-cluster"></a>vSphere-cluster

ESXi-hosts worden geconfigureerd als een cluster om hoge beschikbaarheid van de privécloud.  Wanneer u een privécloud maakt, worden de onderdelen van vSphere op het eerste cluster geïmplementeerd.  Een resourcegroep is gemaakt voor de onderdelen en alle beheer-VM's in deze resourcegroep zijn geïmplementeerd. Het eerste cluster kan niet worden verwijderd als u wilt verkleinen van de privécloud.  vSphere-cluster biedt maximale beschikbaarheid voor virtuele machines met behulp van **vSphere HA**.  Te tolereren fouten zijn gebaseerd op het aantal beschikbare knooppunten in het cluster.  U kunt de formule ```Number of nodes = 2N+1``` waar ```N``` is het aantal te tolereren fouten.

### <a name="vsphere-cluster-limits"></a>limieten voor vSphere-cluster

| Resource | Limiet |
|----------|-------|
| Minimum aantal knooppunten om een privécloud te maken (eerste vSphere cluster) | 3 |
| Maximum aantal knooppunten in een vSphere-Cluster in een privécloud | 16 |
| Maximum aantal knooppunten in een privécloud | 64 |
| Maximum aantal vSphere-Clusters in een privécloud | 21 |
| Minimum aantal knooppunten op een nieuwe vSphere-Cluster | 3 |

## <a name="vmware-infrastructure-maintenance"></a>Onderhoud van de VMware-infrastructuur

Het is soms nodig wijzigingen aanbrengen in de configuratie van de VMware-infrastructuur. Op dit moment wordt deze intervallen kunnen zich voordoen na 1-2 maanden, maar wordt verwacht dat de frequentie weigeren na verloop van tijd. Dit type onderhoud kan doorgaans worden uitgevoerd zonder dat normale verbruik van de services CloudSimple wordt onderbroken. De volgende services blijven werken zonder enige gevolgen tijdens een interval van het onderhoud VMware:

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

* [CloudSimple onderhoud en updates](cloudsimple-maintenance-updates.md)