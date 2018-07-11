---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/29/2018
ms.author: azcspmt;jonbeck;cynthn;danlep
ms.custom: include file
ms.openlocfilehash: 296e92d803bb69376f286aa60cfb4a955b08010f
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "34669339"
---
## <a name="deployment-considerations"></a>Overwegingen bij de implementatie
* **Azure-abonnement** : het implementeren van meer dan een paar rekenintensieve instanties, kunt u een abonnement op gebruiksbasis of andere Aankoopopties. Als u een [gratis account van Azure](https://azure.microsoft.com/free/) gebruikt, kunt u slechts een paar Azure Compute-resources van Azure gebruiken.

* **Prijzen en beschikbaarheid** -deze VM-grootten worden alleen aangeboden in de standaard prijscategorie. Controleren [producten beschikbaar per regio] (https://azure.microsoft.com/regions/services/) voor beschikbaarheid in Azure-regio's. 
* **Quotum voor kerngeheugens** : mogelijk moet u het quotum voor kerngeheugens in uw Azure-abonnement van de standaardwaarde verhogen. Uw abonnement kan het aantal kernen dat u in een bepaalde groottefamilie VM implementeren kunt, met inbegrip van de H-serie ook beperken. Om aan te vragen een quotaverhoging [opent u een ondersteuningsaanvraag online klant](../articles/azure-supportability/how-to-create-azure-support-request.md) gratis. (De standaardlimieten kunnen variëren, afhankelijk van uw abonnementscategorie).
  
  > [!NOTE]
  > Neem contact op met ondersteuning voor Azure hebt u behoeften aan grootschalige capaciteit. Azure-quota tegoed worden beperkt, niet capaciteit garanties. Ongeacht uw quotum u betaalt alleen voor de kernen dat u gebruikt.
  > 
  > 
* **Virtueel netwerk** ; een Azure [virtueel netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) is niet vereist voor het gebruik van de rekenintensieve instanties. Echter, voor veel implementaties moet u ten minste een cloud-gebaseerde Azure-netwerk, of een site-naar-site-verbinding als u nodig hebt voor toegang tot on-premises bronnen. Wanneer dat nodig is, maakt u een nieuw virtueel netwerk voor het implementeren van de exemplaren. Rekenintensieve VM's toe te voegen aan een virtueel netwerk in een affiniteitsgroep wordt niet ondersteund.
* **Vergroten of verkleinen** – vanwege hun gespecialiseerde hardware, u kunt alleen de grootte van rekenintensieve instanties binnen dezelfde grootte familie (H-serie of rekenintensieve A-serie). U kunt bijvoorbeeld alleen een VM uit de H-serie van een grootte van de H-serie naar een ander formaat. Bovendien wordt vergroten of verkleinen van de grootte van een niet-rekenintensieve rekenintensieve grootte niet ondersteund.  

## <a name="rdma-capable-instances"></a>RDMA-compatibele exemplaren
Een subset van de rekenintensieve instanties (H16r, H16mr A8 en A9) zijn voorzien van een netwerkinterface voor remote direct memory access (RDMA) verbinding. (Aangeduid met 'r' zoals NC24r geselecteerde N-serie-grootten zijn ook RDMA-compatibele). Deze interface is naast de standaard Azure netwerkinterface beschikbaar voor andere VM-grootten. 
  
Deze interface kunt de RDMA-functionaliteit om te communiceren via een netwerk InfiniBand (IB), op FDR tarieven voor virtuele machines H16r, H16mr en RDMA-compatibele N-serie, en QDR tarieven voor virtuele machines van A8 en A9-exemplaren. Deze RDMA-mogelijkheden kunnen stimuleren de schaalbaarheid en prestaties van bepaalde Message Passing Interface (MPI)-toepassingen.

> [!NOTE]
> IP-via IB wordt niet ondersteund in Azure. Alleen RDMA over IB wordt ondersteund.
>

Implementeer de RDMA-compatibele HPC-virtuele machines in dezelfde beschikbaarheidsset of VM-schaalset (wanneer u het Azure Resource Manager-implementatiemodel) of dezelfde cloudservice (als u het klassieke implementatiemodel gebruikt). Als u een VM-schaalset gebruikt, zorgt u ervoor dat u de implementatie tot één plaatsingsgroep beperken; bijvoorbeeld in een Resource Manager-sjabloon instellen de *singlePlacementGroup* eigenschap *waar*. Voer de aanvullende vereisten voor RDMA-compatibele HPC-VM's toegang hebben tot de Azure-RDMA-netwerk.