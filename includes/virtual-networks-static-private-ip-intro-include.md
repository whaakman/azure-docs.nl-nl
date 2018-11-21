---
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 11/09/2018
ms.author: genli
ms.openlocfilehash: 5c1caf87f32ddd827b85263ee634d3e15d821124
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52269695"
---
Uw virtuele machines (VM's) van IaaS en PaaS-rolexemplaren in een virtueel netwerk ontvangen automatisch een privé IP-adres van een bereik dat u opgeeft, op basis van de ze zijn verbonden met subnet. Dit adres wordt vastgehouden door de virtuele machines en rollen, totdat ze buiten gebruik gesteld. U buiten gebruik stellen een rol of VM-exemplaar door stoppen via PowerShell, de Azure CLI of Azure portal. In deze gevallen zodra het exemplaar van de VM of het rolexemplaar opnieuw wordt gestart ontvangt deze een beschikbaar IP-adres van de Azure-infrastructuur, die misschien niet dezelfde die het eerder had. Als u de VM of het rolexemplaar-exemplaar van het gastbesturingssysteem afsluit, behoudt hij het IP-adres hebben.  

In bepaalde gevallen wilt u een VM of het rolexemplaar-exemplaar dat een statisch IP-adres, bijvoorbeeld, als uw virtuele machine gaat DNS worden uitgevoerd of wordt een domeincontroller. U kunt dit doen door in te stellen van een statisch privé IP-adres.

