---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 06/19/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 4bec8c8ea29c10b8c0d0351a41ebc9183bb45d4f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449508"
---
## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

* Zie voor de beschikbaarheid van virtuele machines uit de N-serie, [producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).

* N-serie VM's kunnen alleen worden geïmplementeerd in het Resource Manager-implementatiemodel.

* N-serie VM's verschillen in het type Azure-opslag worden ondersteund voor de schijven. NC- en NV-VM's alleen ondersteuning voor VM-schijven die worden ondersteund door Standard Disk Storage (HDD). NCv2-, ND- en NCv3-VM's alleen ondersteuning voor VM-schijven die worden ondersteund door Premium Disk Storage (SSD).

* Als u wilt meer dan een paar virtuele machines voor N-serie te implementeren, kunt u overwegen een abonnement op gebruiksbasis of andere Aankoopopties. Als u een [gratis account van Azure](https://azure.microsoft.com/free/) gebruikt, kunt u slechts een paar Azure Compute-resources van Azure gebruiken.

* U moet mogelijk vergroot het quotum voor kernen (per regio) in uw Azure-abonnement en vergroot het quotum voor afzonderlijke voor NC, NCv2, NCv3, ND of NV kernen. Om aan te vragen een quotaverhoging [opent u een ondersteuningsaanvraag online klant](../articles/azure-supportability/how-to-create-azure-support-request.md) gratis. Standaardlimieten kunnen variëren, afhankelijk van de abonnementscategorie voor uw.




