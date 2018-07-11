---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/29/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: a07f1622f2d41d105c79fa5354d97aa079567e62
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935754"
---
## <a name="supported-operating-systems-and-drivers"></a>Ondersteunde besturingssystemen en stuurprogramma’s

### <a name="nvidia-tesla-cuda-drivers"></a>Stuurprogramma's van NVIDIA Tesla (CUDA)

Stuurprogramma's van NVIDIA Tesla (CUDA) voor NC, NCv2, NCv3 en ND-serie VM's (optioneel voor NV-serie) worden alleen ondersteund op de besturingssystemen die worden vermeld in de volgende tabel. Downloadkoppelingen stuurprogramma zijn actueel op het moment van publicatie. Ga naar de [NVIDIA](http://www.nvidia.com/)-website voor de nieuwste stuurprogramma's.

> [!TIP]
> Als alternatief voor handmatige installatie van CUDA-stuurprogramma op een Windows Server-VM, kunt u een Azure implementeren [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) installatiekopie. De DSVM-edities van Windows Server 2016 wordt voorafgaand aan installatie NVIDIA CUDA-stuurprogramma's, de CUDA Deep Neural Network-bibliotheek en andere hulpprogramma's.


| OS | Stuurprogramma |
| -------- |------------- |
| Windows Server 2016 | [397.44](http://us.download.nvidia.com/Windows/Quadro_Certified/397.44/397.44-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [397.44](http://us.download.nvidia.com/Windows/Quadro_Certified/397.44/397.44-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |


### <a name="nvidia-grid-drivers"></a>NVIDIA GRID-stuurprogramma 's

Microsoft herdistribueert u installatieprogramma's van NVIDIA GRID-stuurprogramma voor NV-serie VM's gebruikt als virtuele werkstations of voor virtuele toepassingen. Alleen deze GRID-stuurprogramma's installeren op Azure NV Virtual machines, alleen op de besturingssystemen die worden vermeld in de volgende tabel. Deze stuurprogramma's bevatten de licentieverlening voor GRID virtuele GPU-Software in Azure.

| OS | Stuurprogramma |
| -------- |------------- |
| Windows Server 2016<br/><br/>Windows 10 | [RASTER 6.2 (391.81)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [RASTER 6.2 (391.81)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |