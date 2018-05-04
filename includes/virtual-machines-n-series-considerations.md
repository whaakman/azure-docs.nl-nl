---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 03/19/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 3267f649e360c512a5523ce1d5948719a1969934
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

* Zie voor de beschikbaarheid van virtuele machines N-serie, [producten die beschikbaar zijn in elke regio](https://azure.microsoft.com/regions/services/).

* N-serie VM's kunnen alleen worden geïmplementeerd in het Resource Manager-implementatiemodel.

* N-serie VMs verschillen in het type van Azure Storage ze voor hun schijven ondersteunen. NC en NV virtuele machines alleen ondersteuning voor VM-schijven die worden ondersteund door standaard schijf opslag (HDD). NCv2 ND en NCv3 VM's ondersteunen alleen de VM-schijven die worden ondersteund door Premium schijf opslag (SSD).

* Als u wilt meer dan een paar N-reeks virtuele machines te implementeren, kunt u een abonnement op gebruiksbasis of andere Aankoopopties. Als u een [gratis account van Azure](https://azure.microsoft.com/free/) gebruikt, kunt u slechts een paar Azure Compute-resources van Azure gebruiken.

* Mogelijk moet u het quotum voor kernen (per regio) in uw Azure-abonnement te verhogen en het verhogen van de afzonderlijke quotum voor kernen NC, NCv2, NCv3, ND of NV. Om aan te vragen een verhoging van het quotum [opent u een ondersteuningsaanvraag online klant](../articles/azure-supportability/how-to-create-azure-support-request.md) zonder kosten. Standaardlimiet kunnen variëren, afhankelijk van de abonnementscategorie van uw.




