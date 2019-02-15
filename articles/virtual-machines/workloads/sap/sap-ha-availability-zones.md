---
title: Configuraties van SAP-werkbelasting met Azure-Beschikbaarheidszones | Microsoft Docs
description: Architectuur voor hoge beschikbaarheid en scenario's voor SAP NetWeaver met behulp van Azure-Beschikbaarheidszones
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
ms.openlocfilehash: 2d98a5ab13c2aecd3b3cef590526031f5bdee594
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56268308"
---
# <a name="sap-workload-configurations-with-azure-availability-zones"></a>Configuraties van SAP-werkbelasting met Azure-Beschikbaarheidszones
[Azure-Beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview) is een van de functies voor hoge beschikbaarheid die Azure biedt. De algemene beschikbaarheid van SAP-workloads op Azure met Beschikbaarheidszones worden verbeterd. Deze functie is al beschikbaar in bepaalde [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/). In de toekomst, is deze beschikbaar in meer regio's.

Deze afbeelding wordt weergegeven de basisarchitectuur van hoge beschikbaarheid voor SAP:

![Standaard maximaal beschikbaarheidsconfiguratie](./media/sap-ha-availability-zones/standard-ha-config.png)

Het niveau van de SAP-toepassing wordt geïmplementeerd binnen één Azure [beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Voor hoge beschikbaarheid van SAP Central Services, kunt u twee virtuele machines in een afzonderlijke beschikbaarheidsset implementeren. Windows Server Failover Clustering of Pacemaker (Linux) gebruiken als een framework voor hoge beschikbaarheid met automatische failover in het geval van een infrastructuur- of softwareprobleem. Zie voor meer informatie over deze implementaties:

- [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van bestandsshare](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
- [Hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server voor SAP-toepassingen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)

Een soortgelijke architectuur geldt voor de DBMS-laag van SAP NetWeaver en S/4HANA of Hybris systemen. U implementeert de DBMS-laag in de modus actief/passief met een oplossing voor failoverclusters te beschermen tegen de infrastructuur- of softwarefout optreedt. De oplossing voor failoverclusters wordt mogelijk een DBMS-specifieke failover-framework, Windows Server Failover Clustering of Pacemaker.

Voor het implementeren van dezelfde architectuur met behulp van Azure-Beschikbaarheidszones, moet u enkele wijzigingen aanbrengen in de architectuur die eerder zijn beschreven. Deze wijzigingen worden beschreven in dit artikel.

## <a name="considerations-for-deploying-across-availability-zones"></a>Overwegingen voor het implementeren in meerdere Beschikbaarheidszones


Overweeg het volgende wanneer u Beschikbaarheidszones gebruiken:

- Er zijn geen garanties met betrekking tot de afstand tussen de verschillende Beschikbaarheidszones binnen een Azure-regio.
- Er zijn geen Beschikbaarheidszones een ideale oplossing voor herstel na Noodgevallen. Natuurrampen kunnen wijdverbreid schade veroorzaken in regio's ter wereld, met inbegrip van zware schade aan de infrastructuur van power. De afstand tussen de verschillende zones zijn mogelijk niet groot genoeg is voor een goede DR-oplossing vormen.
- De netwerklatentie voor verschillende Beschikbaarheidszones is niet hetzelfde als in alle Azure-regio's. In sommige gevallen kunt u implementeren en de SAP-toepassingslaag in andere zones worden uitgevoerd omdat de netwerklatentie tussen één zone aan de actieve VM DBMS acceptabel is. Maar in sommige Azure-regio's, de vertraging tussen het actieve DBMS-VM en het exemplaar van de toepassing SAP wanneer geïmplementeerd in verschillende zones, mogelijk niet geschikt voor de SAP-bedrijfsprocessen. In dergelijke gevallen moet de implementatiearchitectuur verschillend zijn, met een actief/actief-architectuur voor de toepassing of een actief/passief-architectuur waar netwerklatentie tussen zones te hoog is.
- Wanneer u bepaalt waar u wilt gebruiken voor Beschikbaarheidszones, Baseer uw beslissing over de netwerklatentie tussen de zones. De netwerklatentie speelt een belangrijke rol in twee gebieden:
    - De latentie tussen de twee DBMS-instanties die u moet beschikken over synchrone replicatie. Hoe hoger de netwerklatentie, hoe groter de kans heeft dit gevolgen voor de schaalbaarheid van uw workload.
    - Het verschil in de netwerklatentie tussen een virtuele machine met een SAP dialoogvenster exemplaar in de zone met de actieve DBMS-instantie en een vergelijkbare virtuele machine in een andere zone. Als dit verschil toeneemt, de invloed op de uitvoertijd van zakelijke processen en batch taken ook toeneemt, afhankelijk van of ze worden uitgevoerd in de zone met de DBMS-systemen of in een andere zone.

Wanneer u Azure VM's in meerdere Beschikbaarheidszones implementeren en failover-oplossingen binnen dezelfde Azure-regio maken, gelden enkele beperkingen:

- U moet gebruiken [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/) wanneer u implementeert voor Azure-Beschikbaarheidszones. 
- De toewijzing van opsommingen van de zone op de fysieke zones wordt vastgesteld op basis van de Azure-abonnement. Als u verschillende abonnementen voor het implementeren van uw SAP-systemen gebruikt, moet u de ideale zones voor elk abonnement definiëren.
- U kunt geen Azure-beschikbaarheidssets binnen een Beschikbaarheidszone Azure implementeren. Kies een of andere als een implementatie-framework voor virtuele machines.
- U kunt geen gebruiken een [Azure Basic Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) failover om clusteroplossingen te maken op basis van Windows Server Failover Clustering- of Linux-Pacemaker. In plaats daarvan moet u gebruiken de [Azure Standard Load Balancer-SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones).



## <a name="the-ideal-availability-zones-combination"></a>De ideale combinatie van de Beschikbaarheidszones
Voordat u hoe u kunt gebruiken voor Beschikbaarheidszones besluit, moet u om te bepalen:

- De netwerklatentie tussen de drie zones van een Azure-regio. Hierdoor kunt u de zones kiezen met de minimale netwerklatentie in het netwerkverkeer tussen zones.
- Het verschil tussen VM-VM latentie binnen een van de zones van uw keuze, en de netwerklatentie tussen twee zones van uw keuze.
- Bepalen of de VM-typen die u nodig hebt om te implementeren zijn beschikbaar in de twee zones die u hebt geselecteerd. Met enkele VM's, met name uit de M-serie VM's, kunnen er situaties waarin een aantal SKU's beschikbaar in slechts twee van de drie zones zijn.

## <a name="network-latency-between-and-within-zones"></a>De netwerklatentie tussen en tussen zones
Om te bepalen de latentie tussen de verschillende zones, moet u naar:

- Implementeer de VM-SKU u wilt gebruiken voor uw exemplaar DBMS in alle drie zones. Zorg ervoor dat [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) wanneer u deze meting is ingeschakeld.
- Als u de twee zones met de minimale netwerklatentie, implementeert u de andere drie virtuele machines van de VM-SKU die u wilt gebruiken als de virtuele machine voor het niveau van de toepassing in de drie Beschikbaarheidszones. De netwerklatentie meten op basis van de twee DBMS-VM's in de twee DBMS-zones die u hebt geselecteerd. 
- Gebruik **niping** als een meting hulpprogramma. Dit hulpprogramma, SAP, wordt beschreven in de opmerkingen bij de ondersteuning van SAP [#500235](https://launchpad.support.sap.com/#/notes/500235) en [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Richt u op de opdrachten voor replicatielatentie gedocumenteerd. Omdat **ping** werkt niet via de Azure Accelerated Networking code-paden wordt niet aanbevolen dat u deze gebruiken.

Op basis van uw metingen en de beschikbaarheid van uw VM-SKU's in de Beschikbaarheidszones, moet u enkele beslissingen te nemen:

- Definieer de ideale zones voor de DBMS-laag.
- Bepaal of u wilt voor de distributie van uw actieve SAP-toepassingslaag over een, twee of alle drie zones, op basis van verschillen van netwerk latentie in de zone versus in zones.
- Bepaal of u wilt om een actief/passief-configuratie of een actief/actief-configuratie, vanuit het oogpunt van een toepassing te implementeren. (Deze configuraties worden verderop in dit artikel beschreven.)

In deze beslissingen ook rekening van account SAP netwerk latentie aanbevelingen, zoals beschreven in de SAP-notitie [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E).

> [!IMPORTANT]
> De metingen en de beslissingen die u zijn geldig voor het Azure-abonnement dat u hebt gebruikt toen u de metingen uitgevoerd. Als u een andere Azure-abonnement gebruiken, moet u Herhaal de metingen. De toewijzing van vaste zones kan afwijken voor een andere Azure-abonnement.


> [!IMPORTANT]
> Het waarschijnlijk dat de metingen die eerder zijn beschreven biedt verschillende resultaten in elke Azure-regio die ondersteuning biedt voor [Beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview). Zelfs als uw netwerk latentievereisten dezelfde zijn, moet u mogelijk vast te stellen van verschillende implementatiestrategieën in verschillende Azure-regio's, omdat de netwerklatentie tussen zones kan afwijken. In sommige Azure-regio's, kan de netwerklatentie tussen de drie verschillende zones aanzienlijk anders zijn. In andere regio's, kan de netwerklatentie tussen de drie verschillende zones meer uniform zijn. De claim die er altijd is een netwerklatentie tussen 1 en 2 milliseconden is niet correct. De netwerklatentie voor verschillende Beschikbaarheidszones in Azure-regio's kan niet worden gebruikt.

## <a name="activeactive-deployment"></a>Actief/actief-implementatie
Deze architectuur wordt actief/actief genoemd, omdat u uw actieve exemplaren van de SAP-dialoogvenster in twee of drie zones implementeren. Het SAP Central Services-exemplaar dat wordt gebruikt in de wachtrij plaatsen replicatie worden tussen de twee zones geïmplementeerd. Hetzelfde geldt voor de DBMS-laag, die wordt geïmplementeerd in de dezelfde zones als SAP centrale Service.

Wanneer u overweegt deze configuratie, moet u de twee Beschikbaarheidszones vinden in uw regio die aanbieding tussen zones netwerklatentie die aanvaardbaar is voor uw workload en uw synchrone replicatie voor de DBMS-systemen. Wilt u er ook om er zeker van te zijn dat de verschillen tussen de netwerklatentie binnen de zones die u hebt geselecteerd en de netwerklatentie tussen zones niet te groot. Dit is omdat u niet wilt dat grote verschillen, afhankelijk van of een taak wordt uitgevoerd in de zone met de DBMS-server of in meerdere zones, in de actieve tijden van uw bedrijfsprocessen of batch-taken. Enkele variaties worden geaccepteerd, maar niet factoren van verschil.

Een vereenvoudigde schema van de implementatie van een actief/actief in twee zones kan er als volgt:

![Actief/actief-zone-implementatie](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

De volgende overwegingen zijn van toepassing voor deze configuratie:

- U behandelen de Azure-Beschikbaarheidszones als fout- en updatedomeinen voor alle virtuele machines, omdat beschikbaarheidssets kunnen niet worden geïmplementeerd in Azure-Beschikbaarheidszones.
- Voor de load balancers van de failoverclusters van SAP Central Services en de DBMS-laag, die u wilt gebruiken de [standaard SKU Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). De basisversie van Load Balancer werkt niet in zones.
- Het Azure-netwerk dat u hebt geïmplementeerd voor het hosten van de SAP-systeem, samen met de subnetten wordt uitgerekt in zones. U moet afzonderlijke virtuele netwerken voor elke zone.
- Voor alle virtuele machines die u implementeert, moet u gebruiken [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Niet-beheerde schijven worden niet ondersteund voor zonegebonden implementaties.
- Azure Premium Storage en [Ultra SSD-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) bieden geen ondersteuning voor elk type opslagreplicatie in zones. De toepassing (DBMS-systemen of SAP Central Services) moet belangrijke gegevens repliceren.
- Hetzelfde geldt voor de gedeelde sapmnt-map een gedeelde schijf (Windows), een CIFS-share (Windows) of een NFS-share (Linux is). U moet een technologie die worden gerepliceerd van deze gedeelde schijven of shares tussen de zones te gebruiken. Deze technologieën worden ondersteund:
    - Voor Windows, een oplossing die gebruikmaakt van SIOS DataKeeper, zoals beschreven in [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk).
    - Voor SUSE Linux, een NFS-share die ingebouwd, zoals beschreven in [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
    
    Op dit moment de oplossing die gebruikmaakt van Microsoft Scale-Out File Server, zoals beschreven in [voorbereiden Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor instanties van SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share), is het niet ondersteund in zones.
- De derde zone wordt gebruikt voor het hosten van het apparaat SBD in het geval u bouwt een [SUSE Linux Pacemaker cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) of exemplaren van een extra toepassing.
- Als u wilt bereiken consistentie van de uitvoeringstijd voor essentiële bedrijfsprocessen, kunt u proberen om te leiden van bepaalde batch-taken en exemplaren van een toepassing die in de zone worden gebruikers batch met het actieve exemplaar DBMS-systemen met behulp van SAP-servergroepen, aanmeldingsgroepen of RFC-groepen. Echter, in het geval van een zonegebonden failover, moet u deze groepen handmatig verplaatsen naar exemplaren die worden uitgevoerd op virtuele machines die zich in de zone met de actieve DB-virtuele machine.  
- Het is raadzaam om te implementeren inactieve dialoogvenster-exemplaren in elk van de zones. Dit is het inschakelen van een directe terug te keren naar de vorige resourcecapaciteit als een zone die worden gebruikt door een deel van uw toepassingsexemplaren buiten gebruik gesteld is.


## <a name="activepassive-deployment"></a>Actief/passief-implementatie
Als u een acceptabele verschillen tussen de netwerklatentie binnen een zone en de latentie van netwerkverkeer tussen zones niet kunt vinden, kunt u een architectuur met een actief/passief-teken uit de laag oogpunt van SAP-toepassing kunt implementeren. U definieert een *active* zone, dat de zone is waar u het niveau van de volledige toepassing implementeert en waar u probeert uit te voeren van zowel de actieve DBMS en de SAP Central Services-exemplaar. Met een dergelijke configuratie moet u controleren of er geen extreme uitvoeringstijd verschillen, afhankelijk van of een taak wordt uitgevoerd in de zone met de actieve sessie van de DBMS-systemen of niet, in een zakelijke transacties en batch-taken.

De eenvoudige indeling van de architectuur ziet er als volgt:

![Actief/passief-zone-implementatie](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

De volgende overwegingen zijn van toepassing voor deze configuratie:

- Beschikbaarheidssets kunnen niet worden geïmplementeerd in Azure-Beschikbaarheidszones. Dus in dit geval hebt u een update- en foutdomeinen domein voor uw toepassingslaag. Dat komt omdat deze alleen in een zone geïmplementeerd. Deze configuratie is iets minder wenselijk zijn dan de referentiearchitectuur kan dat erop wijzen dat u het niveau van de toepassing in een Azure-beschikbaarheidsset implementeert.
- Wanneer u deze architectuur gebruikt, moet u de status controleren en proberen te houden van de actieve DBMS-systemen en SAP Central Services-exemplaren in dezelfde regio bevindt als het niveau van uw geïmplementeerde toepassing. In het geval van een failover van SAP centrale Service of het exemplaar DBMS-systemen wilt u om ervoor te zorgen dat u handmatig een failover uitvoeren in de zone met het niveau van de SAP-toepassing geïmplementeerd zo snel mogelijk.
- Voor de load balancers van de failoverclusters van SAP Central Services en de DBMS-laag, die u wilt gebruiken de [standaard SKU Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). De basisversie van Load Balancer werkt niet in zones.
- Het Azure-netwerk dat u hebt geïmplementeerd voor het hosten van de SAP-systeem, samen met de subnetten wordt uitgerekt in zones. U moet afzonderlijke virtuele netwerken voor elke zone.
- Voor alle virtuele machines die u implementeert, moet u gebruiken [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Niet-beheerde schijven worden niet ondersteund voor zonegebonden implementaties.
- Azure Premium Storage en [Ultra SSD-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) bieden geen ondersteuning voor elk type opslagreplicatie in zones. De toepassing (DBMS-systemen of SAP Central Services) moet belangrijke gegevens repliceren.
- Hetzelfde geldt voor de gedeelde sapmnt-map een gedeelde schijf (Windows), een CIFS-share (Windows) of een NFS-share (Linux is). U moet een technologie die worden gerepliceerd van deze gedeelde schijven of shares tussen de zones te gebruiken. Deze technologieën worden ondersteund:
    - Voor Windows, een oplossing die gebruikmaakt van SIOS DataKeeper, zoals beschreven in [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk).
    - Voor SUSE Linux, een NFS-share die ingebouwd, zoals beschreven in [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
    
  Op dit moment de oplossing die gebruikmaakt van Microsoft Scale-Out File Server, zoals beschreven in [voorbereiden Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor instanties van SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share), is het niet ondersteund in zones.
- De derde zone wordt gebruikt voor het hosten van het apparaat SBD in het geval u bouwt een [SUSE Linux Pacemaker cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) of exemplaren van een extra toepassing.
- U moet implementeren inactieve VM's in de passieve zone (vanuit het oogpunt van een DBMS), zodat u met toepassingsresources in geval van een storing van de zone beginnen kunt.
    - [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) kan momenteel niet aan de actieve VM's repliceren naar inactieve VM's tussen zones. 
- U moet investeren in automation waarmee u, in het geval van een zone-fout optreedt, op het niveau van de SAP-toepassing automatisch te starten in de tweede zone.

## <a name="combined-high-availability-and-disaster-recovery-configuration"></a>Gecombineerde hoge beschikbaarheid en configuratie voor herstel na noodgevallen
Microsoft heeft geen informatie over de geografische afstand tussen de faciliteiten delen die host verschillende Azure-Beschikbaarheidszones in een Azure-regio. Sommige klanten zijn nog steeds zones gebruikt voor een gecombineerde HA en DR-configuratie die een beoogd herstelpunt (RPO) van nul garandeert. Dit betekent dat u geen databasetransacties die zijn doorgevoerd zelfs in geval van herstel na noodgevallen mag niet meer. 

> [!NOTE]
> U wordt aangeraden dat u een configuratie als volgt alleen in bepaalde omstandigheden. Bijvoorbeeld, kunt u deze als gegevens niet de Azure-regio voor beveiliging of om nalevingsredenen verlaten. 

Hier volgt een voorbeeld van een dergelijke configuratie kan als volgt uitzien:

![Herstel na Noodgevallen met hoge beschikbaarheid in zones gecombineerd](./media/sap-ha-availability-zones/combined_ha_dr_in_zones.png)

De volgende overwegingen zijn van toepassing voor deze configuratie:

- Bent u ervan uitgaande dat er een aanzienlijke afstand tussen de faciliteiten die als host fungeert voor een Beschikbaarheidszone is of u gedwongen bent om binnen een bepaalde Azure-regio te blijven. Beschikbaarheidssets kunnen niet worden geïmplementeerd in Azure-Beschikbaarheidszones. Dus in dit geval hebt u een update- en foutdomeinen domein voor uw toepassingslaag. Dat komt omdat deze alleen in een zone geïmplementeerd. Deze configuratie is iets minder wenselijk zijn dan de referentiearchitectuur kan dat erop wijzen dat u het niveau van de toepassing in een Azure-beschikbaarheidsset implementeert.
- Wanneer u deze architectuur gebruikt, moet u de status controleren en proberen te houden van de actieve DBMS-systemen en SAP Central Services-exemplaren in dezelfde regio bevindt als het niveau van uw geïmplementeerde toepassing. In het geval van een failover van SAP centrale Service of het exemplaar DBMS-systemen wilt u om ervoor te zorgen dat u handmatig een failover uitvoeren in de zone met het niveau van de SAP-toepassing geïmplementeerd zo snel mogelijk.
- U hebt de exemplaren van de productie-toepassing vooraf is geïnstalleerd in de VM's waarop de actieve QA-toepassingsexemplaren.
- Bij een storing zone de toepassingsexemplaren QA afsluiten en start de productie-exemplaren in plaats daarvan. Houd er rekening mee dat u wilt virtuele namen voor de exemplaren van een toepassing gebruiken om deze taak te maken.
- Voor de load balancers van de failoverclusters van SAP Central Services en de DBMS-laag, die u wilt gebruiken de [standaard SKU Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). De basisversie van Load Balancer werkt niet in zones.
- Het Azure-netwerk dat u hebt geïmplementeerd voor het hosten van de SAP-systeem, samen met de subnetten wordt uitgerekt in zones. U moet afzonderlijke virtuele netwerken voor elke zone.
- Voor alle virtuele machines die u implementeert, moet u gebruiken [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Niet-beheerde schijven worden niet ondersteund voor zonegebonden implementaties.
- Azure Premium Storage en [Ultra SSD-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) bieden geen ondersteuning voor elk type opslagreplicatie in zones. De toepassing (DBMS-systemen of SAP Central Services) moet belangrijke gegevens repliceren.
- Hetzelfde geldt voor de gedeelde sapmnt-map een gedeelde schijf (Windows), een CIFS-share (Windows) of een NFS-share (Linux is). U moet een technologie die worden gerepliceerd van deze gedeelde schijven of shares tussen de zones te gebruiken. Deze technologieën worden ondersteund:
    - Voor Windows, een oplossing die gebruikmaakt van SIOS DataKeeper, zoals beschreven in [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk).
    - Voor SUSE Linux, een NFS-share die ingebouwd, zoals beschreven in [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).

  Op dit moment de oplossing die gebruikmaakt van Microsoft Scale-Out File Server, zoals beschreven in [voorbereiden Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor instanties van SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share), is het niet ondersteund in zones.
- De derde zone wordt gebruikt voor het hosten van het apparaat SBD in het geval u bouwt een [SUSE Linux Pacemaker cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) of exemplaren van een extra toepassing.





## <a name="next-steps"></a>Volgende stappen
Hier volgen enkele volgende stappen voor het implementeren in Azure-Beschikbaarheidszones:

- [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Azure-infrastructuur voor hoge beschikbaarheid van SAP met behulp van een Windows failover cluster en de bestandsshare voor SAP ASCS/SCS-instanties voorbereiden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)






