---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: ee32886ddb74bdbbe0f240310629c8ef26230a68
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
## <a name="deployment-considerations"></a>Overwegingen bij de implementatie
* **Azure-abonnement** – aan meer dan een paar rekenintensieve instanties implementeert, overweeg dan een abonnement op gebruiksbasis of andere Aankoopopties. Als u een [gratis account van Azure](https://azure.microsoft.com/free/) gebruikt, kunt u slechts een paar Azure Compute-resources van Azure gebruiken.

* **Prijzen en beschikbaarheid** -deze VM-grootten worden aangeboden in de standaard alleen prijscategorie. Controleer [producten beschikbaar per regio] (https://azure.microsoft.com/regions/services/) voor beschikbaarheid in een Azure-regio's. 
* **Quotum voor kernen** – mogelijk moet u het quotum voor kernen in uw Azure-abonnement vanuit de standaardwaarde verhogen. Uw abonnement mogelijk ook beperken het aantal kernen dat u in de grootte van families van bepaalde VM, waaronder de H-reeks kunt implementeren. Om aan te vragen een verhoging van het quotum [opent u een ondersteuningsaanvraag online klant](../articles/azure-supportability/how-to-create-azure-support-request.md) zonder kosten. (De standaardlimiet kunnen variëren, afhankelijk van uw abonnementscategorie).
  
  > [!NOTE]
  > Neem contact op met ondersteuning van Azure als u grootschalige die u nodig hebt. Azure-quota tegoed worden beperkt, niet de garanties capaciteit. Ongeacht de quota u zijn alleen in rekening gebracht voor kernen dat u gebruikt.
  > 
  > 
* **Virtueel netwerk** : een Azure [virtueel netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) is niet vereist voor de exemplaren rekenintensieve gebruiken. Echter, voor veel implementaties moet u ten minste een cloud-gebaseerde Azure-netwerk of een site-naar-site-verbinding als u nodig hebt voor toegang tot lokale bronnen. Indien nodig, maakt u een nieuw virtueel netwerk voor het implementeren van de exemplaren. Rekenintensieve VM's toe te voegen aan een virtueel netwerk in een affiniteitsgroep wordt niet ondersteund.
* **Vergroten of verkleinen** – vanwege hun speciale hardware, u kunt alleen de grootte van rekenintensieve exemplaren binnen dezelfde grootte familie (H-serie of rekenintensieve A-serie). Bijvoorbeeld, u kunt alleen het formaat van een VM H-serie van één H-serie grootte naar een andere. Bovendien wordt vergroten of verkleinen van de grootte van een computer intensieve rekenintensieve grootte niet ondersteund.  

## <a name="rdma-capable-instances"></a>RDMA-compatibele exemplaren
Een netwerkinterface voor remote direct memory access (RDMA) connectiviteit zijn uitgerust met een subset van de exemplaren rekenintensieve (H16mr H16r A8 en A9). (Geselecteerde N-serie grootten aangeduid met 'r' zoals NC24r zijn ook RDMA-compatibele). Deze interface is naast de standaard Azure netwerkinterface beschikbaar voor andere VM-groottes. 
  
Deze interface kunt de RDMA-compatibele exemplaren om te communiceren via een netwerk InfiniBand (IB), op FDR tarieven voor H16r H16mr en RDMA-compatibele N-reeks virtuele machines en QDR tarieven voor A8 en A9 virtuele machines. Deze RDMA-mogelijkheden kunnen verbeteren de schaalbaarheid en prestaties van bepaalde toepassingen Message Passing Interface (MPI).

> [!NOTE]
> IP-via IB wordt niet ondersteund in Azure. Alleen RDMA over IB wordt ondersteund.
>

Implementeer de RDMA-compatibele HPC-virtuele machines in dezelfde beschikbaarheidsset of VM-schaalaanpassingsset (bij gebruik van het Azure Resource Manager-implementatiemodel) of de dezelfde cloudservice (bij gebruik van het klassieke implementatiemodel). Aanvullende vereisten voor RDMA-compatibele HPC-virtuele machines toegang tot het netwerk van Azure RDMA volgen.