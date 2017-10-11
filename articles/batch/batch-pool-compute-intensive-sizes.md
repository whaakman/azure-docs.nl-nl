---
title: Rekenintensieve Azure VM's gebruiken met Batch | Microsoft Docs
description: Hoe kunt u profiteren van de RDMA-compatibele of GPU ingeschakeld VM-grootte in Azure Batch-pools
services: batch
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: 
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: danlep
ms.openlocfilehash: c52a054e4fc8f61f871acd9f35b9a3e6247e48ef
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2017
---
# <a name="use-rdma-capable-or-gpu-enabled-instances-in-batch-pools"></a>Gebruik van RDMA-compatibele of GPU ingeschakeld exemplaren in Batch-pools

Voor het uitvoeren van bepaalde Batch-taken, is het raadzaam om te profiteren van Azure VM-grootten die zijn ontworpen voor grootschalige berekeningen. Bijvoorbeeld, om het uitvoeren van meerdere exemplaren [MPI belastingen](batch-mpi.md), kunt u A8, A9, of H-serie-groottes met een netwerk interface voor Remote Direct Memory Access (RDMA). Deze formaten verbinden met een InfiniBand-netwerk voor de communicatie tussen knooppunten, die MPI-toepassingen kan versnellen. Of voor CUDA toepassingen, kunt u N-serie grootten die NVIDIA Tesla afbeeldingen verwerken kaarten GPU (unit) bevatten.

Dit artikel bevat richtlijnen en voorbeelden aan sommige gespecialiseerde grootten van Azure gebruiken in de Batch-pools. Zie voor technische specificaties en achtergrond:

* Hoge prestaties compute-VM-grootten ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* VM-grootten GPU-functionaliteit ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 


## <a name="subscription-and-account-limits"></a>Abonnement en limieten

* **Quota's** -een of meer Azure-quota kunnen beperken het nummer of het type van knooppunten die u aan een Batch-pool toevoegen kunt. U waarschijnlijk worden beperkt wanneer u RDMA-compatibele GPU ingeschakeld of andere multicore VM-grootten kiest. Afhankelijk van het type Batch-account die u hebt gemaakt, kunnen het toepassen van de quota's aan het account zelf of uw abonnement.

    * Als u hebt gemaakt van uw Batch-account in de **Batch-service** configuratie, worden beperkt door de [toegewezen kernen quotum per Batch-account](batch-quota-limit.md#resource-quotas). Standaard is dit quotum 20 kernen. Een afzonderlijke quotum van toepassing op [prioriteit Laag VMs](batch-low-pri-vms.md), als u deze gebruikt. 

    * Als het maken van het account in de **gebruikerabonnement** configuratie, uw abonnement limieten voor het aantal VM cores per regio. Zie [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md). Uw abonnement geldt ook een regionale quotum voor bepaalde VM-grootten, met inbegrip van HPC en GPU-exemplaren. In de Gebruikersconfiguratie abonnement geen extra quota van toepassing op het Batch-account. 

  U moet mogelijk een of meer quota verhogen als u een speciale VM-grootte in Batch. Als u een verhoging van het quotum wilt aanvragen, opent u een [online een ondersteuningsverzoek](../azure-supportability/how-to-create-azure-support-request.md). Hiervoor worden geen kosten in rekening gebracht.

* **Beschikbaarheid in regio's** - rekenintensieve VM's mogelijk niet beschikbaar in de regio's waar u uw Batch-accounts maken. Om te controleren of een grootte beschikbaar zijn, Zie [producten die beschikbaar zijn in elke regio](https://azure.microsoft.com/regions/services/).


## <a name="dependencies"></a>Afhankelijkheden

De RDMA en GPU-mogelijkheden van rekenintensieve grootten worden alleen ondersteund in bepaalde besturingssystemen. Afhankelijk van het besturingssysteem moet u mogelijk installeren of configureren van extra stuurprogramma of andere software. De volgende tabellen geven een overzicht van deze afhankelijkheden. Zie gekoppelde artikelen voor meer informatie. Zie voor de opties voor het configureren van de Batch-pools, verderop in dit artikel.


### <a name="linux-pools---virtual-machine-configuration"></a>Groepen van Linux - Virtuele-machineconfiguratie

| Grootte | Mogelijkheid | Besturingssystemen | Vereiste software | Instellingen voor toepassingen |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances) | RDMA | SUSE Linux Enterprise Server 12 HPC of<br/>Op basis van centOS HPC<br/>(Azure Marketplace) | Intel MPI 5 | Communicatie tussen knooppunten inschakelen, uitschakelen van de uitvoering van gelijktijdige taken |
| [NC reeks *](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-vms) | NVIDIA Tesla R80 GPU | Ubuntu 16.04 TNS.<br/>Red Hat Enterprise Linux 7.3, of<br/>7.3 op basis van CentOS<br/>(Azure Marketplace) | NVIDIA CUDA Toolkit 8.0 stuurprogramma 's | N.v.t. | 
| [NV reeks](../virtual-machines/linux/n-series-driver-setup.md#install-grid-drivers-for-nv-vms) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS<br/>Red Hat Enterprise Linux 7.3<br/>7.3 op basis van CentOS<br/>(Azure Marketplace) | NVIDIA RASTER 4.3 stuurprogramma 's | N.v.t. |

* RDMA-connectiviteit op NC24r VM's wordt op 7.3 HPC met Intel MPI op basis van CentOS ondersteund.



### <a name="windows-pools---virtual-machine-configuration"></a>Windows-groepen - configuratie van virtuele machine

| Grootte | Mogelijkheid | Besturingssystemen | Vereiste software | Instellingen voor toepassingen |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2 of<br/>WindowsServer 2012 (Azure Marketplace) | Microsoft MPI 2012 R2 of hoger, of<br/> Intel MPI 5<br/><br/>HpcVMDrivers Azure VM-extensie | Communicatie tussen knooppunten inschakelen, uitschakelen van de uitvoering van gelijktijdige taken |
| [NC reeks *](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla R80 GPU | WindowsServer 2016 of <br/>Windows Server 2012 R2 (Azure Marketplace) | NVIDIA Tesla stuurprogramma's of CUDA Toolkit 8.0 's| N.v.t. | 
| [NV reeks](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | WindowsServer 2016 of<br/>Windows Server 2012 R2 (Azure Marketplace) | NVIDIA RASTER 4.3 stuurprogramma 's | N.v.t. |

* RDMA-connectiviteit op NC24r VM's wordt ondersteund op Windows Server 2012 R2 met de extensie HpcVMDrivers en Microsoft MPI of Intel MPI.

### <a name="windows-pools---cloud-services-configuration"></a>Windows-groepen - configuratie voor Cloud-services

> [!NOTE]
> N-serie grootten worden niet ondersteund in de Batch-pools met de configuratie van de cloud-services.
>

| Grootte | Mogelijkheid | Besturingssystemen | Vereiste software | Instellingen voor toepassingen |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2<br/>WindowsServer 2012 of<br/>Windows Server 2008 R2 (Gast OS-familie) | Microsoft MPI 2012 R2 of hoger, of<br/>Intel MPI 5<br/><br/>HpcVMDrivers Azure VM-extensie | Communicatie tussen knooppunten, inschakelen<br/> uitvoering van gelijktijdige taken uitschakelen |





## <a name="pool-configuration-options"></a>Groep configuratie-opties

Voor het configureren van een gespecialiseerde VM-grootte voor uw Batch-pool, bieden de Batch-API's en hulpprogramma's voor verschillende opties voor het installeren van vereiste software of stuurprogramma's, met inbegrip van:

* [Begintaak](batch-api-basics.md#start-task) -een installatiepakket voor als een resource-bestand uploaden naar Azure storage-account in dezelfde regio bevinden als het Batch-account. Maak een opdrachtregel start-taak voor het bronbestand voor de achtergrond installeren wanneer de groep van toepassingen wordt gestart. Zie voor meer informatie de [REST API-documentatie](/rest/api/batchservice/add-a-pool-to-an-account#bk_starttask).

  > [!NOTE] 
  > De begintaak moet worden uitgevoerd met verhoogde bevoegdheden (admin) machtigingen en het voor een correcte werking moet wachten.
  >

* [Toepassingspakket](batch-application-packages.md) : een gecomprimeerde installatiepakket toevoegen aan uw Batch-account en het configureren van de verwijzing naar een pakket in de groep. Deze instelling wordt geüpload en het pakket op alle knooppunten in de pool wordt uitgepakt. Als het pakket een installatieprogramma, maakt u een begin taak vanaf de opdrachtregel voor de achtergrond de app te installeren op alle knooppunten van de groep van toepassingen. Installeer het pakket desgewenst wanneer een taak is gepland op een knooppunt worden uitgevoerd.

* [Afbeelding van aangepaste toepassingen](batch-api-basics.md#pool) - maken van een aangepaste Windows of Linux-VM-installatiekopie met stuurprogramma's, software, of andere instellingen die vereist zijn voor de VM-grootte. Als u uw Batch-account in de configuratie van het abonnement hebt gemaakt, geeft u de aangepaste afbeelding voor uw Batch-pool. (Aangepaste installatiekopieën worden niet ondersteund in de configuratie van de Batch-service-accounts.) Aangepaste installatiekopieën kunnen alleen worden gebruikt met toepassingen die in de configuratie van de virtuele machine.

  > [!IMPORTANT]
  > U niet in Batch-pools momenteel een aangepaste installatiekopie gemaakt met beheerde schijven of met Premium-opslag gebruiken.
  >



* [Batch-scheepswerf](https://github.com/Azure/batch-shipyard) configureert automatisch de GPU en RDMA transparant werken met beperkte workloads in Azure Batch. Batch scheepswerf wordt volledig aangedreven met configuratiebestanden. Er zijn veel voorbeeld recept configuraties beschikbaar waarmee GPU en RDMA werkbelastingen, zoals de [CNTK GPU recept](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) die configureert vooraf GPU-stuurprogramma's op virtuele machines N-reeks en cognitieve Toolkit voor Microsoft-software als een Docker-installatiekopie wordt geladen.


## <a name="example-microsoft-mpi-on-an-a8-vm-pool"></a>Voorbeeld: Microsoft MPI op een groep A8-VM

Windows MPI-toepassingen op een pool van Azure A8 knooppunten, hebt u nodig voor het installeren van een ondersteunde MPI-implementatie. Hier vindt u voorbeelden van stappen beschreven voor het installeren van [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) op een Windows-groep met behulp van een Batch-toepassingspakket.

1. Download de [installatiepakket](http://go.microsoft.com/FWLink/p/?LinkID=389556) (MSMpiSetup.exe) voor de meest recente versie van Microsoft MPI.
2. Maak een zipbestand van het pakket.
3. Uploaden van het pakket aan uw Batch-account. Zie voor stappen de [toepassingspakketten](batch-application-packages.md) richtlijnen. Geef een toepassings-id zoals *MSMPI*, en een versie zoals *8.1*. 
4. Maak een pool met de Batch-API's of Azure-portal in de cloud services-configuratie met het gewenste aantal knooppunten en de schaal. De volgende tabel ziet u voorbeelden van instellingen voor het instellen van MPI in met behulp van een begintaak zonder toezicht:

| Instelling | Waarde |
| ---- | ----- | 
| **Type installatiekopie** | Cloudservices |
| **OS-familie** | Windows Server 2012 R2 (OS-familie 4) |
| **De grootte van knooppunt** | A8-standaard |
| **De communicatie tussen knooppunten is ingeschakeld** | True |
| **Maximum aantal taken per knooppunt** | 1 |
| **Verwijzingen naar Application-pakket** | MSMPI |
| **Begintaak ingeschakeld** | True<br>**Vanaf de opdrachtregel** - `"cmd /c %AZ_BATCH_APP_PACKAGE_MSMPI#8.1%\\MSMpiSetup.exe -unattend -force"`<br/>**Gebruikersidentiteit** -Pool autouser, admin<br/>**Wacht tot succes** : True

## <a name="example-nvidia-tesla-drivers-on-nc-vm-pool"></a>Voorbeeld: NVIDIA Tesla stuurprogramma's op de NC-VM-groep

CUDA toepassingen op een pool van Linux NC-knooppunten uitgevoerd, moet u CUDA Toolkit 8.0 installeren op de knooppunten. De Toolkit installeert de benodigde NVIDIA Tesla GPU-stuurprogramma's. Hier volgen voorbeelden van stappen beschreven voor het implementeren van een aangepaste installatiekopie Ubuntu 16.04 TNS met de GPU-stuurprogramma's:

1. Implementeer een Azure-NC6 VM Ubuntu 16.04 TNS uitgevoerd. Bijvoorbeeld, de virtuele machine maken in de regio VS Zuid-centraal. Zorg ervoor dat u de virtuele machine met de standard-opslag maken en *zonder* schijven die worden beheerd.
2. Volg de stappen voor het verbinding maken met de virtuele machine en [CUDA stuurprogramma's installeren](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-vms).
3. De Linux-agent inrichting ervan ongedaan en vervolgens vastleggen Linux VM-installatiekopie met de Azure CLI 1.0-opdrachten. Zie voor stappen [vastleggen van een virtuele Linux-machine uitgevoerd op Azure](../virtual-machines/linux/capture-image-nodejs.md). Noteer de URI van de installatiekopie.
  > [!IMPORTANT]
  > Gebruik geen 2.0 voor Azure CLI-opdrachten voor het vastleggen van de afbeelding voor Azure Batch. De opdrachten CLI 2.0 vastleggen op dit moment alleen virtuele machines die zijn gemaakt met behulp van beheerde schijven.
  >
4. Maak een Batch-account met de configuratie van de gebruiker abonnement in een regio die ondersteuning biedt voor NC virtuele machines.
5. Met de Batch-API's of Azure-portal een pool maken met de aangepaste installatiekopie en met het gewenste aantal knooppunten en de schaal. De volgende tabel ziet u voorbeelden van toepassingen van instellingen voor de installatiekopie:

| Instelling | Waarde |
| ---- | ---- |
| **Type installatiekopie** | Aangepaste installatiekopie |
| **Aangepaste installatiekopie** | Afbeelding URI van het formulier`https://yourstorageaccountdisks.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd` |
| **Knooppunt agent SKU** | batch.node.Ubuntu 16.04 |
| **De grootte van knooppunt** | Standard NC6 |



## <a name="next-steps"></a>Volgende stappen

* Zie voor informatie over het uitvoeren van MPI-taken op een Azure Batch-pool de [Windows](batch-mpi.md) of [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) voorbeelden.

* Zie voor voorbeelden van GPU werkbelastingen op Batch de [Batch scheepswerf](https://github.com/Azure/batch-shipyard/) recepten.