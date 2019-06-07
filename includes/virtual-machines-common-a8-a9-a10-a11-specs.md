---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: vermagit
ms.service: virtual-machines
ms.topic: include
ms.date: 05/29/2018
ms.author: azcspmt;jonbeck;cynthn;danlep;amverma
ms.custom: include file
ms.openlocfilehash: 5cbc19d5aade2bbcc8b8dca277352d1b17d1d35a
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755172"
---
## <a name="deployment-considerations"></a>Overwegingen bij de implementatie
* **Azure-abonnement** : het implementeren van meer dan een paar rekenintensieve instanties, kunt u een abonnement op gebruiksbasis of andere Aankoopopties. Als u een [gratis account van Azure](https://azure.microsoft.com/free/) gebruikt, kunt u slechts een paar Azure Compute-resources van Azure gebruiken.

* **Prijzen en beschikbaarheid** -deze VM-grootten worden alleen aangeboden in de standaard prijscategorie. Controleer [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/) voor beschikbaarheid in Azure-regio's. 
* **Quotum voor kerngeheugens** : mogelijk moet u het quotum voor kerngeheugens in uw Azure-abonnement van de standaardwaarde verhogen. Uw abonnement kan het aantal kernen dat u in een bepaalde groottefamilie VM implementeren kunt, met inbegrip van de H-serie ook beperken. Om aan te vragen een quotaverhoging [opent u een ondersteuningsaanvraag online klant](../articles/azure-supportability/how-to-create-azure-support-request.md) gratis. (De standaardlimieten kunnen variëren, afhankelijk van uw abonnementscategorie).
  
  > [!NOTE]
  > Neem contact op met ondersteuning voor Azure hebt u behoeften aan grootschalige capaciteit. Azure-quota tegoed worden beperkt, niet capaciteit garanties. Ongeacht uw quotum u betaalt alleen voor de kernen dat u gebruikt.
  > 
  > 
* **Virtueel netwerk** ; een Azure [virtueel netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) is niet vereist voor het gebruik van de rekenintensieve instanties. Echter, voor veel implementaties moet u ten minste een cloud-gebaseerde Azure-netwerk, of een site-naar-site-verbinding als u nodig hebt voor toegang tot on-premises bronnen. Wanneer dat nodig is, maakt u een nieuw virtueel netwerk voor het implementeren van de exemplaren. Rekenintensieve VM's toe te voegen aan een virtueel netwerk in een affiniteitsgroep wordt niet ondersteund.
* **Vergroten of verkleinen** – vanwege hun gespecialiseerde hardware, u kunt alleen de grootte van rekenintensieve instanties binnen dezelfde grootte familie (H-serie of rekenintensieve A-serie). U kunt bijvoorbeeld alleen een VM uit de H-serie van een grootte van de H-serie naar een ander formaat. Bovendien wordt vergroten of verkleinen van de grootte van een niet-rekenintensieve rekenintensieve grootte niet ondersteund.  

## <a name="rdma-capable-instances"></a>Met RDMA compatibele exemplaren
Een subset van de rekenintensieve instanties (A8, A9, H16r, H16mr, HB en HC) zijn voorzien van een netwerkinterface voor remote direct memory access (RDMA) verbinding. Geselecteerde N-serie beschikbaar aangeduid met 'r', zoals de NC24rs-configuraties (NC24rs_v2 en NC24rs_v3), zijn ook RDMA-functionaliteit. Deze interface is naast de standaard Azure netwerkinterface beschikbaar voor andere VM-grootten. 
  
Deze interface kunt de RDMA-functionaliteit om te communiceren via een netwerk InfiniBand (IB), besturingssysteem EDR tarieven voor HB, HC, FDR tarieven voor virtuele machines H16r, H16mr en RDMA-compatibele N-serie en QDR tarieven voor virtuele machines van A8 en A9-exemplaren. Deze RDMA-mogelijkheden kunnen stimuleren de schaalbaarheid en prestaties van bepaalde Message Passing Interface (MPI)-toepassingen. Zie voor meer informatie over de snelheid, de details in de tabellen op deze pagina.

> [!NOTE]
> In Azure, IP-via IB alleen wordt ondersteund op de SR-IOV ingeschakeld virtuele machines (momenteel HB en HC). RDMA via IB wordt ondersteund voor alle exemplaren van de RDMA-functionaliteit.
>

