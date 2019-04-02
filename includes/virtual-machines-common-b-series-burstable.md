---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 0e0b78d1e492e4d0fbbf0a44b57bebc2d8d3b1e7
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58763291"
---
De B-serie VM-reeks kunt u kiezen welke VM-grootte biedt u de benodigde basisniveau prestaties voor uw werkbelasting, de mogelijkheid om uit te breiden CPU-prestaties tot 100% van een Intel® Broadwell E5-2673 v4-processors 2,3 GHz, of een Intel® Haswell 2,4 GHz E5-2673 v3-processor vCPU.

De B-serie VM's zijn ideaal voor workloads die niet de volledige prestaties van de CPU, zoals webservers, bewijs van concepten, kleine databases en ontwikkeling build-omgevingen hoeft. Deze werkbelastingen hebben meestal ' burstable ' prestatie-eisen. De B-serie biedt u de mogelijkheid om aan te schaffen van een VM-grootte met basislijn en het VM-exemplaar maakt tegoed bij het gebruik van minder dan de basislijn. Wanneer de virtuele machine zijn tegoed verzameld, kan de virtuele machine burst boven de basislijn met maximaal 100% van de vCPU wanneer uw toepassing hogere CPU-prestaties vereist.

De B-serie wordt geleverd in de volgende zes VM-grootten:

| Grootte             | vCPU  | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Basis-CPU-prestaties van virtuele machine | Maximum aantal CPU-prestaties van virtuele machine | Tegoed gestort / uur | Max gestort tegoed | Max. aantal gegevensschijven | Max. doorvoer van caching en tijdelijke opslag: IOPS / MBps | Max. doorvoer voor schijfbewerkingen zonder schijf: IOPS / MBps | Max. aantal NIC's |          
|---------------|-------------|----------------|----------------------------|-----------------------|--------------------|--------------------|----------------|----------------------------------------|-------------------------------------------|-------------------------------------------|----------|
| Standard_B1ls<sup>1</sup>  | 1           | 0.5              | 4                          | 5%                   | 100%                   | 3                  | 72            | 2                                      | 200 / 10                                  | 160 / 10                                  | 2  |
| Standard_B1s  | 1           | 1              | 4                          | 10%                   | 100%                   | 6                  | 144            | 2                       | 400 / 10                                  | 320 / 10                                  | 2  |
| Standard_B1ms | 1           | 2              | 4                          | 20%                   | 100%                   | 12                 | 288           | 2                        | 800 / 10                                  | 640 / 10                                  | 2  |
| Standard_B2s  | 2           | 4              | 8                          | 40%                   | 200%                   | 24                 | 576            | 4                                      | 1600 / 15                                 | 1280 / 15                                 | 3  |
| Standard_B2ms | 2           | 8              | 16                         | 60%                   | 200%                   | 36                 | 864            | 4                                      | 2400 / 22.5                               | 1920 / 22.5                               | 3  |
| Standard_B4ms | 4           | 16             | 32                         | 90%                   | 400%                   | 54                 | 1296           | 8                                      | 3600 / 35                                 | 2880 / 35                                 | 4  |
| Standard_B8ms | 8           | 32             | 64                         | 135%                  | 800%                   | 81                 | 1944           | 16                                     | 4320 / 50                                 | 4320 / 50                                 | 4  |

<sup>1</sup> B1ls wordt alleen ondersteund op Linux

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


### <a name="q-does-the-b-series-support-premium-storage-data-disks"></a>V: De B-serie biedt ondersteuning voor Premium-opslagschijven gegevens?
**A**: Ja, alle grootten van de B-serie ondersteunen nu gegevensschijven van de Premium-opslag.   
    
### <a name="q-why-is-my-remaining-credit-set-to-0-after-a-redeploy-or-a-stopstart"></a>V: Waarom wordt mijn resterende tegoed ingesteld op 0 na een opnieuw implementeren of een stoppen/starten?
**A** : Wanneer een virtuele machine is 'REDPLOYED' en de virtuele machine wordt verplaatst naar een ander knooppunt, het totale tegoed verloren. Als de virtuele machine gestopt/gestart is, maar op hetzelfde knooppunt blijft, wordt het totale tegoed bewaard in de virtuele machine. Wanneer de virtuele machine wordt gestart op een knooppunt vers, krijgt een starttegoed, voor Standard_B8ms 240 minuten is.
    
### <a name="q-what-happens-if-i-deploy-an-unsupported-os-image-on-b1ls"></a>V: Wat gebeurt er als ik een niet-ondersteunde OS-installatiekopie op B1ls implementeert?
**A** : B1ls biedt alleen ondersteuning voor Linux-installatiekopieën en als u een andere installatiekopie van het besturingssysteem implementeert u niet de beste klantervaring kunt krijgen.
    
### <a name="q-why-is-there-no-pricing-information-for-b1ls-windows"></a>V: Waarom is er geen informatie over de prijzen voor B1ls windows?
**A** : B1ls biedt alleen ondersteuning voor Linux-installatiekopieën en als u een andere installatiekopie van het besturingssysteem implementeert, krijgt u mogelijk niet de beste klantervaring maar gefactureerd.


    

    
