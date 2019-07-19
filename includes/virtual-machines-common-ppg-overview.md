---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 06/28/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 9832ae6010f262322328ec22876a35c89e1bedca
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850325"
---
Het plaatsen van Vm's in één regio vermindert de fysieke afstand tussen de exemplaren. Als u ze in één beschikbaarheids zone plaatst, worden deze ook fysiek dichter bij elkaar gebracht. Naarmate de Azure-footprint groeit, kan één beschikbaarheids zone echter meerdere fysieke data centers omvatten, wat kan leiden tot een netwerk latentie die van invloed is op uw toepassing. 

Als u virtuele machines zo dicht mogelijk wilt ophalen, moet u deze implementeren in een proximity-plaatsings groep.

Een proximity-plaatsings groep is een logische groepering die wordt gebruikt om ervoor te zorgen dat Azure Compute-resources zich fysiek dicht bij elkaar bevinden. Proximity-plaatsings groepen zijn handig voor werk belastingen waarbij lage latentie een vereiste is.


- Lage latentie tussen zelfstandige Vm's.
- Lage latentie tussen virtuele machines in één beschikbaarheidsset of een schaalset voor virtuele machines. 
- Lage latentie tussen zelfstandige Vm's, Vm's in meerdere beschikbaarheids sets of meerdere schaal sets. U kunt meerdere reken bronnen in één plaatsings groep hebben om een toepassing met meerdere lagen samen te brengen. 
- Lage latentie tussen meerdere toepassings lagen met verschillende typen hardware. U kunt bijvoorbeeld de back-end uitvoeren met de M-serie in een beschikbaarheidsset en de front-end op een exemplaar van de D-serie, in een schaalset, in één proximity-plaatsings groep.

> [!IMPORTANT]
> Proximity-plaatsings groepen bevindt zich momenteel in een open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>
> Proximity-plaatsings groepen zijn niet beschikbaar in deze regio's tijdens de preview-versie: **Japan-Oost**, **Australië-Oost** en **India centraal**.


## <a name="best-practices"></a>Aanbevolen procedures 
- Voor de laagste latentie gebruikt u proximity-plaatsings groepen samen met versneld netwerken. Zie [een virtuele Linux-machine maken met versneld netwerken](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [een virtuele Windows-machine maken met versneld netwerken](/azure/virtual-network/create-vm-accelerated-networking-powershell?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)voor meer informatie.
- Implementeer alle VM-grootten in één resource manager-sjabloon. Als u wilt voor komen dat er op hardware wordt gelandd die geen ondersteuning biedt voor alle VM-Sku's en-groottes die u nodig hebt, moet u alle toepassings lagen in één sjabloon toevoegen, zodat ze allemaal tegelijk worden geïmplementeerd.
- Als u uw implementatie met Power shell, CLI of de SDK bijwerkt, wordt er mogelijk een `OverconstrainedAllocationRequest`toewijzings fout weer geven. In dit geval moet u de toewijzing van alle bestaande Vm's stoppen of ongedaan maken en de volg orde wijzigen in het implementatie script om te beginnen met de VM-SKU/-grootten die zijn mislukt. 
- Wanneer u een bestaande plaatsings groep hergebruikt van waaruit Vm's zijn verwijderd, wacht u totdat de verwijdering volledig is voltooid voordat u virtuele machines toevoegt.
- Als latentie uw eerste prioriteit is, plaatst u Vm's in een proximity-plaatsings groep en de volledige oplossing in een beschikbaarheids zone. Maar als tolerantie uw hoogste prioriteit is, kunt u uw instanties verspreiden over meerdere beschikbaarheids zones (een enkele proximity-plaatsings groep kan geen zones omvatten).