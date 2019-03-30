---
title: Verbinding maken met een peer-netwerk in Azure Lab Services | Microsoft Docs
description: 'Leer hoe u uw lab-netwerk verbinden met een ander netwerk als een peer. Bijvoorbeeld: uw school/universiteit on-premises netwerk verbinden met virtueel netwerk van het Lab in Azure.'
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: spelluru
ms.openlocfilehash: 465352af52cbc84773e52782233065b3000921e7
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652985"
---
# <a name="connect-your-labs-network-with-a-peer-virtual-network-in-azure-lab-services"></a>Van uw testlab-netwerk verbinden met een virtueel netwerk van de peer in Azure Lab Services 
In dit artikel bevat informatie over uw labs-netwerk met een ander netwerk-peering. 

## <a name="overview"></a>Overzicht
Peering op virtueel netwerk, kunt u naadloos verbinding maken met virtuele netwerken van Azure. Nadat de virtuele netwerken zijn gekoppeld via peering, worden ze voor verbindingsdoeleinden weergegeven als één netwerk. Het verkeer tussen virtuele machines in de gekoppelde virtuele netwerken wordt doorgestuurd via de Microsoft-backbone-infrastructuur, vergelijkbaar met verkeer wordt gerouteerd tussen virtuele machines in hetzelfde virtuele netwerk, via particuliere IP-adressen. Zie voor meer informatie, [peering van virtuele netwerken](../../virtual-network/virtual-network-peering-overview.md).

U moet mogelijk van uw testlab-netwerk verbinden met een virtueel netwerk van de peer in sommige scenario's met inbegrip van de volgende query:

- De virtuele machines in het lab hebben software die is verbonden met on-premises-licentieservers licentie aanvragen
- De virtuele machines in de testomgeving moet toegang tot data-sets (of andere bestanden) op de universiteit van netwerkshares. 

Bepaalde on-premises netwerken zijn verbonden met Azure Virtual Network ofwel via [ExpressRoute](../../expressroute/expressroute-introduction.md) of [virtuele netwerkgateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md). Deze services moeten worden ingesteld buiten Azure Lab-Services. Zie [overzicht van ExpressRoute] voor meer informatie over het verbinden van een on-premises netwerk naar Azure met behulp van ExpressRoute) (.. /expressroute/expressroute-Introduction.MD). Voor on-premises connectiviteit met behulp van een virtuele netwerkgateway, de gateway, opgegeven virtuele netwerk en het lab-account moet zich allemaal in dezelfde regio.


## <a name="configure-at-the-time-of-lab-account-creation"></a>Configureer op het moment van lab-account maken
Tijdens het nieuwe lab-account maken, kunt u een bestaand virtueel netwerk waarin wordt weergegeven in de **virtuele peernetwerk** vervolgkeuzelijst. Het geselecteerde virtuele netwerk is connected(peered) tot labs gemaakt op basis van het lab-account. Alle virtuele machines in labs die zijn gemaakt na het maken van deze wijziging zou hebben toegang tot de resources op het gekoppelde virtuele netwerk. 

![VNet-naar-peer-selecteren](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer.png)

> [!NOTE]
> Zie voor gedetailleerde stapsgewijze instructies voor het maken van een lab-account, [instellen van een lab-account](tutorial-setup-lab-account.md)


## <a name="configure-after-the-lab-is-created"></a>Nadat u hebt gemaakt in het lab configureren
Dezelfde eigenschap kan worden ingeschakeld vanuit de **Labs configuratie** tabblad van de **Lab-Account** pagina als u een peer-netwerk is niet ingesteld op het moment van lab-account maken. Wijzigingen zijn aangebracht aan deze instelling geldt alleen voor de labs die zijn gemaakt na de wijziging.

![In- of uitschakelen van VNet-peering nadat het lab is gemaakt](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer-existing-lab.png) 

Zoals u in de afbeelding ziet, kunt u inschakelen of uitschakelen **virtuele peernetwerk** voor labs in het lab-account. 

> [!IMPORTANT]
> Wijziging van deze instelling geldt alleen voor labs die zijn gemaakt nadat de wijziging wordt aangebracht, niet in de bestaande labs. 



## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Labaccounts maken en beheren als beheerder](how-to-manage-lab-accounts.md)
- [Labs maken en beheren als labeigenaar](how-to-manage-classroom-labs.md)
- [Sjablonen instellen en publiceren als labeigenaar](how-to-create-manage-template.md)
- [Als een lab-gebruiker toegang krijgen tot leslokaallabs](how-to-use-classroom-lab.md)

