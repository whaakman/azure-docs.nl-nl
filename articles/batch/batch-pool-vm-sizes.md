---
title: VM-grootten voor Azure Batch-pools kiezen | Microsoft Docs
description: Het kiezen van de VM-grootten voor rekenknooppunten in Azure Batch pools beschikbaar
services: batch
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: batch
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: danlep
ms.openlocfilehash: addd1e9314a754b40cc5d49c0299f007580f512f
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Kies een VM-grootte voor rekenknooppunten in een Azure Batch-toepassingen

Wanneer u een knooppuntgrootte voor een Azure Batch-pool selecteert, kunt u kiezen uit bijna alle de VM-grootten beschikbaar in Azure. Azure biedt een aantal formaten voor Linux en Windows-VM's voor verschillende werkbelastingen. 

Er zijn enkele uitzonderingen en beperkingen bij het kiezen van een VM-grootte:
* Sommige VM families of VM-grootten worden niet ondersteund in een Batch. 
* Sommige VM-grootten zijn beperkt en moeten specifiek worden ingeschakeld voordat ze kunnen worden toegewezen.


## <a name="supported-vm-families-and-sizes"></a>Ondersteunde VM-families en grootten

### <a name="pools-in-virtual-machine-configuration"></a>Groepen in de configuratie van de virtuele Machine

Batch-pools in de configuratie van de virtuele Machine ondersteunen alle VM-grootten ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)) *behalve* voor het volgende:

| Familie  | Niet-ondersteunde grootten  |
|---------|---------|
| Basic-A-serie | Basic_A0 (A0) |
| A-serie | Standard_A0 |
| B-serie | Alle |
| Fsv2-serie<sup>*</sup> | Alle |

<sup>*</sup>Grootten in deze serie zijn op het overzicht voor toekomstige ondersteuning.

### <a name="pools-in-cloud-service-configuration"></a>Groepen in de Cloud-serviceconfiguratie

Batch-pools in de configuratie van de Service in de Cloud ondersteunen alle [VM-grootten voor Cloudservices](../cloud-services/cloud-services-sizes-specs.md) *behalve* voor het volgende:

| Familie  | Niet-ondersteunde grootten  |
|---------|---------|
| A-serie | ExtraSmall |
| Av2-serie | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="restricted-vm-families"></a>Beperkte VM families
De volgende families van de virtuele machine kunnen worden toegewezen in Batch-pools, maar u moet een verhoging van specifieke quotum aanvragen (Zie [in dit artikel](batch-quota-limit.md#increase-a-quota)):
* NCv2-serie
* NCv3-serie
* ND-serie

Deze formaten kunnen alleen worden gebruikt in toepassingen in de configuratie van de virtuele Machine.

## <a name="size-considerations"></a>De grootte van overwegingen

* **Toepassingsvereisten** -u kunt de kenmerken en vereisten van de toepassing die u op de knooppunten uitvoeren moet. Houd ook rekening met het feit of bijvoorbeeld de toepassing meerdere threads heeft en hoeveel geheugen deze gebruikt. Zo kunt u gemakkelijker de meest geschikte en voordeligste knooppuntgrootte bepalen. Voor meerdere exemplaren [MPI belastingen](batch-mpi.md) of CUDA toepassingen, overweeg gespecialiseerde [HPC](../virtual-machines/linux/sizes-hpc.md) of [GPU ingeschakeld](../virtual-machines/linux/sizes-gpu.md) VM-groottes, respectievelijk. (Zie [gebruik RDMA-compatibele of GPU ingeschakeld-exemplaren in de Batch-pools](batch-pool-compute-intensive-sizes.md).) 

* **Taken per knooppunt** -het is gebruikelijk om te selecteren van een knooppunt wordt de grootte ervan uitgaande dat één taak tegelijk op een knooppunt wordt uitgevoerd. Echter mogelijk om meerdere taken (en daarom meerdere exemplaren van een toepassing) [parallel worden uitgevoerd](batch-parallel-node-tasks.md) op rekenknooppunten tijdens het uitvoeren van de taak. In dit geval is het gebruikelijk om de knooppuntgrootte van een multicore aan de toegenomen vraag van de uitvoering van parallelle taken te kiezen.

* **Niveaus voor de verschillende taken laden** -alle knooppunten in een pool even groot zijn. Als u toepassingen met verschillende systeemvereisten en/of workloadniveaus wilt uitvoeren, moet u afzonderlijke pools maken. 

* **Beschikbaarheid in regio's** -familie van een virtuele machine of de grootte mogelijk niet beschikbaar in de regio's waar u uw Batch-accounts maken. Om te controleren of een grootte beschikbaar zijn, Zie [producten die beschikbaar zijn in elke regio](https://azure.microsoft.com/regions/services/).

* **Quota's** : de [cores quota](batch-quota-limit.md#resource-quotas) in uw Batch account, het aantal knooppunten van een bepaalde grootte die u aan een Batch-pool toevoegen kunt kunt beperken. Om aan te vragen een verhoging van het quotum, Zie [in dit artikel](batch-quota-limit.md#increase-a-quota). 

* **Configuratie van de groep** -In het algemeen hebt u meer opties voor VM-grootte wanneer u een groep in de virtuele-machineconfiguratie maken, vergeleken met de configuratie van de Cloud-Service.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een gedetailleerd overzicht van Batch [ontwikkelen grootschalige parallelle compute-oplossingen met Batch](batch-api-basics.md).
* Zie voor meer informatie over het gebruik van de VM-grootten rekenintensieve [gebruik RDMA-compatibele of GPU ingeschakeld-exemplaren in de Batch-pools](batch-pool-compute-intensive-sizes.md). 


