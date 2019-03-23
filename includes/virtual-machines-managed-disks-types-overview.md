---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 6eae536bd19a2c0e5707d8e0b379774b6eb2707a
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58395720"
---
# <a name="what-disk-types-are-available-in-azure"></a>Welke schijftypen zijn beschikbaar in Azure?

Azure-beheerde schijven biedt momenteel vier schijftypen, drie die algemeen beschikbaar (GA) en één die momenteel in preview zijn. Deze vier schijftypen elke hebben hun eigen juiste doel klantscenario's.

## <a name="disk-comparison"></a>Vergelijking van schijven

De volgende tabel bevat een vergelijking van ultra solid-state-stations (SSD) (preview), premium SSD, standard-SSD en standaard harde schijven (HDD) voor beheerde schijven om te bepalen wat u wilt gebruiken.

|   | Ultra SSD (preview)   | Premium SSD   | Standard - SSD   | Standard HDD   |
|---------|---------|---------|---------|---------|
|Schijftype   |SSD   |SSD   |SSD   |HDD   |
|Scenario   |I/o-intensieve workloads zoals SAP HANA, databases van de bovenste laag (bijvoorbeeld SQL, Oracle) en andere transactie zware workloads.   |Productie- en prestatiegevoelige workloads   |Webservers, bedrijfstoepassingen weinig wordt gebruikt en ontwikkelen en testen   |Back-up, niet-kritieke, incidentele toegang   |
|Schijfgrootte   |65.536 gibibyte (GiB) (Preview)   |32,767 GiB    |32,767 GiB   |32,767 GiB   |
|Max. doorvoer   |2000 MiB/s (Preview)   |900 MiB/s   |750 MiB/s   |500 MiB/s   |
|Max. IOP's   |160.000 (preview)   |20,000   |6,000   |2,000   |

## <a name="ultra-ssd-preview"></a>Ultra SSD (preview)

Azure ultra SSD (preview) bieden hoge doorvoer, hoge IOPS en consistente lage latentie schijfopslag voor Azure IaaS VM's. Enkele aanvullende voordelen van ultra SSD zijn de mogelijkheid om de prestaties van de schijf, samen met uw workloads, zonder dat uw virtuele machines opnieuw moet worden dynamisch te wijzigen. Ultra SSD zijn geschikt voor gegevensintensieve workloads, zoals SAP HANA, databases van de bovenste laag en transactie-zware workloads. Ultra SSD kan alleen worden gebruikt als gegevensschijven. We adviseren premium SSD's gebruiken als de OS-schijven.

### <a name="performance"></a>Prestaties

Als u een ultra schijf inricht, kunt u de capaciteit en de prestaties van de schijf afzonderlijk configureren. Ultra SSD zijn verkrijgbaar in verschillende vaste grootte, variërend van 4 GiB maximaal 64 TiB en voorzien van een flexibele prestaties configuratiemodel waarmee u IOPS en doorvoer onafhankelijk van elkaar te configureren.

Er zijn enkele kernfuncties van Ultra SSD:

- Capaciteit van de schijf: Ultra bereiken van SSD-capaciteit van 4 GiB maximaal 64 TiB.
- Schijf-IOPS: Ultra SSD ondersteuning voor IOPS-limieten van 300 IOPS/GiB, tot een maximum van 160 kB IOPS per schijf. Zorg ervoor dat de geselecteerde schijf-IOPS kleiner dan de IOPS VM zijn voor het bereiken van de IOPS-waarde die u hebt ingericht. De minimale IOPS-schijf zijn 100 IOPS.
- Schijfdoorvoer: Met de ultra SSD, de doorvoerlimiet van één schijf is 256 KiB/s voor elk IOP's, tot een maximum van 2000 MBps per schijf ingericht (waarbij MBps = 10 ^ 6 Bytes per seconde). De schijfdoorvoer, met minimale is 1 MiB.

### <a name="disk-size"></a>Schijfgrootte

|Schijfgrootte (GiB)  |IOPS Caps  |Bovengrens voor doorvoer (MBps)  |
|---------|---------|---------|
|4     |1,200         |300         |
|8     |2,400         |600         |
|16     |4,800         |1,200         |
|32     |9,600         |2,000         |
|64     |19,200         |2,000         |
|128     |38,400         |2,000         |
|256     |76,800         |2,000         |
|512     |80,000         |2,000         |
|1024-65.536 (grootten die in dit bereik verhogen in stappen van 1 TiB)     |160,000         |2,000         |

### <a name="preview-scope-and-limitations"></a>Bereik van de Preview-versie en beperkingen

Tijdens de preview, ultra SSD:

- In VS-Oost 2 in één beschikbaarheidszone worden ondersteund  
- Kan alleen worden gebruikt met beschikbaarheidszones (beschikbaarheidssets en één VM-implementaties buiten zones worden niet hebben de mogelijkheid om te koppelen van een ultra-schijf)
- Worden alleen ondersteund op ES/DS v3-VM 's
- Zijn alleen beschikbaar als gegevensschijven en alleen ondersteuning voor 4k die fysieke sectorgrootte  
- Kunnen alleen worden gemaakt als de lege schijven  
- Momenteel kan alleen worden geïmplementeerd met behulp van Azure Resource Manager-sjablonen, CLI en de python-SDK.
- Ondersteunt nog geen momentopnamen van de schijf, VM-installatiekopieën, beschikbaarheidssets, virtuele-machineschaalsets en Azure disk encryption.
- Integratie met Azure Backup of Azure Site Recovery biedt geen nog ondersteuning.
- Net als bij [meeste previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), deze functie mag niet worden gebruikt voor werkbelastingen voor productie tot algemene beschikbaarheid (GA).
