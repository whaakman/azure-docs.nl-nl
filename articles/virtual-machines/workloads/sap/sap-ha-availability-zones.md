---
title: Configuraties van SAP-werkbelasting met Azure-Beschikbaarheidszones | Microsoft Docs
description: Architectuur voor hoge beschikbaarheid en scenario's voor SAP NetWeaver met behulp van Azure availability zones
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: juergent
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
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56c1ffd314a9a8e9440832b9fd92a51cdaf9f228
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735653"
---
# <a name="sap-workload-configurations-with-azure-availability-zones"></a>Configuraties van SAP-werkbelasting met Azure-Beschikbaarheidszones

Een van de functies van hoge beschikbaarheid voor het verbeteren van de algemene beschikbaarheid van SAP-workloads op Azure, biedt Azure [Azure Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-overview). Beschikbaarheidszones zijn beschikbaar in verschillende [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/) al. Meer regio's volgen. 

De basisarchitectuur van hoge beschikbaarheid van SAP kan worden beschreven, zoals weergegeven in deze afbeelding:

![Standard HA-configuratie](./media/sap-ha-availability-zones/standard-ha-config.png)

Het niveau van de SAP-toepassing wordt geïmplementeerd binnen één Azure [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Voor hoge beschikbaarheid van SAP Central Services u twee virtuele machines in een afzonderlijke beschikbaarheidsset implementeren en Windows Failover Cluster-Services of Pacemaker (Linux) gebruiken om te implementeren, een hoge beschikbaarheid-raamwerk waarmee automatische failover in het geval van een infrastructuur of softwareprobleem. Documentatie met gedetailleerde informatie over de lijst met dergelijke implementaties, zoals:

- [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van bestandsshare](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
- [Hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server voor SAP-toepassingen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)

Een soortgelijke architectuur geldt voor de DBMS-laag van SAP NetWeaver en S/4HANA of Hybris systemen. U implementeert de DBMS-laag in de modus actief/passief met een oplossing voor failoverclusters, dat gebruikmaakt van DBMS specifieke failover-frameworks, Windows Failover Cluster-Services of Pacemaker failover-activiteiten in het geval van infrastructuur of software starten is mislukt. 

Als u wilt implementeren met behulp van Azure availability zones dezelfde architectuur, moeten sommige wijzigingen van de architectuur die beschreven worden aangebracht. Deze wijzigingen worden beschreven in de verschillende onderdelen van dit document.

## <a name="considerations-deploying-across-availability-zones"></a>Overwegingen voor implementatie in meerdere Beschikbaarheidszones

Beschikbaarheidszones bieden enkele aandachtspunten voor. De lijst met overwegingen, zoals:

- Azure-beschikbaarheidszones geeft geen garanties van bepaalde afstand tussen de verschillende zones in een Azure-regio
- Er zijn geen Azure-beschikbaarheidszones een ideale oplossing voor herstel na Noodgevallen. In tijden waar grote natuurrampen breed veroorzaken verspreiding schade in sommige regio's van de hele wereld, met inbegrip van zware schade aan power-infrastructuur, de afstand tussen de verschillende zones mogelijk niet groot genoeg zijn om in aanmerking komen als een goede DR-oplossing
- De netwerklatentie tussen verschillende Azure-beschikbaarheidszones in verschillende Azure-regio's verschilt van de Azure-regio naar regio. Er zijn gevallen, waar u het niveau van de SAP-toepassing geïmplementeerd in verschillende zones, omdat de netwerklatentie tussen één zone aan de actieve VM DBMS nog steeds acceptabel uit een impact op het proces van het bedrijf is kunt uitvoeren. Maar er zijn scenario's in een Azure-regio's waar de latentie tussen de actieve VM DBMS in één zone en het exemplaar van een SAP-toepassing in een andere zone te firewallverbinding en niet geschikt is voor de SAP-bedrijfsprocessen worden kan. Als gevolg hiervan moet de implementatiearchitectuur worden verschillende met een actief/actief-architectuur voor de toepassing of actief/passief-architectuur waarin meerdere zone te hoog is.
- Bepalen in welke configuratie u Azure-beschikbaarheidszones voor kan gebruiken. Op basis van de netwerklatentie die u tussen de verschillende zones meten wilt. De netwerklatentie speelt een belangrijke rol in twee verschillende gebieden:
    - De latentie tussen de twee DBMS-instanties die u beschikken over een synchrone replicatie moet, tot stand gebracht. Hoe hoger de netwerklatentie, hoe hoger de potentiële impact schaalbaarheid van uw workload
    - Het verschil in de netwerklatentie tussen een virtuele machine die een SAP-dialoogvenster-exemplaar wordt uitgevoerd in dezelfde regio bevindt als de actieve DBMS-instantie en een vergelijkbare virtuele machine in een andere zone. Een hogere dit verschil, hoe hoger de impact op de uitvoeringstijd van zakelijke processen en batchtaken, afhankelijk van of ze worden uitgevoerd in dezelfde zone met de DBMS-systemen of in een andere zone


Vanaf het gebruik van Azure-functies zijn er enkele beperkingen van de functionaliteit die kunnen worden gebruikt bij het implementeren van virtuele Azure-machines in verschillende zones en tot stand brengen van failover-oplossingen in verschillende beschikbaarheidszones binnen dezelfde Azure-regio. Deze beperkingen worden vermeld, zoals:

- Met behulp van [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/) is verplicht voor het implementeren in Azure-beschikbaarheidszones 
- De toewijzing van opsommingen van de zone op de fysieke zones wordt vastgesteld op basis van de Azure-abonnement. Als u verschillende abonnementen voor het implementeren van uw SAP-systemen gebruikt, moet u de ideale zones voor elk abonnement afzonderlijk definiëren
- U kunt geen Azure-beschikbaarheidssets in een Azure-beschikbaarheidszone implementeren. Kies een Azure-beschikbaarheidszone of een Azure-beschikbaarheidsset instellen als framework van de implementatie voor virtuele machines.
- U kunt geen gebruiken een [Azure Basic Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) failover om clusteroplossingen te maken op basis van Windows Failover Cluster-Services of Linux-Pacemaker. In plaats daarvan moet u gebruiken de [Azure Standard Load Balancer-SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)



## <a name="ideal-zone-combination"></a>Ideaal zone combinatie
Om te bepalen in hoe u kunt gebruikmaken van beschikbaarheidszones, moet u een onderzoek waarmee u kunt uitvoeren:

- De netwerklatentie tussen de drie verschillende zones van een Azure-regio. Zo, dat u de zones die de minste netwerklatentie hebben kunt in meerdere zone-netwerkverkeer
- Het verschil tussen VM-VM latentie binnen een van de zones die door u gekozen en de netwerklatentie tussen de twee zones die u hebt gekozen
- Een instructie of de virtuele machine bestandstypen die u wilt hebt geïmplementeerd in de beschikbaarheidszones keuze zijn beschikbaar in de twee zones van uw keuze. Met name met M-serie virtuele machines gaat u er omstandigheden zijn waarin de verschillende M-serie VM-SKU's gaat in twee van de drie zones alleen beschikbaar zijn

### <a name="network-latency-between-zones-and-within-zone"></a>De netwerklatentie tussen zones en binnen een zone
Als u wilt weten wat de latentie tussen de verschillende zones is, moet u naar:

- Implementeer de VM-SKU u wilt gebruiken voor uw exemplaar DBMS in alle drie zones. Zorg ervoor dat die [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) bij het uitvoeren van deze meting is ingeschakeld
- Als u de twee zones met de minimale netwerklatentie kunt vinden, implementeert u de andere drie virtuele machines van de VM-SKU die u wilt gebruiken als toepassingslaag VM in de drie beschikbaarheidszones. De netwerklatentie meten op basis van de twee DBMS VMs in de twee verschillende 'DBMS-zones van uw keuze. 
- Als een hulpprogramma voor het meten van gebruik **niping**. Een hulpprogramma van SAP, die werkt zoals beschreven in de opmerkingen bij de SAP-ondersteuning [#500235](https://launchpad.support.sap.com/#/notes/500235) en [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Richt u op de opdrachten voor replicatielatentie gedocumenteerd SAP. Met behulp van **ping** is niet een aanbevolen hulpprogramma sinds **ping** werkt niet via de Azure code netwerkpaden versneld.

Op basis van de metingen en de beschikbaarheid van uw VM-SKU's in de verschillende zones, moet u de beslissingen te nemen:

- Definieer de ideale zones voor de DBMS-laag
- De vraag worden beantwoord of, op basis van verschillen van de netwerklatentie binnen een zone of in zones, kunt u uw actieve SAP-toepassingslaag verdelen over een, twee of alle drie verschillende zones
- De vraag worden beantwoord of u een actief/passief of een actief/actief-configuratie uit het oogpunt van een toepassing implementeren wilt (Zie verderop)

Deze beslissingen ook aanbevelingen voor netwerken latentie van SAP intrekken zoals beschreven in de SAP-notitie [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E).

### <a name="be-aware-of"></a>Houd rekening met

> [!IMPORTANT]
> De metingen en besluiten die u neemt, zijn geldig voor het Azure-abonnement u gebruikt deze metingen maken. Met behulp van een andere Azure-abonnement u de metingen herhalen moet, aangezien de toewijzing van uw abonnement afhankelijke inventarisatie van zones kan worden gewijzigd met een ander abonnement


> [!IMPORTANT]
> Het is waarschijnlijk dat de metingen als hierboven uitgevoerd verschillende resultaten worden weergegeven in elke Azure-regio die ondersteuning biedt voor [beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview). Zelfs met uw vereisten op netwerklatentie niet wijzigen, moet u mogelijk verschillende implementatiestrategieën in verschillende Azure-regio's aan te passen omdat de netwerklatentie tussen zones kan afwijken. Verwacht wordt dat in sommige Azure-regio's, netwerklatentie tussen de drie verschillende zones aanzienlijk afwijken kan. Terwijl in andere regio's, de netwerklatentie tussen de drie verschillende zones worden meer uniform. De claim er **altijd** is een netwerklatentie voor verschillende gebieden van 1 milliseconde op 2 milliseconden **verkeerde**. De netwerklatentie voor verschillende beschikbaarheidszones in Azure-regio's kan niet worden gebruikt.


## <a name="activeactive-deployment"></a>Actief/actief-implementatie
Deze implementatiearchitectuur heet actief/actief, omdat u uw actieve exemplaren van de SAP-dialoogvenster in twee of drie zones implementeren. De SAP Central Services met behulp van de replicatie in de wachtrij plaatsen gaat tussen de twee zones worden geïmplementeerd. Hetzelfde geldt voor de DBMS-laag, die wordt geïmplementeerd in de dezelfde zones als de SAP centrale-Service.

Als u wilt een dergelijke configuratie, bekendmaken, moet u de twee beschikbaarheidszones niet vinden in de regio die meerdere zone netwerklatentie die aanvaardbaar is voor uw workload en uw synchrone replicatie voor de DBMS-systemen bieden. Daarnaast wilt u de verschillen tussen de netwerklatentie binnen de zones die u hebt geselecteerd en de in de zone netwerklatentie niet te groot wordt. Reden voor de laatste is dat u niet dat te grote verschillen in uitvoeringstijden van uw bedrijfsprocessen wilt of batch verwerkt u afhankelijk van of een taak wordt uitgevoerd in de zone met de DBMS-server of in de zone. Enkele variaties worden geaccepteerd, maar niet factoren van verschil.

Een vereenvoudigde schema van de implementatie van een actief/actief in twee zones kan er als volgt uitzien:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

De volgende overwegingen zijn van toepassing voor deze configuratie:

- U behandelen de Azure-beschikbaarheidszones als fout- en updatedomeinen voor alle virtuele machines omdat beschikbaarheidssets kunnen niet worden geïmplementeerd in Azure-beschikbaarheidszones
- De Azure-load balancers die u gebruikt voor de failover-clusters van de SAP Central Services, evenals de DBMS-laag, moet de [standaard-SKU load balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). De basisversie van load balancer werkt niet in zones
- De Azure-netwerk en de subnetten die u hebt geïmplementeerd voor het hosten van de SAP-systeem worden uitgebreid in zones. U **hoeft geen** afzonderlijke virtuele netwerken voor elke zone
- Gebruik voor alle virtuele machines die u implementeert, [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/). Niet-beheerde schijven worden niet ondersteund voor zonegebonden implementaties
- Azure Premium storage of [Ultra SSD-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) worden niet ondersteund door elk type opslagreplicatie in zones. Als gevolg hiervan is de verantwoordelijkheid van de toepassing (DBMS-systemen of SAP Central Services) om belangrijke gegevens te repliceren
- Hetzelfde geldt voor de gedeelde sapmnt-map een gedeelde schijf (Windows), een CIFS-share (Windows is) of NFS-share (Linux). U moet een technologie die worden gerepliceerd die een gedeelde schijf of delen tussen de zones te gebruiken. Op het moment dat worden de volgende technologieën ondersteund:
    - Voor Windows, een oplossing die gebruikmaakt van SIOS Datakeeper zoals beschreven in [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) wordt ondersteund in zones
    - Voor SUSE Linux, een NFS-share ingebouwd, zoals beschreven in [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) wordt ondersteund
    - Op dit moment, de oplossing met behulp van services van Windows-scale-out bestandsserver (SOFS) zoals beschreven in [voorbereiden Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor instanties van SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **wordt niet ondersteund in zones**
- De derde zone wordt gebruikt voor het hosten van het apparaat SBD in het geval u bouwt een [SUSE Linux pacemaker cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) of exemplaren van een extra toepassing
- Uitvoeringstijd om consistentie te bereiken voor bepaalde kritieke zakelijke transacties en/of de taken. U kunt proberen om te leiden van bepaalde batch-taken en gebruikers rechtstreeks naar exemplaren van specifieke toepassingen die zich in de zone met de actieve sessie van de DBMS-systemen met SAP-Batch-servergroepen, aanmeldingsgroepen of RFC-groepen. Echter in het geval van een zonegebonden failover moet u deze groepen handmatig verplaatsen naar dialoogvenster instanties, die zich in de resterende zone (s) 
- Bepaal of u implementeren de exemplaren van sommige inactieve dialoogvenster in elk van de zones wilt kunnen onmiddellijk naar de vorige resourcecapaciteit ophalen als een zone die u hebt geïmplementeerd onderdeel van uw toepassingsexemplaren, buiten gebruik gesteld


## <a name="activepassive-deployment"></a>Actief/passief-implementatie
In gevallen waar u niet een acceptabele verschillen tussen de netwerklatentie binnen een zone en meerdere zone netwerkverkeer vinden, heeft de architectuur die u kunt implementeren een actief/passief-teken uit het oogpunt van SAP-toepassing-laag. U definieert een 'active' zone, is de zone die u implementeert op het niveau van de volledige toepassing in en waar u probeert de actieve DBMS-instantie en de actieve sessie van SAP Central Services uit te voeren. Met deze configuratie kunt ervoor u zorgen dat er geen verschil extreme uitvoeringstijd in zakelijke transacties en batchtaken, afhankelijk van of een taak in dezelfde zone met de actieve DBMS-instantie wordt uitgevoerd of niet.

De basisindeling van dergelijke een architectuur lijkt:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

De volgende overwegingen zijn van toepassing voor deze configuratie:

- Beschikbaarheidssets kunnen niet worden geïmplementeerd in Azure-beschikbaarheidszones. Daarom kan in dit geval zijn u loopt met één domein voor update- en foutdomeinen voor het niveau van uw toepassing. Reden is dat alleen wordt geïmplementeerd in een zone. Deze configuratie is een lichte tegenslag vergeleken met de referentiearchitectuur die voorziet in dat u het niveau van de toepassing in een Azure-beschikbaarheidsset implementeert.
- Deze architectuur van een besturingssysteem, moet u om te controleren en probeer de actieve DBMS-systemen en SAP Central services-exemplaren dat in dezelfde regio bevindt als het niveau van uw geïmplementeerde toepassing. Bij een failover van SAP centrale Service of de DBMS-instantie die u wilt om ervoor te zorgen dat u handmatig terug in de zone met de SAP-toepassingslaag zo vroeg mogelijk geïmplementeerd een kunt
- De Azure-load balancers die u gebruikt voor de failover-clusters van de SAP Central Services, evenals de DBMS-laag, moet de [standaard-SKU load balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). De basisversie van load balancer werkt niet in zones
- De Azure-netwerk en de subnetten die u hebt geïmplementeerd voor het hosten van de SAP-systeem worden uitgebreid in zones. U **hoeft geen** afzonderlijke virtuele netwerken voor elke zone
- Voor alle virtuele machines die u implementeert, moet u gebruiken [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/). Niet-beheerde schijven worden niet ondersteund voor zonegebonden implementaties
- Azure Premium storage of [Ultra SSD-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) worden niet ondersteund door elk type opslagreplicatie in zones. Als gevolg hiervan is de verantwoordelijkheid van de toepassing (DBMS-systemen of SAP Central Services) om belangrijke gegevens te repliceren
- Hetzelfde geldt voor de gedeelde sapmnt-map een gedeelde schijf (Windows), een CIFS-share (Windows is) of NFS-share (Linux). U moet een technologie die worden gerepliceerd die een gedeelde schijf of delen tussen de zones te gebruiken. Op het moment dat worden de volgende technologieën ondersteund:
    - Voor Windows, een oplossing die gebruikmaakt van SIOS Datakeeper zoals beschreven in [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) wordt ondersteund in zones
    - Voor SUSE Linux, een NFS-share ingebouwd, zoals beschreven in [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) wordt ondersteund
    - Op dit moment, de oplossing met behulp van services van Windows-scale-out bestandsserver (SOFS) zoals beschreven in [voorbereiden Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor instanties van SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **wordt niet ondersteund in zones**
- De derde zone wordt gebruikt voor het hosten van het apparaat SBD in het geval u bouwt een [SUSE Linux pacemaker cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) of exemplaren van een extra toepassing
- Inactieve VM's in de passieve zone (vanuit het oogpunt van een DBMS) implementeren om te kunnen starten van de toepassingsresources in het geval van een zone-fout
    - U kunt geen gebruiken [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) actieve virtuele machines repliceren naar inactieve VM's tussen zones. Op dit moment, kan Azure Site Recovery niet voldoen aan een dergelijke functie
- Investeren in automation waarmee u, in het geval van een zone-fout optreedt, op het niveau van de SAP-toepassing automatisch te starten in de tweede zone

## <a name="combined-high-availability-and-disaster-recovery-configuration"></a>Gecombineerde hoge beschikbaarheid en configuratie voor herstel na noodgevallen
Ondanks het feit dat Microsoft niet alle informatie biedt over geografische afstand tussen de faciliteiten die als host andere Azure-beschikbaarheidszones in een bepaalde Azure-regio fungeren, sommige klanten zijn gebruik te maken van zones voor een gecombineerde HA en DR-configuratie die biedt een **R**failoverstatus **P**pnieuw **O**bjective (RPO) van nul. Wat betekent dat, u moet gaan niet verloren databasetransacties die zijn doorgevoerd zelfs in het geval van de herstel na noodgevallen. 

> [!NOTE]
> Een configuratie als volgt wordt aanbevolen voor alleen specifieke omstandigheden. Bijvoorbeeld, gevallen waar de gegevens de Azure-regio vanwege redenen voor beveiliging en naleving niet verlaten. 

Een voorbeeld van hoe een dergelijke configuratie, zoals bekijken kan wordt geïllustreerd in deze afbeelding:

![combined_ha_dr_in_zones](./media/sap-ha-availability-zones/combined_ha_dr_in_zones.png)

De volgende overwegingen zijn van toepassing voor deze configuratie:

- U bent een ervan uitgaande dat er een aanzienlijke afstand tussen de faciliteiten die als host fungeert voor een beschikbaarheidszone. Of u moest houden aan bepaalde Azure-regio. Beschikbaarheidssets kunnen niet worden geïmplementeerd in Azure-beschikbaarheidszones. Daarom kan in dit geval zijn u loopt met één domein voor update- en foutdomeinen voor het niveau van uw toepassing. Reden is dat alleen wordt geïmplementeerd in een zone. Dit is een tegenslag vergeleken met de referentiearchitectuur die voorziet in dat u het niveau van de toepassing in een Azure-beschikbaarheidsset implementeert.
- Deze architectuur van een besturingssysteem, moet u om te controleren en probeer de actieve DBMS-systemen en SAP Central services-exemplaren dat in dezelfde regio bevindt als het niveau van uw geïmplementeerde toepassing. Bij een failover van SAP centrale Service of de DBMS-instantie die u wilt om ervoor te zorgen dat u handmatig terug in de zone met de SAP-toepassingslaag zo vroeg mogelijk geïmplementeerd een kunt
- Hebt u exemplaren van de productie-toepassing vooraf is geïnstalleerd in de VM's waarop de actieve QA-toepassingsexemplaren.
- Bij een storing zonegebonden, de toepassingsexemplaren QA afsluiten en start de productie-exemplaren in plaats daarvan. Houd er rekening mee dat u wilt werken met virtuele namen voor de exemplaren van een toepassing om deze taak te maken
- De Azure-load balancers die u gebruikt voor de failover-clusters van de SAP Central Services, evenals de DBMS-laag, moet de [standaard-SKU load balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). De basisversie van load balancer werkt niet in zones
- De Azure-netwerk en de subnetten die u hebt geïmplementeerd voor het hosten van de SAP-systeem worden uitgebreid in zones. U **hoeft geen** afzonderlijke virtuele netwerken voor elke zone
- Voor alle virtuele machines die u implementeert, moet u gebruiken [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/). Niet-beheerde schijven worden niet ondersteund voor zonegebonden implementaties
- Azure Premium storage of [Ultra SSD-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) worden niet ondersteund door elk type opslagreplicatie in zones. Als gevolg hiervan is de verantwoordelijkheid van de toepassing (DBMS-systemen of SAP Central Services) om belangrijke gegevens te repliceren
- Hetzelfde geldt voor de gedeelde sapmnt-map een gedeelde schijf (Windows), een CIFS-share (Windows is) of NFS-share (Linux). U moet een technologie die worden gerepliceerd die een gedeelde schijf of delen tussen de zones te gebruiken. Op het moment dat worden de volgende technologieën ondersteund:
    - Voor Windows, een oplossing die gebruikmaakt van SIOS Datakeeper zoals beschreven in [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) wordt ondersteund in zones
    - Voor SUSE Linux, een NFS-share ingebouwd, zoals beschreven in [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) wordt ondersteund
    - Op dit moment, de oplossing met behulp van services van Windows-scale-out bestandsserver (SOFS) zoals beschreven in [voorbereiden Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor instanties van SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **wordt niet ondersteund in zones**
- De derde zone wordt gebruikt voor het hosten van het apparaat SBD in het geval u bouwt een [SUSE Linux pacemaker cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) of exemplaren van een extra toepassing





## <a name="next-steps"></a>Volgende stappen
Controleer de volgende stappen om te implementeren in Azure availability zones:

- [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor SAP ASCS/SCS-instanties voorbereiden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)






