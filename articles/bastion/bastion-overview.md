---
title: Azure bastionhost | Microsoft Docs
description: Meer informatie over Azure bastionhost
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: overview
ms.date: 06/17/2019
ms.author: cherylmc
ms.openlocfilehash: cfd68bbacf4cf8171efdba7878ec8c06055a4997
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191166"
---
# <a name="what-is-azure-bastion-preview"></a>Wat is Azure bastionhost? (Preview)

De service Azure bastionhost is een nieuwe volledig platform beheerde PaaS-service die u in uw virtuele netwerk inricht. Het biedt veilige en naadloze RDP/SSH-verbinding met uw virtuele machines rechtstreeks in Azure portal via SSL. Wanneer u verbinding maakt met Azure Bastion, hebben uw virtuele machines geen openbaar IP nodig.

 Bastionhost biedt beveiligde RDP en SSH-verbindingen voor alle virtuele machines in het virtuele netwerk waarin deze is ingericht. Met behulp van Azure bastionhost voorkomt dat uw virtuele machines die RDP/SSH-poorten naar de buitenwereld terwijl nog steeds veilige toegang met behulp van RDP/SSH. Met Azure bastionhost, u verbinding maken met de virtuele machine rechtstreeks vanuit de Azure portal. U hoeft geen een aanvullende, een agent of een stukje software.

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="architecture"></a>Architectuur

Azure Bastionomgeving is geïmplementeerd in uw virtuele netwerk en eenmaal is geïmplementeerd, biedt de beveiligde RDP/SSH-ervaring voor alle virtuele machines in uw virtuele netwerk. Wanneer u een Azure-bastionhost service in uw virtuele netwerk inrichten, is de RDP/SSH-ervaring beschikbaar voor alle virtuele machines in hetzelfde virtuele netwerk. De implementatie is per virtueel netwerk, niet per abonnement/account of een virtuele machine.

RDP en SSH, zijn sommige van de fundamentele middelen waarmee u verbinding kunt maken met uw workloads worden uitgevoerd in Azure. RDP/SSH-poorten die via Internet is niet de gewenste en gezien als een belangrijke bedreigingen voor aanvallen. Dit komt vaak door protocol beveiligingsproblemen. Als u wilt deze bedreiging surface bevatten, kunt u bastionhost hosts (ook wel bekend als landingspagina-servers) implementeren op de openbare-zijde van het perimeternetwerk. Bastionhost host-servers zijn ontworpen en geconfigureerd voor het bestand is tegen aanvallen. Bastionhost servers ook RDP en SSH-verbinding met de werkbelastingen die zich achter de bastionomgeving, evenals verdere binnen het netwerk.

![architectuur](./media/bastion-overview/architecture.png)

In deze afbeelding toont de architectuur van een Azure-bastionhost-implementatie. In dit diagram:

* De bastionhost wordt geïmplementeerd in het virtuele netwerk.
* De gebruiker verbinding maakt met de Azure-portal met behulp van een HTML5-browser.
* De gebruiker selecteert de virtuele machine verbinding maken met.
* De RDP/SSH-sessie geopend met één klik, in de browser.
* Er is geen openbaar IP-adres is vereist op de virtuele machine van Azure.

## <a name="key-features"></a>Belangrijke functies

De volgende functies zijn beschikbaar om te proberen tijdens de openbare preview:

* **RDP en SSH rechtstreeks in Azure portal:** U kunt rechtstreeks ophalen met de RDP en SSH-sessie rechtstreeks in Azure portal met behulp van een naadloze ervaring voor één klik.
* **Externe sessie via SSL en firewall-traversal voor RDP/SSH:** Azure bastionhost maakt gebruik van een op basis van HTML5-webclient die automatisch worden gestreamd naar uw lokale apparaat, zodat u uw RDP/SSH-sessie ophalen via SSL op poort 443 zodat u kunt veilig zakelijke firewalls passeren.
* **Er is geen openbaar IP-adres vereist op de Azure-VM:** Azure bastionhost wordt geopend de RDP/SSH-verbinding met uw Azure-machine met behulp van privé IP-adres op de virtuele machine. U hoeft geen een openbaar IP-adres op uw virtuele machine.
* **Geen gedoe van het beheer van nsg's:** Azure bastionhost is een volledig beheerd platform PaaS-service van Azure die intern voor het beveiligen van RDP/SSH-verbinding is beveiligd. U moet geen nsg's toepassen op Azure bastionhost subnet. Omdat Azure bastionhost verbinding met uw virtuele machines via privé IP-adres maakt, kunt u uw nsg's zodat RDP/SSH vanuit Azure bastionhost alleen configureren. Hiermee verwijdert u de moeite van het beheer van nsg's telkens wanneer die u moet een beveiligde verbinding met uw virtuele machines.
* **De poort beveiliging opnieuw scannen:** Omdat u niet nodig hebt om uw virtuele machines met openbare Internet zichtbaar te maken, wordt uw virtuele machines zijn beschermd tegen poort scannen rogue en kwaadwillende gebruikers die zich buiten het virtuele netwerk bevinden.
* **Beschermen tegen zero day-aanvallen. Beveiliging op slechts één locatie:** Azure bastionhost is een volledig platform beheerde PaaS-service. Omdat deze zich in het perimeternetwerk van uw virtuele netwerk bevindt, moet u geen zorgen te maken over het beperken van elk van de virtuele machines in uw virtuele netwerk. Het Azure-platform biedt bescherming tegen zero day-aanvallen door de Bastionomgeving Azure beveiligd en altijd up-to-date zijn voor u.

## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [Bastion FAQ](../../includes/bastion-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen

* [Maak een resource van de host Azure bastionhost](bastion-create-host-portal.md).
* Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure.