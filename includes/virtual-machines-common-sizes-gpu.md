---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 11/14/2018
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: bbf619f27c9239ea1b42287c7e81f23da1d43fcb
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57350475"
---
GPU-geoptimaliseerde VM-grootten zijn gespecialiseerde virtuele machines die beschikbaar zijn met één of meerdere NVIDIA GPU's. Deze grootten zijn ontworpen voor intensieve compute- en grafisch intensieve visualisatie werkbelastingen. In dit artikel bevat informatie over het aantal en type van GPU's, vcpu's, gegevensschijven en NIC's. De doorvoer en netwerkbandbreedte Storage zijn ook opgenomen voor elke grootte in deze groepering. 

* **NC, NCv2, NCv3, ND en NDv2** grootten zijn geoptimaliseerd voor rekenintensieve en netwerkintensieve toepassingen en algoritmen. Enkele voorbeelden zijn CUDA - en opencl-toepassingen en simulaties, AI en Deep Learning. De NCv3-serie is gericht op high-performance computing-workloads met NVIDIA Tesla V100 GPU.  De ND-serie is gericht op trainings- en deductiescenario's voor deep learning. Deze serie maakt gebruikt van NVIDIA Tesla P40 GPU.

* De **NC-serie** is uitgerust met een Intel Xeon® E5-2690 v3 2,60 GHz-processor.

* De **NCSv3**, **NCSv2**, en **ND** grootten voorzien van een Intel Xeon® E5-2690 v4 2,60 GHz.
                      
* **NV en NVv2** grootten zijn geoptimaliseerd en ontworpen voor externe visualisatie, streaming, games, codering, en VDI-scenario's met behulp van frameworks als OpenGL en DirectX.  Deze VM's worden ondersteund door NVIDIA Tesla M60 GPU.


## <a name="nc-series"></a>NC-serie

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

NC-serie VM's worden aangestuurd door de [NVIDIA Tesla R80](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/tesla-product-literature/Tesla-K80-BoardSpec-07317-001-v05.pdf) kaart. Gebruikers kunnen Verwerk gegevens sneller door gebruik te maken van CUDA voor energie-exploratietoepassingen, vastlopen simulaties, ray getraceerde rendering, deep learning en meer. De NC24r-configuratie biedt een lage latentie en hoge doorvoer network interface die is geoptimaliseerd voor nauw gekoppelde werkbelastingen voor parallelle berekeningen.


| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- | ---- |
| Standard_NC6 |6 |56 | 340 | 1 | 8 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 16 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 32 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 32 | 64 | 4 |

1 GPU = halve K80-kaart.

*RDMA-compatibel

## <a name="ncv2-series"></a>NCv2-serie

Premium-opslag:  Ondersteund

Premium Storage Caching:  Ondersteund

NCv2-serie VM's worden aangestuurd door [NVIDIA Tesla P100](https://www.nvidia.com/data-center/tesla-p100/) GPU's. Deze GPU's bieden meer dan 2 x de verwerkingsprestaties van de NC-serie. Klanten kunnen profiteren van deze bijgewerkte GPU's traditionele HPC-workloads, zoals de modellering van reservoirmodellering, DNA sequentiëren, eiwitanalyse, Monte Carlo-simulaties en anderen. De NC24rs versie 2-configuratie biedt een lage latentie en hoge doorvoer network interface die is geoptimaliseerd voor nauw gekoppelde werkbelastingen voor parallelle berekeningen.

> [!IMPORTANT]
> Voor deze serie grootte van is de vCPU (kern) quotum in uw abonnement in eerste instantie ingesteld op 0 in elke regio. [Een vCPU-quotum verhoging](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze serie in een [beschikbare regio](https://azure.microsoft.com/regions/services/).
>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | ---  | --- |
| Standard_NC6s_v2 |6 |112 | 736 | 1 | 16 | 12 | 4 |
| Standard_NC12s_v2 |12 |224 | 1474 | 2 | 32 | 24 | 8 |
| Standard_NC24s_v2 |24 |448 | 2948 | 4 | 64 | 32 | 8 |
| Standard_NC24rs_v2* |24 |448 | 2948 | 4 | 64 | 32 | 8 |

1 GPU = één P100-kaart.

*RDMA-compatibel

## <a name="ncv3-series"></a>NCv3-serie

Premium-opslag:  Ondersteund

Premium Storage Caching:  Ondersteund

NCv3-serie VM's worden aangestuurd door [NVIDIA Tesla V100](http://www.nvidia.com/content/PDF/Volta-Datasheet.pdf) GPU's. Deze GPU's leveren tot 1,5 keer de verwerkingsprestaties van de NCv2-serie. Klanten kunnen profiteren van deze bijgewerkte GPU's traditionele HPC-workloads, zoals de modellering van reservoirmodellering, DNA sequentiëren, eiwitanalyse, Monte Carlo-simulaties en anderen. De NC24rs v3-configuratie biedt een lage latentie en hoge doorvoer network interface die is geoptimaliseerd voor nauw gekoppelde werkbelastingen voor parallelle berekeningen.

> [!IMPORTANT]
> Voor deze serie grootte van is de vCPU (kern) quotum in uw abonnement in eerste instantie ingesteld op 0 in elke regio. [Een vCPU-quotum verhoging](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze serie in een [beschikbare regio](https://azure.microsoft.com/regions/services/).
>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 |6 |112 | 736 | 1 | 16 | 12 | 4 |
| Standard_NC12s_v3 |12 |224 | 1474 | 2 | 32 | 24 | 8 |
| Standard_NC24s_v3 |24 |448 | 2948 | 4 | 64 | 32 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 8 |

1 GPU = één V100-kaart.

*RDMA-compatibel

## <a name="ndv2-series-preview"></a>NDv2-serie (Preview)


Premium-opslag:  Ondersteund

Premium Storage Caching:  Ondersteund

Infiniband: Niet ondersteund


NDv2-serie virtuele machine is een nieuwe toevoeging aan de GPU die is ontworpen voor de behoeften van de HPC-, AI- en machine learning-werkbelastingen. Deze serie wordt aangedreven door 8 onderling gekoppelde NVIDIA Tesla V100 NVLINK-GPU's en 40 Intel Skylake-kernen en een systeemgeheugen van 672 GiB. NDv2-exemplaren bieden uitstekende FP32- en FP64-prestaties voor HPC- en AI-workloads met behulp van Cuda, TensorFlow, Pytorch, Caffe en andere frameworks.

[Meld u aan en krijg toegang tot deze machines tijdens de preview](https://aka.ms/ndv2signup).
<br>


| Grootte              | vCPU | GPU              | Geheugen  | NIC's (max.) | Met maximaal Schijfgrootte           | Met maximaal gegevensschijven (1023 GB elk) | Maximale netwerkbandbreedte | 
|-------------------|------|------------------|---------|------------|--------------------------|--------------------------------|-----------------------|
| Standard_ND40s_v2 | 40   | 8 V100 (NVLink) | 672 GiB | 8          | Tijdelijke 1344 / 2948XIO | 32                             | 24.000 Mbps           |

## <a name="nd-series"></a>ND-serie

Premium-opslag:  Ondersteund

Premium Storage Caching:  Ondersteund

De ND-serie virtuele machines zijn een nieuwe toevoeging aan de GPU-serie die is ontworpen voor AI- en Deep Learning werkbelastingen. Ze bieden uitstekende prestaties voor training en Deductie. ND-exemplaren worden aangestuurd door [NVIDIA Tesla P40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU's. Deze instanties bieden uitstekende prestaties voor enkele precisie zwevende drijvende-kommaberekeningen, voor AI-workloads met Microsoft Cognitive Toolkit, TensorFlow, Caffe en andere frameworks. De ND-serie biedt daarnaast een veel groter GPU-geheugen (24 GB), waardoor het mogelijk is om veel grotere modellen met een neuraal netwerk in te zetten. Net als de NC-serie, de ND-serie biedt een configuratie met een secundair netwerk met lage latentie en hoge doorvoersnelheid via RDMA en InfiniBand-connectiviteit, zodat u grootschalige trainingstaken waarvoor veel GPU's kunt uitvoeren.

> [!IMPORTANT]
> Voor deze serie grootte van is de vCPU (kern) quotum per regio in uw abonnement in eerste instantie ingesteld op 0. [Een vCPU-quotum verhoging](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze serie in een [beschikbare regio](https://azure.microsoft.com/regions/services/).
>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s |6 |112 | 736 | 1 | 24 | 12 | 4 |
| Standard_ND12s |12 |224 | 1474 | 2 | 48 | 24 | 8 | 
| Standard_ND24s |24 |448 | 2948 | 4 | 96 | 32 | 8 |
| Standard_ND24rs* |24 |448 | 2948 | 4 | 96 | 32 | 8 |

1 GPU = één P40-kaart.

*RDMA-compatibel

## <a name="nv-series"></a>NV-serie

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

De NV-serie virtuele machines worden aangestuurd door [NVIDIA Tesla M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU's en NVIDIA GRID technologie voor desktop versneld en virtuele bureaubladen waarop klanten hun gegevens of simulatie kunnen visualiseren zijn. Gebruikers kunnen hun grafisch intensieve werkstromen op de NV-exemplaren grafische mogelijkheden en daarnaast enkelvoudige precisieworkloads, zoals codering en rendering uitvoeren visualiseren. 

Elke GPU in NV-exemplaren wordt geleverd met een licentie RASTER. Deze licentie geeft u de flexibiliteit om te gebruiken een NV-exemplaar als een virtuele werkstation voor één gebruiker of 25 gelijktijdige gebruikers verbinding kunnen maken met de virtuele machine voor een virtuele toepassing-scenario.

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Max. aantal NIC's | Virtuele werkstations | Virtuele toepassingen | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Standard_NV6 |6 |56 |340 | 1 | 8 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 16 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = halve M60-kaart.

## <a name="nvv2-series-preview"></a>NVv2-serie (Preview)

Premium-opslag:  Ondersteund

Premium Storage Caching:  Ondersteund

Virtuele machines uit de NVv2-serie worden aangedreven door [NVIDIA Tesla M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU's en NVIDIA GRID technologie van Intel Broadwell CPU's. Deze virtuele machines zijn bedoeld voor GPU grafische toepassingen versnelde en virtuele bureaubladen waar klanten willen hun gegevens visualiseren, resultaten wilt weergeven, werken met CAD- of render en stream-inhoud te simuleren. Daarnaast kunnen deze virtuele machines enkelvoudige, nauwkeurige workloads uitvoeren zoals encoding en renderen. NVv2 virtuele machines ondersteunt Premiumopslag en worden geleverd met twee keer het systeemgeheugen (RAM) in vergelijking met diens voorganger NV-serie.  

Elke GPU in NVv2 exemplaren wordt geleverd met een licentie RASTER. Deze licentie geeft u de flexibiliteit om te gebruiken een NV-exemplaar als een virtuele werkstation voor één gebruiker of 25 gelijktijdige gebruikers verbinding kunnen maken met de virtuele machine voor een virtuele toepassing-scenario.

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Max. aantal NIC's | Virtuele werkstations | Virtuele toepassingen | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6s_v2 |6 |112 |320 | 1 | 8 | 12 | 4 | 1 | 25 |
| Standard_NV12s_v2 |12 |224 |640 | 2 | 16 | 24 | 8 | 2 | 50 |
| Standard_NV24s_v2 |24 |448 |1280 | 4 | 32 | 32 | 8 | 4 | 100 |

1 GPU = halve M60-kaart.

 
