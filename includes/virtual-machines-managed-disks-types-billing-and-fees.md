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
ms.openlocfilehash: 268467796e67caf2d20fedb44d83fd455a09b83e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66147851"
---
**Uitgaande gegevensoverdracht**: [Uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/bandwidth/) (gegevens die vanuit Azure-datacenters) worden gefactureerd voor bandbreedtegebruik.

**Transacties**: U wordt gefactureerd voor het aantal transacties die u op een standard-beheerde schijven uitvoert. Voor standard SSD's, elke i/o bewerking kleiner dan of gelijk zijn aan 256 KiB van doorvoer wordt beschouwd als één i/o-bewerking. I/o-bewerkingen die groter zijn dan 256 KiB zijn van doorvoer worden beschouwd als meerdere i/o's van de grootte van 256 KiB. Elke i/o-bewerking is voor Standard HDD's beschouwd als één transactie, ongeacht de i/o-grootte.

Zie voor gedetailleerde informatie over prijzen voor Managed Disks, met inbegrip van de transactiekosten, [Managed Disks-prijzen](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-ssd-vm-reservation-fee"></a>Ultra SSD VM-kosten voor reservering

Azure virtuele machines hebben de mogelijkheid om aan te geven als ze compatibel met ultra SSD zijn. Toegewezen bandbreedtecapaciteit tussen de compute-VM-exemplaar en de schaaleenheid voor opslag van blok op de prestaties te optimaliseren en verminder de latentie van een ultra schijf compatibele virtuele machine kan worden toegewezen. Deze mogelijkheid toevoegen op de resultaten van de virtuele machine in een reservering in rekening gebracht die alleen als u de mogelijkheid ultra schijf op de virtuele machine zonder een ultra schijf koppelen aan het ingeschakeld is opgelegd. Wanneer een ultra-schijf is gekoppeld naar de ultra schijf compatibele virtuele machine, deze kosten niet zouden worden toegepast. Deze kosten is per vCPU ingericht op de virtuele machine.

Raadpleeg de [Azure Disks pagina met prijzen](https://azure.microsoft.com/pricing/details/managed-disks/) voor ultra schijf schijven prijsinformatie.