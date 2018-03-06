---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 03/01/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 22d37ca30f1319f46a52b96be1c527f6f56719ab
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
## <a name="supported-distributions-and-drivers"></a>Ondersteunde distributies en stuurprogramma 's

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-cuda-drivers"></a>NC, NCv2 NCv3 en ND-serie - NVIDIA CUDA stuurprogramma 's
| Distributie | Stuurprogramma |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 of 7.4<br/><br/> CentOS 7.3 of 7.4 | NVIDIA CUDA 9.1, stuurprogramma vertakking R390 |

> [!IMPORTANT]
> Zorg ervoor dat u installeert of upgrade uit naar de nieuwste CUDA-stuurprogramma's voor uw distributiepunt. Stuurprogramma's die ouder is dan versie R390 mogelijk hebt u problemen met de bijgewerkte Linux kernels.
>

### <a name="nv-series---nvidia-grid-drivers"></a>NV-serie - NVIDIA RASTER stuurprogramma 's

| Distributie | Stuurprogramma |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3<br/><br/>7.3 op basis van CentOS | NVIDIA RASTER 5.2 stuurprogramma vertakking R384|

> [!NOTE]
> Microsoft distribueert NVIDIA RASTER stuurprogramma installatieprogramma's voor virtuele machines NV opnieuw. Alleen deze RASTER stuurprogramma's installeren op Azure NV VM's. Deze stuurprogramma's omvatten de licentieverlening voor RASTER Virtual GPU-Software in Azure.
>

> [!WARNING] 
> Installatie van software van derden in Red Hat-producten kan invloed hebben op de ondersteuningsvoorwaarden van Red Hat. Zie het [Knowledge Base-artikel over Red Hat](https://access.redhat.com/articles/1067).
>
