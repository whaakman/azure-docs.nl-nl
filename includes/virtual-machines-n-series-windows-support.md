---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11/14/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: e1885e1b420f5dfda146dd72b831ccfe4989e446
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51707365"
---
## <a name="supported-operating-systems-and-drivers"></a>Ondersteunde besturingssystemen en stuurprogramma’s

### <a name="nvidia-tesla-cuda-drivers"></a>Stuurprogramma's van NVIDIA Tesla (CUDA)

Stuurprogramma's van NVIDIA Tesla (CUDA) voor NC, NCv2, NCv3, ND en NDv2-serie VM's (optioneel voor NV-serie) worden alleen ondersteund op de besturingssystemen die worden vermeld in de volgende tabel. Downloadkoppelingen stuurprogramma zijn actueel op het moment van publicatie. Ga naar de [NVIDIA](http://www.nvidia.com/)-website voor de nieuwste stuurprogramma's.

> [!TIP]
> Als alternatief voor handmatige installatie van CUDA-stuurprogramma op een Windows Server-VM, kunt u een Azure implementeren [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) installatiekopie. De DSVM-edities van Windows Server 2016 wordt voorafgaand aan installatie NVIDIA CUDA-stuurprogramma's, de CUDA Deep Neural Network-bibliotheek en andere hulpprogramma's.


| OS | Stuurprogramma |
| -------- |------------- |
| Windows Server 2016 | [398.75](http://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [398.75](http://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID-stuurprogramma 's

Microsoft herdistribueert u installatieprogramma's van NVIDIA GRID-stuurprogramma voor NV en NVv2-serie VM's gebruikt als virtuele werkstations of voor virtuele toepassingen. Alleen deze GRID-stuurprogramma's installeren op Azure NV Virtual machines, alleen op de besturingssystemen die worden vermeld in de volgende tabel. Deze stuurprogramma's bevatten de licentieverlening voor GRID virtuele GPU-Software in Azure. U hoeft niet voor het instellen van een server met NVIDIA vGPU software-licentie.

| OS | Stuurprogramma |
| -------- |------------- |
| Windows Server 2016<br/><br/>Windows 10 | [RASTER 6.2 (391.81)](https://go.microsoft.com/fwlink/?linkid=874181) (.exe) |
| Windows Server 2012 R2 | [RASTER 6.2 (391.81)](https://go.microsoft.com/fwlink/?linkid=874184) (.exe)  |
