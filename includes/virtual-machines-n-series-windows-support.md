---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 03/20/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 426340a5b452124dcba4f51f2ec2c1e2ec0f54b2
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
## <a name="supported-operating-systems-and-drivers"></a>Ondersteunde besturingssystemen en stuurprogramma’s

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-tesla-cuda-drivers"></a>NC, NCv2, NCv3 en ND-serie - stuurprogramma's NVIDIA Tesla (CUDA)

Stuurprogramma downloaden koppelingen in de volgende tabel zijn huidige op moment van publicatie. Ga naar de [NVIDIA](http://www.nvidia.com/)-website voor de nieuwste stuurprogramma's.

> [!TIP]
> U kunt een Azure implementeren als alternatief voor handmatige installatie op een Windows Server VM CUDA stuurprogramma [gegevens wetenschappelijke virtuele Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) installatiekopie. De edities DSVM voor Windows Server 2016 voorafgaand aan installatie NVIDIA CUDA-stuurprogramma's, de CUDA Deep Neural Network-bibliotheek en andere hulpprogramma's.


| OS | Stuurprogramma |
| -------- |------------- |
| Windows Server 2016 | [390.85](http://us.download.nvidia.com/Windows/Quadro_Certified/390.85/390.85-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [390.85](http://us.download.nvidia.com/Windows/Quadro_Certified/390.85/390.85-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nv-series---nvidia-grid-drivers"></a>NV-serie - NVIDIA RASTER stuurprogramma 's

Microsoft distribueert NVIDIA RASTER stuurprogramma installatieprogramma's voor virtuele machines NV opnieuw. Alleen deze RASTER stuurprogramma's installeren op Azure NV VM's. Deze stuurprogramma's omvatten de licentieverlening voor RASTER Virtual GPU-Software in Azure.

| OS | Stuurprogramma |
| -------- |------------- |
| Windows Server 2016 | [GRID 5.2 (386.09)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [GRID 5.2 (386.09)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |