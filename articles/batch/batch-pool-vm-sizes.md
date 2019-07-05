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
ms.date: 07/01/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 1848891a0a37235c9769b3cee18262239e19df5a
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502661"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Kies een VM-grootte voor rekenknooppunten in een Azure Batch-pool

Wanneer u een knooppuntgrootte voor een Azure Batch-pool selecteert, kunt u kiezen uit bijna alle de VM-grootten beschikbaar zijn in Azure. Azure biedt tal van grootten voor Linux en Windows-VM's voor verschillende werkbelastingen.

Er zijn enkele uitzonderingen en beperkingen voor het kiezen van een VM-grootte:

* Sommige-serie VM's of VM-grootten worden niet ondersteund in Batch.
* Enkele VM-grootten zijn beperkt en moeten specifiek worden ingeschakeld voordat ze kunnen worden toegewezen.

## <a name="supported-vm-series-and-sizes"></a>Ondersteunde VM-reeksen en grootten

### <a name="pools-in-virtual-machine-configuration"></a>Toepassingen in virtuele-machineconfiguratie

Batch-pools in de configuratie van de virtuele Machine ondersteunen bijna alle VM-grootten ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)). Zie de volgende tabel voor meer informatie over ondersteunde grootten en -beperkingen.

Aanbiedingen of VM-grootten van een Preview-versie niet wordt vermeld, zijn niet gegarandeerd voor ondersteuning.

| VM-reeks  | Ondersteunde grootten | Groepstoewijzingsmodus is ingesteld op batch-account<sup>1</sup> |
|------------|---------|-----------------|
| Basic A-serie | Alle grootten *behalve* Basic_A0 (A0) | Alle |
| A-serie | Alle grootten *behalve* Standard_A0 | Alle |
| Av2-serie | Alle grootten | Alle |
| B-serie | Geen | Niet beschikbaar |
| DC-serie | Geen | Niet beschikbaar |
| Dv2, Dsv2-serie | Alle grootten | Alle |
| Dv3, Dsv3-serie | Alle grootten | Alle |
| [Geoptimaliseerd voor geheugen-grootten](../virtual-machines/linux/sizes-memory.md) | Geen | Niet beschikbaar |
| Fsv2-serie | Alle grootten | Alle |
| H-serie | Alle grootten | Alle |
| Hb-series<sup>2</sup> | Alle grootten | Alle |
| Hc-series<sup>2</sup> | Alle grootten | Alle |
| Ls-serie | Alle grootten | Alle |
| Lsv2-serie | Geen | Niet beschikbaar |
| M-serie | Standard_M64ms (met lage prioriteit alleen), Standard_M128s (met lage prioriteit alleen) | Alle |  
| NCv2-series<sup>2</sup> | Alle grootten | Alle |
| NCv3-series<sup>2</sup> | Alle grootten | Alle |
| ND-serie<sup>2</sup> | Alle grootten | Alle |
| NDv2-serie | Alle grootten | Modus Gebruikersabonnement |
| NV-serie | Alle grootten | Alle |
| NVv3-serie | Geen | Niet beschikbaar |
| SAP HANA | Geen | Niet beschikbaar |

<sup>1</sup> sommige nieuwere VM-reeksen gedeeltelijk in eerste instantie worden ondersteund. Deze VM-serie kan worden toegewezen door de Batch-accounts waarvoor de **pooltoewijzingsmodus** ingesteld op **gebruikersabonnement**. Zie [beheren van Batch-accounts](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode) voor meer informatie over de configuratie van de Batch-account. Zie [quota en limieten](batch-quota-limit.md) voor meer informatie over het aanvragen van het quotum voor deze VM-serie voor gedeeltelijk ondersteund **gebruikersabonnement** Batch-accounts.  

<sup>2</sup> deze VM-grootten in Batch-pools in de configuratie virtuele Machine wordt toegewezen, maar u moet een specifieke vragen [verhoging](batch-quota-limit.md#increase-a-quota).

### <a name="pools-in-cloud-service-configuration"></a>Groepen in de Cloud-serviceconfiguratie

Batch-pools in de Cloud-serviceconfiguratie ondersteunen alle [VM-grootten voor Cloud Services](../cloud-services/cloud-services-sizes-specs.md) **behalve** voor het volgende:

| VM-reeks  | Niet-ondersteunde grootten |
|------------|-------------------|
| A-serie   | Extra klein       |
| Av2-serie | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

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
