---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 06/11/2019
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 402379a99be467e9db7fb692d7d532e9d39a54a6
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68700786"
---
Geoptimaliseerde VM-grootten zijn gespecialiseerde virtuele machines die beschikbaar zijn met één of meer NVIDIA-Gpu's. Deze grootten zijn ontworpen voor computerintensieve, grafisch intensieve en visualisatie werk belastingen. Dit artikel bevat informatie over het aantal en het type Gpu's, Vcpu's, gegevens schijven en Nic's. Opslag doorvoer en netwerk bandbreedte worden ook voor elke grootte in deze groepering opgenomen.

* **NC-, NCv2-en NCv3-** grootten zijn geoptimaliseerd voor computerintensieve en netwerkintensieve toepassingen en algoritmen. Enkele voor beelden zijn CUDA-en OpenCL-toepassingen en simulaties, AI en diep gaande lessen. De NCv3-serie is gericht op werk belastingen met hoge prestaties, met de Tesla V100 GPU van NVIDIA. De NC-serie maakt gebruik van de Intel Xeon E5-2690 v3 2,60 GHz v3-processor (Haswell) en de virtuele machines uit de NCv2-en NCv3-serie gebruiken de Intel Xeon E5-2690 v4-processor (Broadwell).

* **ND en NDv2** De ND-serie is gericht op trainings-en ingrijpende scenario's voor diep gaande lessen. De oplossing maakt gebruik van de NVIDIA Tesla P40 GPU en de Intel Xeon E5-2690 v4-processor (Broadwell). De NDv2-serie maakt gebruik van de Intel Xeon Platinum 8168-processor (Skylake).

* De grootte van **NV en NVv3** is geoptimaliseerd en ontworpen voor externe visualisatie-, streaming-, Game-, coderings-en VDI-scenario's met behulp van frameworks zoals OpenGL en DirectX.  Deze Vm's worden ondersteund door de NVIDIA Tesla M60 GPU.

## <a name="nc-series"></a>NC-serie

Premium-opslag:  Niet ondersteund

Premium Storage caching:  Niet ondersteund

Virtuele machines in de NC-serie worden aangedreven door de [Nvidia Tesla K80](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/tesla-product-literature/Tesla-K80-BoardSpec-07317-001-v05.pdf) -kaart en de Intel Xeon E5-2690 v3-processor (Haswell). Gebruikers kunnen sneller gegevens belichten door gebruik te maken van CUDA voor energie onderzoek toepassingen, crash simulaties, Ray traceed rendering, dieper leren en meer. De NC24r-configuratie biedt een netwerk interface met lage latentie en hoge door Voer die is geoptimaliseerd voor nauw gekoppelde werk belastingen op parallelle computers.

| Size | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- | ---- |
| Standard_NC6 |6 |56 | 340 | 1 | 12 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 24 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 48 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 48 | 64 | 4 |

1 GPU = halve K80-kaart.

*RDMA-compatibel

## <a name="ncv2-series"></a>NCv2-serie

Premium-opslag:  Ondersteund

Premium Storage caching:  Ondersteund

Virtuele machines uit de NCv2-serie worden aangedreven door [Nvidia Tesla P100](https://www.nvidia.com/en-us/data-center/tesla-p100/) gpu's. Deze Gpu's kunnen meer dan twee maal de reken prestaties van de NC-serie bieden. Klanten kunnen profiteren van deze bijgewerkte Gpu's voor traditionele HPC-workloads, zoals het maken van tank modellen, DNA-sequentiëren, eiwithoudende analyse, Monte Carlo-simulaties en andere. Naast de Gpu's worden de virtuele machines uit de NCv2-serie ook aangedreven door de Intel Xeon E5-2690 v4-Cpu's (Broadwell).

De NC24rs v2-configuratie biedt een netwerk interface met lage latentie en hoge door Voer die is geoptimaliseerd voor nauw gekoppelde werk belastingen voor parallelle berekeningen.

> [!IMPORTANT]
> Voor deze grootte familie is het vCPU-quotum (core) in uw abonnement in eerste instantie ingesteld op 0 in elke regio. [Vraag een toename van een vCPU-quotum](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze familie aan in een beschik [bare regio](https://azure.microsoft.com/regions/services/).
>

| Size | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD): GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | ---  | ---| --- |
| Standard_NC6s_v2 | 6 |112 | 736 | 1 | 16 | 12 | 20000/ 200 | 4 |
| Standard_NC12s_v2 | 12 |224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v2 | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |
| Standard_NC24rs_v2* | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

1 GPU = One P100-kaart.

*RDMA-compatibel

## <a name="ncv3-series"></a>NCv3-serie

Premium-opslag:  Ondersteund

Premium Storage caching:  Ondersteund

Virtuele machines uit de NCv3-serie worden aangedreven door [Nvidia Tesla V100](https://www.nvidia.com/en-us/data-center/tesla-v100/) gpu's. Deze Gpu's kunnen 1,5 x de reken prestaties van de NCv2-serie bieden. Klanten kunnen profiteren van deze bijgewerkte Gpu's voor traditionele HPC-workloads, zoals het maken van tank modellen, DNA-sequentiëren, eiwithoudende analyse, Monte Carlo-simulaties en andere. De NC24rs v3-configuratie biedt een netwerk interface met lage latentie en hoge door Voer die is geoptimaliseerd voor nauw gekoppelde werk belastingen voor parallelle berekeningen. Naast de Gpu's worden de virtuele machines uit de NCv3-serie ook aangedreven door de Intel Xeon E5-2690 v4-Cpu's (Broadwell).

> [!IMPORTANT]
> Voor deze grootte familie is het vCPU-quotum (core) in uw abonnement in eerste instantie ingesteld op 0 in elke regio. [Vraag een toename van een vCPU-quotum](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze familie aan in een beschik [bare regio](https://azure.microsoft.com/regions/services/).
>

| Size | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD): GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 | 6 |112 | 736 | 1 | 16 | 12 | 20000/200 | 4 |
| Standard_NC12s_v3 | 12 |224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v3 | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

1 GPU = One V100-kaart.

*RDMA-compatibel

## <a name="ndv2-series-preview"></a>NDv2-serie (preview-versie)

Premium-opslag:  Ondersteund

Premium Storage caching:  Ondersteund

InfiniBand Niet ondersteund

Virtuele machine uit de NDv2-serie is een nieuwe toevoeging aan de GPU-familie, ontworpen voor de behoeften van de HPC-, AI-en machine learning-workloads. Het is mogelijk met 8 NVIDIA Tesla V100 NVLINK interconnected Gpu's en 40 Intel Xeon Platinum 8168 (Skylake)-kernen en 672 GiB van het systeem geheugen. NDv2-exemplaren bieden uitstekende FP32- en FP64-prestaties voor HPC- en AI-workloads met behulp van Cuda, TensorFlow, Pytorch, Caffe en andere frameworks.

[Meld u aan en krijg tijdens de preview-periode toegang tot deze machines](https://aka.ms/ndv2signup).
<br>

| Size | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD): GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijf zonder caching: IOPS/MBps | Maximale netwerk bandbreedte | Max. aantal NIC's |
|---|---|---|---|---|---|---|---|---|---|
| Standard_ND40s_v2 | 40 | 672 | 2948 | 8 V100 (NVLink) | 16 | 32 | 80000/800 | 24000 Mbps | 8 |

## <a name="nd-series"></a>ND-serie

Premium-opslag:  Ondersteund

Premium Storage caching:  Ondersteund

De virtuele machines uit de ND-serie vormen een nieuwe toevoeging aan de GPU-familie die is ontworpen voor AI en diep gaande werk belastingen. Ze bieden uitstekende prestaties voor training en demijnen. ND-instanties worden aangedreven door [Nvidia Tesla P40](https://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) -Gpu's en Intel Xeon E5-2690 v4-Cpu's (Broadwell). Deze instanties bieden uitstekende prestaties voor drijvende-komma bewerkingen met één precisie, voor AI-workloads die gebruikmaken van Microsoft Cognitive Toolkit, tensor flow, Caffe en andere frameworks. De ND-serie biedt daarnaast een veel groter GPU-geheugen (24 GB), waardoor het mogelijk is om veel grotere modellen met een neuraal netwerk in te zetten. Net als de NC-serie biedt de ND-serie een configuratie met een secundaire netwerk met lage latentie en een hoge door Voer via RDMA en InfiniBand-connectiviteit, zodat u grootschalige trainings taken kunt uitvoeren die veel Gpu's bespannen.

> [!IMPORTANT]
> Voor deze grootte serie wordt het vCPU (kern quotum) per regio in uw abonnement in eerste instantie ingesteld op 0. [Vraag een toename van een vCPU-quotum](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze familie aan in een beschik [bare regio](https://azure.microsoft.com/regions/services/).
>

| Size | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s | 6 |112 | 736 | 1 | 24 | 12 | 20000/200 | 4 |
| Standard_ND12s | 12 |224 | 1474 | 2 | 48 | 24 | 40000/400 | 8 | 
| Standard_ND24s | 24 |448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |
| Standard_ND24rs* | 24 |448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |

1 GPU = One P40-kaart.

*RDMA-compatibel

## <a name="nv-series"></a>NV-serie

Premium-opslag:  Niet ondersteund

Premium Storage caching:  Niet ondersteund

De virtuele machines van de NV-serie worden aangedreven door [Nvidia Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) gpu's en de NVIDIA Grid-technologie voor desktop-versnelde toepassingen en virtuele Bureau bladen waar klanten hun gegevens of simulaties kunnen visualiseren. Gebruikers kunnen hun grafische intensieve werk stromen op de NV-exemplaren visualiseren om de superieure grafische mogelijkheden te verkrijgen en daarnaast ook werk belastingen met één precisie, zoals code ring en rendering, uit te voeren. Virtuele machines van de NV-serie worden ook aangedreven door de Intel Xeon E5-2690 v3-Cpu's (Haswell).

Elke GPU in NV-exemplaren wordt geleverd met een GRID-licentie. Deze licentie geeft u de flexibiliteit om een NV-exemplaar te gebruiken als een virtueel werk station voor één gebruiker, of 25 gelijktijdige gebruikers kunnen verbinding maken met de virtuele machine voor een virtuele-toepassings scenario.

| Size | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Max. aantal NIC's | Virtuele werk stations | Virtuele toepassingen |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |340 | 1 | 8 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 16 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = halve M60-kaart.

## <a name="nvv3-series--sup1sup"></a>NVv3-serie <sup>1</sup>

Premium-opslag:  Ondersteund

Premium Storage caching:  Ondersteund

De virtuele machines uit de NVv3-serie worden aangedreven door [Nvidia Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) gpu's en de NVIDIA-raster technologie met Intel E5-2690 v4-Cpu's (Broadwell). Deze virtuele machines zijn gericht op GPU-versnelde grafische toepassingen en virtuele Bureau bladen waar klanten hun gegevens willen visualiseren, de resultaten kunnen simuleren om te bekijken, te werken met CAD of inhoud te genereren en te streamen. Daarnaast kunnen deze virtuele machines enkelvoudige, nauwkeurige workloads uitvoeren zoals encoding en renderen. Virtuele machines van NVv3 ondersteunen Premium Storage en komen twee maal zoveel systeem geheugen (RAM) te staan in vergelijking met de bijbehorende voorafgaande NV-serie.  

Elke GPU in NVv3-exemplaren wordt geleverd met een GRID-licentie. Deze licentie geeft u de flexibiliteit om een NV-exemplaar te gebruiken als een virtueel werk station voor één gebruiker, of 25 gelijktijdige gebruikers kunnen verbinding maken met de virtuele machine voor een virtuele-toepassings scenario.

| Size | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's | Virtuele werk stations | Virtuele toepassingen | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV12s_v3 |12 |112 |320 | 1 | 8 | 12 | 20000/200 | 4 | 1 | 25 |
| Standard_NV24s_v3 |24 |224 |640 | 2 | 16 | 24 | 40000/400 | 8 | 2 | 50 |
| Standard_NV48s_v3 |48 |448 |1280 | 4 | 32 | 32 | 80000/800 | 8 | 4 | 100 |

1 GPU = halve M60-kaart.

<sup>1</sup> virtuele machines uit de NVv3-serie beschikken over de Intel Hyper-Threading-technologie
