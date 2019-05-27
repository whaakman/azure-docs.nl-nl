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
ms.openlocfilehash: 24fbab2bb637edf6c7e31215cdc7ac82bdb0ebab
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158489"
---
Sommige workloads van databases, zoals SQL Server of Oracle vereisen hoge geheugen-, opslag, en i/o-bandbreedte, maar niet een aantal hoge kernen. Veel databaseworkloads zijn niet CPU-intensief. Azure biedt een bepaalde VM-grootten waar u het aantal VM vCPU's te verminderen de kosten voor softwarelicenties, behoud van de dezelfde geheugen-, opslag- en i/o-bandbreedte kunt beperken.

Het aantal vCPU's kan worden beperkt tot één of een halve kwartaal van de oorspronkelijke VM-grootte. Deze nieuwe VM-grootten hebben een achtervoegsel dat Hiermee geeft u het aantal actieve vcpu's zodat ze gemakkelijker kunt identificeren.

Bijvoorbeeld, de huidige VM-grootte Standard_GS5 wordt geleverd met 32 vcpu's en 448 GB RAM-geheugen, 64 schijven (tot 256 TB), en 80.000 IOP's of 2 GB/s van i/o-bandbreedte. De nieuwe VM-grootten Standard_GS5-16 en Standard_GS5-8 wordt geleverd met 16 en 8 active vcpu's, terwijl de rest van de specificaties van de Standard_GS5 voor geheugen-, opslag- en i/o-bandbreedte.

De licentie kosten in rekening gebracht voor SQL Server of Oracle zijn beperkt tot het nieuwe aantal vCPU's en andere producten moeten in rekening gebracht op basis van de nieuwe vCPU aantal. Dit resulteert in een toename van 50% tot 75% in de verhouding van de specificaties van de virtuele machine aan active (factureerbare) vcpu's. Deze nieuwe VM-grootten kunnen klanten workloads gebruikmaken van dezelfde geheugen-, opslag- en i/o-bandbreedte tijdens het optimaliseren van hun kosten voor softwarelicenties. Op dit moment blijft de compute-kosten, zoals de OS-licentieverlening, de dezelfde versie als de oorspronkelijke grootte. Zie voor meer informatie, [Azure VM-grootten voor meer rendabele databaseworkloads](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).


| Name                | vCPU | Specificaties           |
|---------------------|------|-----------------|
| Standard_M8-2ms     | 2    | Hetzelfde als M8ms    |
| Standard_M8-4ms     | 4    | Hetzelfde als M8ms    |
| Standard_M16-4ms    | 4    | Hetzelfde als M16ms   |
| Standard_M16-8ms    | 8    | Hetzelfde als M16ms   |
| Standard_M32-8ms    | 8    | Hetzelfde als M32ms   |
| Standard_M32-16ms   | 16   | Hetzelfde als M32ms   |
| Standard_M64-32ms   | 32   | Hetzelfde als M64ms   |
| Standard_M64-16ms   | 16   | Hetzelfde als M64ms   |
| Standard_M128-64ms  | 64   | Hetzelfde als M128ms  |
| Standard_M128-32ms  | 32   | Hetzelfde als M128ms  |
| Standard_E4-2s_v3   | 2    | Hetzelfde als E4s_v3  |
| Standard_E8-4s_v3   | 4    | Hetzelfde als E8s_v3  |
| Standard_E8-2s_v3   | 2    | Hetzelfde als E8s_v3  |
| Standard_E16-8s_v3  | 8    | Hetzelfde als E16s_v3 |
| Standard_E16-4s_v3  | 4    | Hetzelfde als E16s_v3 |
| Standard_E32-16s_v3 | 16   | Hetzelfde als E32s_v3 |
| Standard_E32-8s_v3  | 8    | Hetzelfde als E32s_v3 |
| Standard_E64-32s_v3 | 32   | Hetzelfde als E64s_v3 |
| Standard_E64-16s_v3 | 16   | Hetzelfde als E64s_v3 |
| Standard_GS4-8      | 8    | Hetzelfde als GS4     |
| Standard_GS4-4      | 4    | Hetzelfde als GS4     |
| Standard_GS5-16     | 16   | Hetzelfde als GS5     |
| Standard_GS5-8      | 8    | Hetzelfde als GS5     |
| Standard_DS11-1_v2  | 1    | Hetzelfde als DS11_v2 |
| Standard_DS12-2_v2  | 2    | Hetzelfde als DS12_v2 |
| Standard_DS12-1_v2  | 1    | Hetzelfde als DS12_v2 |
| Standard_DS13-4_v2  | 4    | Hetzelfde als DS13_v2 |
| Standard_DS13-2_v2  | 2    | Hetzelfde als DS13_v2 |
| Standard_DS14-8_v2  | 8    | Hetzelfde als DS14_v2 |
| Standard_DS14-4_v2  | 4    | Hetzelfde als DS14_v2 |
