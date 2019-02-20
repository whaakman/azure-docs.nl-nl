---
title: Kies VM-grootten voor pools - Azure Batch | Microsoft Docs
description: Hoe u kunt kiezen uit de beschikbare VM-grootten voor rekenknooppunten in Azure Batch-pools
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 43094839c9da9b00c97d1dffd53f98a3acd119d5
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56417139"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Kies een VM-grootte voor rekenknooppunten in een Azure Batch-pool

Wanneer u een knooppuntgrootte voor een Azure Batch-pool selecteert, kunt u kiezen uit bijna alle de VM-grootten beschikbaar zijn in Azure. Azure biedt tal van grootten voor Linux en Windows-VM's voor verschillende werkbelastingen.

Er zijn enkele uitzonderingen en beperkingen voor het kiezen van een VM-grootte:

* Sommige families van de virtuele machine of VM-grootten worden niet ondersteund in Batch. 
* Enkele VM-grootten zijn beperkt en moeten specifiek worden ingeschakeld voordat ze kunnen worden toegewezen.

## <a name="supported-vm-families-and-sizes"></a>Ondersteunde VM-reeksen en grootten

### <a name="pools-in-virtual-machine-configuration"></a>Toepassingen in virtuele-machineconfiguratie

Batch-pools in de configuratie van de virtuele Machine ondersteuning voor alle VM-grootten ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)) *behalve* voor het volgende:

| Gezin  | Niet-ondersteunde grootten  |
|---------|---------|
| Basic A-serie | Basic_A0 (A0) |
| A-serie | Standard_A0 |
| B-serie | Alle |
| DC-serie | Alle |
| Extreme geoptimaliseerd voor geheugen | Alle |
| HB-serie<sup>1,2</sup> | Alle |
| Hc-series<sup>1,2</sup> | Alle |
| Lsv2-serie | Alle |
| NDv2-serie<sup>1,2</sup> | Alle |
| NVv2-series<sup>1</sup> | Alle |
| SAP HANA | Alle |


<sup>1</sup> gepland voor ondersteuning.  
<sup>2</sup> kan worden gebruikt door Batch-accounts in de gebruikersabonnementmodus; de gebruikersabonnementmodus Batch-account moet beschikken over het quotum voor kerngeheugens instellen. Zie [configuratie voor de gebruikersabonnementmodus](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode) voor meer informatie.

De volgende VM-grootten worden alleen ondersteund voor knooppunten met lage prioriteit:

| Gezin  | Ondersteunde grootten  |
|---------|---------|
| M-serie | Standard_M64ms |
| M-serie | Standard_M128s |

Andere VM-grootten in de M-serie zijn momenteel niet ondersteund.

### <a name="pools-in-cloud-service-configuration"></a>Groepen in de Cloud-serviceconfiguratie

Batch-pools in de Cloud-serviceconfiguratie ondersteunen alle [VM-grootten voor Cloud Services](../cloud-services/cloud-services-sizes-specs.md) *behalve* voor het volgende:

| Gezin  | Niet-ondersteunde grootten  |
|---------|---------|
| A-serie | ExtraSmall |
| Av2-serie | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="restricted-vm-families"></a>Beperkte VM-families

De volgende VM-families kunnen worden toegewezen in Batch-pools, maar u moet een bepaalde verhoging aanvragen (Zie [in dit artikel](batch-quota-limit.md#increase-a-quota)):

* NCv2-serie
* NCv3-serie
* ND-serie

Deze grootten kunnen alleen worden gebruikt in toepassingen in de configuratie van de virtuele Machine.

## <a name="size-considerations"></a>Overwegingen voor grootte

* **Toepassingsvereisten** -Houd rekening met de kenmerken en vereisten van de toepassing die u op de knooppunten uitvoert. Houd ook rekening met het feit of bijvoorbeeld de toepassing meerdere threads heeft en hoeveel geheugen deze gebruikt. Zo kunt u gemakkelijker de meest geschikte en voordeligste knooppuntgrootte bepalen. Voor meerdere instanties [MPI workloads](batch-mpi.md) of CUDA-toepassingen, kunt u gespecialiseerde [HPC](../virtual-machines/linux/sizes-hpc.md) of [met GPU](../virtual-machines/linux/sizes-gpu.md) VM-grootten, respectievelijk. (Zie [gebruik RDMA-compatibele of met GPU exemplaren in de Batch-pools](batch-pool-compute-intensive-sizes.md).)

* **Taken per knooppunt** -meestal selecteert u een knooppunt in de veronderstelling dat één taak tegelijk op een knooppunt wordt uitgevoerd. Echter, is het mogelijk nuttig om meerdere taken (en waarbij meerdere toepassingsinstanties) [parallel uit voeren](batch-parallel-node-tasks.md) op rekenknooppunten tijdens het uitvoeren van taak. Het is in dit geval kiest u de grootte van een multicore knooppunt aan de toegenomen vraag van de uitvoering van de parallelle taak.

* **Laden van prestatieniveaus voor verschillende taken** -alle knooppunten in een pool zijn even groot is. Als u toepassingen met verschillende systeemvereisten en/of workloadniveaus wilt uitvoeren, moet u afzonderlijke pools maken. 

* **Beschikbaarheid in regio** -een VM-reeks of de grootte mogelijk niet beschikbaar in de regio's waar u uw Batch-accounts maken. Als u wilt controleren of een grootte beschikbaar is, Zie [producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).

* **Quota** : de [cores quota](batch-quota-limit.md#resource-quotas) in de Batch account, het aantal knooppunten van een bepaalde grootte die u aan een Batch-pool toevoegen kunt kunt beperken. Zie voor het aanvragen van een quotaverhoging [in dit artikel](batch-quota-limit.md#increase-a-quota). 

* **Configuratie van de groep** -In het algemeen hebt u meer opties voor VM-grootte wanneer u een pool in de configuratie van de virtuele Machine maakt, vergeleken met de configuratie van Cloud Service.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een gedetailleerd overzicht van Batch, [grootschalige parallelle rekenoplossingen ontwikkelen met Batch](batch-api-basics.md).
* Zie voor meer informatie over het gebruik van rekenintensieve VM-grootten [gebruik RDMA-compatibele of met GPU exemplaren in de Batch-pools](batch-pool-compute-intensive-sizes.md).
