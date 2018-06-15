---
title: Testresultaten voor replicatie van Hyper-V-machines in VMM-clouds naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: In dit artikel bevat informatie over de prestatietests voor replicatie van Hyper-V-machines in VMM-clouds naar een secundaire site met Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/13/2018
ms.author: raynew
ms.openlocfilehash: e15f435a3f32b8908b5b93bccc6c57710ab589bc
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
ms.locfileid: "29378895"
---
# <a name="test-results-for-hyper-v-replication-to-a-secondary-site"></a>Testresultaten voor Hyper-V-replicatie naar een secundaire site

Dit artikel bevat de resultaten van de prestaties bij het Hyper-V-machines in System Center Virtual Machine Manager (VMM)-clouds repliceren naar een secundair datacenter te testen.

## <a name="test-goals"></a>Doelstellingen testen

Het doel van de testen is om te onderzoeken hoe Site Recovery uitvoert tijdens de replicatie van de actieve status.

- Actieve status replicatie treedt op wanneer virtuele machines eerste replicatie is voltooid en deltawijzigingen synchroniseert.
- Dit is belangrijk om te meten prestaties met actieve status, omdat deze de status in welke meeste VM's blijven, tenzij er onverwachte storingen optreden.
- De testimplementatie bestond uit twee lokale sites, met een VMM-server op elke site. Deze testimplementatie is typerend van een head office/branch office-implementatie met hoofdkantoor fungeert als de primaire site en het filiaal als de site secundaire of herstel.

## <a name="what-we-did"></a>Wat er gedaan

Dit is wat er is in de test geslaagd:

1. Gemaakte virtuele machines met behulp van sjablonen in VMM.
2. Virtuele machines gestart en vastgelegd maatstaven voor prestaties van basislijn meer dan 12 uur.
3. Gemaakte clouds op de primaire en herstel-VMM-servers.
4. Geconfigureerde replicatie in Site Recovery, met inbegrip van toewijzing tussen bron- en herstelinstellingen clouds.
5. Beveiliging voor VM's ingeschakeld en kunnen initiële replicatie is voltooid.
6. Een paar uur voor systeem stabilization gewacht.
7. Vastgelegd maatstaven voor prestaties van meer dan 12 uur, waarbij alle VM's bleef een verwachte replicatiestatus voor deze 12 uur.
8. De verschillen tussen de basislijn-maatstaven voor prestaties en de maatstaven voor prestaties van replicatie wordt gemeten.


## <a name="primary-server-performance"></a>Prestaties van de primaire server

* Hyper-V Replica (gebruikt door Site Recovery) asynchroon houdt wijzigingen in een logboekbestand met minimale opslag overhead op de primaire server.
* Hyper-V Replica maakt gebruik van zelf handhaven geheugencache om te minimaliseren IOPS overhead voor bijhouden. Hierin worden opgeslagen schrijft de VHDX in het geheugen en schrijft ze in het logboekbestand voor de tijd die het logboek wordt verzonden naar de site recovery. Een schijf leegmaken gebeurt ook als een vooraf vastgestelde limiet bereikt, wordt de geschreven.
* Het volgende diagram toont de actieve status IOPS-overhead voor replicatie. U ziet dat de IOPS overhead als gevolg van replicatie is ongeveer 5% klein.

  ![Primaire resultaten](./media/hyper-v-vmm-performance-results/IC744913.png)

Hyper-V Replica gebruikt geheugen op de primaire server en schijfprestaties te optimaliseren. Zoals in de volgende grafiek wordt weergegeven, is het geheugen overhead op alle servers in de primaire cluster marginale. De overhead geheugen is het percentage van het geheugen gebruikt door replicatie, vergeleken met het totaal geïnstalleerde geheugen op de Hyper-V-server.

![Primaire resultaten](./media/hyper-v-vmm-performance-results/IC744914.png)

Hyper-V Replica heeft minimale CPU-overhead. Zoals in de grafiek wordt weergegeven, is het replicatieoverhead in het bereik van % 2-3.

![Primaire resultaten](./media/hyper-v-vmm-performance-results/IC744915.png)

## <a name="secondary-server-performance"></a>Prestaties van de secundaire server

Hyper-V Replica maakt gebruik van een kleine hoeveelheid geheugen op de herstelserver, het aantal opslagbewerkingen te optimaliseren. De grafiek bevat een overzicht van het geheugengebruik op de herstelserver. De overhead geheugen is het percentage van het geheugen gebruikt door replicatie, vergeleken met het totaal geïnstalleerde geheugen op de Hyper-V-server.

![Secundaire resultaten](./media/hyper-v-vmm-performance-results/IC744916.png)

De hoeveelheid i/o-bewerkingen op de site recovery is een functie van het aantal schrijfbewerkingen op de primaire site. Laten we kijken naar de totale i/o-bewerkingen op de herstelsite ten opzichte van de totale i/o-bewerkingen en schrijfbewerkingen op de primaire site. De grafieken laat zien dat het totale aantal IOPS op de site recovery

* Ongeveer 1,5 keer de schrijfbewerking IOPS op de primaire.
* Ongeveer 37% van het totale aantal IOPS dat op de primaire site.

![Secundaire resultaten](./media/hyper-v-vmm-performance-results/IC744917.png)

![Secundaire resultaten](./media/hyper-v-vmm-performance-results/IC744918.png)

## <a name="effect-on-network-utilization"></a>Effect op netwerkgebruik

Een gemiddelde van 275 Mb per seconde van de netwerkbandbreedte is tussen de primaire en herstelsites knooppunten (met gebruik van compressie), gebruikt met een bestaande bandbreedte van 5 Gb per seconde.

![Netwerkgebruik resultaten](./media/hyper-v-vmm-performance-results/IC744919.png)

## <a name="effect-on-vm-performance"></a>Effect op de prestaties van de virtuele machine

Een belangrijk aandachtspunt is de impact van replicatie op de productieworkloads die worden uitgevoerd op de virtuele machines. Als de primaire site correct is geconfigureerd voor replicatie, mogen er zich niet invloed op de werkbelastingen. Hyper-V-Replica lightweight bijhouden mechanisme zorgt ervoor werkbelastingen in de virtuele machines niet negatief beïnvloed tijdens de replicatie van de actieve status. Dit wordt geïllustreerd in de volgende grafieken.

Deze grafiek toont dat IOP 's uitgevoerd door virtuele machines van verschillende werkbelastingen uitgevoerd voordat en nadat de replicatie is ingeschakeld. U kunt zien dat er geen verschil tussen de twee is.

![Replica effect resultaten](./media/hyper-v-vmm-performance-results/IC744920.png)

De volgende grafiek ziet de doorvoer van virtuele machines met verschillende werkbelastingen voordat en nadat de replicatie is ingeschakeld. U kunt zien dat de replicatie heeft geen aanzienlijke invloed.

![Resultaten replica effecten](./media/hyper-v-vmm-performance-results/IC744921.png)

## <a name="conclusion"></a>Conclusie

De resultaten duidelijk dat Site is hersteld, samen met Hyper-V Replica ook met minimale overhead voor een grote cluster schaalt. Site Recovery biedt een eenvoudige implementatie, replicatie, beheer en bewaking. Hyper-V Replica bevat de benodigde infrastructuur voor het schalen van geslaagde replicatie. 

## <a name="test-environment-details"></a>Details van de omgeving

### <a name="primary-site"></a>Primaire site

* De primaire site heeft een cluster met vijf Hyper-V-servers, 470 virtuele machines worden uitgevoerd.
* De virtuele machines uitvoeren verschillende werkbelastingen en alle is de Site Recovery-beveiliging ingeschakeld.
* Opslag voor het clusterknooppunt wordt verstrekt door een iSCSI-SAN. Model – Hitachi HUS130.
* Elke clusterserver heeft vier netwerkkaarten (NIC's) van een Gbps.
* Twee van de netwerkkaarten zijn verbonden met een iSCSI-particulier netwerk en twee zijn verbonden met een externe bedrijfsnetwerk. Een van de externe netwerken is gereserveerd voor alleen-clustercommunicatie is ingericht.

![Primaire hardwarevereisten](./media/hyper-v-vmm-performance-results/IC744922.png)

| Server | RAM | Model | Processor | Aantal processors | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V-servers in het cluster: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |128ESTLAB HOST25 is 256 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 @ 2.20GHz |4 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-functie |
| VMM Server |2 | | |2 |1 Gbps |Windows Server-Database 2012 R2 (x 64) + VMM 2012 R2 |

### <a name="secondary-site"></a>Secundaire site

* De secundaire site heeft een failovercluster met zes knooppunten.
* Opslag voor het clusterknooppunt wordt verstrekt door een iSCSI-SAN. Model – Hitachi HUS130.

![Primaire hardwarespecificaties](./media/hyper-v-vmm-performance-results/IC744923.png)

| Server | RAM | Model | Processor | Aantal processors | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V-servers in het cluster: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell™ PowerEdge™ R720 |Intel(R) Xeon(R) CPU E5-2630 0 @ 2.30GHz |2 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-functie |
| ESTLAB-HOST17 |128 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 @ 2.20GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-functie |
| ESTLAB-HOST24 |256 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 @ 2.20GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-functie |
| VMM Server |2 | | |2 |1 Gbps |Windows Server-Database 2012 R2 (x 64) + VMM 2012 R2 |

### <a name="server-workloads"></a>Server-werkbelastingen

* We gekozen workloads die vaak worden gebruikt in zakelijke klant scenario's voor testdoeleinden.
* We gebruiken [IOMeter](http://www.iometer.org) met de werkbelasting kenmerk samengevat in de tabel voor de simulatie.
* Alle IOMeter profielen zijn ingesteld om te schrijven te simuleren slechtste willekeurige bytes schrijven patronen voor werkbelastingen.

| Workload | I/o-grootte (KB) | % Toegang | % Lezen | Openstaande i/o 's | I/o-patroon |
| --- | --- | --- | --- | --- | --- |
| Bestandsserver |48163264 |60%20%5%5%10% |80%80%80%80%80% |88888 |Alle 100% willekeurige |
| SQL Server (volume 1) SQL Server (volume 2) |864 |100%100% |70%0% |88 |100% random100% sequentiële |
| Exchange |32 |100% |67% |8 |100% willekeurige |
| Werkstation/VDI |464 |66%34% |70%95% |11 |Beide willekeurige 100% |
| Web-bestandsserver |4864 |33%34%33% |95%95%95% |888 |Alle 75% willekeurige |

### <a name="vm-configuration"></a>VM-configuratie

* 470 virtuele machines op de primaire cluster.
* Alle virtuele machines met VHDX-schijf.
* Virtuele machines waarop werkbelastingen samengevat in de tabel worden uitgevoerd. Alle zijn gemaakt met de VMM-sjablonen.

| Workload | # Virtuele machines | Minimum RAM (GB) | Maximum-RAM (GB) | Grootte van logische schijf (GB) per VM | Maximale IOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| Bestandsserver |50 |1 |2 |552 |22 |
| VDI |149 |.5 |1 |80 |6 |
| Webserver |149 |.5 |1 |80 |6 |
| TOTAAL |470 | | |96.83 TB |4108 |

### <a name="site-recovery-settings"></a>Site Recovery-instellingen

* Site Recovery is geconfigureerd voor on-premises naar on-premises beveiliging
* De VMM-server heeft vier clouds zijn geconfigureerd, met de Hyper-V-cluster-servers en de bijbehorende virtuele machines.

| Primaire VMM-cloud | Beveiligde virtuele machines | Replicatiefrequentie | Extra herstelpunten |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 minuten |Geen |
| PrimaryCloudRpo30s |47 |30 seconden |None |
| PrimaryCloudRpo30sArp1 |47 |30 seconden |1 |
| PrimaryCloudRpo5m |235 |5 minuten |None |

### <a name="performance-metrics"></a>Metrische gegevens voor prestaties

De tabel ziet u de maatstaven voor prestaties en de items die zijn gemeten in de implementatie.

| Gegevens | Prestatiemeteritems |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Beschikbaar geheugen |\Memory\Available MBytes |
| IOPS |\PhysicalDisk(_Total)\Disk Transfers/sec |
| Virtuele machine lezen (IOPS)-bewerkingen per seconde |\Hyper-V virtueel opslagapparaat (<VHD>) \Read bewerkingen per seconde |
| VM geschreven (IOPS)-bewerkingen per seconde |\Hyper-V virtueel opslagapparaat (<VHD>) \Write Operations/S |
| VM doorvoer lezen |\Hyper-V virtueel opslagapparaat (<VHD>) \Read Bytes per seconde |
| VM-schrijfbewerkingen |\Hyper-V virtueel opslagapparaat (<VHD>) \Write Bytes per seconde |

## <a name="next-steps"></a>Volgende stappen

[De replicatie instellen](hyper-v-vmm-disaster-recovery.md)
