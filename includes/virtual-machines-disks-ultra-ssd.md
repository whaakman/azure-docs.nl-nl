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
ms.openlocfilehash: 4b5d2de2e9ccd44517e083a435e127bd5678f002
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331199"
---
# <a name="ultra-disks-preview-managed-disks-for-azure-virtual-machine-workloads"></a>Ultra-schijven (preview) beheerde schijven voor virtuele machine van Azure-workloads

Azure ultra-schijven (preview) biedt hoge doorvoer, een hoge IOPS en consistente lage latentie disk-opslag voor Azure IaaS VM's. Deze nieuwe aanbieding biedt boven aan de prestaties van de regel op het niveau van de dezelfde beschikbaarheid als onze bestaande schijven-aanbiedingen. Extra voordelen van ultra schijven zijn de mogelijkheid om de prestaties van de schijf, samen met uw workloads zonder dat uw virtuele machines opnieuw moet worden dynamisch te wijzigen. Ultra-schijven zijn geschikt voor gegevensintensieve workloads, zoals SAP HANA, databases van de bovenste laag en transactie-zware workloads.

## <a name="ultra-disk-features"></a>Ultra schijf functies

**Beheerde schijven**: Ultra-schijven zijn alleen beschikbaar als beheerde schijven. Ultra schijven kunnen niet worden geïmplementeerd als een niet-beheerde schijf of een pagina-blobs. Tijdens het maken van een beheerde schijf, geeft u de sku van de schijf als UltraSSD_LRS en geven van de grootte van de schijf, de IOPS en doorvoer die u nodig en Azure maakt en beheert de schijf voor u.  

**Virtuele Machines**: Ultra-schijven zijn ontworpen voor gebruik met alle Premium SSD ingeschakelde Azure VM-SKU 's omdat deze zich momenteel in Preview-versie, worden de virtuele machines echter grootte als ES/DS v3.

**Dynamische Prestatieconfiguratie**: Ultra-schijven kunnen u de prestaties van de schijf, samen met de behoeften van uw werkbelasting (IOPS en doorvoer) dynamisch wijzigen zonder te hoeven starten van uw virtuele machines.

## <a name="scalability-and-performance-targets"></a>Schaalbaarheids- en prestatiedoelen

Als u een ultra schijven inricht, hebt u de optie voor de capaciteit en de prestaties van de schijf afzonderlijk te configureren. Ultra-schijven zijn in verschillende vaste grootte zijn afkomstig uit 4 GiB maximaal 64 TiB en voorzien van een flexibele prestaties configuratiemodel waarmee u IOPS en doorvoer onafhankelijk van elkaar te configureren. Ultra schijven kunnen alleen worden gebruikt als gegevensschijven. We adviseren Premium SSD's gebruiken als de OS-schijven.

Er zijn enkele kernfuncties van ultra schijven:

- Capaciteit van de schijf: Ultra schijven biedt u tal van verschillende schijfgrootten van 4 GiB maximaal 64 TiB.
- Schijf-IOPS: Ultra schijven ondersteuning voor IOPS-limieten van 300 IOPS/GiB, tot een maximum van 160 kB IOPS per schijf. Zorg ervoor dat de geselecteerde schijf-IOPS kleiner dan de IOPS VM zijn voor het bereiken van de IOPS-waarde die u hebt ingericht. De minimale IOPS-schijf is 100 IOPS.
- Schijfdoorvoer: Met ultra schijven, de doorvoerlimiet van één schijf is 256 KiB/s voor elke IOP's, tot een maximum van 2000 MBps per schijf ingericht (waarbij MBps = 10 ^ 6 Bytes per seconde). De schijfdoorvoer, met minimale is 1 MiB.

De volgende tabel geeft een kort overzicht van verschillende configuraties die voor de verschillende schijfgroottes worden ondersteund:  

### <a name="ultra-disks-managed-disk-offerings"></a>Ultra-schijven beheerd schijf aanbiedingen

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

## <a name="pricing-and-billing"></a>Prijzen en facturering

Wanneer u uiterst schijven gebruikt, worden de volgende factureringsvoorwaarden toegepast:

- Beheerde schijfgrootte
- Beheerde schijf ingerichte IOPS
- Beheerde schijf ingerichte doorvoer
- Kosten voor reservering Ultra schijf-VM

### <a name="managed-disk-size"></a>Beheerde schijfgrootte

Beheerde schijven worden in rekening gebracht op de VM-grootten die u kiest tijdens het inrichten van een nieuwe virtuele machine van Azure. Azure wijst de ingerichte grootte (omhoog afgerond) toe aan de aanbieding voor schrijfgrootte die het dichtst in de buurt komt. Zie de tabel in Scalability and Performance Targets sectie hierboven voor meer informatie van de grootte van de schijven die worden aangeboden. Elke schijf wordt toegewezen aan een grootte van de ondersteunde ingerichte schijf en worden kosten in rekening gebracht dienovereenkomstig op uurbasis. Bijvoorbeeld, als u een 200 GiB ultra schijf ingericht en deze na 20 uur, deze wordt toegewezen aan de aanbieding van de schijf grootte van 256 GB en u wordt gefactureerd voor de 256 GB voor 20 uur. Deze facturering is gebaseerd op verbruik van de compute-uur, ongeacht de hoeveelheid gegevens daadwerkelijk worden geschreven naar de schijf.

### <a name="managed-disk-provisioned-iops"></a>Beheerde schijf ingerichte IOPS

IOP's zijn het aantal aanvragen dat uw toepassing verzendt naar de schijven per seconde. Een i/o-bewerking kan worden sequentieel lezen of schrijven of willekeurige lezen of schrijven. Op basis van de grootte van de schijf of het aantal schijven die zijn gekoppeld aan de virtuele machine, worden het gemiddelde aantal IOP's gefactureerd op uurbasis. Zie de tabel in Scalability and Performance Targets sectie hierboven voor meer informatie van de schijf IOP's die worden aangeboden.

### <a name="managed-disk-provisioned-throughput"></a>Beheerde schijf ingerichte doorvoer

Doorvoer is de hoeveelheid gegevens die uw toepassing naar de schijven in een opgegeven interval verzendt, gemeten in bytes per seconde. Als uw toepassing grote i/o-bewerkingen uitvoert, is er een hoge doorvoer vereist.  

Er is een relatie tussen de doorvoer en IOPS, zoals wordt weergegeven in de volgende formule:  IOPS x de grootte van de i/o-doorvoer =

Daarom is het belangrijk om te bepalen van de optimale doorvoer en IOPS-waarden die uw toepassing vereist. Als u een optimaliseren probeert, wordt de andere ook beïnvloed. U wordt aangeraden beginnen met een doorvoer die overeenkomt met 16 KiB i/o-grootte en aan te passen als er meer doorvoer nodig is.

Zie voor meer informatie over de ondersteunde schijfdoorvoer op ultra schijven, de tabel in Scalability and Performance Targets hierboven. Net als de schijfgrootte en IOP's, de ingerichte doorvoer is per uur in rekening gebracht per MBps ingericht.

### <a name="ultra-disk-vm-reservation-fee"></a>Kosten voor reservering Ultra schijf-VM

We introduceren een mogelijkheid van de virtuele machine die Hiermee geeft u dat de virtuele machine is ultra schijf die compatibel is. Toegewezen bandbreedtecapaciteit tussen de compute-VM-exemplaar en de schaaleenheid voor opslag van blok op de prestaties te optimaliseren en verminder de latentie van een ultra schijf compatibele virtuele machine kan worden toegewezen. Deze mogelijkheid toevoegen op de resultaten van de virtuele machine in een reservering in rekening gebracht die alleen als u de mogelijkheid ultra schijf op de virtuele machine zonder een ultra schijf koppelen aan het ingeschakeld is opgelegd. Wanneer een ultra-schijf is gekoppeld aan de ultra compatibele virtuele machine, zou deze kosten niet worden toegepast. Deze kosten is per vCPU ingericht op de virtuele machine.

Raadpleeg de [Azure Disks pagina met prijzen](https://azure.microsoft.com/pricing/details/managed-disks/) voor de nieuwe ultra schijven prijs details beschikbaar in de beperkte Preview-versie.

### <a name="ultra-disk-preview-scope-and-limitations"></a>Ultra schijf preview bereik en beperkingen

Preview-versie is uiterst schijven:

- In eerste instantie worden ondersteund in VS-Oost 2 in één Beschikbaarheidszone  
- Kan alleen worden gebruikt met Beschikbaarheidszones (Beschikbaarheidssets en één VM-implementaties buiten Zones worden niet hebben de mogelijkheid om te koppelen van een ultra-schijf)
- Worden alleen ondersteund op ES/DS v3-VM 's
- Zijn alleen beschikbaar als gegevensschijven en alleen ondersteuning voor 4k die fysieke sectorgrootte  
- Kunnen alleen worden gemaakt als de lege schijven  
- Momenteel kunnen alleen worden geïmplementeerd met Resource Manager-sjablonen, CLI en Python-SDK.
- Wordt nog niet is () ondersteuning voor schijf-momentopnamen, VM-installatiekopieën, Beschikbaarheidssets, Virtual Machine Scale Sets en Azure Disk Encryption.
- Geen (nog) ondersteuning bieden voor integratie met Azure Backup of Azure Site Recovery.
- Net als bij [meeste previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), deze functie mag niet worden gebruikt voor werkbelastingen voor productie tot algemene beschikbaarheid (GA).
