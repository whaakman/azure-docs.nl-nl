---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: include
ms.date: 06/25/2019
ms.author: azcspmt;ayshak;cynthn
ms.custom: include file
ms.openlocfilehash: 6a3e2034792fdc0a4a8fed7885c7d5ad78ea24d9
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67501227"
---
De B-serie VM-reeks kunt u kiezen welke VM-grootte biedt u de benodigde basisniveau prestaties voor uw werkbelasting, de mogelijkheid om uit te breiden CPU-prestaties tot 100% van een Intel® Broadwell E5-2673 v4-processors 2,3 GHz, of een Intel® Haswell 2,4 GHz E5-2673 v3-processor vCPU.

De B-serie VM's zijn ideaal voor workloads die niet de volledige prestaties van de CPU, zoals webservers, bewijs van concepten, kleine databases en ontwikkeling build-omgevingen hoeft. Deze werkbelastingen hebben meestal ' burstable ' prestatie-eisen. De B-serie biedt u de mogelijkheid om aan te schaffen van een VM-grootte met basislijn en het VM-exemplaar maakt tegoed bij het gebruik van minder dan de basislijn. Wanneer de virtuele machine zijn tegoed verzameld, kan de virtuele machine burst boven de basislijn met maximaal 100% van de vCPU wanneer uw toepassing hogere CPU-prestaties vereist.

De B-serie wordt geleverd in de volgende VM-grootten:

| Size             | vCPU  | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Basis-CPU-prestaties van VM | Maximale CPU-prestaties van VM | Starttegoed | Tegoed gestort/uur | Maximaal gestorte tegoed | Max. aantal gegevensschijven | Maximale doorvoer voor opslag in cache en tijdelijke opslag: IOPS / MBps | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's |          
|---------------|-------------|----------------|----------------------------|-----------------------|--------------------|--------------------|--------------------|----------------|----------------------------------------|-------------------------------------------|-------------------------------------------|----------|
| Standard_B1ls<sup>1</sup>  | 1           | 0,5              | 4                          | 5%                   | 100%                   | 30                   | 3                  | 72            | 2                                      | 200 / 10                                  | 160 / 10                                  | 2  |
| Standard_B1s  | 1           | 1              | 4                          | 10%                   | 100%                   | 30                   | 6                  | 144            | 2                        | 400 / 10                                  | 320 / 10                                  | 2  |
| Standard_B1ms | 1           | 2              | 4                          | 20%                   | 100%                   | 30                   | 12                 | 288           | 2                         | 800 / 10                                  | 640 / 10                                  | 2  |
| Standard_B2s  | 2           | 4              | 8                          | 40%                   | 200%                   | 60                   | 24                 | 576            | 4                                      | 1600 / 15                                 | 1280 / 15                                 | 3  |
| Standard_B2ms | 2           | 8              | 16                         | 60%                   | 200%                   | 60                   | 36                 | 864            | 4                                      | 2400 / 22.5                               | 1920 / 22.5                               | 3  |
| Standard_B4ms | 4           | 16             | 32                         | 90%                   | 400%                   | 120                   | 54                 | 1296           | 8                                      | 3600 / 35                                 | 2880 / 35                                 | 4  |
| Standard_B8ms | 8           | 32             | 64                         | 135%                  | 800%                   | 240                   | 81                 | 1944           | 16                                     | 4320 / 50                                 | 4320 / 50                                 | 4  |
| Standard_B12ms | 12           | 48             | 96                         | 202%                  | 1200%                   | 360                   | 121                 | 2909           | 16                                     | 6480 / 75                                 | 4320 / 50                                  | 6  |
| Standard_B16ms | 16           | 64             | 128                         | 270%                  | 1600%                   | 480                   | 162                 | 3888           | 32                                     | 8640 / 100                                 | 4320 / 50                                 | 8  |
| Standard_B20ms | 20           | 80             | 160                         | 337%                  | 2000%                   | 600                   | 203                 | 4860           | 32                                     | 10800 / 125                                 | 4320 / 50                                 | 8  |

<sup>1</sup> B1ls wordt alleen ondersteund op Linux

## <a name="workload-example"></a>Voorbeeld van de werkbelasting

Houd rekening met een office-toepassing voor check-in/uit. De toepassing moet CPU bursts tijdens kantooruren, maar niet veel rekenkracht tijdens daluren. In dit voorbeeld is de werkbelasting vereist een 16vCPU virtuele machine met 64GiB aan RAM-geheugen efficiënt werken.

De tabel ziet u het per uur verkeersgegevens en de grafiek is een visuele representatie van dat verkeer.

B16 kenmerken:

Maximaal CPU-prestaties: 16vCPU * 100% = 1600%

Basislijn: 270%

![Grafiek van per uur verkeersgegevens](./media/virtual-machines-common-b-series-burstable/office-workload.png)

| Scenario | Time | CPU-verbruik (%) | Tegoed samengevoegd<sup>1</sup> | Tegoed beschikbaar |
| --- | --- | --- | --- | --- |
| B16ms implementatie | Implementatie | Implementatie  | 480 (initiële tegoed) | 480 |
| Geen verkeer | 0:00 | 0 | 162 | 642 |
| Geen verkeer | 1:00 | 0 | 162 | 804 |
| Geen verkeer | 2:00 | 0 | 162 | 966 |
| Geen verkeer | 3:00 | 0 | 162 | 1128 |
| Geen verkeer | 4:00 | 0 | 162 | 1290 |
| Geen verkeer | 5:00 | 0 | 162 | 1452 |
| Weinig verkeer | 6:00 | 270 | 0 | 1452 |
| Werknemers worden geleverd bij office (80% vCPU app vereist) | 7:00 | 1280 | -606 | 846 |
| Werknemers blijven binnenkort Office (80% vCPU app vereist) | 8:00 | 1280 | -606 | 240 |
| Weinig verkeer | 9:00 | 270 | 0 | 240 |
| Weinig verkeer | 10:00 | 100 | 102 | 342 |
| Weinig verkeer | 11:00 | 50 | 132 | 474 |
| Weinig verkeer | 12:00 | 100 | 102 | 576 |
| Weinig verkeer | 13:00 | 100 | 102 | 678 |
| Weinig verkeer | 14:00 | 50 | 132 | 810 |
| Weinig verkeer | 15:00 | 100 | 102 | 912 |
| Weinig verkeer | 16:00 | 100 | 102 | 1014 |
| Werknemers uitchecken (vCPU app behoeften 100%) | 17:00 | 1600 | -798 | 216 |
| Weinig verkeer | 18:00 | 270 | 0 | 216 |
| Weinig verkeer | 19:00 | 270 | 0 | 216 |
| Weinig verkeer | 20:00 | 50 | 132 | 348 |
| Weinig verkeer | 21:00 | 50 | 132 | 480 |
| Geen verkeer | 22:00 | 0 | 162 | 642 |
| Geen verkeer | 23:00 | 0 | 162 | 804 |

<sup>1</sup> tegoed samengevoegd /-tegoed gebruikt in een uur is gelijk aan: `((Base CPU perf of VM - CPU Usage) / 100) * 60 minutes`.  

Het uurtarief is voor een D16s_v3 met 16 vcpu's en 64 GiB geheugen $0,936 per uur (maandelijks $673.92) en voor B16ms met 16 vcpu's en 64 GiB geheugen het tarief is $0.794 per uur (maandelijks $547.86). <b> Dit resulteert in 15% besparing!</b>

## <a name="q--a"></a>Vragen en antwoorden

### <a name="q-how-do-you-get-135-baseline-performance-from-a-vm"></a>V: Hoe krijg u 135% basislijn-prestaties van een virtuele machine?
**A**: De % 135 wordt gedeeld tussen de 8 vCPU's die gezamenlijk de VM-grootte. Als uw toepassing gebruikmaakt van 4 van de 8 kernen batchverwerking gewerkt en elk van deze 4 vCPU's worden uitgevoerd bij gebruik van 30% zou de totale hoeveelheid VM-CPU-prestaties 120% gelijk.  Wat betekent dat uw virtuele machine krediet tijd op basis van de delta 15% van de basislijnprestaties van uw wilt bouwen.  Maar dit betekent ook dat wanneer u hebt tegoed beschikbaar waarmee dezelfde VM 100% van alle 8 vCPU kunt die virtuele machine een maximaal CPU-prestaties van 800 krijgen %.


### <a name="q-how-can-i-monitor-my-credit-balance-and-consumption"></a>V: Hoe kan ik mijn tegoed saldo en het verbruik controleren
**A**: We 2 nieuwe metrische gegevens inleiding in de komende weken de **tegoed** metrische gegevens kunt u bekijken hoeveel tegoed in uw virtuele machine heeft gestort en de **ConsumedCredit** metrische waarde wordt weergegeven hoeveel CPU-tegoed uw virtuele machine is vanuit de bank gebruikt.    U kunt zich deze metrische gegevens in het deelvenster metrische gegevens weergeven in de portal of programmatisch via de Azure Monitor-API's.

Zie voor meer informatie over toegang krijgen tot de metrische gegevens voor Azure [overzicht van metrische gegevens in Microsoft Azure](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="q-how-are-credits-accumulated"></a>V: Hoe kan ik tegoeden samengevoegd?
**A**: De tarieven voor opeenstapeling en het verbruik van VM zijn ingesteld dat een VM met het op exact het niveau van de algemene prestatiegegevens heeft een net opeenstapeling, noch verbruik van cloudbursting tegoed.  Een virtuele machine heeft een net toename in tegoed wanneer deze wordt uitgevoerd onder het niveau van de algemene prestatiegegevens en heeft een net afname in tegoed wanneer de virtuele machine wordt gebruikt door de CPU van meer dan de base prestatieniveau.

**Voorbeeld**:  Kan ik implementeren een virtuele machine met behulp van de grootte van de B1ms voor mijn kleine tijd en aanwezigheid database-toepassing. Deze grootte kan mijn toepassing maximaal 20% van een vCPU gebruiken als de basislijn die 0,2 tegoed per minuut die ik kan gebruiken of een bank. 

Mijn toepassing is bezet aan het begin en einde van de werkdag werknemers tussen 7:00-9:00 uur en 4:00-18:00 uur. In de andere 20 uur van de dag, mijn toepassing is doorgaans op inactief, alleen gebruiken 10% van de vCPU. Voor de niet-piekuren ik Verdien 0,2 tegoed per minuut, maar alleen gebruiken 0.l tegoed per minuut, zodat mijn virtuele machine wordt 0.1 x 60 = 6 bank tegoed per uur.  Voor de 20 uur dat ik buiten piektijden, zal ik 120 tegoed bank.  

Tijdens piekuren mijn toepassing berekent het gemiddelde gebruik van 60% vCPU, ik nog steeds 0,2 tegoed per minuut behalen, maar ik 0,6 tegoed per minuut in beslag nemen, voor een net kosten 0,4 tegoed een minuut of 0.4 x 60 = 24 tegoed per uur. Ik heb 4 uur per dag van piekgebruik, zodat hiervoor 4 x 24 uur per dag = 96 kosten-tegoed voor mijn piekgebruik.

Als ik het 120 tegoed die ik verdiend buiten piektijden nemen en aftrekken van het 96 tegoed die ik voor mijn piektijden gebruikt, bank kan ik een extra 24 tegoed per dag dat ik kan gebruiken voor andere pieken van activiteit.

### <a name="q-how-can-i-calculate-credits-accumulated-and-used"></a>V: Hoe kan ik tegoeden samengevoegd en gebruikt berekenen?
**A**: U kunt de volgende formule: 

(Basis CPU-prestaties van VM - CPU-gebruik) / 100 = tegoed bank of gebruik per minuut

bijvoorbeeld in bovenstaande exemplaar van de basislijn is 20% en als u 10% van de CPU u zijn opgebouwd (20-10%) / 100 = 0,1 tegoed per minuut.

### <a name="q-does-the-b-series-support-premium-storage-data-disks"></a>V: De B-serie biedt ondersteuning voor Premium-opslagschijven gegevens?
**A**: Ja, alle grootten van de B-serie ondersteunen nu gegevensschijven van de Premium-opslag.   
    
### <a name="q-why-is-my-remaining-credit-set-to-0-after-a-redeploy-or-a-stopstart"></a>V: Waarom wordt mijn resterende tegoed ingesteld op 0 na een opnieuw implementeren of een stoppen/starten?
**A** : Wanneer een virtuele machine is 'REDPLOYED' en de virtuele machine wordt verplaatst naar een ander knooppunt, het totale tegoed verloren. Als de virtuele machine gestopt/gestart is, maar op hetzelfde knooppunt blijft, wordt het totale tegoed bewaard in de virtuele machine. Wanneer de virtuele machine wordt gestart op een knooppunt vers, krijgt een starttegoed, voor Standard_B8ms 240 minuten is.
    
### <a name="q-what-happens-if-i-deploy-an-unsupported-os-image-on-b1ls"></a>V: Wat gebeurt er als ik een niet-ondersteunde OS-installatiekopie op B1ls implementeert?
**A** : B1ls biedt alleen ondersteuning voor Linux-installatiekopieën en als u een andere installatiekopie van het besturingssysteem implementeert u niet de beste klantervaring kunt krijgen.
