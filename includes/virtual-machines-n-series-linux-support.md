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
ms.openlocfilehash: 1813367a2d143f75fb51a3160dd00219c709c57b
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935750"
---
## <a name="supported-distributions-and-drivers"></a>Ondersteunde distributies en stuurprogramma 's

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA-stuurprogramma 's

NVIDIA CUDA-stuurprogramma's voor NC, NCv2, NCv3 en ND-serie VM's (optioneel voor NV-serie) worden alleen ondersteund op de Linux-distributies die worden vermeld in de volgende tabel. Informatie over CUDA-stuurprogramma is actueel op het moment van publicatie. Voor de meest recente CUDA-stuurprogramma's, gaat u naar de [NVIDIA](https://developer.nvidia.com/cuda-zone) website. Zorg ervoor dat u installeren of naar de nieuwste CUDA-stuurprogramma's voor uw distributie upgraden. 

> [!TIP]
> Als alternatief voor handmatige installatie van CUDA-stuurprogramma op een Linux-VM, kunt u een Azure implementeren [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) installatiekopie. De DSVM-edities voor Ubuntu 16.04 LTS of CentOS 7.4 voorafgaand aan installatie NVIDIA CUDA-stuurprogramma's, de CUDA Deep Neural Network-bibliotheek en andere hulpprogramma's.

| Distributie | Stuurprogramma |
| --- | -- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 of 7.4<br/><br/> 7.4 HPC op basis van centOS 7.3 of 7,4, op basis van CentOS | NVIDIA CUDA 9.1, stuurprogramma-vertakking R390 |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID-stuurprogramma 's

Microsoft herdistribueert u installatieprogramma's van NVIDIA GRID-stuurprogramma voor NV-serie VM's gebruikt als virtuele werkstations of voor virtuele toepassingen. Alleen deze GRID-stuurprogramma's installeren op Azure NV Virtual machines, alleen op de distributies die worden vermeld in de volgende tabel. Deze stuurprogramma's bevatten de licentieverlening voor GRID virtuele GPU-Software in Azure.

| Distributie | Stuurprogramma |
| --- | -- |
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 of 7.4<br/><br/>Op basis van centOS 7.3 of 7.4 | NVIDIA GRID 6.2, stuurprogramma-vertakking R390|



> [!WARNING] 
> Installatie van software van derden in Red Hat-producten kan invloed hebben op de ondersteuningsvoorwaarden van Red Hat. Zie het [Knowledge Base-artikel over Red Hat](https://access.redhat.com/articles/1067).
>
