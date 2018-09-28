---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: bb9a2a884439b00f52adfa9b7c1010a4610a77f7
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47401645"
---
# <a name="ultra-ssd-preview-managed-disks-for-azure-virtual-machine-workloads"></a>Ultra SSD (preview) Managed Disks voor Azure Virtual Machine-werkbelasting

Azure Ultra SSD (preview) biedt hoge doorvoer, een hoge IOPS en consistente lage latentie disk-opslag voor Azure IaaS VM's. Deze nieuwe aanbieding biedt boven aan de prestaties van de regel op het niveau van de dezelfde beschikbaarheid als onze bestaande schijven-aanbiedingen. Extra voordelen van Ultra SSD omvatten de mogelijkheid om de prestaties van de schijf, samen met uw workloads zonder dat uw virtuele machines opnieuw moet worden dynamisch te wijzigen. Ultra SSD is geschikt voor gegevensintensieve workloads, zoals SAP HANA, databases van de bovenste laag en transactie-zware workloads.

## <a name="ultra-ssd-features"></a>Ultra SSD-functies

**Beheerde schijven**: Ultra SSD's zijn alleen beschikbaar als beheerde schijven. Ultra SSD's kunnen niet worden geïmplementeerd als een niet-beheerde schijf of een pagina-blobs. Tijdens het maken van een beheerde schijf maken, geeft u de sku van de schijf als UltraSSD_LRS en geven van de grootte van de schijf, de IOPS en doorvoer die u nodig en Azure maakt en beheert de schijf voor u.  

**Virtuele Machines**: Ultra SSD's zijn ontworpen om te werken met alle voorraadeenheden voor virtuele Azure-Machine Premium SSD is ingeschakeld, maar preview tijdens de VM-grootten beperkt tot ES/DS v3 VM-exemplaren zijn.

**Dynamische Prestatieconfiguratie**: Ultra SSD's kunt u de prestaties van de schijf, samen met de behoeften van uw werkbelasting (IOPS en doorvoer) dynamisch wijzigen zonder te hoeven starten van uw virtuele machines.

## <a name="scalability-and-performance-targets"></a>Schaalbaarheids- en prestatiedoelen

Als u een Ultra SSD inricht, hebt u de optie voor de capaciteit en de prestaties van de schijf afzonderlijk te configureren. Ultra SSD's zijn in verschillende vaste grootte zijn afkomstig uit 4 GiB maximaal 64 TiB en voorzien van een flexibele prestaties configuratiemodel waarmee u IOPS en doorvoer onafhankelijk van elkaar te configureren. Ultra SSD's kunnen alleen worden gebruikt als gegevensschijven. We adviseren Premium SSD's gebruiken als de OS-schijven.

Er zijn enkele kernfuncties van Ultra SSD:

- Schijfcapaciteit: Ultra SSD biedt u tal van verschillende schijfgrootten van 4 GiB maximaal 64 TiB.
- IOPS-schijven: Ultra SSD's ondersteuning IOPS-limieten van 300 IOPS/GiB, tot een maximum van 160 kB IOPS per schijf. Zorg ervoor dat de geselecteerde schijf-IOPS kleiner dan de IOPS VM zijn voor het bereiken van de IOPS-waarde die u hebt ingericht. De minimale IOPS-schijf is 100 IOPS.
- Schijfdoorvoer: Met de Ultra SSD's, de doorvoerlimiet van één schijf is 256 KiB/s voor elke IOP's, tot een maximum van 2000 MBps per schijf ingericht (waarbij MBps = 10 ^ 6 Bytes per seconde). De schijfdoorvoer, met minimale is 1 MiB.

De volgende tabel geeft een kort overzicht van verschillende configuraties die voor de verschillende schijfgroottes worden ondersteund:  

### <a name="ultra-ssd-managed-disk-offerings"></a>Aanbiedingen Ultra SSD Managed Disks

|Schijfgrootte (GiB)  |IOPS-limieten  |Bovengrens voor doorvoer (MBps)  |
|---------|---------|---------|
|4     |1,200         |300         |
|8     |2,400         |600         |
|16     |4,800         |1,200         |
|32     |9600         |2,000         |
|64     |19,200         |2,000         |
|128     |38.400         |2,000         |
|256     |76,800         |2,000         |
|512     |80,000         |2,000         |
|1024-65.536 (grootten die in dit bereik verhogen in stappen van 1 TiB)     |160.000         |2,000         |

## <a name="pricing-and-billing"></a>Prijzen en facturering

Wanneer u uiterst SSD's, zijn de volgende factureringsvoorwaarden van toepassing:

- Beheerde schijfgrootte
- Beheerde schijf ingerichte IOPS
- Beheerde schijf ingerichte doorvoer
- Ultra SSD VM-kosten voor reservering

### <a name="managed-disk-size"></a>Beheerde schijfgrootte

Beheerde schijven worden in rekening gebracht op de ingerichte grootte. Azure wijst de ingerichte grootte (omhoog afgerond) toe aan de aanbieding voor schrijfgrootte die het dichtst in de buurt komt. Zie de tabel in Scalability and Performance Targets sectie hierboven voor meer informatie van de grootte van de schijven die worden aangeboden. Elke schijf wordt toegewezen aan een grootte van de ondersteunde ingerichte schijf en deze worden dienovereenkomstig op uurbasis gefactureerd. Bijvoorbeeld, als u een schijf 200 GiB Ultra SSD ingericht en deze na 20 uur, deze wordt toegewezen aan de aanbieding van de schijf grootte van 256 GB en u wordt gefactureerd voor de 256 GB voor 20 uur. Dit is, ongeacht de hoeveelheid feitelijke gegevens geschreven naar de schijf.

### <a name="managed-disk-provisioned-iops"></a>Beheerde schijf ingerichte IOPS

IOP's zijn het aantal aanvragen dat uw toepassing naar de schijven in één seconde verzendt. Een i/o-bewerking kan worden op volgorde of willekeurige, leest of schrijft. Bijvoorbeeld schijfgrootte, wordt de ingerichte IOPS op uurbasis gefactureerd. Zie de tabel in Scalability and Performance Targets sectie hierboven voor meer informatie van de schijf IOP's die worden aangeboden.

### <a name="managed-disk-provisioned-throughput"></a>Beheerde schijf ingerichte doorvoer

Doorvoer is de hoeveelheid gegevens die uw toepassing naar de schijven in een opgegeven interval verzendt, gemeten in bytes per seconde. Als uw toepassing grote i/o-bewerkingen uitvoert, is er een hoge doorvoer vereist.  

Er is een relatie tussen de doorvoer en IOPS zoals wordt weergegeven in de volgende formule: IOPS x de grootte van de i/o-doorvoer =

Daarom is het belangrijk om te bepalen van de optimale doorvoer en IOPS-waarden die uw toepassing vereist. Als u een optimaliseren probeert, wordt de andere ook beïnvloed. U wordt aangeraden beginnen met een doorvoer die overeenkomt met 16 KiB i/o-grootte en aan te passen als er meer doorvoer nodig is.

Zie voor meer informatie over de ondersteunde schijfdoorvoer op Ultra SSD, de tabel in Scalability and Performance Targets hierboven. Net als de schijfgrootte en IOP's, de ingerichte doorvoer is per uur in rekening gebracht per MBps ingericht.

### <a name="ultra-ssd-vm-reservation-fee"></a>Ultra SSD VM-kosten voor reservering

We introduceren een mogelijkheid van de virtuele machine die Hiermee geeft u dat uw virtuele machine is uiterst compatibel SSD. Een Ultra SSD compatibele virtuele machine wordt toegewezen capaciteit van toegewezen bandbreedte tussen de compute-VM-exemplaar en de schaaleenheid voor opslag van blok op de prestaties te optimaliseren en verminder de latentie. Deze mogelijkheid toevoegen op de resultaten van de virtuele machine in een reservering in rekening gebracht die alleen als u Ultra SSD-capaciteit op de virtuele machine zonder een Ultra SSD-schijf koppelen aan het ingeschakeld is opgelegd. Wanneer een Ultra SSD-schijf is gekoppeld aan de compatibele VM Ultra SSD, zou deze kosten niet worden toegepast. Deze kosten is per vCPU ingericht op de virtuele machine.

Raadpleeg de [Azure Disks pagina met prijzen](https://azure.microsoft.com/pricing/details/managed-disks/) prijs details beschikbaar in beperkte Preview-versie voor de nieuwe Ultra SSD-schijven.

### <a name="ultra-ssd-preview-scope-and-limitations"></a>Ultra SSD Preview bereik en beperkingen

Preview-versie is uiterst SSD-schijven:

- In eerste instantie worden ondersteund in VS-Oost 2 in één Beschikbaarheidszone  
- Kan alleen worden gebruikt met Beschikbaarheidszones (Beschikbaarheidssets en één VM-implementaties buiten Zones worden niet hebben de mogelijkheid om te koppelen van een Ultra SSD-schijf)
- Worden alleen ondersteund op ES/DS v3-VM 's
- Zijn alleen beschikbaar als gegevensschijven en alleen ondersteuning voor 4k die fysieke sectorgrootte  
- Kunnen alleen worden gemaakt als de lege schijven  
- Momenteel kunnen alleen worden geïmplementeerd met Resource Manager-sjablonen, CLI en Python-SDK.
- Wordt nog niet is () ondersteuning voor schijf-momentopnamen, VM-installatiekopieën, Beschikbaarheidssets, Virtual Machine Scale Sets en Azure Disk Encryption.
- Geen (nog) ondersteuning bieden voor integratie met Azure Backup of Azure Site Recovery.
- Net als bij [meeste previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), deze functie mag niet worden gebruikt voor werkbelastingen voor productie tot algemene beschikbaarheid (GA).