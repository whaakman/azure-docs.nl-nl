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
ms.openlocfilehash: 8f3d57f8f1f3631421618e31e943606a16e6bf5b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34670016"
---
## <a name="supported-operating-systems-and-drivers"></a>Ondersteunde besturingssystemen en stuurprogramma’s

### <a name="nvidia-tesla-cuda-drivers"></a>Stuurprogramma's NVIDIA Tesla (CUDA)

NVIDIA Tesla (CUDA) stuurprogramma's voor NC, NCv2 NCv3 en ND-serie VMs (optioneel voor NV-serie) worden alleen ondersteund op de besturingssystemen die worden vermeld in de volgende tabel. Stuurprogramma downloadkoppelingen zijn huidige op moment van publicatie. Ga naar de [NVIDIA](http://www.nvidia.com/)-website voor de nieuwste stuurprogramma's.

> [!TIP]
> U kunt een Azure implementeren als alternatief voor handmatige installatie op een Windows Server VM CUDA stuurprogramma [gegevens wetenschappelijke virtuele Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) installatiekopie. De edities DSVM voor Windows Server 2016 voorafgaand aan installatie NVIDIA CUDA-stuurprogramma's, de CUDA Deep Neural Network-bibliotheek en andere hulpprogramma's.


| OS | Stuurprogramma |
| -------- |------------- |
| Windows Server 2016 | [397.44](http://us.download.nvidia.com/Windows/Quadro_Certified/397.44/397.44-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [397.44](http://us.download.nvidia.com/Windows/Quadro_Certified/397.44/397.44-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |


### <a name="nvidia-grid-drivers"></a>NVIDIA RASTER stuurprogramma 's

Microsoft herdistribueert NVIDIA RASTER stuurprogramma installatieprogramma's voor NV-serie virtuele machines gebruikt als virtuele werkstations of voor virtuele toepassingen. Installeer alleen deze RASTER stuurprogramma's op Azure NV virtuele machines, alleen op de besturingssystemen die worden vermeld in de volgende tabel. Deze stuurprogramma's omvatten de licentieverlening voor RASTER Virtual GPU-Software in Azure.

| OS | Stuurprogramma |
| -------- |------------- |
| Windows Server 2016<br/><br/>Windows 10 | [RASTER 6.1 (391.58)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [RASTER 6.1 (391.58)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |