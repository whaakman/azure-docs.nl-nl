---
title: Testresultaten voor replicatie van Hyper-V-machines in VMM-clouds naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: In dit artikel bevat informatie over de prestatietests voor replicatie van Hyper-V virtuele machines in VMM-clouds naar een secundaire site met Azure Site Recovery.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sutalasi
ms.openlocfilehash: 62137f64f0f138ea1c2dfbdf97bde791227617db
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793033"
---
# <a name="test-results-for-hyper-v-replication-to-a-secondary-site"></a>Resultaten van belastingstests voor Hyper-V-replicatie naar een secundaire site


Dit artikel bevat de resultaten van de prestaties bij het Hyper-V-machines in System Center Virtual Machine Manager (VMM)-clouds, repliceren naar een secundair datacenter te testen.

## <a name="test-goals"></a>Doelstellingen testen

Het doel van het testen is om te onderzoeken hoe de Site Recovery tijdens de onveranderlijke replicatie uitvoert.

- Onveranderlijke replicatie treedt op wanneer virtuele machines eerste replicatie is voltooid en wijzigingen worden gesynchroniseerd.
- Dit is belangrijk om te meten van prestaties met stabiele status, omdat deze de status waarin de meeste VM's blijven ongewijzigd, tenzij er onverwachte storingen optreden.
- De testimplementatie bestaat uit twee on-premises locaties, met een VMM-server op elke site. Deze testimplementatie is gebruikelijk van een implementatie van de office head kantoren/filialen met hoofdkantoor die fungeert als de primaire site en het filiaal als de site van secundaire of herstel.

## <a name="what-we-did"></a>Wat we hebben gedaan

Dit is wat we is in de test geslaagd:

1. Virtuele machines met behulp van VMM-sjablonen.
2. Virtuele machines gestart en maatstaven voor prestaties van basislijn meer dan 12 uur vastgelegd.
3. Gemaakte clouds op de primaire en het herstel-VMM-servers.
4. Geconfigureerde replicatie in Site Recovery, met inbegrip van toewijzing tussen de bron- en herstelinstellingen clouds.
5. Beveiliging voor virtuele machines ingeschakeld en mag deze initiële replicatie is voltooid.
6. Een paar uur voor systeem stabilization gewacht.
7. Metrische gegevens voor prestaties meer dan 12 uur, waarbij alle virtuele machines bleef een verwachte replicatiestatus voor deze 12 uur vastgelegd.
8. De verschillen tussen de basislijn-maatstaven voor prestaties en de maatstaven voor prestaties van replicatie wordt gemeten.


## <a name="primary-server-performance"></a>Prestaties van de primaire server

* Hyper-V Replica (gebruikt door Site Recovery) asynchroon houdt wijzigingen in een logboekbestand, met minimale storage overhead op de primaire server.
* Hyper-V Replica maakt gebruik van zelf onderhouden geheugencache om te minimaliseren IOPS overhead voor het bijhouden. Opgeslagen schrijft de VHDX in het geheugen en schrijft ze in het logboekbestand voor de tijd die het logboek wordt verzonden naar de herstelsite. Een diskette leegmaken gebeurt ook als de schrijfbewerkingen een vooraf vastgestelde limiet bereikt.
* Het onderstaande diagram ziet u de overhead van de IOPS onveranderlijke voor replicatie. U ziet dat de IOPS overhead vanwege replicatie is ongeveer 5%, dit klein is.

  ![Primaire resultaten](./media/hyper-v-vmm-performance-results/IC744913.png)

Hyper-V Replica gebruikt geheugen op de primaire server, het optimaliseren van de schijf. Zoals in het volgende diagram wordt weergegeven, is het geheugen overhead op alle servers in de primaire cluster marginale. De overhead geheugen is de hoeveelheid geheugen die wordt gebruikt door replicatie, vergeleken met het totale geïnstalleerd geheugen op de Hyper-V-server.

![Primaire resultaten](./media/hyper-v-vmm-performance-results/IC744914.png)

Hyper-V Replica heeft minimale CPU-overhead. Zoals u in de grafiek, is replicatieoverhead in het bereik van 2-3%.

![Primaire resultaten](./media/hyper-v-vmm-performance-results/IC744915.png)

## <a name="secondary-server-performance"></a>Prestaties van de secundaire server

Hyper-V Replica maakt gebruik van een kleine hoeveelheid geheugen op de herstelserver, het optimaliseren van het aantal opslagbewerkingen. De grafiek bevat een overzicht van het geheugengebruik op de herstelserver. De overhead geheugen is de hoeveelheid geheugen die wordt gebruikt door replicatie, vergeleken met het totale geïnstalleerd geheugen op de Hyper-V-server.

![Secundaire resultaten](./media/hyper-v-vmm-performance-results/IC744916.png)

Het bedrag van i/o-bewerkingen op de site recovery is een functie van het aantal schrijfbewerkingen op de primaire site. Laten we kijken naar de totale i/o-bewerkingen op de site recovery in vergelijking met de totale i/o-bewerkingen en schrijfbewerkingen op de primaire site. De grafieken ziet u dat de totale IOP's op de site recovery is

* Ongeveer 1,5 keer de IOPS voor schrijven op de primaire.
* Ongeveer 37% van de totale IOP's op de primaire site.

![Secundaire resultaten](./media/hyper-v-vmm-performance-results/IC744917.png)

![Secundaire resultaten](./media/hyper-v-vmm-performance-results/IC744918.png)

## <a name="effect-on-network-utilization"></a>Effect op het netwerkgebruik

Een gemiddelde van 275 Mb per seconde van de netwerkbandbreedte is tussen de primaire en knooppunten (met compressie ingeschakeld) wordt gebruikt op basis van een bestaande bandbreedte van 5 Gb per seconde.

![Netwerkgebruik resultaten](./media/hyper-v-vmm-performance-results/IC744919.png)

## <a name="effect-on-vm-performance"></a>Effect op de prestaties van de virtuele machine

Een belangrijk aandachtspunt is de impact van replicatie op de virtuele machines waarop werkbelastingen voor productie. Als de primaire site naar behoren is ingericht voor replicatie, mogen er zich niet dit van invloed is op de werkbelasting. Hyper-V-Replica lightweight bijhouden mechanisme zorgt ervoor dat workloads die worden uitgevoerd in de virtuele machines worden niet beïnvloed tijdens de onveranderlijke replicatie. Dit wordt geïllustreerd in de volgende grafieken.

Deze grafiek toont dat IOPS uitgevoerd door virtuele machines met verschillende workloads, voordat en nadat de replicatie is ingeschakeld. U kunt zien dat er geen verschil tussen de twee is.

![Replica effect resultaten](./media/hyper-v-vmm-performance-results/IC744920.png)

In het volgende diagram ziet u de doorvoer van virtuele machines met verschillende workloads, voordat en nadat de replicatie is ingeschakeld. U kunt zien dat de replicatie heeft geen aanzienlijke invloed.

![Resultaten replica effecten](./media/hyper-v-vmm-performance-results/IC744921.png)

## <a name="conclusion"></a>Conclusie

De resultaten blijkt duidelijk dat Site Recovery, in combinatie met Hyper-V Replica, kan worden geschaald en met minimale overhead voor een groot cluster. Site Recovery biedt een eenvoudige implementatie, replicatie, beheer en bewaking. Hyper-V Replica biedt de benodigde infrastructuur voor het schalen van geslaagde replicatie. 

## <a name="test-environment-details"></a>Details van de omgeving testen

### <a name="primary-site"></a>Primaire site

* De primaire site heeft een cluster met vijf Hyper-V-servers, 470 virtuele machines worden uitgevoerd.
* De virtuele machines uitvoeren in verschillende workloads en Site Recovery-beveiliging ingeschakeld hebben.
* Opslag voor het clusterknooppunt wordt geleverd door een iSCSI-SAN. Model – Hitachi HUS130.
* Elke clusterserver heeft vier netwerkkaarten (NIC's) van een Gbps.
* Twee van de netwerkkaarten zijn verbonden met een particulier netwerk voor iSCSI- en twee zijn verbonden met een externe bedrijfsnetwerk. Een van de externe netwerken is gereserveerd voor clustercommunicatie alleen.

![Primaire hardwarevereisten](./media/hyper-v-vmm-performance-results/IC744922.png)

| Server | RAM | Model | Processor | Aantal processors | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V-servers in het cluster: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |128ESTLAB HOST25 is 256 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2,20 GHz |4 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rol |
| VMM-Server |2 | | |2 |1 Gbps |Windows Server-Database 2012 R2 (x 64) en VMM 2012 R2 |

### <a name="secondary-site"></a>Secundaire site

* De secundaire site heeft een failovercluster met zes knooppunten.
* Opslag voor het clusterknooppunt wordt geleverd door een iSCSI-SAN. Model – Hitachi HUS130.

![Primaire hardwarespecificaties](./media/hyper-v-vmm-performance-results/IC744923.png)

| Server | RAM | Model | Processor | Aantal processors | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V-servers in het cluster: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell™ PowerEdge™ R720 |Intel(R) Xeon(R) CPU E5-2630 0 \@ 2,30 GHz |2 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rol |
| ESTLAB-HOST17 |128 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2,20 GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rol |
| ESTLAB-HOST24 |256 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2,20 GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rol |
| VMM-Server |2 | | |2 |1 Gbps |Windows Server-Database 2012 R2 (x 64) en VMM 2012 R2 |

### <a name="server-workloads"></a>Server-workloads

* Voor test-doeleinden we workloads die vaak worden gebruikt in zakelijke klant scenario's opgehaald.
* We gebruiken [IOMeter](http://www.iometer.org) met de werkbelasting kenmerk samengevat in de tabel voor de simulatie.
* Alle IOMeter profielen zijn ingesteld op het schrijven van willekeurige bytes voor het simuleren van ergste schrijven patronen voor workloads.

| Workload | I/o-grootte (KB) | % Toegang | % Lezen | Openstaande i/o 's | I/o-patroon |
| --- | --- | --- | --- | --- | --- |
| Bestandsserver |48163264 |60%20%5%5%10% |80%80%80%80%80% |88888 |Alle 100% willekeurige |
| SQL Server (volume 1) van SQL Server (volume 2) |864 |100%100% |70%0% |88 |100% random100% opeenvolgende |
| Exchange |32 |100% |67% |8 |100% willekeurige |
| Werkstation/VDI |464 |66%34% |70%95% |11 |Beide willekeurige 100% |
| Web-bestandsserver |4864 |33%34%33% |95%95%95% |888 |Alle 75% willekeurige |

### <a name="vm-configuration"></a>VM-configuratie

* 470 VM's op de primaire-cluster.
* Alle virtuele machines met VHDX-schijftypen.
* VM's het uitvoeren van workloads in de tabel wordt samengevat. Alle zijn gemaakt met de VMM-sjablonen.

| Workload | # VM 's | Minimaal RAM (GB) | Maximum-RAM (GB) | Grootte van logische schijf (GB) per VM | Maximale IOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| Bestandsserver |50 |1 |2 |552 |22 |
| VDI |149 |.5 |1 |80 |6 |
| Webserver |149 |.5 |1 |80 |6 |
| TOTAAL |470 | | |96.83 TB |4108 |

### <a name="site-recovery-settings"></a>Site Recovery-instellingen

* Site Recovery is geconfigureerd voor on-premises naar on-premises bescherming
* De VMM-server heeft vier clouds zijn geconfigureerd, met de servers met Hyper-V-cluster en de bijbehorende virtuele machines.

| Primaire VMM-cloud | Beveiligde virtuele machines | Frequentie van replicatie | Aanvullende herstelpunten |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 minuten |Geen |
| PrimaryCloudRpo30s |47 |30 seconden |Geen |
| PrimaryCloudRpo30sArp1 |47 |30 seconden |1 |
| PrimaryCloudRpo5m |235 |5 minuten |Geen |

### <a name="performance-metrics"></a>Metrische gegevens voor prestaties

De tabel bevat een overzicht van de metrische gegevens voor prestaties en de items die zijn gemeten in de implementatie.

| Gegevens | Teller |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Beschikbaar geheugen |\Memory\Available megabytes (MB) |
| IOPS |\PhysicalDisk(_Total)\Disk Transfers/sec |
| Virtuele machine lezen (IOPS) per seconde |\Hyper-V virtueel opslagapparaat (<VHD>) \Read bewerkingen/Sec |
| Virtuele machine schrijfbewerkingen (IOPS) per seconde |\Hyper-V virtueel opslagapparaat (<VHD>) \Write bewerkingen/sec |
| VM-leesdoorvoer |\Hyper-V virtueel opslagapparaat (<VHD>) \Read Bytes per seconde |
| VM-schrijfbewerkingen |\Hyper-V virtueel opslagapparaat (<VHD>) \Write Bytes per seconde |

## <a name="next-steps"></a>Volgende stappen

[Replicatie instellen](hyper-v-vmm-disaster-recovery.md)
