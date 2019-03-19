---
title: Rekenintensieve Azure-VM's gebruiken met Batch | Microsoft Docs
description: Hoe u kunt profiteren van HPC en GPU VM grootten in Azure Batch-pools
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/17/2018
ms.author: lahugh
ms.openlocfilehash: 3974be886b57fbf685b211369094edf844d96ab6
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57975556"
---
# <a name="use-rdma-or-gpu-instances-in-batch-pools"></a>Gebruik van RDMA of GPU-exemplaren in de Batch-pools

Als u wilt uitvoeren van bepaalde batchtaken, kunt u profiteren van Azure VM-grootten die zijn ontworpen voor grootschalige berekeningen. Bijvoorbeeld:

* Om uit te voeren van meerdere exemplaren [MPI workloads](batch-mpi.md), kiest u uit de H-serie of andere grootten waarvoor een netwerkinterface voor Remote Direct Memory Access (RDMA). Deze grootten verbinden met een InfiniBand-netwerk voor de communicatie tussen knooppunten, die MPI-toepassingen kunt versnellen. 

* Kies grootten uit de N-serie met NVIDIA Tesla grafische verwerking van GPU (unit) kaarten voor CUDA-toepassingen.

In dit artikel bevat richtlijnen en voorbeelden op sommige gespecialiseerde grootten van Azure gebruiken in de Batch-pools. Zie voor technische specificaties en de achtergrond:

* High performance computing-VM-grootten ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* Met GPU VM-grootten ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 

> [!NOTE]
> Bepaalde VM-grootten zijn mogelijk niet in de regio's waar het maken van uw Batch-accounts beschikbaar. Als u wilt controleren of een grootte beschikbaar is, Zie [producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) en [kiest u een VM-grootte voor een Batch-pool](batch-pool-vm-sizes.md).

## <a name="dependencies"></a>Afhankelijkheden

De RDMA of GPU-mogelijkheden van rekenintensieve grootten in Batch worden alleen in bepaalde besturingssystemen ondersteund. (De lijst met ondersteunde besturingssystemen is een subset van die worden ondersteund voor virtuele machines die in deze grootten zijn gemaakt.) Afhankelijk van hoe u uw Batch-pool maakt, moet u mogelijk te installeren of configureren van extra stuurprogramma- of andere software op de knooppunten. De volgende tabellen geven een overzicht van deze afhankelijkheden. Zie de gekoppelde artikelen voor meer informatie. Zie voor de opties voor het configureren van de Batch-pools, verderop in dit artikel.

### <a name="linux-pools---virtual-machine-configuration"></a>Pools voor Linux - VM-configuratie

| Grootte | Mogelijkheid | Besturingssystemen | Vereiste software | Poolinstellingen |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances)<br/>[NC24r, NC24rs_v2, NC24rs_v3, ND24rs<sup>*</sup>](../virtual-machines/linux/n-series-driver-setup.md#rdma-network-connectivity) | RDMA | Ubuntu 16.04 LTS, of<br/>Op basis van centOS HPC<br/>(Azure Marketplace) | Intel MPI 5<br/><br/>Stuurprogramma's voor Linux RDMA | De communicatie tussen knooppunten inschakelen, uitschakelen van de uitvoering van gelijktijdige taken |
| [NC, de NCv2, NCv3, NDv2-serie](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla GPU (verschilt per serie) | Ubuntu 16.04 LTS, of<br/>CentOS 7.3 of 7.4<br/>(Azure Marketplace) | Stuurprogramma's van NVIDIA CUDA of CUDA Toolkit | N/A | 
| [NV, NVv2-serie](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS, of<br/>CentOS 7.3<br/>(Azure Marketplace) | NVIDIA GRID-stuurprogramma 's | N/A |

<sup>*</sup>RDMA-compatibele N-serie-grootten zijn ook NVIDIA Tesla GPU 's

### <a name="windows-pools---virtual-machine-configuration"></a>Pools van Windows - VM-configuratie

| Grootte | Mogelijkheid | Besturingssystemen | Vereiste software | Poolinstellingen |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances)<br/>[NC24r, NC24rs_v2, NC24rs_v3, ND24rs<sup>*</sup>](../virtual-machines/windows/n-series-driver-setup.md#rdma-network-connectivity) | RDMA | WindowsServer 2016, 2012 R2, of<br/>2012 (Azure Marketplace) | Microsoft MPI 2012 R2 of hoger, of<br/> Intel MPI 5<br/><br/>Windows RDMA-stuurprogramma 's | De communicatie tussen knooppunten inschakelen, uitschakelen van de uitvoering van gelijktijdige taken |
| [NC, de NCv2, NCv3, ND, NDv2-serie](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla GPU (verschilt per serie) | WindowsServer 2016 of <br/>2012 R2 (Azure Marketplace) | Stuurprogramma's van NVIDIA CUDA of CUDA Toolkit| N/A | 
| [NV, NVv2-serie](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | WindowsServer 2016 of<br/>2012 R2 (Azure Marketplace) | NVIDIA GRID-stuurprogramma 's | N/A |

<sup>*</sup>RDMA-compatibele N-serie-grootten zijn ook NVIDIA Tesla GPU 's

### <a name="windows-pools---cloud-services-configuration"></a>Windows-pools - Cloud services-configuratie

> [!NOTE]
> N-serie worden niet ondersteund in Batch-pools met de configuratie van de Service in de Cloud.
>

| Grootte | Mogelijkheid | Besturingssystemen | Vereiste software | Poolinstellingen |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | WindowsServer 2016, 2012 R2, 2012, of<br/>2008 R2 (Guest OS family) | Microsoft MPI 2012 R2 of hoger, of<br/>Intel MPI 5<br/><br/>Windows RDMA-stuurprogramma 's | Klik in de communicatie tussen knooppunten inschakelen<br/> uitvoering van gelijktijdige taken uitschakelen |

## <a name="pool-configuration-options"></a>Groep configuratie-opties

U hebt verschillende mogelijkheden voor het installeren van vereiste software of stuurprogramma's voor het configureren van een gespecialiseerde VM-grootte voor uw Batch-pool:

* Voor pools in de configuratie van de virtuele machine, kiest u een vooraf geconfigureerde [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) VM-installatiekopie met stuurprogramma's en -software vooraf is geïnstalleerd. Voorbeelden: 

  * [Op basis van centOS 7.4 HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased74HPC?tab=Overview) -stuurprogramma's voor RDMA en Intel MPI 5.1 bevat

  * [Data Science Virtual Machine](../machine-learning/data-science-virtual-machine/overview.md) bevat voor Linux of Windows - stuurprogramma's van NVIDIA CUDA

  * Linux-installatiekopieën voor Batch-workloads voor containers die ook bevat de GPU- en RDMA-stuurprogramma's:

    * [CentOS (met GPU- en RDMA-stuurprogramma's) voor Azure Batch-pools van container](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-batch.centos-container-rdma?tab=Overview)

    * [Ubuntu-Server (met GPU- en RDMA-stuurprogramma's) voor Azure Batch-pools van container](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-batch.ubuntu-server-container-rdma?tab=Overview)

* Maak een [aangepaste installatiekopie van Windows of Linux-VM](batch-custom-images.md) op die u hebt geïnstalleerd stuurprogramma's, software of andere instellingen die vereist zijn voor de VM-grootte. 

* Maak een Batch [toepassingspakket](batch-application-packages.md) vanuit een ZIP- of het installatieprogramma van de toepassing en het configureren van Batch voor het pakket implementeren op de pool en installeren als bij elk knooppunt is gemaakt. Bijvoorbeeld, als het toepassingspakket een installatieprogramma is, maakt u een [begintaak](batch-api-basics.md#start-task) vanaf de opdrachtregel op de achtergrond installeren de app op alle knooppunten in de pool. Overweeg het gebruik van een toepassingspakket en een pool-begintaak als uw werkbelasting, is afhankelijk van de versie van een bepaald stuurprogramma.

  > [!NOTE] 
  > De begintaak moet worden uitgevoerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder) en deze moet wachten op voltooiing. Langlopende taken verhoogt de tijd voor het inrichten van een Batch-pool.
  >

* [Batch-scheepswerf](https://github.com/Azure/batch-shipyard) configureert automatisch de stuurprogramma's voor GPU- en RDMA transparant werken met beperkte workloads op Azure Batch. Batch scheepswerf is volledig aangestuurd met-configuratiebestanden. Er zijn veel voorbeelden recept configuraties beschikbaar waarmee GPU- en RDMA-werkbelastingen, zoals de [CNTK GPU recept](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) die configureert vooraf GPU-stuurprogramma's op uit de N-serie VM's en Microsoft Cognitive Toolkit-software als een Docker-installatiekopie wordt geladen.


## <a name="example-nvidia-gpu-drivers-on-windows-nc-vm-pool"></a>Voorbeeld: NVIDIA GPU-stuurprogramma's op Windows NC VM-pool

CUDA-toepassingen op een pool van Windows NC knooppunten uitgevoerd, moet u NVDIA GPU-stuurprogramma's installeren. De volgende stappen uit voor voorbeeld gebruiken een toepassingspakket voor het installeren van de NVIDIA GPU-stuurprogramma's. U kunt deze optie selecteren als uw werkbelasting, is afhankelijk van een specifieke versie van de GPU-stuurprogramma.

1. Download een installatiepakket voor de GPU-stuurprogramma's op Windows Server 2016 van de [NVIDIA website](https://www.nvidia.com/Download/index.aspx) - bijvoorbeeld [versie 411.82](https://us.download.nvidia.com/Windows/Quadro_Certified/411.82/411.82-tesla-desktop-winserver2016-international.exe). Sla het bestand lokaal via een korte naam, zoals *GPUDriverSetup.exe*.
2. Maak een zipbestand van het pakket.
3. Uploaden van het pakket aan uw Batch-account. Zie voor stappen de [toepassingspakketten](batch-application-packages.md) richtlijnen. Geef een toepassings-id bijvoorbeeld *GPUDriver*, en de versie zoals *411.82*.
1. Met behulp van de Batch-API's of Azure-portal, een pool maakt in de configuratie van de virtuele machine met het gewenste aantal knooppunten en de schaal. De volgende tabel ziet u voorbeelden van instellingen voor het installeren van de NVIDIA GPU-stuurprogramma's op de achtergrond met behulp van een begintaak:

| Instelling | Value |
| ---- | ----- | 
| **Type installatiekopie** | Marketplace (Linux/Windows) |
| **Publisher** | MicrosoftWindowsServer |
| **Aanbieding** | WindowsServer |
| **SKU** | 2016-Datacenter |
| **Knooppuntgrootte** | NC6 Standard |
| **Toepassingspakketverwijzingen** | GPUDriver, versie 411.82 |
| **De begintaak is ingeschakeld** | True<br>**Vanaf de opdrachtregel** - `cmd /c "%AZ_BATCH_APP_PACKAGE_GPUDriver#411.82%\\GPUDriverSetup.exe /s"`<br/>**Gebruikers-id** -Pool van autouser, admin<br/>**Wachten op voltooiing** : True

## <a name="example-nvidia-gpu-drivers-on-a-linux-nc-vm-pool"></a>Voorbeeld: NVIDIA GPU-stuurprogramma's in een pool NC virtuele Linux-machine

CUDA-toepassingen op een pool met Linux NC knooppunten uitgevoerd, moet u die nodig zijn NVIDIA Tesla GPU-stuurprogramma's installeren vanaf de CUDA-Toolkit. De volgende stappen uit voor voorbeeld maken en implementeren van een aangepaste Ubuntu 16.04 LTS-installatiekopie met de GPU-stuurprogramma's:

1. Implementeer een Azure NC-serie VM waarop Ubuntu 16.04 LTS wordt uitgevoerd. Bijvoorbeeld, de virtuele machine maken in de regio VS Zuid-centraal. 
2. Voeg de [NVIDIA GPU-stuurprogramma's extensie](../virtual-machines/extensions/hpccompute-gpu-linux.md
) aan de virtuele machine met behulp van Azure portal, een clientcomputer die verbinding maakt met de Azure-abonnement of Azure Cloud Shell. U kunt ook de stappen volgen om verbinding maken met de virtuele machine en [CUDA-stuurprogramma's installeren](../virtual-machines/linux/n-series-driver-setup.md) handmatig.
3. Volg de stappen voor het maken van een [momentopname en aangepaste Linux VM-installatiekopie](batch-custom-images.md) voor Batch.
4. Maak een Batch-account in een regio die ondersteuning biedt voor Netwerkcontroller-VM's.
5. Een pool met de Batch-API's of Azure portal, maakt [met behulp van de aangepaste](batch-custom-images.md) en met het gewenste aantal knooppunten en de schaal. De volgende tabel ziet u voorbeelden van toepassingen van instellingen voor de installatiekopie:

| Instelling | Value |
| ---- | ---- |
| **Type installatiekopie** | Aangepaste installatiekopie |
| **Aangepaste installatiekopie** | *Naam van de installatiekopie* |
| **Knooppuntagent-SKU** | batch.node.ubuntu 16.04 |
| **Knooppuntgrootte** | NC6 Standard |

## <a name="example-microsoft-mpi-on-a-windows-h16r-vm-pool"></a>Voorbeeld: Microsoft MPI op een Windows H16r VM-pool

Om uit te voeren Windows MPI-toepassingen op een pool van Azure H16r VM-knooppunten, moet u de extensie HpcVmDrivers configureren en installeren [Microsoft MPI](https://docs.microsoft.com/message-passing-interface/microsoft-mpi). Hier vindt u voorbeeldstappen voor het implementeren van een aangepaste installatiekopie van Windows Server 2016 met de benodigde stuurprogramma's en software:

1. Implementeer een VM in Azure H16r met Windows Server 2016. Bijvoorbeeld, de virtuele machine maken in de regio VS-West. 
2. De extensie HpcVmDrivers toevoegen aan de virtuele machine door [met een Azure PowerShell-opdracht](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances
) vanaf een clientcomputer die verbinding maakt met uw Azure-abonnement, of met behulp van Azure Cloud Shell. 
1. Een extern bureaublad-verbinding maken met de virtuele machine.
1. Download de [installatiepakket](https://www.microsoft.com/download/details.aspx?id=57467) (MSMpiSetup.exe) voor de meest recente versie van Microsoft MPI, en installeer Microsoft MPI.
1. Volg de stappen voor het maken van een [momentopname en aangepaste Windows-VM-installatiekopie](batch-custom-images.md) voor Batch.
1. Een pool met de Batch-API's of Azure portal, maakt [met behulp van de aangepaste](batch-custom-images.md) en met het gewenste aantal knooppunten en de schaal. De volgende tabel ziet u voorbeelden van toepassingen van instellingen voor de installatiekopie:

| Instelling | Value |
| ---- | ---- |
| **Type installatiekopie** | Aangepaste installatiekopie |
| **Aangepaste installatiekopie** | *Naam van de installatiekopie* |
| **Knooppuntagent-SKU** | batch.node.Windows amd64 |
| **Knooppuntgrootte** | H16r Standard |
| **De communicatie tussen knooppunten is ingeschakeld** | True |
| **Maximum aantal taken per knooppunt** | 1 |

## <a name="example-intel-mpi-on-a-linux-h16r-vm-pool"></a>Voorbeeld: Intel MPI in een pool H16r virtuele Linux-machine

Voor het uitvoeren van MPI-toepassingen op een pool van Linux H-serie knooppunten, een optie te gebruiken is de [op basis van CentOS 7.4 HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased74HPC?tab=Overview) -installatiekopie van de Azure Marketplace. Stuurprogramma's voor Linux RDMA en Intel MPI vooraf zijn geïnstalleerd. Deze afbeelding biedt ook ondersteuning voor Docker-container-workloads.

Een pool met behulp van deze installatiekopie met de Batch-API's of Azure portal, maken en met het gewenste aantal knooppunten en de schaal. De volgende tabel bevat instellingen voor voorbeeld van toepassingen:

| Instelling | Value |
| ---- | ---- |
| **Type installatiekopie** | Marketplace (Linux/Windows) |
| **Publisher** | OpenLogic |
| **Aanbieding** | CentOS-HPC |
| **SKU** | 7.4 |
| **Knooppuntgrootte** | H16r Standard |
| **De communicatie tussen knooppunten is ingeschakeld** | True |
| **Maximum aantal taken per knooppunt** | 1 |

## <a name="next-steps"></a>Volgende stappen

* Zie voor informatie over het uitvoeren van MPI-opdrachten op een Azure Batch-pool de [Windows](batch-mpi.md) of [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) voorbeelden.

* Zie voor meer voorbeelden van GPU-workloads in Batch de [Batch scheepswerf](https://github.com/Azure/batch-shipyard/) recepten.