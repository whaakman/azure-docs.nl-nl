---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 0f87b122f81fbd04767354e623dcb808466ee1be
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673310"
---
## <a name="supported-operating-systems-and-drivers"></a>Ondersteunde besturingssystemen en stuurprogramma’s

### <a name="nvidia-tesla-cuda-drivers"></a>Stuurprogramma's van NVIDIA Tesla (CUDA)

Stuurprogramma's van NVIDIA Tesla (CUDA) voor NC, NCv2, NCv3, ND en NDv2-serie VM's (optioneel voor NV-serie) worden alleen ondersteund op de besturingssystemen die worden vermeld in de volgende tabel. Downloadkoppelingen stuurprogramma zijn actueel op het moment van publicatie. Ga naar de [NVIDIA](https://www.nvidia.com/)-website voor de nieuwste stuurprogramma's.

> [!TIP]
> Als alternatief voor handmatige installatie van CUDA-stuurprogramma op een Windows Server-VM, kunt u een Azure implementeren [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) installatiekopie. De DSVM-edities van Windows Server 2016 wordt voorafgaand aan installatie NVIDIA CUDA-stuurprogramma's, de CUDA Deep Neural Network-bibliotheek en andere hulpprogramma's.


| OS | Stuurprogramma |
| -------- |------------- |
| Windows Server 2016 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID-stuurprogramma 's

Microsoft herdistribueert u installatieprogramma's van NVIDIA GRID-stuurprogramma voor NV en NVv2-serie VM's gebruikt als virtuele werkstations of voor virtuele toepassingen. Alleen deze GRID-stuurprogramma's installeren op Azure NV Virtual machines, alleen op de besturingssystemen die worden vermeld in de volgende tabel. Deze stuurprogramma's bevatten de licentieverlening voor GRID virtuele GPU-Software in Azure. U hoeft niet voor het instellen van een server met NVIDIA vGPU software-licentie.

| OS | Stuurprogramma |
| -------- |------------- |
| Windows Server 2019<br/><br/>Windows Server 2016<br/><br/>Windows 10 (maximaal 1809-versie. Zonder de patch/patchniveau 0 van 1809 wordt niet ondersteund.) | [RASTER 8.0 (425.31)](https://go.microsoft.com/fwlink/?linkid=874181) (.exe) |
| Windows Server 2012 R2 | [RASTER 8.0 (425.31)](https://go.microsoft.com/fwlink/?linkid=874184) (.exe)  |
