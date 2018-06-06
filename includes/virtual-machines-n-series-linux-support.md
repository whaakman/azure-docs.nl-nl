---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 05/29/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0264f92fa10bd503a2811ce40ee0b8d4edd5f3b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34669829"
---
## <a name="supported-distributions-and-drivers"></a>Ondersteunde distributies en stuurprogramma 's

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA stuurprogramma 's

NVIDIA CUDA stuurprogramma's voor NC, NCv2 NCv3 en ND-serie VMs (optioneel voor NV-serie) worden alleen ondersteund op de Linux-distributies die worden vermeld in de volgende tabel. CUDA stuurprogramma-informatie is huidige op moment van publicatie. Voor de nieuwste stuurprogramma's CUDA gaat u naar de [NVIDIA](https://developer.nvidia.com/cuda-zone) website. Zorg ervoor dat u installeert of upgrade uit naar de nieuwste CUDA-stuurprogramma's voor uw distributiepunt. 

> [!TIP]
> U kunt een Azure implementeren als alternatief voor handmatige installatie op een Linux-VM CUDA stuurprogramma [gegevens wetenschappelijke virtuele Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) installatiekopie. De edities DSVM voor Ubuntu 16.04 LTS of CentOS 7.4 voorafgaand aan installatie NVIDIA CUDA-stuurprogramma's, de CUDA Deep Neural Network-bibliotheek en andere hulpprogramma's.

| Distributie | Stuurprogramma |
| --- | -- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 of 7.4<br/><br/> 7.4 HPC op basis van centOS 7.3 of 7,4, op basis van CentOS | NVIDIA CUDA 9.1, stuurprogramma vertakking R390 |

### <a name="nvidia-grid-drivers"></a>NVIDIA RASTER stuurprogramma 's

Microsoft herdistribueert NVIDIA RASTER stuurprogramma installatieprogramma's voor NV-serie virtuele machines gebruikt als virtuele werkstations of voor virtuele toepassingen. Installeer alleen deze RASTER stuurprogramma's op Azure NV virtuele machines, alleen op de distributies die worden vermeld in de volgende tabel. Deze stuurprogramma's omvatten de licentieverlening voor RASTER Virtual GPU-Software in Azure.

| Distributie | Stuurprogramma |
| --- | -- |
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 of 7.4<br/><br/>Op basis van centOS 7.3 of 7.4 | NVIDIA RASTER 6.0, stuurprogramma vertakking R390|



> [!WARNING] 
> Installatie van software van derden in Red Hat-producten kan invloed hebben op de ondersteuningsvoorwaarden van Red Hat. Zie het [Knowledge Base-artikel over Red Hat](https://access.redhat.com/articles/1067).
>
