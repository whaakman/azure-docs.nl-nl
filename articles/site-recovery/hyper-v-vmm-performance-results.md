---
title: Test resultaten voor replicatie van virtuele Hyper-V-machines in VMM-Clouds naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat informatie over prestatie tests voor de replicatie van virtuele Hyper-V-machines in VMM-Clouds naar een secundaire site met behulp van Azure Site Recovery.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sutalasi
ms.openlocfilehash: a7413b2dcb24a42092eb2af9816b1d29a8306e19
ms.sourcegitcommit: b49431b29a53efaa5b82f9be0f8a714f668c38ab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68377224"
---
# <a name="test-results-for-hyper-v-replication-to-a-secondary-site"></a>Test resultaten voor Hyper-V-replicatie naar een secundaire site


In dit artikel vindt u de resultaten van het testen van de prestaties bij het repliceren van virtuele Hyper-V-machines in System Center Virtual Machine Manager-Clouds (VMM) naar een secundair Data Center.

## <a name="test-goals"></a>Test doelen

Het doel van testen is om te onderzoeken hoe Site Recovery tijdens stabiele status replicatie uitvoert.

- Stabiele status replicatie vindt plaats wanneer virtuele machines de initiële replicatie hebben voltooid en wijzigingen in de Delta worden gesynchroniseerd.
- Het is belang rijk om prestaties te meten met behulp van een stabiele status, omdat het de status is van de meeste Vm's, tenzij er onverwachte storingen optreden.
- De test implementatie bestaat uit twee on-premises sites, met een VMM-server in elke site. Deze test implementatie is een typische implementatie van een hoofd kantoor/filiaal, met hoofd kantoor dat fungeert als de primaire site en het filiaal als de secundaire of herstel site.

## <a name="what-we-did"></a>Wat we hebben gedaan

Dit is wat we in de test fase hebben gedaan:

1. Vm's gemaakt met VMM-sjablonen.
2. Gestarte Vm's en vastgelegde gegevens over de basislijn prestaties over 12 uur.
3. Er zijn Clouds gemaakt op de primaire en herstel VMM-servers.
4. Geconfigureerde replicatie in Site Recovery, inclusief toewijzing tussen bron-en herstel Clouds.
5. Ingeschakelde beveiliging voor Vm's en toestemming geven om de initiële replicatie te volt ooien.
6. Er is een aantal uur gewacht op systeem stabilisatie.
7. Vastgelegde prestatie gegevens over 12 uur, waarbij alle Vm's in de verwachte replicatie status voor die 12 uur bleven.
8. De Delta tussen de basislijn prestaties van de basis lijn en de metrische gegevens over de replicatie prestaties gemeten.


## <a name="primary-server-performance"></a>Prestaties van primaire server

* Met Hyper-V replica (gebruikt door Site Recovery) worden wijzigingen in een logboek bestand asynchroon bijgehouden, met minimale opslag overhead op de primaire server.
* Hyper-V replica maakt gebruik van de door het systeem opgeslagen geheugen cache om de belasting van IOPS voor het bijhouden van nummers te minimaliseren. Er worden in het geheugen schrijf bewerkingen naar de VHDX opgeslagen en deze worden in het logboek bestand geplaatst voordat het logboek naar de herstel site wordt verzonden. Het leegmaken van de schijf gebeurt ook als de schrijf bewerkingen een vooraf vastgestelde limiet bereiken.
* In de onderstaande afbeelding ziet u de overhead van de stabiele status IOPS voor replicatie. We kunnen zien dat de belasting van IOPS als gevolg van replicatie ongeveer 5% is. Dit is zeer laag.

  ![Primaire resultaten](./media/hyper-v-vmm-performance-results/IC744913.png)

Hyper-V replica gebruikt geheugen op de primaire server om de schijf prestaties te optimaliseren. Zoals in de volgende grafiek wordt weer gegeven, is de geheugen overhead op alle servers in het primaire cluster rand nummer. De weer gegeven geheugen overhead is het percentage geheugen dat wordt gebruikt door replicatie, vergeleken met het totale geïnstalleerde geheugen op de Hyper-V-Server.

![Primaire resultaten](./media/hyper-v-vmm-performance-results/IC744914.png)

De Hyper-V-replica heeft een minimale CPU-overhead. Zoals weer gegeven in de grafiek, ligt de overhead van de replicatie binnen het bereik van 2-3%.

![Primaire resultaten](./media/hyper-v-vmm-performance-results/IC744915.png)

## <a name="secondary-server-performance"></a>Secundaire server prestaties

Hyper-V replica maakt gebruik van een kleine hoeveelheid geheugen op de herstel server om het aantal opslag bewerkingen te optimaliseren. De grafiek bevat een samen vatting van het geheugen gebruik op de herstel server. De weer gegeven geheugen overhead is het percentage geheugen dat wordt gebruikt door replicatie, vergeleken met het totale geïnstalleerde geheugen op de Hyper-V-Server.

![Secundaire resultaten](./media/hyper-v-vmm-performance-results/IC744916.png)

De hoeveelheid I/O-bewerkingen op de herstel site is een functie van het aantal schrijf bewerkingen op de primaire site. Laten we eens kijken naar de totale I/O-bewerkingen op de herstel site in vergelijking met de totale I/O-bewerkingen en schrijf bewerkingen op de primaire site. In de grafieken ziet u dat het totale aantal IOPS op de herstel site

* Ongeveer 1,5 keer de schrijf-IOPS op de primaire.
* Rond 37% van het totale aantal IOPS op de primaire site.

![Secundaire resultaten](./media/hyper-v-vmm-performance-results/IC744917.png)

![Secundaire resultaten](./media/hyper-v-vmm-performance-results/IC744918.png)

## <a name="effect-on-network-utilization"></a>Effect op het netwerk gebruik

Een gemiddelde van 275 MB per seconde van de netwerk bandbreedte wordt gebruikt tussen de primaire en herstel knooppunten (waarbij compressie is ingeschakeld), tegen een bestaande band breedte van 5 GB per seconde.

![Resultaat netwerk gebruik](./media/hyper-v-vmm-performance-results/IC744919.png)

## <a name="effect-on-vm-performance"></a>Gevolgen voor de prestaties van de virtuele machine

Een belang rijke overweging is de impact van de replicatie van productie-workloads die worden uitgevoerd op de virtuele machines. Als de primaire site adequaat is ingericht voor replicatie, is er geen gevolgen voor de werk belastingen. Het lichte traceer mechanisme van de Hyper-V-replica zorgt ervoor dat werk belastingen die worden uitgevoerd op de virtuele machines, niet worden beïnvloed tijdens de replicatie van de stabiele status. Dit wordt geïllustreerd in de volgende grafieken.

In deze grafiek worden IOPS weer gegeven die worden uitgevoerd door virtuele machines die verschillende werk belastingen uitvoeren, vóór en nadat de replicatie is ingeschakeld. U kunt zien dat er geen verschil is tussen de twee.

![Resultaten van replica-effect](./media/hyper-v-vmm-performance-results/IC744920.png)

In de volgende grafiek ziet u de door Voer van virtuele machines met verschillende werk belastingen, vóór en na de replicatie. U kunt zien dat replicatie geen aanzienlijke gevolgen heeft.

![Resultaten replica-effecten](./media/hyper-v-vmm-performance-results/IC744921.png)

## <a name="conclusion"></a>Conclusie

De resultaten laten duidelijk zien dat Site Recovery, gekoppeld aan Hyper-V replica, goed schaalbaar is met minimale overhead voor een groot cluster. Site Recovery biedt eenvoudige implementatie, replicatie, beheer en bewaking. Hyper-V replica biedt de benodigde infra structuur voor geslaagde replicatie schaling. 

## <a name="test-environment-details"></a>Details van test omgeving

### <a name="primary-site"></a>Primaire site

* De primaire site heeft een cluster met vijf Hyper-V-servers, waarop 470 virtuele machines worden uitgevoerd.
* De Vm's voeren verschillende werk belastingen uit en alle hebben Site Recovery beveiliging ingeschakeld.
* Opslag voor het cluster knooppunt wordt verschaft door een iSCSI-SAN. Model – Hitachi HUS130.
* Elke cluster server heeft vier netwerk kaarten (Nic's) van één Gbps.
* Twee van de netwerk kaarten zijn verbonden met een particulier iSCSI-netwerk en twee zijn verbonden met een extern bedrijfs netwerk. Een van de externe netwerken is gereserveerd voor cluster communicatie.

![Primaire hardwarevereisten](./media/hyper-v-vmm-performance-results/IC744922.png)

| Server | RAM | Model | Processor | Aantal processors | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V-servers in het cluster: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |128<br />ESTLAB-HOST25 heeft 256 |Dell™ PowerEdge™ R820 |Intel (r) Xeon (r) CPU E5-4620 0 \@ 2.20 GHz |4 |I Gbps x 4 |Windows Server Data Center 2012 R2 (x64) + Hyper-V-rol |
| VMM-Server |2 | | |2 |1 Gbps |Windows Server-Data Base 2012 R2 (x64) + VMM 2012 R2 |

### <a name="secondary-site"></a>Secundaire site

* De secundaire site heeft een failovercluster met zes knoop punten.
* Opslag voor het cluster knooppunt wordt verschaft door een iSCSI-SAN. Model – Hitachi HUS130.

![Primaire hardware-specificatie](./media/hyper-v-vmm-performance-results/IC744923.png)

| Server | RAM | Model | Processor | Aantal processors | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V-servers in het cluster: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell™ PowerEdge™ R720 |Intel (r) Xeon (r) CPU E5-2630 0 \@ 2.30 GHz |2 |I Gbps x 4 |Windows Server Data Center 2012 R2 (x64) + Hyper-V-rol |
| ESTLAB-HOST17 |128 |Dell™ PowerEdge™ R820 |Intel (r) Xeon (r) CPU E5-4620 0 \@ 2.20 GHz |4 | |Windows Server Data Center 2012 R2 (x64) + Hyper-V-rol |
| ESTLAB-HOST24 |256 |Dell™ PowerEdge™ R820 |Intel (r) Xeon (r) CPU E5-4620 0 \@ 2.20 GHz |2 | |Windows Server Data Center 2012 R2 (x64) + Hyper-V-rol |
| VMM-Server |2 | | |2 |1 Gbps |Windows Server-Data Base 2012 R2 (x64) + VMM 2012 R2 |

### <a name="server-workloads"></a>Server werkbelastingen

* Voor test doeleinden zijn de gepickte workloads meestal in bedrijfs scenario's voor ondernemingen.
* We gebruiken [IOMeter](http://www.iometer.org) met het werk belasting kenmerk dat in de tabel voor simulatie is samenvatten.
* Alle IOMeter-profielen zijn ingesteld om wille keurige bytes te schrijven voor het simuleren van niet-hoofdletter schrijf patronen voor werk belastingen.

| Workload | I/O-grootte (KB) | % Access | % Lezen | Openstaande I/O's | I/O-patroon |
| --- | --- | --- | --- | --- | --- |
| Bestandsserver |4<br />8<br />16<br />32<br />64 |60%<br />20%<br />5%<br />5%<br />10% |80%<br />80%<br />80%<br />80%<br />80% |8<br />8<br />8<br />8<br />8 |Alle 100% wille keurig |
| SQL Server (volume 1)<br />SQL Server (volume 2) |8<br />64 |100%<br />100% |70%<br />0% |8<br />8 |100% wille keurig<br />100% sequentieel |
| Exchange |32 |100% |67% |8 |100% wille keurig |
| Workstation/VDI |4<br />64 |66%<br />34% |70%<br />95% |1<br />1 |Beide 100% wille keurig |
| Web-Bestands server |4<br />8<br />64 |33%<br />34%<br />33% |95%<br />95%<br />95% |8<br />8<br />8 |Alle 75% wille keurig |

### <a name="vm-configuration"></a>VM-configuratie

* 470 Vm's op het primaire cluster.
* Alle Vm's met VHDX-schijf.
* Virtuele machines waarop werk belastingen worden uitgevoerd, worden in de tabel samenvatten. Alle zijn met VMM-sjablonen gemaakt.

| Workload | Aantal Vm's | Mini maal RAM-geheugen (GB) | Maximum-RAM (GB) | Grootte van logische schijf (GB) per VM | Maximum aantal IOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| Bestandsserver |50 |1 |2 |552 |22 |
| VDI |149 |.5 |1 |80 |6 |
| Webserver |149 |.5 |1 |80 |6 |
| TOTAAL |470 | | |96,83 TB |4108 |

### <a name="site-recovery-settings"></a>Site Recovery instellingen

* Site Recovery is geconfigureerd voor on-premises naar on-premises beveiliging
* Op de VMM-server zijn vier Clouds geconfigureerd, met de Hyper-V-cluster servers en de bijbehorende Vm's.

| Primaire VMM-Cloud | Beveiligde VM's | Replicatie frequentie | Aanvullende herstel punten |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 minuten |Geen |
| PrimaryCloudRpo30s |47 |30 seconden |Geen |
| PrimaryCloudRpo30sArp1 |47 |30 seconden |1 |
| PrimaryCloudRpo5m |235 |5 minuten |Geen |

### <a name="performance-metrics"></a>Metrische gegevens voor prestaties

De tabel bevat een overzicht van de prestatie gegevens en prestatie meter items die in de implementatie zijn gemeten.

| Gegevens | Teller |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Available memory |\Memory\Available MBytes |
| IOPS |\PhysicalDisk(_Total)\Disk Transfers/sec |
| VM read (IOPS) operations/sec |\Hyper-V Virtual Storage Device(\<VHD>)\Read Operations/Sec |
| VM write (IOPS) operations/sec |\Hyper-V Virtual Storage Device(\<VHD>)\Write Operations/S |
| VM read throughput |\Hyper-V Virtual Storage Device(\<VHD>)\Read Bytes/sec |
| VM write throughput |\Hyper-V Virtual Storage Device(\<VHD>)\Write Bytes/sec |

## Next steps

[Set up replication](hyper-v-vmm-disaster-recovery.md)\Processor(_Total)\` Processor Timeation of Hyper-V VMs in VMM clouds to a secondary site with Azure Site Recovery | Microsoft Docs
description: This article provides information about performance testing for replication of Hyper-V VMs in VMM clouds to a secondary site using Azure Site Recovery.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sutalasi

---
# Test results for Hyper-V replication to a secondary site


This article provides the results of performance testing when replicating Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds, to a secondary datacenter.

## Test goals

The goal of testing was to examine how Site Recovery performs during steady state replication.

- Steady state replication occurs when VMs have completed initial replication, and are synchronizing delta changes.
- It’s important to measure performance using steady state, because it’s the state in which most VMs remain, unless unexpected outages occur.
- The test deployment consisted of two on-premises sites, with a VMM server in each site. This test deployment is typical of a head office/branch office deployment, with head office acting as the primary site, and the branch office as the secondary or recovery site.

## What we did

Here's what we did in the test pass:

1. Created VMs using VMM templates.
2. Started VMs, and captured baseline performance metrics over 12 hours.
3. Created clouds on the primary and recovery VMM servers.
4. Configured replication in Site Recovery, including mapping between source and recovery clouds.
5. Enabled protection for VMs, and allowed them to complete initial replication.
6. Waited a couple of hours for system stabilization.
7. Captured performance metrics over 12 hours, where all VMs remained in an expected replication state for those 12 hours.
8. Measured the delta between the baseline performance metrics, and the replication performance metrics.


## Primary server performance

* Hyper-V Replica (used by Site Recovery) asynchronously tracks changes to a log file, with minimum storage overhead on the primary server.
* Hyper-V Replica utilizes self-maintained memory cache to minimize IOPS overhead for tracking. It stores writes to the VHDX in memory, and flushes them into the log file before the time that the log is sent to the recovery site. A disk flush also happens if the writes hit a predetermined limit.
* The graph below shows the steady state IOPS overhead for replication. We can see that the IOPS overhead due to replication is around 5%, which is quite low.

  ![Primary results](./media/hyper-v-vmm-performance-results/IC744913.png)

Hyper-V Replica uses memory on the primary server, to optimize disk performance. As shown in the following graph, memory overhead on all servers in the primary cluster is marginal. The memory overhead shown is the percentage of memory used by replication, compared to the total installed memory on the Hyper-V server.

![Primary results](./media/hyper-v-vmm-performance-results/IC744914.png)

Hyper-V Replica has minimum CPU overhead. As shown in the graph, replication overhead is in the range of 2-3%.

![Primary results](./media/hyper-v-vmm-performance-results/IC744915.png)

## Secondary server performance

Hyper-V Replica uses a small amount of memory on the recovery server, to optimize the number of storage operations. The graph summarizes the memory usage on the recovery server. The memory overhead shown is the percentage of memory used by replication, compared to the total installed memory on the Hyper-V server.

![Secondary results](./media/hyper-v-vmm-performance-results/IC744916.png)

The amount of I/O operations on the recovery site is a function of the number of write operations on the primary site. Let’s look at the total I/O operations on the recovery site in comparison with the total I/O operations and write operations on the primary site. The graphs show that the total IOPS on the recovery site is

* Around 1.5 times the write IOPS on the primary.
* Around 37% of the total IOPS on the primary site.

![Secondary results](./media/hyper-v-vmm-performance-results/IC744917.png)

![Secondary results](./media/hyper-v-vmm-performance-results/IC744918.png)

## Effect on network utilization

An average of 275 Mb per second of network bandwidth was used between the primary and recovery nodes (with compression enabled), against an existing bandwidth of 5 Gb per second.

![Results network utilization](./media/hyper-v-vmm-performance-results/IC744919.png)

## Effect on VM performance

An important consideration is the impact of replication on production workloads running on the virtual machines. If the primary site is adequately provisioned for replication, there shouldn’t be any impact on the workloads. Hyper-V Replica’s lightweight tracking mechanism ensures that workloads running in the virtual machines are not impacted during steady-state replication. This is illustrated in the following graphs.

This graph shows IOPS performed by virtual machines running different workloads, before and after replication was enabled. You can observe that there is no difference between the two.

![Replica effect results](./media/hyper-v-vmm-performance-results/IC744920.png)

The following graph shows the throughput of virtual machines running different workloads, before and after replication was enabled. You can observe that replication has no significant impact.

![Results replica effects](./media/hyper-v-vmm-performance-results/IC744921.png)

## Conclusion

The results clearly show that Site Recovery, coupled with Hyper-V Replica, scales well with minimum overhead for a large cluster. Site Recovery provides simple deployment, replication, management and monitoring. Hyper-V Replica provides the necessary infrastructure for successful replication scaling. 

## Test environment details

### Primary site

* The primary site has a cluster containing five Hyper-V servers, running 470 virtual machines.
* The VMs run different workloads, and all have Site Recovery protection enabled.
* Storage for the cluster node is provided by an iSCSI SAN. Model – Hitachi HUS130.
* Each cluster server has four network cards (NICs) of one Gbps each.
* Two of the network cards are connected to an iSCSI private network, and two are connected to an external enterprise network. One of the external networks is reserved for cluster communications only.

![Primary hardware requirements](./media/hyper-v-vmm-performance-results/IC744922.png)

| Server | RAM | Model | Processor | Number of processors | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V servers in cluster: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |128<br />ESTLAB-HOST25 has 256 |Dell ™ PowerEdge ™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2.20GHz |4 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| VMM Server |2 | | |2 |1 Gbps |Windows Server Database 2012 R2 (x64) + VMM 2012 R2 |

### Secondary site

* The secondary site has a six-node failover cluster.
* Storage for the cluster node is provided by an iSCSI SAN. Model – Hitachi HUS130.

![Primary hardware specification](./media/hyper-v-vmm-performance-results/IC744923.png)

| Server | RAM | Model | Processor | Number of processors | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V servers in cluster: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell ™ PowerEdge ™ R720 |Intel(R) Xeon(R) CPU E5-2630 0 \@ 2.30GHz |2 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| ESTLAB-HOST17 |128 |Dell ™ PowerEdge ™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2.20GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| ESTLAB-HOST24 |256 |Dell ™ PowerEdge ™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2.20GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| VMM Server |2 | | |2 |1 Gbps |Windows Server Database 2012 R2 (x64) + VMM 2012 R2 |

### Server workloads

* For test purposes we picked workloads commonly used in enterprise customer scenarios.
* We use [IOMeter](http://www.iometer.org) with the workload characteristic summarized in the table for simulation.
* All IOMeter profiles are set to write random bytes to simulate worst-case write patterns for workloads.

| Workload | I/O size (KB) | % Access | %Read | Outstanding I/Os | I/O pattern |
| --- | --- | --- | --- | --- | --- |
| File Server |4<br />8<br />16<br />32<br />64 |60%<br />20%<br />5%<br />5%<br />10% |80%<br />80%<br />80%<br />80%<br />80% |8<br />8<br />8<br />8<br />8 |All 100% random |
| SQL Server (volume 1)<br />SQL Server (volume 2) |8<br />64 |100%<br />100% |70%<br />0% |8<br />8 |100% random<br />100% sequential |
| Exchange |32 |100% |67% |8 |100% random |
| Workstation/VDI |4<br />64 |66%<br />34% |70%<br />95% |1<br />1 |Both 100% random |
| Web File Server |4<br />8<br />64 |33%<br />34%<br />33% |95%<br />95%<br />95% |8<br />8<br />8 |All 75% random |

### VM configuration

* 470 VMs on the primary cluster.
* All VMs with VHDX disk.
* VMs running workloads summarized in the table. All were created with VMM templates.

| Workload | # VMs | Minimum RAM (GB) | Maximum RAM (GB) | Logical disk size (GB) per VM | Maximum IOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| File Server |50 |1 |2 |552 |22 |
| VDI |149 |.5 |1 |80 |6 |
| Web server |149 |.5 |1 |80 |6 |
| TOTAL |470 | | |96.83 TB |4108 |

### Site Recovery settings

* Site Recovery was configured for on-premises to on-premises protection
* The VMM server has four clouds configured, containing the Hyper-V cluster servers and their VMs.

| Primary VMM cloud | Protected VMs | Replication frequency | Additional recovery points |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 mins |None |
| PrimaryCloudRpo30s |47 |30 secs |None |
| PrimaryCloudRpo30sArp1 |47 |30 secs |1 |
| PrimaryCloudRpo5m |235 |5 mins |None |

### Performance metrics

The table summarizes the performance metrics and counters that were measured in the deployment.

| Metric | Counter |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Beschikbaar geheugen |\Memory\Available MBytes |
| IOPS |\PhysicalDisk(_Total)\Disk Transfers/sec |
| Gelezen VM-bewerkingen (IOPS) per seconde |\Hyper-V virtuele-opslag apparaat\<(VHD >) \Read bewerkingen per seconde |
| Bewerkingen voor het schrijven van VM'S (IOPS) per seconde |\Hyper-V virtuele-opslag apparaat\<(VHD >) \Write-bewerkingen/S |
| Lees doorvoer van VM |\Hyper-V (\<VHD >) \Read bytes per seconde |
| Schrijf doorvoer VM |\Hyper-V (\<VHD >) \Write bytes per seconde |

## <a name="next-steps"></a>Volgende stappen

[Replicatie instellen](hyper-v-vmm-disaster-recovery.md)
