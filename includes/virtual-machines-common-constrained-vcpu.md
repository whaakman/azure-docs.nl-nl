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
ms.openlocfilehash: eaf490979167ea5cbfe5846e62444526310452f8
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36269541"
---
Sommige werkbelastingen database zoals SQL Server- of Oracle vereisen veel geheugen, opslag, en i/o-bandbreedte, maar niet op een hoog core-telling. Veel werkbelastingen van de database zijn geen CPU-intensief. Azure biedt bepaalde VM-grootten waar u het aantal VM vCPU's te verlagen de kosten van de software licensing behoud van hetzelfde geheugen-, opslag- en i/o-bandbreedte kunt beperken.

Het aantal vCPU kan worden beperkt tot kwartaal helft of een van de oorspronkelijke VM-grootte. Deze nieuwe VM-grootten een achtervoegsel hebben die het aantal actieve vcpu's geeft zodat ze gemakkelijker kunt identificeren.

Bijvoorbeeld, de huidige VM-grootte Standard_GS5 wordt geleverd met 32 Vcpu, 448 GB RAM-geheugen, 64 schijven (maximaal 256 TB) en 80.000 IOPs of 2 GB/s i/o-bandbreedte. De nieuwe VM-groottes Standard_GS5-16 en Standard_GS5-8 wordt geleverd met 16 en 8 active Vcpu respectievelijk, terwijl de rest van de specificaties van de Standard_GS5 voor geheugen, opslag en i/o-bandbreedte te behouden.

De licentie kosten in rekening gebracht voor SQL Server of Oracle zijn beperkt tot het aantal nieuwe vCPU en andere producten moeten worden in rekening gebracht op basis van de nieuwe vCPU aantal. Dit leidt tot een toename van 50% tot 75% op de verhouding tussen het VM-specificaties voor actieve (factureerbaar) vcpu's. Deze nieuwe VM-grootten zijn alleen beschikbaar in Azure, waardoor voor workloads een CPU-verbruik kan worden gerealiseerd tegen een fractie van de licentiekosten (per kerngeheugen). Op dit moment blijft de compute-kosten, waaronder OS-licentieverlening, de dezelfde versie als de oorspronkelijke grootte. Zie voor meer informatie [Azure VM-grootten voor meer rendabele database werkbelastingen](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).


| Naam                | vCPU | Specificaties           |
|---------------------|------|-----------------|
| Standard_M8-2 MS     | 2    | Zelfde als M8ms    |
| Standard_M8 4ms     | 4    | Zelfde als M8ms    |
| Standard_M16 4ms    | 4    | Zelfde als M16ms   |
| Standard_M16 8ms    | 8    | Zelfde als M16ms   |
| Standard_M32 8ms    | 8    | Zelfde als M32ms   |
| Standard_M32-16 MS   | 16   | Zelfde als M32ms   |
| Standard_M64-32ms   | 32   | Zelfde als M64ms   |
| Standard_M64-16ms   | 16   | Zelfde als M64ms   |
| Standard_M128-64ms  | 64   | Zelfde als M128ms  |
| Standard_M128-32ms  | 32   | Zelfde als M128ms  |
| Standard_E4-2s_v3   | 2    | Zelfde als E4s_v3  |
| Standard_E8-4s_v3   | 4    | Zelfde als E8s_v3  |
| Standard_E8-2s_v3   | 2    | Zelfde als E8s_v3  |
| Standard_E16-8s_v3  | 8    | Zelfde als E16s_v3 |
| Standard_E16-4s_v3  | 4    | Zelfde als E16s_v3 |
| Standard_E32-16_v3  | 16   | Zelfde als E32s_v3 |
| Standard_E32-8s_v3  | 8    | Zelfde als E32s_v3 |
| Standard_E64-32s_v3 | 32   | Zelfde als E64s_v3 |
| Standard_E64-16s_v3 | 16   | Zelfde als E64s_v3 |
| Standard_GS4-8      | 8    | Zelfde als GS4     |
| Standard_GS4-4      | 4    | Zelfde als GS4     |
| Standard_GS5-16     | 16   | Zelfde als GS5     |
| Standard_GS5-8      | 8    | Zelfde als GS5     |
| Standard_DS11-1_v2  | 1    | Zelfde als DS11_v2 |
| Standard_DS12-2_v2  | 2    | Zelfde als DS12_v2 |
| Standard_DS12-1_v2  | 1    | Zelfde als DS12_v2 |
| Standard_DS13-4_v2  | 4    | Zelfde als DS13_v2 |
| Standard_DS13-2_v2  | 2    | Zelfde als DS13_v2 |
| Standard_DS14-8_v2  | 8    | Zelfde als DS14_v2 |
| Standard_DS14-4_v2  | 4    | Zelfde als DS14_v2 |
