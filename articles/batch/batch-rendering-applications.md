---
title: Toepassingen - rendering Azure Batch
description: Vooraf geïnstalleerde toepassingen van de Batch rendering
services: batch
author: laurenhughes
ms.author: lahugh
ms.date: 12/11/2018
ms.topic: conceptual
ms.openlocfilehash: d17a740959f36a11835f6d4548b8f8bc1a5c5f80
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53536318"
---
# <a name="pre-installed-applications-on-rendering-vm-images"></a>Vooraf geïnstalleerde toepassingen op de VM-installatiekopieën weergeven

Het is mogelijk voor het gebruik van een rendering-toepassingen met Azure Batch. Azure Marketplace VM-installatiekopieën zijn echter beschikbaar met algemene toepassingen die vooraf zijn geïnstalleerd.

Indien van toepassing, is waarbij u betaalt per gebruik licentieverlening beschikbaar voor de rendering van vooraf geïnstalleerde toepassingen. Als u een Batch-pool maakt, de vereiste toepassingen kunnen worden opgegeven en zowel de kosten van de virtuele machine en toepassingen wordt gefactureerd per minuut. Prijzen van de toepassing worden weergegeven op de [Azure Batch-pagina met prijzen](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

Sommige toepassingen ondersteunen alleen Windows, maar de meeste worden ondersteund op zowel Windows als Linux.

## <a name="applications-on-centos-7-rendering-nodes"></a>Toepassingen op CentOS 7 rendering knooppunten

* Autodesk Maya I/O 2017 Update 5 (cut 201708032230)
* Autodesk Maya I/O 2018 Update 2 (knippen 201711281015)
* Autodesk Arnold for Maya 2017 (Arnold versie 5.0.1.1) MtoA-2.0.1.1-2017
* Autodesk Arnold for Maya 2018 (Arnold versie 5.0.1.4) MtoA-2.1.0.3-2018
* Chaos Group V-Ray for Maya 2017 (versie 3.60.04)
* Chaos Group V-Ray for Maya 2018 (versie 3.60.04)
* Blender (2.68)

## <a name="applications-on-windows-server-2016-rendering-nodes"></a>Toepassingen op Windows Server 2016 rendering knooppunten

* Autodesk Maya I/O 2017 Update 5 (versie 17.4.5459)
* Autodesk Maya I/O 2018 Update 4 (versie 18.4.0.7622)  
* Autodesk 3ds Max i/o 2019 Update 1 (versie 21.2.0.2219)
* Autodesk 3ds Max I/O 2018 Update 4 (versie 20.4.0.4254)
* Autodesk Arnold voor Maya 2017 (versie 5.2.0.1 Arnold) MtoA-3.1.0.1-2017
* Autodesk Arnold voor Maya 2018 (versie 5.2.0.1 Arnold) MtoA-3.1.0.1-2018
* Autodesk Arnold for 3ds Max (Arnold versie 5.0.2.4)(version 1.2.926)
* Chaos Group V-Ray for Maya (versie 3.52.03)
* Chaos Group V-Ray for 3ds Max (versie 3.60.02)
* Blender (2.79)

## <a name="next-steps"></a>Volgende stappen

Voor het gebruik van de VM-installatiekopieën voor rendering, moeten ze worden opgegeven in de configuratie van de pool wanneer een groep is gemaakt. Zie de [Batch-pool mogelijkheden voor rendering](https://docs.microsoft.com/azure/batch/batch-rendering-functionality#batch-pools).