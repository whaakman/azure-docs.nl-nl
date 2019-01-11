---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 11/27/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: a1200494eac2d60083a6608d1af91379d8298070
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54201496"
---
## <a name="supported-distributions-and-drivers"></a>Ondersteunde distributies en stuurprogramma 's

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA-stuurprogramma 's

NVIDIA CUDA-stuurprogramma's voor NC, NCv2, NCv3, ND en NDv2-serie VM's (optioneel voor NV-serie) worden alleen ondersteund op de Linux-distributies die worden vermeld in de volgende tabel. Informatie over CUDA-stuurprogramma is actueel op het moment van publicatie. Voor de meest recente CUDA-stuurprogramma's, gaat u naar de [NVIDIA](https://developer.nvidia.com/cuda-zone) website. Zorg ervoor dat u installeren of naar de nieuwste CUDA-stuurprogramma's voor uw distributie upgraden. 

> [!TIP]
> Als alternatief voor handmatige installatie van CUDA-stuurprogramma op een Linux-VM, kunt u een Azure implementeren [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) installatiekopie. De DSVM-edities voor Ubuntu 16.04 LTS of CentOS 7.4 voorafgaand aan installatie NVIDIA CUDA-stuurprogramma's, de CUDA Deep Neural Network-bibliotheek en andere hulpprogramma's.

| Distributie | Stuurprogramma |
| --- | -- | 
| Ubuntu 16.04 LTS, 18.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3, 7.4, 7.5, 7,6<br/><br/> Op basis van centOS 7.3, 7.4, 7.5, 7.6, op basis van CentOS 7.4 HPC | NVIDIA CUDA 10.0, stuurprogramma-vertakking R410 |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID-stuurprogramma 's

Microsoft herdistribueert u installatieprogramma's van NVIDIA GRID-stuurprogramma voor NV en NVv2-serie VM's gebruikt als virtuele werkstations of voor virtuele toepassingen. Alleen deze GRID-stuurprogramma's installeren op Azure NV Virtual machines, alleen op de besturingssystemen die worden vermeld in de volgende tabel. Deze stuurprogramma's bevatten de licentieverlening voor GRID virtuele GPU-Software in Azure. U hoeft niet voor het instellen van een server met NVIDIA vGPU software-licentie.

| Distributie | Stuurprogramma |
| --- | -- |
| Ubuntu 16.04 LTS, 18.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3, 7.4, 7.5, 7,6<br/><br/>Op basis van centOS 7.3, 7.4, 7.5, 7,6 | NVIDIA GRID 7, stuurprogramma-vertakking R410|

> [!WARNING] 
> Installatie van software van derden in Red Hat-producten kan invloed hebben op de ondersteuningsvoorwaarden van Red Hat. Zie het [Knowledge Base-artikel over Red Hat](https://access.redhat.com/articles/1067).
>
