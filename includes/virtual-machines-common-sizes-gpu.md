---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 03/01/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 34b38ff02d401e87be10f1f72cb2025b66317c9e
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
GPU geoptimaliseerd VM grootten gespecialiseerde virtuele machines met één of meerdere NVIDIA GPU's beschikbaar zijn. Deze grootten zijn ontworpen voor rekenintensieve en grafisch-intensieve visualisatie werkbelastingen. In dit artikel bevat informatie over het aantal en type GPU's, Vcpu, gegevensschijven, en NIC's, evenals opslag doorvoer en bandbreedte voor elke grootte in deze groepering van. 

* **NC, NCv2 NCv3 en ND** grootten zijn geoptimaliseerd voor algoritmen, zoals CUDA - en OpenCL-toepassingen en simulaties, AI en grondige Learning en rekenintensieve en netwerk-intensieve toepassingen. 
* **NV** grootten zijn geoptimaliseerd en ontworpen om externe visualisatie, streaming games, codering en VDI-scenario's met behulp van frameworks zoals OpenGL- en DirectX.  


## <a name="nc-series"></a>NC-serie

NC-serie virtuele machines worden mogelijk gemaakt door de [NVIDIA Tesla R80](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) kaart. Gebruikers kunnen crisis door gegevens sneller door gebruik te maken van CUDA voor energie exploratie toepassingen, crash simulaties, ray getraceerde rendering, grondige learning en meer. De configuratie van de NC24r biedt een lage latentie en hoge gegevensdoorvoer netwerkinterface is geoptimaliseerd voor nauw gekoppeld parallelle workloads computing.


| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | Max. aantal gegevensschijven | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 64 | 4 |

1 GPU = halve K80-kaart.

*RDMA-compatibel

## <a name="ncv2-series"></a>NCv2-serie

NCv2-serie VM's zijn gemaakt, aangeboden via [NVIDIA Tesla P100](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) GPU's. Deze GPU's krijgt u meer dan 2 x de rekenkundige prestaties van de NC-serie. Klanten kunnen profiteren van deze bijgewerkte GPU's voor traditionele HPC-workloads zoals modellering van DNA sequentiëren, eiwit analyse, Monte Carlo-simulaties en anderen. De NC24rs v2-configuratie biedt een lage latentie en hoge gegevensdoorvoer netwerkinterface is geoptimaliseerd voor nauw gekoppeld parallelle workloads computing.

> [!IMPORTANT]
> Voor deze familie grootte, is het quotum vCPU (core) in uw abonnement ingesteld op 0 in elke regio. [Aanvragen van een verhoging van het quotum vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze reeks in een [beschikbare regio](https://azure.microsoft.com/regions/services/).
>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | Max. aantal gegevensschijven | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | ---  |
| Standard_NC6s_v2 |6 |112 | 336 | 1 | 12 | 4 |
| Standard_NC12s_v2 |12 |224 | 672 | 2 | 24 | 8 |
| Standard_NC24s_v2 |24 |448 | 1344 | 4 | 32 | 8 |
| Standard_NC24rs_v2* |24 |448 | 1344 | 4 | 32 | 8 |

1 GPU = één P100-kaart.

*RDMA-compatibel

## <a name="ncv3-series"></a>NCv3-serie

NCv3-serie VM's zijn gemaakt, aangeboden via [NVIDIA Tesla V100](http://www.nvidia.com/content/PDF/Volta-Datasheet.pdf) GPU's. Deze GPU's kunnen 1,5 x de rekenkundige prestaties van de NCv2-reeks opgeven. Klanten kunnen profiteren van deze bijgewerkte GPU's voor traditionele HPC-workloads zoals modellering van DNA sequentiëren, eiwit analyse, Monte Carlo-simulaties en anderen. De NC24rs v3-configuratie biedt een lage latentie en hoge gegevensdoorvoer netwerkinterface is geoptimaliseerd voor nauw gekoppeld parallelle workloads computing.

> [!IMPORTANT]
> Voor deze familie grootte, is het quotum vCPU (core) in uw abonnement ingesteld op 0 in elke regio. [Aanvragen van een verhoging van het quotum vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze reeks in een [beschikbare regio](https://azure.microsoft.com/regions/services/).
>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | Max. aantal gegevensschijven | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 |6 |112 | 336 | 1 | 12 | 4 |
| Standard_NC12s_v3 |12 |224 | 672 | 2 | 24 | 8 |
| Standard_NC24s_v3 |24 |448 | 1344 | 4 | 32 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 1344 | 4 | 32 | 8 |

1 GPU = één V100-kaart.

*RDMA-compatibel

## <a name="nd-series"></a>ND-serie

De ND-serie virtuele machines zijn een nieuwe aanvulling op de GPU-familie die zijn bestemd voor AI en grondige Learning werkbelastingen. Ze bieden uitstekende prestaties voor trainings- en Deductie. ND-exemplaren worden van stroom voorzien door [NVIDIA Tesla p 40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU's. Deze instanties bieden uitstekende prestaties voor enkele precisie drijvende punt bewerkingen, voor gebruik van Microsoft cognitieve Toolkit, TensorFlow Caffe en andere frameworks AI-werkbelastingen. De ND-serie biedt daarnaast een veel groter GPU-geheugen (24 GB), waardoor het mogelijk is om veel grotere modellen met een neuraal netwerk in te zetten. Net als bij de NC-serie, krijgen de ND-reeks een configuratie met een secundaire lage latentie, een hoge gegevensdoorvoer netwerk via RDMA, en InfiniBand connectiviteit, zodat u taken van grootschalige training spanning veel GPU's kunt uitvoeren.

> [!IMPORTANT]
> Voor deze reeks grootte is vCPU (core) quota per regio in uw abonnement ingesteld op 0. [Aanvragen van een verhoging van het quotum vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze reeks in een [beschikbare regio](https://azure.microsoft.com/regions/services/).
>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | Max. aantal gegevensschijven | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s |6 |112 | 336 | 1 | 12 | 4 |
| Standard_ND12s |12 |224 | 672 | 2 | 24 | 8 | 
| Standard_ND24s |24 |448 | 1344 | 4 | 32 | 8 |
| Standard_ND24rs* |24 |1448 | 1344 | 4 | 32 | 8 |

1 GPU = één p 40-kaart.

*RDMA-compatibel

## <a name="nv-series"></a>NV-serie

De NV-reeks worden van stroom voorzien door [NVIDIA Tesla M60 ](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU's en NVIDIA RASTER technologie voor bureaublad versnelde toepassingen en virtuele bureaubladen waarop klanten kunnen hun gegevens of simulaties visualiseren. Gebruikers kunnen hun grafisch geheugenintensieve werkstromen op de exemplaren NV krijgen hogere grafische mogelijkheden en ook uitvoeren met enkele precisie werkbelastingen, zoals codering en -rendering visualiseren. 

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | Max. aantal gegevensschijven | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 24 | 1 |
| Standard_NV12 |12 |112 |680 | 2 | 48 | 2 |
| Standard_NV24 |24 |224 |1440 | 4 | 64 | 4 |

1 GPU = halve M60-kaart.


