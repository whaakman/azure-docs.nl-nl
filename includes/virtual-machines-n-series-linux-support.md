---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 05/01/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 7139ec67536a1c0e41c991db6d867b956f995c11
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
## <a name="supported-distributions-and-drivers"></a>Ondersteunde distributies en stuurprogramma 's

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-cuda-drivers"></a>NC, NCv2 NCv3 en ND-serie - NVIDIA CUDA stuurprogramma 's

CUDA stuurprogramma-informatie in de volgende tabel is huidige op moment van publicatie. Voor de nieuwste stuurprogramma's CUDA gaat u naar de [NVIDIA](https://developer.nvidia.com/cuda-zone) website. Zorg ervoor dat u installeert of upgrade uit naar de nieuwste CUDA-stuurprogramma's voor uw distributiepunt. 

> [!TIP]
> U kunt een Azure implementeren als alternatief voor handmatige installatie op een Linux-VM CUDA stuurprogramma [gegevens wetenschappelijke virtuele Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) installatiekopie. De edities DSVM voor Ubuntu 16.04 LTS of CentOS 7.4 voorafgaand aan installatie NVIDIA CUDA-stuurprogramma's, de CUDA Deep Neural Network-bibliotheek en andere hulpprogramma's.

| Distributie | Stuurprogramma |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 of 7.4<br/><br/> 7.4 HPC op basis van centOS 7.3 of 7,4, op basis van CentOS | NVIDIA CUDA 9.1, stuurprogramma vertakking R390 |

### <a name="nv-series---nvidia-grid-drivers"></a>NV-serie - NVIDIA RASTER stuurprogramma 's

Microsoft distribueert NVIDIA RASTER stuurprogramma installatieprogramma's voor virtuele machines NV opnieuw. Alleen deze RASTER stuurprogramma's installeren op Azure NV VM's. Deze stuurprogramma's omvatten de licentieverlening voor RASTER Virtual GPU-Software in Azure.

| Distributie | Stuurprogramma |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 of 7.4<br/><br/>Op basis van centOS 7.3 of 7.4 | NVIDIA RASTER 6.0, stuurprogramma vertakking R390|



> [!WARNING] 
> Installatie van software van derden in Red Hat-producten kan invloed hebben op de ondersteuningsvoorwaarden van Red Hat. Zie het [Knowledge Base-artikel over Red Hat](https://access.redhat.com/articles/1067).
>
