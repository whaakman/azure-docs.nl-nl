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
ms.openlocfilehash: 82bfdfd7481b47c08f76d077585f85763e58b87b
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331188"
---
**Uitgaande gegevensoverdracht**: [Uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/bandwidth/) (gegevens die vanuit Azure-datacenters) worden gefactureerd voor bandbreedtegebruik.

**Transacties**: U wordt gefactureerd voor het aantal transacties die u op een standard-beheerde schijven uitvoert. Azure rekent $0.0036 per 100.000 transacties voor standard HDD's. Transacties bestaan zowel uit lees- als schrijfbewerkingen voor opslag.

Standard-SSD's gebruik van i/o-grootte van 256 KB. Als de gegevens worden overgebracht minder dan 256 KB is, wordt deze beschouwd als één i/o-eenheid. I/o-grotere worden geteld als meerdere i/o's van de grootte van 256 KB. Bijvoorbeeld, wordt een 1100 KB i/o geteld als vijf i/o-eenheden.

Er zijn geen kosten verbonden voor transacties voor een premium-beheerde schijf.

Zie voor gedetailleerde informatie over prijzen voor Managed Disks [Managed Disks-prijzen](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-disks-vm-reservation-fee"></a>Ultra schijven VM reservering kosten

Azure virtuele machines hebben de mogelijkheid om aan te geven als ze compatibel met ultra schijven zijn. Toegewezen bandbreedtecapaciteit tussen de compute-VM-exemplaar en de schaaleenheid voor opslag van blok op de prestaties te optimaliseren en verminder de latentie van een ultra schijf compatibele virtuele machine kan worden toegewezen. Deze mogelijkheid toevoegen op de resultaten van de virtuele machine in een reservering in rekening gebracht die alleen als u de mogelijkheid ultra schijf op de virtuele machine zonder een ultra schijf koppelen aan het ingeschakeld is opgelegd. Wanneer een ultra-schijf is gekoppeld naar de ultra schijf compatibele virtuele machine, deze kosten niet zouden worden toegepast. Deze kosten is per vCPU ingericht op de virtuele machine.

Raadpleeg de [Azure Disks pagina met prijzen](https://azure.microsoft.com/pricing/details/managed-disks/) voor ultra schijf schijven prijsinformatie.