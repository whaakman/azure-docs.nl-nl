---
title: bestand opnemen
description: bestand opnemen
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 06/17/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e29a9265e010c3f442b742faf62b16dae02739fa
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191180"
---
### <a name="preview"></a>Hoe kan ik deelnemen aan de openbare preview?

U moet voor de onboarding willen deelnemen aan de openbare preview. Gebruik de stappen in [in dit artikel](../articles/bastion/bastion-create-host-portal.md) om een nieuwe bastionhost Azure-resource te maken. Op dit moment wanneer toegang tot en met behulp van deze service, moet u de [Azure portal - preview](https://aka.ms/BastionHost) in plaats van de normale Azure-portal.

### <a name="regions"></a>Welke regio's zijn beschikbaar tijdens de preview?

U kunt implementeren en gebruiken van de Bastionomgeving resources in een van deze preview-regio's via de [Azure portal - preview koppeling](https://aka.ms/BastionHost).

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="portal"></a>Ik kan de resource bastionhost vinden in de Azure-portal. Wat moet ik doen?

Zorg ervoor dat u gebruikmaakt van de [Azure portal - preview koppeling](https://aka.ms/BastionHost), niet de normale Azure portal.

### <a name="publicip"></a>Heb ik een openbaar IP-adres nodig voor mijn virtuele machine?

U hoeft niet in een openbaar IP-adres op de Azure-Machine die u verbinding met de service Azure bastionhost maakt. De service van de Bastionomgeving wordt geopend de RDP/SSH-sessie/verbinding met uw virtuele machine via het privé IP-adres van uw virtuele machine, binnen het virtuele netwerk.

### <a name="rdpssh"></a>Moet ik een RDP of SSH-client?

U hebt geen RDP- of SSH-client nodig voor RDP/SSH-toegang tot uw virtuele Azure-machine in Azure Portal. Gebruik de [Azure portal - preview koppeling](https://aka.ms/BastionHost) voor toegang tot de vlucht Preview-versie van de portal. Zo krijgt u rechtstreeks vanuit de browser RDP/SSH-toegang tot uw virtuele machine.

### <a name="agent"></a>Moet ik een agent die wordt uitgevoerd in virtuele machine van Azure?

U hoeft geen agent of aanvullende software te installeren in uw browser of op uw virtuele Azure-machine. De Bastion-service werkt zonder agent en vereist geen aanvullende software voor RDP/SSH.

### <a name="browsers"></a>Welke browsers worden ondersteund?

Gebruik tijdens de openbare preview-versie, de browser Microsoft Edge of Google Chrome op Windows. Gebruik voor de Apple Mac de browser Google Chrome. Microsoft Edge Chromium wordt ook ondersteund in Windows en op de Mac.

### <a name="roles"></a>Alle rollen die zijn vereist voor toegang tot een virtuele machine zijn?

Als u wilt een verbinding maakt, zijn de volgende rollen zijn vereist:

* Rol van lezer voor de virtuele machine
* Rol van lezer voor de NIC met particuliere IP-adres van de virtuele machine
* Rol van lezer voor de Bastionomgeving voor Azure-resource

### <a name="previewbill"></a>Prijzen - worden gefactureerd voor uw deelname aan de Preview-versie?

U wordt enkel gefactureerd gedeeltelijk tijdens de openbare preview. Er is echter geen SLA die is gekoppeld met uw implementatie. Zie de pagina [prijzen](https://aka.ms/BastionHostPricing) voor meer informatie.