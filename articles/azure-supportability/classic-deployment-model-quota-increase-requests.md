---
title: Azure-klassieke implementatiemodel | Microsoft Docs
description: Model van de klassieke Azure-implementatie
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/20/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 5ae2a56c084116ae8d57a16696223e7990258558
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313261"
---
# <a name="classic-deployment-model"></a>Klassieke implementatiemodel

Klassiek implementatiemodel is de oudere Azure-implementatie modus land een globale vCPU-quotumlimiet voor virtuele machines en virtuele-machineschaalsets dwingt. Klassieke implementatiemodel niet meer wordt aanbevolen en is nu vervangen door Resource Manager-model. Voor meer informatie over de implementatie van deze twee verwijzen modellen en gebruik van Resource Manager naar Resource Manager-implementatiemodel-pagina. Wanneer een nieuw abonnement is gemaakt, wordt een standaardquotum van vcpu's toegewezen aan deze. Telkens wanneer een nieuwe virtuele machine is om te worden geïmplementeerd met het klassieke implementatiemodel gebruikt, moet de som van de nieuwe en bestaande vcpu's gebruik in alle regio's niet langer zijn dan het vCPU-quotum goedgekeurd voor het klassieke implementatiemodel. Meer informatie over quota voor Azure-abonnement en pagina met service-limieten.

U kunt een verhoging in vcpu's limiet voor het klassieke implementatiemodel via Help + ondersteuning-blade of de gebruik + Quota-blade in de portal.

