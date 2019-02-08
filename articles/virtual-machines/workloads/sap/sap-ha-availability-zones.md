---
title: Configuraties van SAP-werkbelasting met Azure-Beschikbaarheidszones | Microsoft Docs
description: Architectuur voor hoge beschikbaarheid en scenario's voor SAP NetWeaver met behulp van Azure availability zones
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/03/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e319fb5bc1e0ed3354a09b685405656c7f3131bc
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55858802"
---
# <a name="sap-workload-configurations-with-azure-availability-zones"></a>Configuraties van SAP-werkbelasting met Azure-Beschikbaarheidszones

Een van de functies voor hoge beschikbaarheid Azure biedt is [Azure Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-overview). De algemene beschikbaarheid van SAP-workloads op Azure met beschikbaarheidszones worden verbeterd. Beschikbaarheidszones worden aangeboden in verschillende [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/) al. Meer regio's volgen. 

De basisarchitectuur van hoge beschikbaarheid van SAP kan worden beschreven, zoals weergegeven in deze afbeelding:

![Standard HA-configuratie](./media/sap-ha-availability-zones/standard-ha-config.png)

Het niveau van de SAP-toepassing wordt geïmplementeerd binnen één Azure [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Voor hoge beschikbaarheid van SAP Central Services, moet u twee virtuele machines in een afzonderlijke beschikbaarheidsset implementeren. Windows Failover Cluster-Services of Pacemaker (Linux) gebruiken als een framework voor hoge beschikbaarheid met automatische failover in het geval van infrastructuur- of softwareprobleem. Documenten met gedetailleerde informatie over deze implementaties worden hier weergegeven:

- [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van bestandsshare](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
- [Hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server voor SAP-toepassingen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)

Een soortgelijke architectuur geldt voor de DBMS-laag van SAP NetWeaver en S/4HANA of Hybris systemen. U implementeert de DBMS-laag in de modus actief/passief met een oplossing voor failoverclusters te beschermen tegen de infrastructuur- of softwarefout optreedt. De oplossing voor failoverclusters wordt mogelijk een van beide DBMS specifieke failover-framework, Windows Failover Cluster-Services of Pacemaker. 

Als u wilt implementeren met behulp van Azure availability zones dezelfde architectuur, zijn enkele wijzigingen aan de die hierboven worden beschreven architectuur nodig. Deze wijzigingen worden beschreven in dit document.

## <a name="considerations-deploying-across-availability-zones"></a>Overwegingen voor implementatie in meerdere Beschikbaarheidszones

Wanneer u Beschikbaarheidszones, zijn er enkele aandachtspunten voor:

- Azure-beschikbaarheidszones niet garanderen dat bepaalde afstand tussen de verschillende zones in een Azure-regio.
- Er zijn geen Azure-beschikbaarheidszones een ideale oplossing voor herstel na Noodgevallen. Natuurrampen kunnen leiden tot breed verdeeld schade in sommige regio's ter wereld, met inbegrip van zware schade aan de infrastructuur voor power. De afstand tussen de verschillende zones mogelijk niet groot genoeg zijn om in aanmerking komen als een goede DR-oplossing.
- De netwerklatentie voor verschillende beschikbaarheidszones varieert in verschillende Azure-regio's. Er zijn gevallen waarin u kunt implementeren en uitvoeren van de SAP-toepassingslaag in verschillende zones, als de netwerklatentie tussen één zone met de actieve DBMS-VM acceptabel uit een impact op het proces van het bedrijf is. Maar in sommige Azure-regio's de latentie tussen de actieve DBMS-VM en het SAP-toepassing-exemplaar, geïmplementeerd in verschillende zones kan worden te firewallverbinding en niet acceptabel voor de SAP-bedrijfsprocessen. Als gevolg hiervan moet de implementatiearchitectuur worden verschillende met een actief/actief-architectuur voor de toepassing of actief/passief-architectuur waarin meerdere zone te hoog is.
- Bepaal in welke configuratie u Azure-beschikbaarheidszones, op basis van de netwerklatentie die u tussen de verschillende zones meten kan gebruiken. De netwerklatentie speelt een belangrijke rol in twee gebieden:
    - De latentie tussen de twee DBMS-instanties die u beschikken over een synchrone replicatie moet, tot stand gebracht. Hoe hoger de netwerklatentie, hoe hoger de potentiële impact schaalbaarheid van uw workload
    - Het verschil in de netwerklatentie tussen een virtuele machine met een SAP dialoogvenster exemplaar in de zone met de actieve DBMS-instantie en een vergelijkbare virtuele machine in een andere zone. Een hogere dit verschil, hoe hoger de impact op de uitvoering van bedrijfsprocessen en batchtaken, afhankelijk van of ze worden uitgevoerd in de zone met de DBMS-systemen of in een andere zone.


Er zijn enkele beperkingen bij het implementeren van virtuele Azure-machines in verschillende beschikbaarheidszones en tot stand brengen van failover-oplossingen binnen dezelfde Azure-regio. Deze beperkingen worden behandeld in de volgende lijst:

- Met behulp van [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/) is verplicht voor het implementeren in Azure-beschikbaarheidszones 
- De toewijzing van opsommingen van de zone op de fysieke zones wordt vastgesteld op basis van de Azure-abonnement. Als u verschillende abonnementen voor het implementeren van uw SAP-systemen gebruikt, moet u de ideale zones voor elk abonnement definiëren.
- U kunt geen Azure-beschikbaarheidssets in een Azure-beschikbaarheidszone implementeren. Kies een Azure-beschikbaarheidszone of een Azure-beschikbaarheidsset instellen als framework van de implementatie voor virtuele machines.
- U kunt geen gebruiken een [Azure Basic Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) failover om clusteroplossingen te maken op basis van Windows Failover Cluster-Services of Linux-Pacemaker. In plaats daarvan moet u gebruiken de [Azure Standard Load Balancer-SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)



## <a name="ideal-availability-zones-combination"></a>Ideaal voor Beschikbaarheidszones combinatie
Voordat u hoe u kunt gebruikmaken van beschikbaarheidszones besluit, moet u een onderzoek waarmee u kunt uitvoeren:

- De netwerklatentie tussen de drie verschillende zones van een Azure-regio. Dus dat kunt u de zones met de minimale netwerklatentie in meerdere zone-netwerkverkeer
- Het verschil tussen VM-VM latentie binnen een van de zones die door u gekozen en de netwerklatentie tussen de twee zones die u hebt gekozen
- Een instructie of de virtuele machine bestandstypen die u wilt implementeren zijn beschikbaar in de twee zones van uw keuze. Met name met M-serie virtuele machines gaat u er omstandigheden zijn waarin de verschillende M-serie VM-SKU's gaat alleen beschikbaar in twee van de drie zones

### <a name="network-latency-between-zones-and-within-zone"></a>De netwerklatentie tussen zones en binnen een zone
Als u wilt weten wat de latentie tussen de verschillende zones is, moet u naar:

- Implementeer de VM-SKU u wilt gebruiken voor uw exemplaar DBMS in alle drie zones. Zorg ervoor dat [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) bij het uitvoeren van deze meting is ingeschakeld.
- Als u de twee zones met de minimale netwerklatentie kunt vinden, implementeert u de andere drie virtuele machines van de VM-SKU die u wilt gebruiken als toepassingslaag VM in de drie beschikbaarheidszones. De netwerklatentie meten op basis van de twee DBMS VMs in de twee verschillende 'DBMS-zones van uw keuze. 
- Als een hulpprogramma voor het meten van gebruik **niping**. Een hulpprogramma van SAP, die werkt zoals beschreven in de opmerkingen bij de SAP-ondersteuning [#500235](https://launchpad.support.sap.com/#/notes/500235) en [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Richt u op de opdrachten voor replicatielatentie gedocumenteerd SAP. Met behulp van **ping** wordt niet aanbevolen, aangezien **ping** werkt niet via de Azure code netwerkpaden versneld.

Op basis van de metingen en de beschikbaarheid van uw VM-SKU's in de beschikbaarheidszones, moet u de beslissingen te nemen:

- Definieer de ideale zones voor de DBMS-laag
- Antwoord van de vraag als u wilt distribueren van uw actieve SAP-toepassingslaag in één, twee of alle drie zones, op basis van verschillen van netwerk latentie in de zone versus in zones
- De vraag worden beantwoord als u een actief/passief of een actief/actief-configuratie uit het oogpunt van een toepassing implementeren wilt (Zie verderop)

Deze beslissingen ook aanbevelingen voor netwerken latentie van SAP intrekken zoals beschreven in de SAP-notitie [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E).

### <a name="be-aware-of"></a>Houd rekening met

> [!IMPORTANT]
> De metingen en besluiten die u neemt, zijn geldig voor het Azure-abonnement u gebruikt deze metingen maken. Als u een andere Azure-abonnement, moet u Herhaal de metingen, omdat de toewijzing van vaste zones voor een andere Azure-abonnement verschillen kan.


> [!IMPORTANT]
> Het is waarschijnlijk dat de metingen als hierboven uitgevoerd verschillende resultaten worden weergegeven in elke Azure-regio die ondersteuning biedt voor [beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview). Zelfs als uw netwerk latentievereisten dezelfde zijn, moet u mogelijk vast te stellen van verschillende implementatiestrategieën in verschillende Azure-regio's, omdat de netwerklatentie tussen zones kan afwijken. Verwacht wordt dat in sommige Azure-regio's, de netwerklatentie tussen de drie verschillende zones aanzienlijk afwijken kan. Terwijl de netwerklatentie tussen de drie verschillende zones in andere regio's, meer uniform is. De claim er **altijd** is een netwerklatentie voor verschillende gebieden van 1 milliseconde op 2 milliseconden **verkeerde**. De netwerklatentie voor verschillende beschikbaarheidszones in Azure-regio's kan niet worden gebruikt.


## <a name="activeactive-deployment"></a>Actief/actief-implementatie
Deze implementatiearchitectuur heet actief/actief, omdat u uw actieve exemplaren van de SAP-dialoogvenster in twee of drie zones implementeren. De SAP Central Services met behulp van de replicatie in de wachtrij plaatsen gaat tussen de twee zones worden geïmplementeerd. Hetzelfde geldt voor de DBMS-laag, die wordt geïmplementeerd in de dezelfde zones als de SAP centrale-Service.

Als u wilt een dergelijke configuratie, bekendmaken, moet u de twee beschikbaarheidszones niet vinden in de regio die meerdere zone netwerklatentie die aanvaardbaar is voor uw workload en uw synchrone replicatie voor de DBMS-systemen bieden. Daarnaast wilt u de verschillen tussen de netwerklatentie binnen de zones die u hebt geselecteerd en de in de zone netwerklatentie niet te groot wordt. Reden voor de laatste, is dat u niet te grote verschillen in uitvoeringstijden van uw bedrijfsprocessen wilt of batch-taken, afhankelijk van of een taak wordt uitgevoerd in de zone met de DBMS-server of in de zone. Enkele variaties worden geaccepteerd, maar niet factoren van verschil.

Een vereenvoudigde schema van de implementatie van een actief/actief in twee zones kan er als volgt uitzien:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

De volgende overwegingen zijn van toepassing voor deze configuratie:

- U behandelen de Azure-beschikbaarheidszones als fout- en updatedomeinen voor alle virtuele machines omdat beschikbaarheidssets kunnen niet worden geïmplementeerd in Azure-beschikbaarheidszones
- De Azure-load balancers die u gebruikt voor de failover-clusters van de SAP Central Services en de DBMS-laag, moet de [standaard-SKU load balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). De basisversie van load balancer werkt niet in zones
- De Azure-netwerk en de subnetten die u hebt geïmplementeerd voor het hosten van de SAP-systeem worden uitgebreid in zones. U **hoeft geen** afzonderlijke virtuele netwerken voor elke zone
- Gebruik voor alle virtuele machines die u implementeert, [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/). Niet-beheerde schijven worden niet ondersteund voor zonegebonden implementaties
- Azure Premium storage of [Ultra SSD-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) worden niet ondersteund door elk type opslagreplicatie in zones. Als gevolg hiervan is de verantwoordelijkheid van de toepassing (DBMS-systemen of SAP Central Services) om belangrijke gegevens te repliceren
- Hetzelfde geldt voor de gedeelde sapmnt-map een gedeelde schijf (Windows), een CIFS-share (Windows is) of NFS-share (Linux). U moet een technologie die worden gerepliceerd die een gedeelde schijf of delen tussen de zones te gebruiken. Op het moment dat worden de volgende technologieën ondersteund:
    - Voor Windows, een oplossing die gebruikmaakt van SIOS Datakeeper zoals beschreven in [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) wordt ondersteund in zones
    - Voor SUSE Linux, een NFS-share die is ingebouwd, zoals beschreven in [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) wordt ondersteund
    - Op dit moment de oplossing met behulp van services van Windows-scale-out bestandsserver (SOFS) zoals beschreven in [voorbereiden Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor instanties van SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **wordt niet ondersteund in zones**
- De derde zone wordt gebruikt voor het hosten van het apparaat SBD in het geval u bouwt een [SUSE Linux pacemaker cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) of exemplaren van een extra toepassing
- Als u wilt bereiken consistentie van de uitvoeringstijd voor essentiële bedrijfsprocessen, kunt u proberen om te leiden van bepaalde batch-taken en exemplaren van een toepassing die in de zone zijn gebruikers met het actieve exemplaar DBMS-systemen met behulp van SAP-Batch-servergroepen, aanmeldingsgroepen of RFC-groepen. In het geval van een zonegebonden failover moet u echter deze groepen handmatig verplaatsen naar exemplaren die worden uitgevoerd op virtuele machines in de zone met de actieve DB-virtuele machine.  
- Beslissen of u implementeren de exemplaren van sommige inactieve dialoogvenster in elk van de zones wilt kunnen onmiddellijk gebruikt om toegang te krijgen tot de voormalige resourcecapaciteit als een zone die worden gebruikt door een deel van de exemplaren van een toepassing buiten gebruik gesteld


## <a name="activepassive-deployment"></a>Actief/passief-implementatie
Als u een acceptabele verschillen tussen de netwerklatentie binnen een zone en meerdere zone netwerkverkeer niet kunt vinden, heeft de architectuur die u kunt implementeren in een actief/passief-teken uit het oogpunt van SAP-toepassing-laag. U definieert een 'active' zone, de zone is waarin u het niveau van de volledige toepassing implementeren en wanneer u probeert uit te voeren van zowel de actieve DBMS-systemen en SAP Central Services-exemplaar. Met deze configuratie kunt ervoor u zorgen u geen extreme uitvoeringstijd variaties in zakelijke transacties en batchtaken, afhankelijk van of een taak wordt uitgevoerd in de zone met de actieve sessie van de DBMS-systemen of niet.

De basisindeling van dergelijke een architectuur lijkt:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

De volgende overwegingen zijn van toepassing voor deze configuratie:

- Beschikbaarheidssets kunnen niet worden geïmplementeerd in Azure-beschikbaarheidszones. Daarom kan in dit geval hebt u een update- en foutdomeinen domein voor uw toepassingslaag. Reden is dat alleen wordt geïmplementeerd in een zone. Deze configuratie is een lichte tegenslag, vergeleken met de referentiearchitectuur die voorziet in dat u het niveau van de toepassing in een Azure-beschikbaarheidsset implementeert.
- Deze architectuur van een besturingssysteem, moet u om te controleren en probeer de actieve DBMS-systemen en SAP Central services-exemplaren dat in dezelfde regio bevindt als het niveau van uw geïmplementeerde toepassing. Bij een failover van SAP centrale Service of de DBMS-instantie die u wilt om ervoor te zorgen dat u handmatig terug in de zone met de SAP-toepassingslaag zo vroeg mogelijk geïmplementeerd een kunt
- De Azure-load balancers die u gebruikt voor de failover-clusters van de SAP Central Services en de DBMS-laag, moet de [standaard-SKU load balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). De basisversie van load balancer werkt niet in zones
- De Azure-netwerk en de subnetten die u hebt geïmplementeerd voor het hosten van de SAP-systeem worden uitgebreid in zones. U **hoeft geen** afzonderlijke virtuele netwerken voor elke zone
- Voor alle virtuele machines die u implementeert, moet u gebruiken [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/). Niet-beheerde schijven worden niet ondersteund voor zonegebonden implementaties
- Azure Premium storage of [Ultra SSD-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) worden niet ondersteund door elk type opslagreplicatie in zones. Als gevolg hiervan is de verantwoordelijkheid van de toepassing (DBMS-systemen of SAP Central Services) om belangrijke gegevens te repliceren
- Hetzelfde geldt voor de gedeelde sapmnt-map een gedeelde schijf (Windows), een CIFS-share (Windows is) of NFS-share (Linux). U moet een technologie die worden gerepliceerd die een gedeelde schijf of delen tussen de zones te gebruiken. Op het moment dat worden de volgende technologieën ondersteund:
    - Voor Windows, een oplossing die gebruikmaakt van SIOS Datakeeper zoals beschreven in [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) wordt ondersteund in zones
    - Voor SUSE Linux, een NFS-share die is ingebouwd, zoals beschreven in [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) wordt ondersteund
    - Op dit moment de oplossing met behulp van services van Windows-scale-out bestandsserver (SOFS) zoals beschreven in [voorbereiden Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor instanties van SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **wordt niet ondersteund in zones**
- De derde zone wordt gebruikt voor het hosten van het apparaat SBD in het geval u bouwt een [SUSE Linux pacemaker cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) of exemplaren van een extra toepassing
- Inactieve VM's in de passieve zone (vanuit het oogpunt van een DBMS) implementeren om te kunnen starten van de toepassingsresources in het geval van een zone-fout
    - U kunt geen gebruiken [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) actieve virtuele machines repliceren naar inactieve VM's tussen zones. Op dit moment, kan Azure Site Recovery niet voldoen aan een dergelijke functie
- Investeren in automation waarmee u, in het geval van een zone-fout optreedt, op het niveau van de SAP-toepassing automatisch te starten in de tweede zone

## <a name="combined-high-availability-and-disaster-recovery-configuration"></a>Gecombineerde hoge beschikbaarheid en configuratie voor herstel na noodgevallen
Microsoft deelt geen informatie over de geografische afstand tussen de faciliteiten niet dat beschikbaarheid van andere Azure-zones hosten in Azure-regio. Ondanks deze feit, sommige klanten zijn gebruik te maken van zones voor een gecombineerde HA en DR-configuratie die garandeert een **R**failoverstatus **P**pnieuw **O**bjective (RPO) van nul. Wat betekent dat, u moet gaan niet verloren databasetransacties die zijn doorgevoerd zelfs in het geval van de herstel na noodgevallen. 

> [!NOTE]
> Een configuratie als volgt wordt aanbevolen voor alleen specifieke omstandigheden. Bijvoorbeeld, gevallen waar de gegevens de Azure-regio vanwege redenen voor beveiliging en naleving niet verlaten. 

Een voorbeeld van hoe een dergelijke configuratie, zoals bekijken kan wordt geïllustreerd in deze afbeelding:

![combined_ha_dr_in_zones](./media/sap-ha-availability-zones/combined_ha_dr_in_zones.png)

De volgende overwegingen zijn van toepassing voor deze configuratie:

- U bent een ervan uitgaande dat er een aanzienlijke afstand tussen de faciliteiten die als host fungeert voor een beschikbaarheidszone. Of u moest houden aan bepaalde Azure-regio. Beschikbaarheidssets kunnen niet worden geïmplementeerd in Azure-beschikbaarheidszones. Daarom kan in dit geval zijn u loopt met één domein voor update- en foutdomeinen voor het niveau van uw toepassing. Reden is dat alleen wordt geïmplementeerd in een zone. Dit is een tegenslag vergeleken met de referentiearchitectuur die voorziet in dat u het niveau van de toepassing in een Azure-beschikbaarheidsset implementeert.
- Deze architectuur van een besturingssysteem, moet u om te controleren en probeer de actieve DBMS-systemen en SAP Central services-exemplaren dat in dezelfde regio bevindt als het niveau van uw geïmplementeerde toepassing. Bij een failover van SAP centrale Service of de DBMS-instantie die u wilt om ervoor te zorgen dat u handmatig terug in de zone met de SAP-toepassingslaag zo vroeg mogelijk geïmplementeerd een kunt
- Hebt u exemplaren van de productie-toepassing vooraf is geïnstalleerd in de VM's waarop de actieve QA-toepassingsexemplaren.
- Bij een storing zonegebonden, de toepassingsexemplaren QA afsluiten en start de productie-exemplaren in plaats daarvan. Houd er rekening mee dat u wilt werken met virtuele namen voor de exemplaren van een toepassing om deze taak te maken
- De Azure-load balancers die u gebruikt voor de failover-clusters van de SAP Central Services en de DBMS-laag, moet de [standaard-SKU load balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). De basisversie van load balancer werkt niet in zones
- De Azure-netwerk en de subnetten die u hebt geïmplementeerd voor het hosten van de SAP-systeem worden uitgebreid in zones. U **hoeft geen** afzonderlijke virtuele netwerken voor elke zone
- Voor alle virtuele machines die u implementeert, moet u gebruiken [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/). Niet-beheerde schijven worden niet ondersteund voor zonegebonden implementaties
- Azure Premium storage of [Ultra SSD-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) worden niet ondersteund door elk type opslagreplicatie in zones. Als gevolg hiervan is de verantwoordelijkheid van de toepassing (DBMS-systemen of SAP Central Services) om belangrijke gegevens te repliceren
- Hetzelfde geldt voor de gedeelde sapmnt-map een gedeelde schijf (Windows), een CIFS-share (Windows is) of NFS-share (Linux). U moet een technologie die worden gerepliceerd die een gedeelde schijf of delen tussen de zones te gebruiken. Op het moment dat worden de volgende technologieën ondersteund:
    - Voor Windows, een oplossing die gebruikmaakt van SIOS Datakeeper zoals beschreven in [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) wordt ondersteund in zones
    - Voor SUSE Linux, een NFS-share die is ingebouwd, zoals beschreven in [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) wordt ondersteund
    - Op dit moment, de oplossing met behulp van services van Windows-scale-out bestandsserver (SOFS) zoals beschreven in [voorbereiden Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor instanties van SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **wordt niet ondersteund in zones**
- De derde zone wordt gebruikt voor het hosten van het apparaat SBD in het geval u bouwt een [SUSE Linux pacemaker cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) of exemplaren van een extra toepassing





## <a name="next-steps"></a>Volgende stappen
Controleer de volgende stappen om te implementeren in Azure availability zones:

- [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor SAP ASCS/SCS-instanties voorbereiden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)






