---
title: Rekenintensieve Azure-VM's gebruiken met Batch | Microsoft Docs
description: Hoe u kunt profiteren van RDMA-compatibele of met GPU VM-grootten in Azure Batch-pools
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: danlep
ms.openlocfilehash: 6969f0c6a05ebf5b34fb746d2a83b884687ad710
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258252"
---
# <a name="use-rdma-capable-or-gpu-enabled-instances-in-batch-pools"></a>Gebruik van RDMA-compatibele of met GPU exemplaren in de Batch-pools

Als u wilt uitvoeren van bepaalde batchtaken, is het raadzaam om te profiteren van Azure VM-grootten die zijn ontworpen voor grootschalige berekeningen. Bijvoorbeeld, om uit te voeren van meerdere exemplaren [MPI workloads](batch-mpi.md), kunt u A8, A9, of grootten uit de H-serie die beschikken over een netwerk interface voor Remote Direct Memory Access (RDMA). Deze grootten verbinden met een InfiniBand-netwerk voor de communicatie tussen knooppunten, die MPI-toepassingen kunt versnellen. Of voor CUDA-toepassingen, kunt u grootten uit de N-serie met NVIDIA Tesla grafische verwerking van GPU (unit) kaarten.

In dit artikel bevat richtlijnen en voorbeelden op sommige gespecialiseerde grootten van Azure gebruiken in de Batch-pools. Zie voor technische specificaties en de achtergrond:

* High performance computing-VM-grootten ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* Met GPU VM-grootten ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 


## <a name="subscription-and-account-limits"></a>Abonnement en limieten

* **Quota en limieten** : de [quotum voor kerngeheugens per Batch-account](batch-quota-limit.md#resource-quotas) kunt beperken het aantal knooppunten van een bepaalde grootte die u aan een Batch-pool toevoegen kunt. U waarschijnlijk een quotum bereikt wanneer u RDMA-compatibele GPU ingeschakeld of andere multicore VM-grootten. 

  Gebruik van bepaalde VM-families in uw Batch-account, zoals NCv2, NCv3 en ND, is bovendien beperkt vanwege beperkte capaciteit. Gebruik van deze groepen is alleen beschikbaar door aan te vragen een verhoging van de standaardwaarde van 0 kernen.  

  Als u wilt, [een verhoging aanvragen](batch-quota-limit.md#increase-a-quota) gratis.

* **Beschikbaarheid in regio** - rekenintensieve VM's mogelijk niet beschikbaar in de regio's waar u uw Batch-accounts maken. Als u wilt controleren of een grootte beschikbaar is, Zie [producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).


## <a name="dependencies"></a>Afhankelijkheden

De RDMA en GPU-mogelijkheden van rekenintensieve grootten worden alleen in bepaalde besturingssystemen ondersteund. Afhankelijk van uw besturingssysteem moet u mogelijk te installeren of configureren van extra stuurprogramma- of andere software. De volgende tabellen geven een overzicht van deze afhankelijkheden. Zie de gekoppelde artikelen voor meer informatie. Zie voor de opties voor het configureren van de Batch-pools, verderop in dit artikel.


### <a name="linux-pools---virtual-machine-configuration"></a>Pools voor Linux - VM-configuratie

| Grootte | Mogelijkheid | Besturingssystemen | Vereiste software | Poolinstellingen |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances) | RDMA | Ubuntu 16.04 LTS,<br/>SUSE Linux Enterprise Server 12 HPC, of<br/>Op basis van centOS HPC<br/>(Azure Marketplace) | Intel MPI 5 | De communicatie tussen knooppunten inschakelen, uitschakelen van de uitvoering van gelijktijdige taken |
| [NC, de NCv2, NCv3, ND-serie *](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla GPU (verschilt per serie) | Ubuntu 16.04 LTS,<br/>Red Hat Enterprise Linux 7.3 of 7.4, of<br/>CentOS 7.3 of 7.4<br/>(Azure Marketplace) | Stuurprogramma's van NVIDIA CUDA Toolkit | N/A | 
| [NV-serie](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS,<br/>Red Hat Enterprise Linux 7.3, of<br/>CentOS 7.3<br/>(Azure Marketplace) | NVIDIA GRID-stuurprogramma 's | N/A |

* RDMA-verbindingen van RDMA-compatibele N-serie VM's mogelijk [aanvullende configuratie](../virtual-machines/linux/n-series-driver-setup.md#rdma-network-connectivity) die is afhankelijk van de distributie.



### <a name="windows-pools---virtual-machine-configuration"></a>Pools van Windows - VM-configuratie

| Grootte | Mogelijkheid | Besturingssystemen | Vereiste software | Poolinstellingen |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | WindowsServer 2016, 2012 R2, of<br/>2012 (azure Marketplace) | Microsoft MPI 2012 R2 of hoger, of<br/> Intel MPI 5<br/><br/>HpcVMDrivers Azure VM-extensie | De communicatie tussen knooppunten inschakelen, uitschakelen van de uitvoering van gelijktijdige taken |
| [NC, de NCv2, NCv3, ND-serie *](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla GPU (verschilt per serie) | WindowsServer 2016 of <br/>2012 R2 (Azure Marketplace) | NVIDIA Tesla-stuurprogramma's of CUDA Toolkit stuurprogramma 's| N/A | 
| [NV-serie](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | WindowsServer 2016 of<br/>2012 R2 (Azure Marketplace) | NVIDIA GRID-stuurprogramma 's | N/A |

* RDMA-verbindingen van RDMA-compatibele N-serie VM's wordt ondersteund op Windows Server 2016 of Windows Server 2012 R2 (vanuit Azure Marketplace) met de extensie HpcVMDrivers en Microsoft MPI of Intel MPI.

### <a name="windows-pools---cloud-services-configuration"></a>Windows-pools - Cloud services-configuratie

> [!NOTE]
> N-serie worden niet ondersteund in Batch-pools met de configuratie van de Service in de Cloud.
>

| Grootte | Mogelijkheid | Besturingssystemen | Vereiste software | Poolinstellingen |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | WindowsServer 2016, 2012 R2, 2012, of<br/>2008 R2 (Guest OS family) | Microsoft MPI 2012 R2 of hoger, of<br/>Intel MPI 5<br/><br/>HpcVMDrivers Azure VM-extensie | Klik in de communicatie tussen knooppunten inschakelen<br/> uitvoering van gelijktijdige taken uitschakelen |





## <a name="pool-configuration-options"></a>Groep configuratie-opties

Voor het configureren van een gespecialiseerde VM-grootte voor uw Batch-pool, bieden de Batch-API's en hulpprogramma's voor verschillende opties voor het installeren van vereiste software of stuurprogramma's, met inbegrip van:

* [Begintaak](batch-api-basics.md#start-task) -een installatiepakket voor als een resourcebestand uploaden naar Azure storage-account in dezelfde regio als het Batch-account. Maak een begin taak vanaf de opdrachtregel voor het installeren van het bronbestand op de achtergrond wanneer de pool wordt gestart. Zie de [documentatie van de REST-API](/rest/api/batchservice/add-a-pool-to-an-account#bk_starttask) voor meer informatie.

  > [!NOTE] 
  > De begintaak moet worden uitgevoerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder) en deze moet wachten op voltooiing.
  >

* [Toepassingspakket](batch-application-packages.md) : een gecomprimeerd installatiepakket toevoegen aan uw Batch-account en een verwijzing naar het toepassingspakket configureren in de groep. Deze instelling wordt geüpload en wordt het pakket op alle knooppunten in de pool wordt uitgepakt. Als het pakket een installatieprogramma is, maakt u een begin taak vanaf de opdrachtregel voor het installeren van de app op de achtergrond op alle knooppunten in de pool. Installeer het pakket (optioneel) wanneer een taak is gepland om uit te voeren op een knooppunt.

* [Aangepaste groep afbeelding](batch-custom-images.md) : Maak een aangepaste Windows of Linux-VM-installatiekopie met stuurprogramma's, software of andere instellingen die vereist zijn voor de VM-grootte. 

* [Batch-scheepswerf](https://github.com/Azure/batch-shipyard) configureert automatisch de GPU- en RDMA transparant werken met beperkte workloads op Azure Batch. Batch scheepswerf is volledig aangestuurd met-configuratiebestanden. Er zijn veel voorbeelden recept configuraties beschikbaar waarmee GPU- en RDMA-werkbelastingen, zoals de [CNTK GPU recept](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) die configureert vooraf GPU-stuurprogramma's op uit de N-serie VM's en Microsoft Cognitive Toolkit-software als een Docker-installatiekopie wordt geladen.


## <a name="example-microsoft-mpi-on-an-a8-vm-pool"></a>Voorbeeld: Microsoft MPI op een A8 VM-pool

Windows MPI-toepassingen uitvoeren op een pool van Azure A8-knooppunten, moet u een ondersteunde MPI-implementatie te installeren. Hier vindt u voorbeeld van de stappen voor het installeren van [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) op een Windows-groep met behulp van een Batch-toepassingspakket.

1. Download de [installatiepakket](https://go.microsoft.com/FWLink/p/?LinkID=389556) (MSMpiSetup.exe) voor de meest recente versie van Microsoft MPI.
2. Maak een zipbestand van het pakket.
3. Uploaden van het pakket aan uw Batch-account. Zie voor stappen de [toepassingspakketten](batch-application-packages.md) richtlijnen. Geef een toepassings-id bijvoorbeeld *MSMPI*, en de versie zoals *8.1*. 
4. Met behulp van de Batch-API's of Azure-portal, een pool maakt in de cloud services-configuratie met het gewenste aantal knooppunten en de schaal. De volgende tabel ziet u voorbeelden van instellingen MPI instellen in de modus zonder toezicht met behulp van een begintaak:

| Instelling | Waarde |
| ---- | ----- | 
| **Type installatiekopie** | Cloud Services |
| **OS-familie** | Windows Server 2012 R2 (OS family 4) |
| **Knooppuntgrootte** | Standard a8 |
| **De communicatie tussen knooppunten is ingeschakeld** | True |
| **Maximum aantal taken per knooppunt** | 1 |
| **Toepassingspakketverwijzingen** | MSMPI |
| **De begintaak is ingeschakeld** | True<br>**Vanaf de opdrachtregel** - `"cmd /c %AZ_BATCH_APP_PACKAGE_MSMPI#8.1%\\MSMpiSetup.exe -unattend -force"`<br/>**Gebruikers-id** -Pool van autouser, admin<br/>**Wachten op voltooiing** : True

## <a name="example-nvidia-tesla-drivers-on-nc-vm-pool"></a>Voorbeeld: NVIDIA Tesla-stuurprogramma's op de NC-VM-pool

CUDA-toepassingen op een pool met Linux NC knooppunten uitgevoerd, moet u CUDA Toolkit 9.0 installeren op de knooppunten. De Toolkit installeert de benodigde stuurprogramma's van NVIDIA Tesla-GPU's. Hier vindt u voorbeeldstappen voor het implementeren van een aangepaste Ubuntu 16.04 LTS-installatiekopie met de GPU-stuurprogramma's:

1. Implementeer een Azure NC-serie VM waarop Ubuntu 16.04 LTS wordt uitgevoerd. Bijvoorbeeld, de virtuele machine maken in de regio VS Zuid-centraal. Zorg ervoor dat u de virtuele machine met een beheerde schijf maakt.
2. Volg de stappen voor het verbinding maken met de virtuele machine en [CUDA-stuurprogramma's installeren](../virtual-machines/linux/n-series-driver-setup.md).
3. Inrichting van de Linux-agent ongedaan maken en vervolgens [vastleggen van de Linux-VM-installatiekopie](../virtual-machines/linux/capture-image.md).
4. Maak een Batch-account in een regio die ondersteuning biedt voor Netwerkcontroller-VM's.
5. Een pool met de Batch-API's of Azure portal, maakt [met behulp van de aangepaste](batch-custom-images.md) en met het gewenste aantal knooppunten en de schaal. De volgende tabel ziet u voorbeelden van toepassingen van instellingen voor de installatiekopie:

| Instelling | Waarde |
| ---- | ---- |
| **Type installatiekopie** | Aangepaste installatiekopie |
| **Aangepaste installatiekopie** | Naam van de installatiekopie |
| **Knooppuntagent-SKU** | batch.node.ubuntu 16.04 |
| **Knooppuntgrootte** | Standard NC6 |



## <a name="next-steps"></a>Volgende stappen

* Zie voor informatie over het uitvoeren van MPI-opdrachten op een Azure Batch-pool de [Windows](batch-mpi.md) of [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) voorbeelden.

* Zie voor meer voorbeelden van GPU-workloads in Batch de [Batch scheepswerf](https://github.com/Azure/batch-shipyard/) recepten.